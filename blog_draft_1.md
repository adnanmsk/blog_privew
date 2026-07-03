# From Smartphone to 3D: Reconstructing a Cricket Bat's Trajectory with 6 AI Models

*How I chained YOLO, Grounding DINO, SAM, Depth Anything, PCA, and a Kalman filter to track a cricket bat in three dimensions — using nothing but a phone camera.*

---

<!-- [IMAGE: Hero composite — raw video frame on left, 3D trajectory animation on right] -->

## The Problem No One Has Solved Cheaply

Cricket coaching, at every level from gully cricket to international stadiums, relies on one thing: the coach's eyes. A coach watches a batsman play a cover drive and says "your bat face was open" or "you were playing across the line." This is subjective. It depends on the angle, the lighting, and whether the coach blinked at the wrong moment.

Professional systems exist to solve this. Hawk-Eye uses six synchronized cameras. Vicon motion capture uses reflective markers glued to the bat and infrared arrays. These systems cost upwards of $100,000 and require permanent installation at a venue.

So I asked a question that turned into a 3-month engineering project:

> **Can a single smartphone video give us the full 3D trajectory of a cricket bat swing?**

The answer, it turns out, is yes — if you're willing to chain together six different AI models, a Kalman filter, and quite a lot of duct tape.

---

## What We're Building

The pipeline I built — internally called **BatPlane** — takes a raw cricket video (the kind your friend films on their phone during a net session) and produces:

1. **A stabilized, trimmed clip** focused only on the actual shot
2. **A pixel-perfect bat mask** for every frame
3. **A dense depth map** for every frame
4. **The exact 3D coordinates** (X, Y, Z) of the bat's blade tip and handle
5. **An animated 3D visualization** showing the bat's swept trajectory as a colored ribbon in space

All of this runs on a single consumer GPU in about 30 seconds per video. I've batch-processed 60 videos without a single failure.

<!-- [VIDEO/GIF: The final side_by_side_4in1.mp4 output showing all 4 panels] -->

---

## The Architecture at a Glance

The pipeline has 7 stages. Here's the full stack:

| Stage | What Happens | Model / Algorithm |
|-------|-------------|-------------------|
| 1. **Trim** | Isolate the exact moment the batsman is playing a shot | YOLO11x + YOLOv8-Pose |
| 2. **Stabilize** | Lock the camera onto the cricket pitch | Grounding DINO + SAM 2 |
| 3a. **Depth** | Generate per-pixel depth maps | Depth Anything V2 Large |
| 3b. **Segment** | Isolate the bat from every frame | SAM 3.1 Multiplex |
| 4. **Keypoints** | Find the blade tip and handle coordinates | PCA + YOLOv8-Pose wrists |
| 5. **3D Fusion** | Merge 2D keypoints with depth; fill gaps | 9-State Kalman RTS Smoother |
| 6. **Render** | Animate the 3D trajectory | Matplotlib + FFmpeg |
| 7. **Export** | Compile side-by-side comparison video | OpenCV + FFmpeg |

All six models are loaded once (~10 seconds) and reused across every video. There is zero fine-tuning — every model is used off-the-shelf with its pretrained weights.

---

## Stage 1: Finding the Shot in the Noise

A typical cricket net session video is 10–20 seconds long. The actual bat swing occupies around 1 second of that. The rest is the bowler walking back, the batsman adjusting their guard, or the camera person chatting.

The trimmer uses two YOLO models simultaneously:

- **YOLO11x** scans every frame for the `baseball bat` class (COCO class ID 34 — the closest thing COCO has to a cricket bat)
- **YOLOv8x-Pose** detects the batsman and extracts their skeleton, including wrist positions

For every frame, the trimmer computes a weighted matching score between each detected bat box and each detected person:

$$\text{Score} = 0.50 \times \text{IoU} + 0.30 \times \text{BottomDist} + 0.15 \times \text{AspectRatio} + 0.05 \times \text{RelPos}$$

Frames where a bat is found near a person are grouped into contiguous blocks, and the longest block is selected as the "action window." This typically reduces a 200-frame video down to ~25 frames of pure swing.

<!-- [IMAGE: Frame timeline showing bat detection confidence, with the selected block highlighted] -->

---

## Stage 2: Killing the Camera Shake

This is the stage that makes everything else possible. Without it, the entire pipeline falls apart.

Here's the problem: when a person films cricket on their phone, the camera pans to follow the batsman. This means the bat's pixel coordinates shift between frames even when the bat hasn't moved. If you try to project these shifting 2D coordinates into 3D space, you get garbage.

The solution is to lock the coordinate system to something that doesn't move: the cricket pitch.

Every cricket pitch has two white crease lines — the popping crease and the bowling crease. These are painted on the ground and don't move. If we can find them in every frame, we can compute a warp that transforms every frame so the crease lines are always in the same pixel position.

**Finding the crease lines with zero training data:**

This is where Grounding DINO earns its keep. I feed it the text prompt:

```
"the vertical white line . white boundary line"
```

Grounding DINO has never seen a cricket pitch in its training data. But it understands language. It finds the white lines. On the first frame where it detects two valid candidates (tall, narrow, vertically-oriented white objects in the top 60% of the frame), we feed those detection boxes into **SAM 2** as point prompts. SAM 2 then propagates the segmentation forward and backward through every frame in the video, giving us the exact pixel mask of both crease lines across the entire clip.

From the masks, we extract each line's center x-coordinate and top y-coordinate per frame, then compute a simple affine warp:

```python
scale = TARGET_PITCH_WIDTH / max(10, right_x - left_x)
M = np.float32([
    [scale, 0, TARGET_LEFT_X - left_x * scale],
    [0,     scale, TARGET_LINE_Y - left_y_top * scale]
])
out = cv2.warpAffine(frame, M, (1280, 1280))
```

Every frame is now projected into a fixed 1280x1280 coordinate system where the crease lines are always at the same pixel positions. The camera shake is gone.

<!-- [IMAGE: Before/after — raw shaky frame vs. stabilized frame] -->

---

## Stage 3a: Seeing Depth from a Single Camera

A phone camera captures a 2D image. There is no Z-axis. But we need depth to reconstruct a 3D trajectory.

**Depth Anything V2 Large** is a 335-million-parameter transformer that has been trained on millions of images to predict relative depth from a single image. It doesn't give you absolute distances in meters, but it tells you which parts of the image are closer to the camera and which are farther away. For our purposes, relative depth is sufficient.

The model outputs a disparity map (where higher values = closer). We convert this to relative depth:

$$\text{Depth} = \frac{1.0}{\text{Disparity} + \epsilon}$$

One critical detail: the stabilization stage adds black padding borders around the warped frames. If these black pixels get non-zero depth values, they corrupt the downstream statistics. So we explicitly zero out any pixel in the depth map where the corresponding RGB pixel is pure black:

```python
black_mask = np.all(frames[fi] == 0, axis=-1)
depth_map[black_mask] = 0.0
```

<!-- [IMAGE: RGB frame alongside its TURBO-colormapped depth map] -->

---

## Stage 3b: The Bat vs. Wicket War

This is where the real engineering happened.

**SAM 3.1 Multiplex** is a 3.5 GB video segmentation model that can track objects across frames with remarkable temporal consistency. You give it a text prompt — `"bat"` — and it finds the bat in one frame and tracks it through the entire video.

Except it doesn't just find the bat. It also finds the wicket stumps. And sometimes the batting pad. And sometimes the edge of the pitch.

Cricket bats and wicket stumps look almost identical to a segmentation model: long, thin, vertical objects made of wood. In early experiments, SAM locked onto the stumps instead of the bat in roughly 40% of videos.

### The Dual-Session Subtraction Trick

The fix is to run SAM twice:

1. **Session 1:** Track `"bat"` -> get bat candidate masks (which may include stumps)
2. **Session 2:** Track `"wicket"` -> get wicket masks
3. **Subtract:** `clean_mask = bat_mask & ~wicket_mask`

After subtraction, connected components smaller than a minimum area threshold are removed as noise blobs. Then, a **wicket exclusion zone** is computed from the average bounding rectangle of all wicket masks — any remaining blob that overlaps significantly with this zone and is too small to be the bat gets discarded.

Finally, temporal continuity is enforced: across frames, we track the centroid of the selected blob, preferring the blob closest to the previous frame's centroid. This prevents the tracker from jumping between the bat and random noise.

```python
# The core subtraction
clean_masks = {f: bat_masks[f] & ~wkt_masks[f] for f in all_frames}
```

This dual-session approach isn't cricket-specific. It's a general technique: whenever SAM confuses two similar objects, track both and subtract.

<!-- [IMAGE: Side-by-side — raw bat mask with stumps contamination vs. clean mask after subtraction] -->

---

## Stage 4: Turning a Blob into a Vector

At this point, we have a clean binary mask of the bat in every frame. But a mask is just a blob of white pixels. We need the exact coordinates of two specific points: the **blade tip** (the bottom of the bat) and the **handle** (the top, where the batsman grips).

### PCA for Orientation

We apply Principal Component Analysis to the mask pixel coordinates. The first principal component gives us the bat's major axis — the line along which the bat is oriented:

```python
ys, xs = np.where(bat_mask)
cx, cy = xs.mean(), ys.mean()
pts = np.column_stack([xs - cx, ys - cy])
cov = (pts.T @ pts) / len(pts)
eigvals, eigvecs = np.linalg.eigh(cov)
major_axis = eigvecs[:, 1]  # Largest eigenvalue
```

This gives us two endpoints along the bat's axis. But which end is the tip and which is the handle?

### Wrist Tracking for Classification

**YOLOv8-Pose** detects the batsman's wrists (COCO keypoints 9 and 10). The endpoint of the bat axis that is closest to the wrist midpoint is classified as the **handle**. The opposite end is the **tip**.

If wrist detection fails (which happens when the batsman's hands are occluded), we fall back to a **width vote**: the tip of a cricket bat is wider than the handle (the blade flares out), so the end of the mask with more pixels is classified as the tip.

<!-- [IMAGE: PCA axis overlaid on bat mask, with tip and handle endpoints marked] -->

---

## Stage 5: The Mathematical Heart — 3D Fusion and Kalman Smoothing

Now we have, for each frame:
- The 2D coordinates (X, Y) of the blade tip and handle from Stage 4
- A dense depth map from Stage 3a

We sample the depth map at the exact (X, Y) location of each keypoint to get its Z-coordinate. This gives us raw 3D observations.

But these observations are noisy. Monocular depth estimation has no temporal consistency — the depth value at the same physical point can flicker between frames. And in some frames, the bat is partially occluded, so we get no observation at all.

### The 9-State Kalman RTS Smoother

To produce smooth, gap-free 3D trajectories, I use a **Rauch-Tung-Striebel (RTS) smoother** — a two-pass extension of the Kalman filter.

The state vector tracks position, velocity, and acceleration in all three dimensions:

$$X_t = [x, y, z, \dot{x}, \dot{y}, \dot{z}, \ddot{x}, \ddot{y}, \ddot{z}]^T$$

The constant-acceleration transition matrix is:

```python
F = [[1, 0, 0, 1, 0, 0, 0.5, 0,   0  ],
     [0, 1, 0, 0, 1, 0, 0,   0.5, 0  ],
     [0, 0, 1, 0, 0, 1, 0,   0,   0.5],
     [0, 0, 0, 1, 0, 0, 1,   0,   0  ],
     [0, 0, 0, 0, 1, 0, 0,   1,   0  ],
     [0, 0, 0, 0, 0, 1, 0,   0,   1  ],
     [0, 0, 0, 0, 0, 0, 1,   0,   0  ],
     [0, 0, 0, 0, 0, 0, 0,   1,   0  ],
     [0, 0, 0, 0, 0, 0, 0,   0,   1  ]]
```

Why 9 states instead of 3? Because a bat swing isn't constant-velocity. It accelerates through the downswing, reaches peak speed at impact, and decelerates during the follow-through. A position-only Kalman filter would assume straight-line motion during occlusions. The acceleration states allow the filter to predict curved trajectories through gaps.

The RTS smoother runs two passes:
1. **Forward pass** (standard Kalman filter): estimates states using only past observations
2. **Backward pass** (RTS equations): corrects the forward estimates using future observations

This means that if the bat disappears behind the batsman's body for 3 frames, the smoother uses *both* the frames before and after the occlusion to infer where the bat was — producing much better gap-filled trajectories than a forward-only filter.

Before feeding observations into the filter, I apply **depth outlier rejection**: any depth sample more than 3 sigma from the frame's mean depth is discarded. This handles the occasional frame where Depth Anything produces a wildly incorrect estimate.

---

## Stage 6: The 3D Animation

The final trajectory is visualized as a **ruled-surface ribbon** — the swept plane of the bat face. At each frame, a quadrilateral is drawn between the previous and current positions of the tip and handle, creating a surface that shows the bat's path through 3D space.

The ribbon is colored by depth using a violet-to-yellow gradient, giving an intuitive sense of the bat moving toward or away from the camera. Leading dots mark the current tip and handle positions, and a white line connects them to show the bat's current orientation.

Each frame is rendered as a separate Matplotlib 3D figure and stitched into an MP4 using FFmpeg. The rendering is parallelized across CPU cores using `ProcessPoolExecutor`. Three camera angles are generated per video to give a comprehensive view of the trajectory.

<!-- [IMAGE: A single 3D rendered frame showing the ribbon, blade line, dots, and depth colorbar] -->

---

## Stage 7: The Final Export

The pipeline assembles a 4-panel grid video:
- **Top-left:** Stabilized RGB (the original footage, camera-corrected)
- **Top-right:** 3D trajectory from angle 1 (azimuth -10 degrees)
- **Bottom-left:** 3D trajectory from angle 2 (azimuth -80 degrees)
- **Bottom-right:** 3D trajectory from angle 3 (azimuth -45 degrees)

For batch runs, all individual grid videos are concatenated into a single master reel with overlay banners showing the clip name and frame counter. This uses raw BGR frame piping through FFmpeg's stdin — not the `concat` demuxer, which breaks on mixed-resolution inputs.

<!-- [VIDEO: The final 4-in-1 side-by-side output] -->

---

## Results: 60 Videos, Zero Failures

I tested the pipeline on a batch of 60 cricket videos covering a range of shot types — cover drives, pull shots, cuts, flicks, defensive blocks, lofted shots — from both left-handed and right-handed batsmen. The videos were filmed on phones at varying angles and distances.

| Metric | Value |
|--------|-------|
| Videos processed | 60 / 60 |
| Success rate | 100% |
| Average processing time | ~30 seconds / video |
| Average frames per clip | ~24 (trimmed) |
| Bat mask detection rate | ~95% of frames |
| Valid 3D depth observations | ~93% of frames |
| Kalman gap-fills needed | Median 1 frame / video |

The pipeline handles:
- Different camera angles (behind the bowler, side-on, slightly elevated)
- Different lighting conditions (bright sun, overcast, indoor nets)
- Both left-handed and right-handed batsmen (the PCA + wrist system is hand-agnostic)
- Partial bat occlusion during the backswing

<!-- [GALLERY: 4-6 side-by-side results showing different shot types] -->

---

## What Went Wrong (Lessons Learned)

### SAM Tracking the Wrong Object
In early versions, SAM 3.1 locked onto the wicket stumps instead of the bat in ~40% of videos. The stumps and bat are both long, thin, wooden objects. The dual-session subtraction trick (Stage 3b) solved this completely.

### Camera Shake Corrupting Depth
Without pitch stabilization, the sampled Z-coordinates drifted wildly between frames. The bat hadn't moved, but the camera had — shifting the bat to a different part of the depth map with a different relative depth. Stabilizing to the crease lines eliminated this class of error entirely.

### Depth Anything's Frame-to-Frame Jitter
Monocular depth models process each frame independently. The same physical point can get a slightly different depth value in consecutive frames, causing the 3D ribbon to vibrate. The Kalman RTS smoother's per-axis moving-average post-processing tames this.

### cuDNN Crashing Under CUDA 12
On my system, the installed cuDNN version conflicted with the CUDA 12 runtime, causing random convolution crashes inside SAM. The fix was brutal but effective: disable cuDNN entirely with `torch.backends.cudnn.enabled = False`. The native PyTorch ATen kernels are fast enough for our model sizes.

### FFmpeg Concatenation Artifacts
The first batch export used `ffmpeg -f concat -c copy` to stitch individual side-by-side videos. This produced playback glitches because the individual clips had slightly different resolutions (1280x640 vs 1280x638 due to rounding). The fix was to pipe raw BGR frames through OpenCV, resize each frame to the target dimensions, and encode through FFmpeg's stdin.

---

## The Full Model Stack

| Model | Parameters | Size | Role |
|-------|-----------|------|------|
| YOLO11x | ~57M | 115 MB | Object detection (bat, person) |
| YOLOv8x-Pose | ~69M | 139 MB | Human pose estimation (wrists) |
| Grounding DINO Tiny | ~170M | ~350 MB | Zero-shot text-prompted detection |
| SAM 2 Hiera Large | ~200M | 898 MB | Promptable image segmentation |
| SAM 3.1 Multiplex | ~1.2B | 3.5 GB | Video object segmentation |
| Depth Anything V2 Large | ~335M | ~1.3 GB | Monocular depth estimation |

Total VRAM footprint: all models are loaded once and share GPU memory. The pipeline runs comfortably on a GPU with 12+ GB VRAM.

---

## What's Next

This is version 2.3. The trajectory data is there. The next steps are about making it useful:

- **Shot classification:** A cover drive, a pull shot, and a defensive block produce visually distinct 3D trajectories. A classifier trained on the trajectory data could automatically label shot types.
- **Trajectory comparison:** Overlay two players' trajectories to see how their bat paths differ — like a 3D version of "your bat path vs. Virat Kohli's bat path."
- **Interactive 3D viewer:** Replace the Matplotlib static renders with a web-based 3D viewer (Three.js or Plotly) where users can orbit, zoom, and scrub through the trajectory.
- **Real-time processing:** The persistent worker architecture (`run_worker.py`) already supports streaming jobs over stdin. With model quantization and frame batching optimizations, sub-10-second processing per clip is achievable.

---

## Try It Yourself

The full source code, model weights, and test videos are available on GitHub.

```bash
# Clone and setup
git clone https://github.com/[username]/batplane.git
cd batplane
conda activate ml
pip install -r requirements.txt

# Process a single video
python run_single.py --input your_cricket_video.mp4

# Batch process a folder
python run_batch.py --input-dir ./your_videos/ --output-dir ./results/
```

The pipeline is configured through a single `config.py` file containing over 150 tunable constants — everything from YOLO confidence thresholds to Kalman filter noise parameters. If something doesn't work on your videos, the fix is almost certainly a config tweak, not a code change.

---

## References

1. Ultralytics YOLO — https://docs.ultralytics.com/
2. Segment Anything Model 2 (SAM 2) — https://github.com/facebookresearch/segment-anything-2
3. Depth Anything V2 — https://huggingface.co/depth-anything/Depth-Anything-V2-Large-hf
4. Grounding DINO — https://github.com/IDEA-Research/GroundingDINO
5. Rauch, H. E., Tung, F., and Striebel, C. T. (1965). *Maximum Likelihood Estimates of Linear Dynamic Systems.* AIAA Journal.
6. OpenCV — https://docs.opencv.org/

---

*Built by Adnan. If you have cricket videos and a GPU, I'd love to see what trajectories your shots produce.*

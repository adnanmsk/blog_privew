# Pipeline Diagram Options

Here are three different styles of Mermaid diagrams illustrating the Bat Swing Plane Pipeline. You can copy the code block for your favorite one and paste it directly into your blog, or we can use it to build a visual graphic!

### Option 1: The Classic Top-Down Flow
*Simple, easy to read, and shows the direct chronological steps of the pipeline.*

```mermaid
graph TD
    A[Raw Cricket Video] --> B(Stage 1: YOLOv8 Action Trimming)
    B --> C(Stage 2: Pitch Stabilization via Homography)
    C --> D(Stage 3a: Depth Mapping via DepthAnythingV2)
    C --> E(Stage 3b: Bat & Wicket Segmentation via SAM 2)
    D --> F(Stage 4: PCA Axis & Sub-Pixel Keypoints)
    E --> F
    F --> G(Stage 5: 3D Fusion & Kalman Smoothing)
    G --> H[Final 3D Swing Trajectory]
```

---

### Option 2: The Horizontal System Architecture
*Grouped by logical phases (Preprocessing, AI Extraction, Fusion). Great for technical architecture sections.*

```mermaid
graph LR
    subgraph Input
        A[Raw Mobile Video]
    end
    subgraph Preprocessing
        B(1. Action Trimming <br/>YOLOv8)
        C(2. Camera Stabilization <br/>Homography)
    end
    subgraph AI Extraction
        D(3a. Depth Mapping <br/>DepthAnythingV2)
        E(3b. Mask Segmentation <br/>SAM 2)
        F(4. Keypoint Geometry <br/>PCA & Pose)
    end
    subgraph Fusion
        G(5. 3D Tracking <br/>Kalman Filter)
    end
    
    A --> B --> C
    C --> D
    C --> E
    D --> F
    E --> F
    F --> G
```

---

### Option 3: The "Data Flow" Layout
*Focuses heavily on the actual data structures being produced at each step (cylinders) rather than just the actions.*

```mermaid
flowchart TD
    Vid[(Raw Video)] --> S1[Stage 1: YOLO Trim]
    S1 --> Clip[(1-Sec Action Clip)]
    
    Clip --> S2[Stage 2: Pitch Stabilization]
    S2 --> Stable[(Stable Frames)]
    
    Stable --> S3A{Stage 3a: Depth Models}
    Stable --> S3B{Stage 3b: SAM 2 Models}
    
    S3A --> ZData[(Z-Axis Depth Maps)]
    S3B --> Mask[(Clean Bat Masks)]
    
    Mask --> S4[Stage 4: PCA Geometry]
    S4 --> XYData[(X,Y Tip & Handle)]
    
    ZData --> S5((Stage 5: 3D Fusion))
    XYData --> S5((Stage 5: 3D Fusion))
    
    S5 --> Out[(Final 3D Trajectory Output)]
```

<!-- # Pipeline Diagram Options

Here are upgraded, premium-styled Mermaid diagrams for the Bat Swing Plane Pipeline. They use custom colors, rounded corners, and shapes to look significantly better than the default basic layout! 

*(Note: GitHub Markdown does not support standard CSS keyframe animations inside SVGs for security reasons. However, I have applied the `stroke-dasharray` technique which gives the connecting lines a dashed, "moving data" look in supported markdown renderers!)*

### Option 1: The "Premium" Dark Mode Flow
*Features custom hex colors, rounded corners, and drop shadows to look like a modern system architecture.*

```mermaid
flowchart TD
    %% Custom Styles
    classDef input fill:#2b2b2b,stroke:#fca311,stroke-width:2px,color:#fff,rx:10px,ry:10px;
    classDef stage fill:#14213d,stroke:#e5e5e5,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef data fill:#003049,stroke:#669bbc,stroke-width:2px,color:#fff,rx:15px,ry:15px;
    classDef output fill:#006400,stroke:#38b000,stroke-width:3px,color:#fff,rx:10px,ry:10px,font-weight:bold;

    <!-- Vid[/"Raw Video Input"/]:::input --> S1
    
    subgraph Pre-Processing
    S1("1. Trim Video (YOLO11x + Pose)"):::stage --> Clip[("1-Sec Action Clip")]:::data
    Clip --> S2("2. Stabilize Camera (Grounding DINO + SAM 2)"):::stage
    S2 --> Stable[("Clean Frames")]:::data
    end
    
    Stable --> S3A
    Stable --> S3B

    subgraph AI Inference
    S3A{"3a. Depth Maps (Depth Anything V2)"}:::stage
    S3B{"3b. Bat Mask (SAM 3.1)"}:::stage
    S3A --> ZData[("Z-Depth Maps")]:::data
    S3B --> Mask[("X,Y Pixels")]:::data
    Mask --> S4("4. 3D Keypoints (PCA + Wrists)"):::stage
    S4 --> XYData[("Raw 3D Points")]:::data
    end
    
    ZData --> S5
    XYData --> S5

    subgraph Physics & Fusion
    S5(("5. 3D Fusion (Kalman RTS)")):::stage
    end
    
    S5 ===> Out[/"6. Render Bat Swing Plane (Matplotlib + FFmpeg)"\]:::output
    
    %% Dashed Flow Lines
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11 stroke:#fff,stroke-width:2px,stroke-dasharray: 5 5;
    linkStyle 12 stroke:#38b000,stroke-width:4px;
``` -->

---

### Option 2: The "Neon" Horizontal Layout
*Uses bright, high-contrast neon styling for a cyberpunk/AI feel, laid out left-to-right.*

```mermaid
flowchart LR
    %% Custom Styles
    classDef pink fill:#2d001a,stroke:#ff007f,stroke-width:2px,color:#fff;
    classDef blue fill:#001233,stroke:#00f5d4,stroke-width:2px,color:#fff;
    classDef green fill:#002910,stroke:#38b000,stroke-width:2px,color:#fff;

    A[/"Raw Video"/]:::pink --> B("1. YOLO11x + Pose"):::blue
    B --> C("2. Grounding DINO + SAM 2"):::blue
    
    C --> D{"3a. Depth Anything V2"}:::pink
    C --> E{"3b. SAM 3.1"}:::pink
    
    D --> F("4. PCA + Wrists"):::blue
    E --> F
    
    F --> G(("5. Kalman RTS")):::green
    G ===> H[/"6. Matplotlib + FFmpeg"/]:::green
    
    %% Animated-looking dashed lines
    linkStyle 0,1,2,3,4,5,6 stroke:#fff,stroke-width:2px,stroke-dasharray: 5 5;
    linkStyle 7 stroke:#00f5d4,stroke-width:4px;
```

---

### Option 3: The "Cyberpunk Detailed" Horizontal Layout
*An upgraded version of Option 2 that explicitly lists the technical models used at each step, ensuring perfect context for the pipeline while maintaining the premium neon aesthetic.*

```mermaid
flowchart LR
    %% Custom Styles
    classDef pink fill:#2d001a,stroke:#ff007f,stroke-width:2px,color:#fff;
    classDef blue fill:#001233,stroke:#00f5d4,stroke-width:2px,color:#fff;
    classDef green fill:#002910,stroke:#38b000,stroke-width:2px,color:#fff;

    A[/"Raw Mobile Video"/]:::pink --> B("1. Trim Video<br><i>(YOLO11x + Pose)</i>"):::blue
    B --> C("2. Stabilize Camera<br><i>(Grounding DINO + SAM 2)</i>"):::blue
    
    C --> D{"3a. Depth Maps<br><i>(Depth Anything V2)</i>"}:::pink
    C --> E{"3b. Bat Mask<br><i>(SAM 3.1)</i>"}:::pink
    
    D --> F("4. 3D Keypoints<br><i>(PCA + Wrists)</i>"):::blue
    E --> F
    
    F --> G(("5. 3D Fusion<br><i>(Kalman RTS)</i>")):::green
    G ===> H[/"6. Render Trajectory<br><i>(Matplotlib + FFmpeg)</i>"/]:::green
    
    %% Animated-looking dashed lines
    linkStyle 0,1,2,3,4,5,6 stroke:#fff,stroke-width:2px,stroke-dasharray: 8 4;
    linkStyle 7 stroke:#00f5d4,stroke-width:4px,stroke-dasharray: 10 5;
```

---

### Option 4: The "Original Diagram" Upgraded
*This layout keeps the exact same structure as the original diagram in your blog draft (grouped by Pre-Processing, AI Inference, Physics), but significantly upgrades the visuals with bright colors, 3D shapes, and dashed flow paths (no emojis).*

```mermaid
graph TD
    %% Custom Premium Styles
    classDef prep fill:#ff9f43,stroke:#c87b32,color:#000,stroke-width:3px,rx:10px,ry:10px;
    classDef infer fill:#0abde3,stroke:#0984e3,color:#000,stroke-width:3px,rx:10px,ry:10px;
    classDef phys fill:#1dd1a1,stroke:#10ac84,color:#000,stroke-width:3px,rx:10px,ry:10px;
    classDef out fill:#ff7675,stroke:#d63031,color:#000,stroke-width:4px,rx:15px,ry:15px,font-weight:bold;

    subgraph Pre-Processing
        T("1. Trim Video<br><i>YOLO11x + Pose</i>"):::prep --> S("2. Stabilize Camera<br><i>Grounding DINO + SAM 2</i>"):::prep
    end
    
    subgraph AI Inference
        S -- "Stable Frames" --> D{"3a. Depth Maps<br><i>Depth Anything V2</i>"}:::infer
        S -- "Stable Frames" --> Seg{"3b. Bat Mask<br><i>SAM 3.1</i>"}:::infer
    end
    
    subgraph Physics & Fusion
        D -- "Z-Depth" --> K("4. 3D Keypoints<br><i>PCA + Wrists</i>"):::phys
        Seg -- "X, Y Pixels" --> K
        K -- "Raw 3D Points" --> F(("5. 3D Fusion<br><i>Kalman RTS</i>")):::phys
    end
    
    F == "Smooth Trajectory" === R[/"6. Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"/]:::out

    %% Animated Dashed Flow Lines
    linkStyle 0,1,2,3,4,5 stroke:#fff,stroke-width:2px,stroke-dasharray: 5 5;
    linkStyle 6 stroke:#ff7675,stroke-width:4px,stroke-dasharray: 10 5;
``` -->

---

### Option 5: The Uniform Layout
*This version uses the exact text and flow from Option 4 but simplifies all shapes into clean rectangles. The animation style uses the dashed strokes from Option 3, and the flow goes top-to-bottom.*

```mermaid
graph TD
    %% Custom Premium Styles
    classDef prep fill:#3b0764,stroke:#a855f7,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef infer fill:#083344,stroke:#06b6d4,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef phys fill:#064e3b,stroke:#10b981,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef out fill:#881337,stroke:#f43f5e,color:#fff,stroke-width:4px,rx:8px,ry:8px,font-weight:bold;

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA + Wrists</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    %% Animation like Option 3
    linkStyle 0,1,2,3,4,5 stroke:#fff,stroke-width:2px,stroke-dasharray: 8 4;
    linkStyle 6 stroke:#ff7675,stroke-width:4px,stroke-dasharray: 10 5;
```
### option 6
```mermaid
flowchart TD
    %% Clean, modern color palette for standard Markdown rendering
    classDef prep fill:#f0f4f8,stroke:#3b82f6,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef infer fill:#f0fdf4,stroke:#10b981,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef phys fill:#fffbeb,stroke:#f59e0b,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef out fill:#fef2f2,stroke:#ef4444,stroke-width:3px,color:#1e293b,rx:12px,ry:12px,font-weight:bold

    %% Nodes and relationships (Text strictly preserved)
    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    %% Link styling designed to be visible on any Markdown background
    linkStyle 0,1,2,3,4,5 stroke:#64748b,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#ef4444,stroke-width:4px
```

### Option 7: Cyberpunk Neon
```mermaid
flowchart TD
    classDef prep fill:#1a1a2e,stroke:#e94560,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef infer fill:#1a1a2e,stroke:#0f3460,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef phys fill:#1a1a2e,stroke:#f9a826,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef out fill:#1a1a2e,stroke:#43d8c9,stroke-width:3px,color:#fff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#e94560,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#43d8c9,stroke-width:4px
```

### Option 8: Earthy Natural
```mermaid
flowchart TD
    classDef prep fill:#e6eed6,stroke:#606c38,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef infer fill:#fefae0,stroke:#dda15e,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef phys fill:#faedcd,stroke:#bc6c25,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef out fill:#83c5be,stroke:#006d77,stroke-width:3px,color:#fff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#606c38,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#006d77,stroke-width:4px
```

### Option 9: Soft Pastel
```mermaid
flowchart TD
    classDef prep fill:#ffc8dd,stroke:#ffafcc,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef infer fill:#bde0fe,stroke:#a2d2ff,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef phys fill:#cdb4db,stroke:#b492c7,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef out fill:#fdffb6,stroke:#ffd6a5,stroke-width:3px,color:#5c4d5c,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#b492c7,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#ffd6a5,stroke-width:4px
```

### Option 10: Minimalist Monochrome
```mermaid
flowchart TD
    classDef prep fill:#ffffff,stroke:#000000,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef infer fill:#f0f0f0,stroke:#333333,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef phys fill:#d0d0d0,stroke:#666666,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef out fill:#000000,stroke:#000000,stroke-width:3px,color:#ffffff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#000000,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#000000,stroke-width:4px
```

### Option 11: Vibrant Tech
```mermaid
flowchart TD
    classDef prep fill:#e0f7fa,stroke:#00acc1,stroke-width:2px,color:#006064,rx:8px,ry:8px
    classDef infer fill:#fff3e0,stroke:#fb8c00,stroke-width:2px,color:#e65100,rx:8px,ry:8px
    classDef phys fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px,color:#4a148c,rx:8px,ry:8px
    classDef out fill:#e8eaf6,stroke:#3949ab,stroke-width:3px,color:#1a237e,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#00acc1,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#3949ab,stroke-width:4px
```

### Option 12: Autumn Sunset
```mermaid
flowchart TD
    classDef prep fill:#ffedd8,stroke:#f3c5b5,stroke-width:2px,color:#8b4513,rx:8px,ry:8px
    classDef infer fill:#f3c5b5,stroke:#e3856b,stroke-width:2px,color:#5c2c0b,rx:8px,ry:8px
    classDef phys fill:#e3856b,stroke:#c25437,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef out fill:#c25437,stroke:#8c3420,stroke-width:3px,color:#ffffff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#e3856b,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#8c3420,stroke-width:4px
```

### Option 13: Midnight Ocean
```mermaid
flowchart TD
    classDef prep fill:#03045e,stroke:#0077b6,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef infer fill:#0077b6,stroke:#00b4d8,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef phys fill:#00b4d8,stroke:#90e0ef,stroke-width:2px,color:#03045e,rx:8px,ry:8px
    classDef out fill:#caf0f8,stroke:#ffffff,stroke-width:3px,color:#03045e,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>YOLO11x + Pose</i>"]:::prep --> S["Stabilize Video<br><i>Grounding DINO + SAM 2</i>"]:::prep
    S -- "Stable Frames" --> D["Depth Maps<br><i>Depth Anything V2</i>"]:::infer
    S -- "Stable Frames" --> Seg["Bat Mask<br><i>SAM 3.1</i>"]:::infer
    D -- "Z-Depth" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "X, Y Pixels" --> K
    K -- "Raw 3D Points" --> F["3D Fusion<br><i>Kalman RTS</i>"]:::phys
    F == "Smooth Trajectory" === R["Render Bat Swing Plane<br><i>Matplotlib + FFmpeg</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#90e0ef,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#caf0f8,stroke-width:4px
```

# Pipeline Diagram Options

Here are upgraded, premium-styled Mermaid diagrams for the Bat Swing Plane Pipeline. They use custom colors, rounded corners, shapes, and emojis to look significantly better than the default basic layout! 

*(Note: GitHub Markdown does not support animated SVGs or animated Mermaid diagrams for security reasons. If you want a truly animated diagram, you would need to export a GIF from a tool like Canva or Figma. However, these styled diagrams are the absolute best looking static ones you can get purely through code!)*

### Option 1: The "Premium" Dark Mode Flow
*Features custom hex colors, rounded corners, drop shadows, and emojis to look like a modern system architecture.*

```mermaid
flowchart TD
    %% Custom Styles
    classDef input fill:#2b2b2b,stroke:#fca311,stroke-width:2px,color:#fff,rx:10px,ry:10px;
    classDef stage fill:#14213d,stroke:#e5e5e5,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef data fill:#003049,stroke:#669bbc,stroke-width:2px,color:#fff,rx:15px,ry:15px;
    classDef output fill:#006400,stroke:#38b000,stroke-width:3px,color:#fff,rx:10px,ry:10px,font-weight:bold;

    Vid[/"🎥 Raw Video Input"/]:::input --> S1
    
    subgraph Preprocessing
    S1("✂️ Stage 1: YOLO Trim"):::stage --> Clip[("🎞️ 1-Sec Action Clip")]:::data
    Clip --> S2("⚖️ Stage 2: Pitch Stabilization"):::stage
    S2 --> Stable[("📐 Stable Frames")]:::data
    end
    
    Stable --> S3A
    Stable --> S3B

    subgraph Dual AI Extraction
    S3A{"👁️ Stage 3a: Depth Models"}:::stage
    S3B{"🤖 Stage 3b: SAM 2 Models"}:::stage
    S3A --> ZData[("📊 Z-Axis Depth Maps")]:::data
    S3B --> Mask[("🦇 Clean Bat Masks")]:::data
    Mask --> S4("📏 Stage 4: PCA Geometry"):::stage
    S4 --> XYData[("📍 X,Y Tip & Handle")]:::data
    end
    
    ZData --> S5
    XYData --> S5

    subgraph 3D Engine
    S5(("🧠 Stage 5: 3D Fusion & Kalman")):::stage
    end
    
    S5 ===> Out[/"🚀 Final 3D Trajectory Output"\]:::output
```

---

### Option 2: The "Neon" Horizontal Layout
*Uses bright, high-contrast neon styling for a cyberpunk/AI feel, laid out left-to-right.*

```mermaid
flowchart LR
    %% Custom Styles
    classDef pink fill:#2d001a,stroke:#ff007f,stroke-width:2px,color:#fff;
    classDef blue fill:#001233,stroke:#00f5d4,stroke-width:2px,color:#fff;
    classDef green fill:#002910,stroke:#38b000,stroke-width:2px,color:#fff;
    classDef textNode fill:none,stroke:none,color:#fff,font-weight:bold;

    A[/"📱 Raw Video"/]:::pink --> B("🤖 YOLO Trim"):::blue
    B --> C("⚖️ Homography"):::blue
    
    C --> D{"👁️ Depth Maps"}:::pink
    C --> E{"🦇 SAM 2"}:::pink
    
    D --> F("📏 PCA Axis"):::blue
    E --> F
    
    F --> G(("🧠 3D Fusion")):::green
    G ===> H[/"🚀 3D Trajectory"/]:::green
    
    %% Animated-looking dashed lines (some renderers support this)
    linkStyle 0,1,2,3,4,5,6 stroke:#fff,stroke-width:2px,stroke-dasharray: 5 5;
    linkStyle 7 stroke:#00f5d4,stroke-width:4px;
```



### Option 5: The Uniform Layout
*This version uses the exact text and flow from Option 4 but simplifies all shapes into clean rectangles. The animation style uses the dashed strokes from Option 3, and the flow goes top-to-bottom.*

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
graph TD
    %% Custom Premium Styles
    classDef prep fill:#3b0764,stroke:#a855f7,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef infer fill:#083344,stroke:#06b6d4,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef phys fill:#064e3b,stroke:#10b981,color:#fff,stroke-width:3px,rx:5px,ry:5px;
    classDef out fill:#881337,stroke:#f43f5e,color:#fff,stroke-width:4px,rx:8px,ry:8px,font-weight:bold;

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #a855f7; color:#a855f7; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #a855f7; color:#a855f7; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #06b6d4; color:#06b6d4; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #06b6d4; color:#06b6d4; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #10b981; color:#10b981; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #f43f5e; color:#f43f5e; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    %% Animation like Option 3
    linkStyle 0,1,2,3,4,5 stroke:#fff,stroke-width:2px,stroke-dasharray: 8 4;
    linkStyle 6 stroke:#ff7675,stroke-width:4px,stroke-dasharray: 10 5;
```

### option 6
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    %% Clean, modern color palette for standard Markdown rendering
    classDef prep fill:#f0f4f8,stroke:#3b82f6,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef infer fill:#f0fdf4,stroke:#10b981,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef phys fill:#fffbeb,stroke:#f59e0b,stroke-width:2px,color:#1e293b,rx:8px,ry:8px
    classDef out fill:#fef2f2,stroke:#ef4444,stroke-width:3px,color:#1e293b,rx:12px,ry:12px,font-weight:bold

    %% Nodes and relationships (Text strictly preserved)
    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #3b82f6; color:#3b82f6; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #3b82f6; color:#3b82f6; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #10b981; color:#10b981; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #10b981; color:#10b981; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #f59e0b; color:#f59e0b; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #ef4444; color:#ef4444; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    %% Link styling designed to be visible on any Markdown background
    linkStyle 0,1,2,3,4,5 stroke:#64748b,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#ef4444,stroke-width:4px
```

### Option 7: Cyberpunk Neon
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#1a1a2e,stroke:#e94560,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef infer fill:#1a1a2e,stroke:#0f3460,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef phys fill:#1a1a2e,stroke:#f9a826,stroke-width:2px,color:#fff,rx:8px,ry:8px
    classDef out fill:#1a1a2e,stroke:#43d8c9,stroke-width:3px,color:#fff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #e94560; color:#e94560; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #e94560; color:#e94560; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #0f3460; color:#0f3460; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #0f3460; color:#0f3460; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #f9a826; color:#f9a826; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #43d8c9; color:#43d8c9; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#e94560,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#43d8c9,stroke-width:4px
```

### Option 8: Earthy Natural
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#e6eed6,stroke:#606c38,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef infer fill:#fefae0,stroke:#dda15e,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef phys fill:#faedcd,stroke:#bc6c25,stroke-width:2px,color:#283618,rx:8px,ry:8px
    classDef out fill:#83c5be,stroke:#006d77,stroke-width:3px,color:#fff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #606c38; color:#606c38; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #606c38; color:#606c38; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #dda15e; color:#dda15e; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #dda15e; color:#dda15e; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #bc6c25; color:#bc6c25; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #006d77; color:#006d77; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#606c38,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#006d77,stroke-width:4px
```

### Option 9: Soft Pastel
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#ffc8dd,stroke:#ffafcc,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef infer fill:#bde0fe,stroke:#a2d2ff,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef phys fill:#cdb4db,stroke:#b492c7,stroke-width:2px,color:#5c4d5c,rx:8px,ry:8px
    classDef out fill:#fdffb6,stroke:#ffd6a5,stroke-width:3px,color:#5c4d5c,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #ffafcc; color:#ffafcc; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #ffafcc; color:#ffafcc; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #a2d2ff; color:#a2d2ff; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #a2d2ff; color:#a2d2ff; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #b492c7; color:#b492c7; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #ffd6a5; color:#ffd6a5; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#b492c7,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#ffd6a5,stroke-width:4px
```

### Option 10: Minimalist Monochrome
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#ffffff,stroke:#000000,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef infer fill:#f0f0f0,stroke:#333333,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef phys fill:#d0d0d0,stroke:#666666,stroke-width:2px,color:#000000,rx:8px,ry:8px
    classDef out fill:#000000,stroke:#000000,stroke-width:3px,color:#ffffff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #000000; color:#000000; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #000000; color:#000000; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #333333; color:#333333; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #333333; color:#333333; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #666666; color:#666666; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #000000; color:#000000; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#000000,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#000000,stroke-width:4px
```

### Option 11: Vibrant Tech
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#e0f7fa,stroke:#00acc1,stroke-width:2px,color:#006064,rx:8px,ry:8px
    classDef infer fill:#fff3e0,stroke:#fb8c00,stroke-width:2px,color:#e65100,rx:8px,ry:8px
    classDef phys fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px,color:#4a148c,rx:8px,ry:8px
    classDef out fill:#e8eaf6,stroke:#3949ab,stroke-width:3px,color:#1a237e,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #00acc1; color:#00acc1; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #00acc1; color:#00acc1; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #fb8c00; color:#fb8c00; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #fb8c00; color:#fb8c00; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #8e24aa; color:#8e24aa; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #3949ab; color:#3949ab; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#00acc1,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#3949ab,stroke-width:4px
```

### Option 12: Autumn Sunset
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#ffedd8,stroke:#f3c5b5,stroke-width:2px,color:#8b4513,rx:8px,ry:8px
    classDef infer fill:#f3c5b5,stroke:#e3856b,stroke-width:2px,color:#5c2c0b,rx:8px,ry:8px
    classDef phys fill:#e3856b,stroke:#c25437,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef out fill:#c25437,stroke:#8c3420,stroke-width:3px,color:#ffffff,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #f3c5b5; color:#f3c5b5; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #f3c5b5; color:#f3c5b5; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #e3856b; color:#e3856b; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #e3856b; color:#e3856b; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #c25437; color:#c25437; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #8c3420; color:#8c3420; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#e3856b,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#8c3420,stroke-width:4px
```

### Option 13: Midnight Ocean
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'edgeLabelBackground':'transparent' }}}%%
flowchart TD
    classDef prep fill:#03045e,stroke:#0077b6,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef infer fill:#0077b6,stroke:#00b4d8,stroke-width:2px,color:#ffffff,rx:8px,ry:8px
    classDef phys fill:#00b4d8,stroke:#90e0ef,stroke-width:2px,color:#03045e,rx:8px,ry:8px
    classDef out fill:#caf0f8,stroke:#ffffff,stroke-width:3px,color:#03045e,rx:12px,ry:12px,font-weight:bold

    T["Trim Video<br><i>Pose Detection</i>"]:::prep --> S["Stabilize Video<br><i>Crease Line Tracking</i>"]:::prep
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #0077b6; color:#0077b6; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> D["Depth Maps<br><i>Depth Estimation</i>"]:::infer
    S -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #0077b6; color:#0077b6; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Stable Frames</span>" --> Seg["Bat Mask<br><i>Object Tracking</i>"]:::infer
    D -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #00b4d8; color:#00b4d8; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Z-Depth</span>" --> K["3D Keypoints<br><i>PCA for Bat Axis</i>"]:::phys
    Seg -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #00b4d8; color:#00b4d8; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>X, Y Pixels</span>" --> K
    K -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #90e0ef; color:#90e0ef; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Raw 3D Points</span>" --> F["3D Fusion<br><i>Physics-based Smoothing</i>"]:::phys
    F -- "<span style='background:rgba(11,15,25,0.7); border:1px solid #ffffff; color:#ffffff; padding:2px 6px; border-radius:4px; font-size:10px; font-weight:bold; display:inline-block;'>Smooth Trajectory</span>" --> R["Render Bat Swing Plane<br><i>3D Visualization</i>"]:::out

    linkStyle 0,1,2,3,4,5 stroke:#90e0ef,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 6 stroke:#caf0f8,stroke-width:4px
```

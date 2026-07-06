# Pipeline Diagram Options

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

    Vid[/"Raw Video Input"/]:::input --> S1
    
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
```

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
```mermaid
flowchart LR
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

# Comprehensive IT Support Call Flow

## Overview

This process log details the automated and human-agent handling paths for incoming calls to the IT support center. It illustrates both the Standard and High Volume call flows, with a specific focus on the complex decision branching during peak hours and the subsequent voicemail handling protocols.

## Process Flow Log (Vertical View)

This vertical diagram provides a step-by-step sequential view of the call handling process, stacked top-to-bottom for easy following as a 'log' of events.

---
**Standard Call Flow**
*A simple path where an available technician handles the call.*

*   **1. Start Call**
    *   *Description:* Call is initiated by the user.
*   **2. Standard Call Queue** (Group: STANDARD CALL FLOW)
    *   *Description:* Caller enters the general support queue for initial triaging or standard requests.
*   **3. Decision: Technician Available?**
    *   *Yes:* Call is immediately picked up. -> **[GO TO: Call Answered]**
    *   *No:* Wait time exists, and the user is offered a callback or voicemail option. -> **[GO TO: Prompt to Voicemail]**

---
**High Volume Call Flow**
*Detailed branching path for high call volumes, with menu-based routing to specialized agents.*

*   **4. Play Menu Options** (Group: HIGH VOLUME CALL FLOW)
    *   *Description:* Automated attendant plays initial greeting and main menu options.
*   **5. Decision: Menu Selection**
    *   **Selection 1: Password Reset** (Group: PASSWORD QUEUE)
        *   *Queue:* User enters the specialized Password Reset Queue.
        *   *Decision: Password Agent Available?*
            *   *Yes:* Call Answered -> **[GO TO: Call Answered]**
            *   *No:* Wait time exists. -> **[GO TO: Password Voicemail]**
    *   **Selection 2: Software Access** (Group: SOFTWARE QUEUE)
        *   *Queue:* User enters the specialized Software Access Queue.
        *   *Decision: Software Agent Available?*
            *   *Yes:* Call Answered -> **[GO TO: Call Answered]**
            *   *No:* Wait time exists. -> **[GO TO: Software Voicemail]**
    *   **Selection 3: Other Issues** (Group: OTHER QUEUE)
        *   *Queue:* User enters the generalized "Other Issues" Queue.
        *   *Decision: Other Agent Available?*
            *   *Yes:* Call Answered -> **[GO TO: Call Answered]**
            *   *No:* Wait time exists. -> **[GO TO: Other Voicemail]**

---
**Voicemail Handling Process**
*The standard back-end processing for all unserviced calls.*

*   **6. Caller Leaves Voicemail** (Group: VOICEMAIL HANDLING)
    *   *Inputs:* All 'No' outcomes from previous queues (Standard Voicemail, Password Voicemail, Software Voicemail, Other Voicemail) merge here.
    *   *Description:* User records their issue via the automated prompt.
*   **7. Ticket Auto Created**
    *   *Description:* The voicemail system triggers an automated service that generates an IT support ticket, attaching the audio file.
*   **8. Technician Follows Up**
    *   *Description:* A technician claims the ticket and contacts the user for resolution.
*   **9. Issue Resolved**
    *   *Description:* The issue is confirmed as fixed, and the ticket is closed.

## Mermaid Diagram Code

The following Mermaid.js code generates the vertical process diagram for use in compatible markdown editors and GitHub.

```mermaid
graph TD

%% Define color classes for the light theme
classDef blueFill fill:#E6F7FF,stroke:#1890FF,color:#000,stroke-width:1.5px
classDef greenFill fill:#F6FFED,stroke:#52C41A,color:#000,stroke-width:1.5px
classDef orangeFill fill:#FFFBE6,stroke:#FAAD14,color:#000,stroke-width:1.5px
classDef cyanFill fill:#E6FFFB,stroke:#13C2C2,color:#000,stroke-width:1.5px
classDef purpleFill fill:#F9F0FF,stroke:#722ED1,color:#000,stroke-width:1.5px
classDef greyFill fill:#F5F5F5,stroke:#D9D9D9,color:#000,stroke-width:1.5px
classDef darkRedFill fill:#FFF1F0,stroke:#F5222D,color:#000,stroke-width:1.5px

%% Start Call Node
N_START(:phone: Start Call)
N_START --> |Call Incoming| SG_STANDARD

%% --- STANDARD CALL FLOW Subgraph ---
subgraph SG_STANDARD [STANDARD CALL FLOW]
    direction TD
    style SG_STANDARD stroke:#E6F7FF,fill:#FFF
    N_S_Q(:headset: Standard Call Queue) --> N_S_A_D{Available?}
    N_S_A_D -- Yes --> N_S_ANS(:checkmark: Call Answered)
    N_S_A_D -- No --> N_S_VOICEMAIL(:microphone: Prompt to Voicemail)
end

class N_S_Q blueFill
class N_S_A_D blueFill
class N_S_ANS greenFill
class N_S_VOICEMAIL orangeFill

%% Start Call feeds into both flows
N_START --> |High Volume Detect| SG_HIGH_VOLUME

%% --- HIGH VOLUME CALL FLOW Subgraph ---
subgraph SG_HIGH_VOLUME [HIGH VOLUME CALL FLOW]
    direction TD
    style SG_HIGH_VOLUME stroke:#FFFBE6,fill:#FFF
    N_HV_P_M(:list: Play Menu Options) --> N_HV_M_S{Selection}
    
    N_HV_M_S -- 1 --> SG_PASSWORD
    N_HV_M_S -- 2 --> SG_SOFTWARE
    N_HV_M_S -- 3 --> SG_OTHER

    %% Nested Parallel Password Queue
    subgraph SG_PASSWORD [PASSWORD QUEUE]
        direction TD
        style SG_PASSWORD stroke:#F9F0FF,fill:#FFF
        N_HV_P_Q(:key: Password Reset Queue) --> N_HV_P_A_D{Available?}
        N_HV_P_A_D -- Yes --> N_HV_P_ANS(:checkmark: Ans)
        N_HV_P_A_D -- No --> N_HV_P_VOICEMAIL(:microphone: Voicemail)
    end
    
    %% Nested Parallel Software Queue
    subgraph SG_SOFTWARE [SOFTWARE QUEUE]
        direction TD
        style SG_SOFTWARE stroke:#E6FFFB,fill:#FFF
        N_HV_S_Q(:computer: Software Access Queue) --> N_HV_S_A_D{Available?}
        N_HV_S_A_D -- Yes --> N_HV_S_ANS(:checkmark: Ans)
        N_HV_S_A_D -- No --> N_HV_S_VOICEMAIL(:microphone: Voicemail)
    end
    
    %% Nested Parallel Other Queue
    subgraph SG_OTHER [OTHER QUEUE]
        direction TD
        style SG_OTHER stroke:#F5F5F5,fill:#FFF
        N_HV_O_Q(:gear: Other Issues Queue) --> N_HV_O_A_D{Available?}
        N_HV_O_A_D -- Yes --> N_HV_O_ANS(:checkmark: Ans)
        N_HV_O_A_D -- No --> N_HV_O_VOICEMAIL(:microphone: Voicemail)
    end

end

class N_HV_P_M orangeFill
class N_HV_M_S orangeFill
class N_HV_P_Q,N_HV_P_A_D purpleFill
class N_HV_P_ANS greenFill
class N_HV_P_VOICEMAIL orangeFill
class N_HV_S_Q,N_HV_S_A_D cyanFill
class N_HV_S_ANS greenFill
class N_HV_S_VOICEMAIL orangeFill
class N_HV_O_Q,N_HV_O_A_D greyFill
class N_HV_O_ANS greenFill
class N_HV_O_VOICEMAIL orangeFill

%% --- VOICEMAIL HANDLING Subgraph ---
%% Connect inputs to the main handling block
N_S_VOICEMAIL --> SG_VOICEMAIL
N_HV_P_VOICEMAIL --> SG_VOICEMAIL
N_HV_S_VOICEMAIL --> SG_VOICEMAIL
N_HV_O_VOICEMAIL --> SG_VOICEMAIL

subgraph SG_VOICEMAIL [VOICEMAIL HANDLING PROCESS]
    direction TD
    style SG_VOICEMAIL stroke:#FFF1F0,fill:#FFF
    N_V_L(:microphone: Caller Leaves Voicemail) --> N_V_T(:page_facing_up: Ticket Auto Created)
    N_V_T --> N_V_F(:construction_worker: Technician Follows Up)
    N_V_F --> N_V_R(:checkmark: Issue Resolved)
end

class N_V_L,N_V_T,N_V_F darkRedFill
class N_V_R greenFill

```

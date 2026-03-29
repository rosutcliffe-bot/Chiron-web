# Kayak Simulator

An **ultra-realistic kayak simulator** built in Unity (URP), featuring physically
accurate water physics, immersive visuals, and both Arcade and Simulation control modes.
Play it directly in your browser via the included **WebGL build**, or run it as a
standalone desktop app.

---

## ✨ Features

| Category | Details |
|---|---|
| **Water Physics** | Gerstner wave simulation, Archimedes buoyancy, hydrodynamic drag, water current & Perlin turbulence |
| **Paddle System** | Per-blade hydrodynamic lift/drag, splash & wake VFX, feathered double-blade animation |
| **Controls** | Keyboard / gamepad / touch (mobile browser) via Unity Input System; Arcade mode (direct steering) & Simulation mode (realistic strokes) |
| **Camera** | Third-person chase, first-person cockpit, and orbiting cinematic views — switchable with `C` |
| **Visuals** | Custom HLSL ocean shader (depth fade, Gerstner displacement, foam, refraction, Fresnel reflections, animated normal maps) |
| **UI** | Main menu, pause menu, settings (audio/graphics/controls), in-game HUD (speed, heading, stroke indicators) |
| **Web Interface** | Custom WebGL template with responsive layout, loading screen, fullscreen toggle, mute button, and mobile touch controls |
| **Architecture** | Modular C# namespace structure, singleton managers, object pooling for VFX |

---

## 🚀 How to Run This Project

There are **three ways** to run Kayak Simulator — pick whichever suits you best.

### Option A — Play in the Unity Editor (recommended for development)

> **Best for:** testing changes, tweaking values, debugging.

1. **Install Unity 2022.3 LTS** (or newer) via [Unity Hub](https://unity.com/download).
2. **Clone this repository:**
   ```bash
   git clone https://github.com/rosutcliffe-bot/Kayak-game.git
   ```
3. **Open the project:** In Unity Hub → click **Open** → navigate to the cloned
   folder and select it.
4. Unity will import the project and install packages listed in
   `Packages/manifest.json` (URP, Input System, TextMeshPro, Cinemachine).
5. **Configure URP:**
   - Go to **Edit → Project Settings → Graphics**.
   - If no URP asset is assigned, create one via
     **Assets → Create → Rendering → URP Asset (with Universal Renderer)** and
     drag it into the Graphics settings.
6. **Enable the Input System:**
   - Go to **Edit → Project Settings → Player → Other Settings →
     Active Input Handling** → set to **Both**.
7. **Create the scenes** by following
   [`Documentation/SceneSetupGuide.md`](Documentation/SceneSetupGuide.md):
   - Create `Assets/Scenes/MainMenu.unity` (main menu UI, managers).
   - Create `Assets/Scenes/GameScene.unity` (ocean, kayak, HUD).
8. **Add scenes to Build Settings:**
   - **File → Build Settings** → drag in `MainMenu` (index 0) and
     `GameScene` (index 1).
9. **Press ▶ Play** in the Unity Editor to run the game.

### Option B — Build & Play in Your Browser (WebGL)

> **Best for:** sharing with anyone — they only need a web browser.

1. Complete steps 1–8 from Option A so the project compiles and scenes are ready.
2. **Build for WebGL** using one of these methods:

   **From the menu:**
   - Click **Kayak Simulator → Build WebGL** in the Unity menu bar.
   - The build output goes to a `WebGLBuild/` folder next to the project.

   **From the command line (CI / headless):**
   ```bash
   Unity -batchmode -projectPath . \
         -executeMethod KayakSimulator.Editor.WebGLBuilder.Build \
         -quit
   ```

   **Manually:**
   - **File → Build Settings** → select **WebGL** → click **Switch Platform**.
   - Set the WebGL template: **Edit → Project Settings → Player → Resolution and
     Presentation → WebGL Template** → choose **KayakSimulator**.
   - Click **Build** and choose an output folder.

3. **Serve the build locally** to test it in your browser:

   ```bash
   # Using Python (installed on most systems)
   cd WebGLBuild
   python3 -m http.server 8000

   # OR using the included helper script
   python3 server.py
   ```
   Then open **http://localhost:8000** in Chrome, Firefox, or Edge.

4. **Deploy to the web** (optional) — upload the `WebGLBuild/` folder to any
   static host:
   - [GitHub Pages](https://pages.github.com) (free)
   - [Netlify](https://www.netlify.com) (free tier)
   - [itch.io](https://itch.io) (free, built for games)
   - Any web server (Apache, Nginx, S3, etc.)

   See [`Documentation/WebGLDeploymentGuide.md`](Documentation/WebGLDeploymentGuide.md)
   for detailed hosting instructions.

### Option C — Standalone Desktop Build

> **Best for:** distributing an .exe / .app without needing a browser.

1. Complete steps 1–8 from Option A.
2. **File → Build Settings** → select **PC, Mac & Linux Standalone** →
   click **Build** → choose an output folder.
3. Run the generated executable.

---

## 🎮 Controls

| Action | Keyboard | Gamepad | Mobile Browser |
|---|---|---|---|
| Left paddle stroke | `A` | Left Trigger | Left touch zone |
| Right paddle stroke | `D` | Right Trigger | Right touch zone |
| Steer | `←` / `→` arrow keys | Left Stick X | Swipe centre zone |
| Lean (Simulation mode) | `Q` / `E` | Right Stick X | — |
| Pause | `Escape` | Start | — |
| Switch camera | `C` | — | — |
| Toggle fullscreen | — | — | ⛶ toolbar button |
| Mute / unmute audio | — | — | 🔊 toolbar button |

---

## 📁 Project Structure

```
Assets/
├── Scripts/
│   ├── Core/
│   │   ├── GameManager.cs          – Game state & simulation mode singleton
│   │   └── InputManager.cs         – Unified keyboard / gamepad input
│   ├── Physics/
│   │   ├── GerstnerWaveSystem.cs   – CPU/GPU Gerstner wave model
│   │   ├── BuoyancySystem.cs       – Multi-point Archimedes buoyancy
│   │   ├── KayakPhysicsController.cs – Propulsion, steering, righting torque
│   │   └── WaterPhysics.cs         – High-level water API (height, normal, velocity)
│   ├── Paddle/
│   │   ├── PaddleBlade.cs          – Hydrodynamic blade forces + VFX
│   │   └── PaddleController.cs     – Paddle mesh animation & stroke timing
│   ├── Controls/
│   │   └── CameraController.cs     – Third-person / FP / orbital camera
│   ├── Water/
│   │   ├── WaterSurface.cs         – Procedural mesh + CPU vertex update
│   │   └── WaterEffects.cs         – Pooled splash & ripple particle system
│   ├── UI/
│   │   ├── MainMenuManager.cs
│   │   ├── PauseMenuManager.cs
│   │   ├── SettingsManager.cs      – Audio, graphics, sensitivity (PlayerPrefs)
│   │   └── HUDManager.cs           – Speed, heading, stroke power bars
│   ├── WebGL/
│   │   ├── WebGLBridge.cs          – C# ↔ browser JavaScript bridge
│   │   └── WebGLInitializer.cs     – Bootstrap the bridge on scene load
│   └── Utilities/
│       ├── SmoothFollow.cs         – Smooth transform follower
│       └── ObjectPool.cs           – Generic component object pool
│
├── Editor/
│   └── WebGLBuilder.cs             – One-click WebGL build script
│
├── Plugins/
│   └── WebGL/
│       └── WebGLBridge.jslib       – JavaScript plugin for browser APIs
│
├── WebGLTemplates/
│   └── KayakSimulator/
│       └── index.html              – Custom web page (loading screen, toolbar, touch controls)
│
├── Shaders/
│   └── Water/
│       └── OceanSurface.shader     – URP HLSL shader with foam, refraction, Fresnel
│
├── Materials/           – (create OceanSurface.mat, assign shader)
├── Prefabs/             – (kayak, paddle, splash/ripple particles)
├── Scenes/              – (MainMenu.unity, GameScene.unity)
├── Textures/            – (normal maps, foam texture)
└── Audio/               – (paddle splash, ambient water/wind)

Packages/
└── manifest.json        – URP 14, Input System 1.7, TMPro 3.0, Cinemachine 2.9

ProjectSettings/
└── ProjectVersion.txt   – Unity 2022.3 LTS

Documentation/
├── SceneSetupGuide.md           – Full step-by-step scene assembly instructions
└── WebGLDeploymentGuide.md      – How to build, test, and deploy the web version

server.py                        – Tiny local HTTP server for testing WebGL builds
```

---

## 🔧 Architecture Notes

- All systems communicate through **singletons** (`GameManager`, `InputManager`,
  `WaterPhysics`, `WaterEffects`) to keep components loosely coupled.
- **GerstnerWaveSystem** runs the same wave math on both the CPU (for physics
  accuracy) and the GPU (via shader parameters synced each frame).
- Replacing Gerstner with an FFT ocean requires only changing `GerstnerWaveSystem`;
  all callers use the `WaterPhysics` façade.
- Object pooling (`ObjectPool<T>` and `WaterEffects`) avoids runtime GC allocations
  from particle spawning.
- The **WebGL bridge** (`WebGLBridge.cs` + `WebGLBridge.jslib`) lets C# code talk
  to the browser (fullscreen, mute, filesystem sync) and lets the HTML toolbar
  call back into Unity via `SendMessage`.

---

## 📖 Documentation

| Guide | Description |
|---|---|
| [`SceneSetupGuide.md`](Documentation/SceneSetupGuide.md) | Hierarchy layouts, material settings, Rigidbody values, extension points |
| [`WebGLDeploymentGuide.md`](Documentation/WebGLDeploymentGuide.md) | Building for WebGL, local testing, GitHub Pages / Netlify / itch.io hosting |


---
name: unity-xr
description: Use when building Unity VR/AR experiences — the XR Interaction Toolkit or AR Foundation. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity XR (VR/AR)

## Retrieval Sources

The local Unity 6.3 docs at `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` have **far deeper XR coverage than a shallow pass suggests**. Beyond the core plug-in-architecture/input/scene-setup pages, the Manual has a full `XR/` section tree covering project setup, XR Origin, input, graphics/stereo rendering, foveated rendering, Multiview Render Regions, resolution scaling, tile-GPU optimization, frame timing, audio, and platform-specific workflows for Android XR and Meta Quest — plus package *landing pages* (description/version/keywords only, not full guides) for every XR provider plug-in and support package Unity ships (OpenXR, Oculus, Meta-OpenXR, Android XR-OpenXR, visionOS, XR Interaction Toolkit, AR Foundation, ARCore, ARKit, XR Hands, XR Composition Layers, XR Plugin Management). ScriptReference has exhaustive member-level coverage of the `UnityEngine.XR` namespace (input, display, mesh subsystems, tracking) but **no entries for XRI or AR Foundation types** (`XRGrabInteractable`, `ARSession`, `ARPlane`, `ARAnchorManager`, etc.) — those live in separate, non-local package documentation, so verify exact member names/signatures against the installed package's own docs (Window > Package Manager > select package > View documentation) before citing them as fact.

| Source | Path | Use for |
|--------|------|---------|
| XR overview / landing | `Manual/XR.html` | VR/MR/AR definitions, top-level topic map, sample projects |
| XR architecture | `Manual/XRPluginArchitecture.html` | Plug-in framework, subsystem interfaces (`XRDisplaySubsystem`, `XRInputSubsystem`, `XRMeshSubsystem`), provider model |
| XR project set up (index) | `Manual/configuring-project-for-xr.html` | Checklist: plug-in management → support packages → scene components |
| Create an XR project | `Manual/xr-create-projects.html` | Unity Hub XR templates (AR Mobile, Mixed Reality, MR multiplayer tabletop, VR, VR Multiplayer) vs. converting a non-XR project |
| Choose/configure XR provider plug-ins | `Manual/xr-configure-providers.html` | Step-by-step: install XR Plug-in Management, enable providers per build-target tab, configure, validate |
| XR Plug-in Management settings reference | `Manual/xr-plugin-management.html` | Settings UI reference, Project Validation system, severity levels, Fix/Edit buttons |
| XR Origin | `Manual/xr-origin.html` | Tracking-space-to-world-space transform, XR Origin configurations table (VR/AR/MR variants) |
| XR scene setup (landing) | `Manual/xr-scene-setup-landing.html` | Section index for scene setup topics |
| Set up an XR scene | `Manual/xr-scene-setup.html` | Prerequisites, step-by-step scene setup, `XRSettings.enabled` runtime check |
| XR input options | `Manual/xr-input-overview.html` | XRI vs. OpenXR interaction profiles vs. Input System vs. `XR.InputDevice`/`XR.Node` vs. third-party; visionOS input modes |
| Unity XR Input (deep API guide) | `Manual/xr_input.html` | `InputFeatureUsage`, `InputDevice`, `InputDeviceCharacteristics`, `InputDeviceRole`, `XRNode`, hand/eye tracking, haptics — with full C# examples |
| Run an XR application | `Manual/xr-run.html` | Build & Run, hybrid Play Mode, Mock HMD, XR Device Simulator, XR Simulation |
| XR graphics (index) | `Manual/xr-graphics.html` | Topic map for stereo rendering, foveation, Multiview Render Regions, frame timing, resolution scaling, tile rendering |
| Stereo rendering (index) | `Manual/xr-stereo-rendering.html` | Multi-pass vs. single-pass instanced overview |
| URP compatibility in XR | `Manual/xr-render-pipeline-compatibility.html` | Per-feature support table (Bloom, MSAA, GI, Shader Graph caveats, camera stacking, etc.) |
| Foveated rendering | `Manual/xr-foveated-rendering.html` | Fixed vs. gaze-based foveation, prerequisites, `XRDisplaySubsystem.foveatedRenderingLevel`/`foveatedRenderingFlags`, full C# example |
| Multiview Render Regions | `Manual/xr-multiview-render-regions.html` | Vulkan-only nasal-region culling, prerequisites, render-graph pass-compatibility list |
| Resolution control in XR projects | `Manual/xr-graphics-resolution-scaling.html` | `XRSettings.renderViewportScale` vs. URP render-scale asset vs. HDRP `eyeTextureResolutionScale`, dynamic resolution |
| Optimize for untethered XR devices in URP | `Manual/xr-untethered-device-optimization.html` | Tile-GPU checklist: Vulkan, OpenXR, forward rendering, on-tile post-processing, avoid geometry shaders, MSAA, disable depth priming/SSAO/HDR |
| VR frame timing | `Manual/VRFrameTiming.html` | Multithreaded render sync, dropped-frame/reprojection behavior, `XR.WaitForGPU` profiler marker, GPU- vs. CPU-bound diagnosis |
| XR audio | `Manual/xr-audio.html` | Ambisonic decoders, spatializer plug-ins for immersive 3D audio |
| XR packages (overview) | `Manual/xr-support-landing.html` | Section index for the packages page |
| XR packages (full list) | `Manual/xr-support-packages.html` | Complete provider-plug-in table + support-package table, version-matching rule (AR Foundation/ARCore/ARKit must match), per-platform notes (visionOS, Magic Leap EOL, WMR, Meta Quest, Quest 1 downgrade path) |
| Develop for Android XR | `Manual/xr-android-xr-develop.html` | Entry point for Android XR resources |
| Android XR build platform/profile | `Manual/xr-android-xr-build-profile.html` | Build Profiles window setup, auto-enabled Foveated Rendering |
| Packages/templates for Android XR | `Manual/xr-android-xr-packages.html` | Unity OpenXR plug-in + Android XR-OpenXR extension package, Google partner packages, templates |
| Develop for Meta Quest workflow | `Manual/xr-meta-quest-develop.html` | Entry point for Meta Quest resources, supported headsets (Quest 2/3/3S/Pro) |
| Meta Quest build platform/profile | `Manual/xr-meta-quest-build-profile.html` | Auto-installed OpenXR Plugin, default Player/Quality settings table (Vulkan, IL2CPP, ARM64, Instancing) |
| Packages/templates for Meta Quest | `Manual/xr-meta-quest-packages.html` | Unity OpenXR: Meta package, Platform-Browser-installable Meta SDKs, passthrough via Mixed Reality template |
| XR Plugin Management (package landing) | `Manual/com.unity.xr.management.html` | `com.unity.xr.management` description/version |
| OpenXR Plugin (package landing) | `Manual/com.unity.xr.openxr.html` | `com.unity.xr.openxr` description/version — Khronos open standard |
| Oculus XR Plugin (package landing) | `Manual/com.unity.xr.oculus.html` | `com.unity.xr.oculus` description/version |
| Unity OpenXR: Meta (package landing) | `Manual/com.unity.xr.meta-openxr.html` | `com.unity.xr.meta-openxr` — Meta OpenXR vendor-extension layer |
| Unity OpenXR: Android XR (package landing) | `Manual/com.unity.xr.androidxr-openxr.html` | `com.unity.xr.androidxr-openxr` — Android XR vendor-extension layer |
| Apple visionOS XR Plugin (package landing) | `Manual/com.unity.xr.visionos.html` | `com.unity.xr.visionos` description/version |
| XR Interaction Toolkit (package landing) | `Manual/com.unity.xr.interaction.toolkit.html` | `com.unity.xr.interaction.toolkit` — Interactor/Interactable/Interaction Manager summary, version table |
| XR Hands (package landing) | `Manual/com.unity.xr.hands.html` | `com.unity.xr.hands` — cross-platform hand-tracking subsystem |
| XR Composition Layers (package landing) | `Manual/com.unity.xr.compositionlayers.html` | `com.unity.xr.compositionlayers` — compositor-submitted layers for sharper UI/text/video |
| AR Foundation (package landing) | `Manual/com.unity.xr.arfoundation.html` | `com.unity.xr.arfoundation` description/version table |
| Google ARCore XR Plugin (package landing) | `Manual/com.unity.xr.arcore.html` | Feature list: background rendering, horizontal planes, depth, anchors, hit testing, occlusion |
| Apple ARKit XR Plugin (package landing) | `Manual/com.unity.xr.arkit.html` | Feature list: adds face tracking, environment probes, meshing vs. ARCore |
| XR module (built-in package landing) | `Manual/com.unity.modules.xr.html` | `com.unity.modules.xr` — built-in VR/AR platform support module |
| `XRInputSubsystem` | `ScriptReference/XR.XRInputSubsystem.html` | Tracking-origin mode, boundary points, recentering, per-subsystem device list |
| `XRDisplaySubsystem` | `ScriptReference/XR.XRDisplaySubsystem.html` | Render passes, foveation properties, mirror-view blit modes, dropped-frame/GPU-time stats |
| `XRMeshSubsystem` | `ScriptReference/XR.XRMeshSubsystem.html` | Async mesh generation from spatial mapping (AR/MR scene reconstruction) |
| `XR.InputDevices` / `XR.InputDevice` | `ScriptReference/XR.InputDevices.html`, `ScriptReference/XR.InputDevice.html` | Device enumeration by characteristics/role/node, `TryGetFeatureValue`, haptics |
| `XR.CommonUsages` | `ScriptReference/XR.CommonUsages.html` | Canonical `InputFeatureUsage` names (trigger, grip, primaryButton, etc.) |
| `XR.InputTracking` | `ScriptReference/XR.InputTracking.html` | Legacy-path local position/rotation per `XRNode`, recenter |
| `XR.XRNode` | `ScriptReference/XR.XRNode.html` | Head/eye/hand/tracking-reference node enum |
| `XR.TrackingOriginModeFlags` | `ScriptReference/XR.TrackingOriginModeFlags.html` | Device / Floor / TrackingReference / Unbounded origin modes |
| `XR.XRSettings` | `ScriptReference/XR.XRSettings.html` | `enabled`, `renderViewportScale`, `eyeTextureResolutionScale`, `stereoRenderingMode` |
| `XR.Bone` / `XR.Hand` / `XR.Eyes` | `ScriptReference/XR.Hand.html`, `XR.Bone.html`, `XR.Eyes.html` | Low-level hand-skeleton and gaze data structures |

## Key Guidelines

### XR Plug-in Management Setup

XR support is off by default; nothing renders in a headset or detects an AR surface until a provider plug-in is enabled for the active build target. Per `Manual/xr-configure-providers.html`, the flow is: install the **XR Plug-in Management** package (installable directly from the Project Settings window if missing) → open **Edit > Project Settings > XR Plug-in Management** → select the tab for the target build platform → check the box for the desired provider (OpenXR, Oculus, Apple visionOS XR, etc.) — this both enables the provider and installs its package via the Package Manager. Disabling a provider does **not** remove its package; use the Package Manager for that. Each enabled provider gets its own settings page in the left-hand XR Plug-in Management menu. Always run **Project Validation** (same section) before shipping — its rules are per-platform and per-severity: a red stop icon blocks the build and must be fixed, a yellow warning can be deferred but usually indicates a real compatibility or performance issue, and green checks (hidden unless "Show all" is enabled) confirm a passed rule. Many issues have a one-click **Fix** button.

```csharp
// Detect at runtime whether the XR subsystems are loaded/active
// (Manual/xr-scene-setup.html) — useful for scenes that support both
// XR and non-XR contexts.
public void CheckXRStatus()
{
    if (UnityEngine.XR.XRSettings.enabled)
    {
        Debug.Log("XR is active.");
    }
    else
    {
        Debug.Log("XR is not available.");
    }
}
// Note: XRSettings.enabled is read-only at runtime for this purpose —
// setting it does nothing. Loader lifecycle (init/deinit) is managed
// through the XR Plug-in Management API, not this property.
```

### Creating and Configuring an XR Project

The fastest path is a Unity Hub XR template — **AR Mobile**, **Mixed Reality**, **Mixed Reality multiplayer tabletop**, **VR**, or **VR Multiplayer** — each of which pre-configures URP, the relevant provider plug-ins (OpenXR, AR Foundation), the XR Interaction Toolkit, and an example scene (`Manual/xr-create-projects.html`). Templates still require you to open XR Plug-in Management and enable plug-ins for every target platform you actually ship to — the template only wires up the primary target. Converting an existing non-XR project follows the same three steps manually: configure XR Plug-in Management, add packages (AR Foundation and/or XR Interaction Toolkit) via Package Manager, then set up the scene. If a build-target tab is missing from XR Plug-in Management, the platform module isn't installed in the Editor — add it from the Unity Hub, not from within the project.

### XR Origin & Scene Setup

Unlike a standard scene, an XR scene needs an **XR Origin** to transform physical tracking-space data (head, hands/controllers) into scene world space (`Manual/xr-origin.html`). XR devices pick their own tracking-space origin at init (typically at/below the HMD or handheld device); without an XR Origin, the user would appear to stand at world-space `(0,0,0)`. Add the XR Origin GameObject wherever you want the user to start, and rotate it around Y to set facing direction — never move the scene-origin content itself. **Never have more than one active XR Origin in a scene at once**; if you need multiple configurations for different purposes, only enable one at a time. Which configuration to add via **GameObject > XR** depends on installed packages:

| Configuration | Package | Notes |
|---|---|---|
| XR Rig (legacy) | XR Legacy Input Helpers | "Convert Main Camera to XR Rig"; removed once XRI is installed; least compatible with newer features |
| XR Origin | XR Core Utils (via XRI) | No controller GameObjects included |
| XR Origin (VR) | XR Interaction Toolkit | Includes action-based (or device-based) controller GameObjects |
| XR Origin (AR) | AR Foundation (+ XRI) | Handheld AR tracking origin, includes controller GameObjects |
| XR Origin (Mobile AR) | AR Foundation | Handheld AR tracking origin, no controllers; superseded by XR Origin (AR) once XRI is installed |

visionOS is a special case: AR/MR apps there use a **Volume Camera** instead of an XR Origin, and VR apps still need an `ARSession` object from AR Foundation purely to access head/hand tracking and bonus ARKit data (plane detection, scene meshes, image tracking).

### XR Input

`Manual/xr-input-overview.html` lists five overlapping input paths — XRI, OpenXR interaction profiles, the Input System/Input Manager, the `XR.InputDevice`/`XR.Node` APIs, and third-party libraries — and it's normal to mix them (e.g., XRI for grabbing, Input System for a pause button, `XR.Node` for animating a controller mesh). **The one hard rule: if OpenXR is your provider, the legacy Input Manager is not supported at all — you must use the Input System** (its `TrackedPoseDriver` component tracks an HMD/controller GameObject).

The low-level `UnityEngine.XR` input API (`Manual/xr_input.html`) is platform-agnostic by design: `InputFeatureUsage` gives every physical control (trigger, grip, primary2DAxis, primaryButton, batteryLevel, etc.) a stable cross-platform name regardless of which XR system reports it. Devices are enumerated via `InputDevices`, filtered by `InputDeviceCharacteristics` (HeadMounted, HeldInHand, HandTracking, EyeTracking, Left/Right, …) or `InputDeviceRole` or `XRNode`, and values are read with `InputDevice.TryGetFeatureValue`, which returns `false` for unsupported features or a disconnected device — always check the return value, not just the out-param.

```csharp
// Enumerate connected devices (Manual/xr_input.html)
var inputDevices = new List<UnityEngine.XR.InputDevice>();
UnityEngine.XR.InputDevices.GetDevices(inputDevices);
foreach (var device in inputDevices)
    Debug.Log($"Device '{device.name}' role '{device.role}'");

// Filter by characteristics (left-handed controller)
var desired = UnityEngine.XR.InputDeviceCharacteristics.HeldInHand
            | UnityEngine.XR.InputDeviceCharacteristics.Left
            | UnityEngine.XR.InputDeviceCharacteristics.Controller;
var leftHandedControllers = new List<UnityEngine.XR.InputDevice>();
UnityEngine.XR.InputDevices.GetDevicesWithCharacteristics(desired, leftHandedControllers);

// Read a feature value defensively
bool triggerValue;
if (device.TryGetFeatureValue(UnityEngine.XR.CommonUsages.triggerButton, out triggerValue)
    && triggerValue)
{
    Debug.Log("Trigger button is pressed.");
}

// Haptics
if (device.TryGetHapticCapabilities(out var capabilities) && capabilities.supportsImpulse)
{
    device.SendHapticImpulse(channel: 0, amplitude: 0.5f, duration: 1.0f);
}
```

Devices connect/disconnect at any time — subscribe to `InputDevices.deviceConnected`/`deviceDisconnected` rather than polling, and check `InputDevice.isValid` at the top of each frame before using a cached reference. Hand-tracking devices carry the `HandTracking` characteristic and a `CommonUsages.handData` feature (`Hand` type, up to 21 `Bone`s, root bone + per-finger chains via `TryGetRootBone`/`TryGetFingerBones`); eye-tracking devices expose `CommonUsages.eyesData` (`Eyes` type: per-eye position/rotation, gaze fixation point, blink amount). The `XRInputSubsystem` associated with a device (get via `SubsystemManager.GetInstances<XRInputSubsystem>()`) owns global input state not tied to one device — tracking-origin mode (`Device`/`Floor`/`TrackingReference`/`Unbounded`), boundary/guardian points, and recentering.

### XR Interaction Toolkit Concepts (pretrained knowledge — local docs are landing-page-only)

**The local Manual only has a package landing page** (`Manual/com.unity.xr.interaction.toolkit.html`) with a description and version table, not a full component guide — XRI's actual API (`XRGrabInteractable`, `XRRayInteractor`, `XRSocketInteractor`, `XRDirectInteractor`, `ActionBasedController`, `LocomotionMediator`, etc.) is documented only in the separately-versioned package docs. Treat the following as pretrained knowledge and **verify exact type/member names against the installed package's own documentation** (Package Manager > XR Interaction Toolkit > View documentation) before relying on them in generated code.

XRI's core pattern is **Interactor / Interactable**, coordinated by a scene-wide **Interaction Manager**: an Interactor (usually driven by a controller or hand and typically a `XRDirectInteractor`, `XRRayInteractor`, or `XRPokeInteractor`) detects nearby/targeted Interactables (`XRBaseInteractable`-derived components such as `XRGrabInteractable` or `XRSimpleInteractable`) and the Interaction Manager brokers select/hover/activate state transitions between them. Don't hand-roll raycasting or trigger-collider grab logic — this system already owns that, and reimplementing it tends to fight the built-in locomotion, UI-interaction, and audio/haptic feedback systems that assume the standard event flow (`selectEntered`/`selectExited`/`hoverEntered`/`hoverExited`). The toolkit also ships locomotion providers (teleportation, continuous/smooth movement, climbing, turning) and Unity UI (uGUI) interaction support so world-space canvases work with ray interactors out of the box.

```csharp
// Pretrained-knowledge sketch of the Interactor/Interactable event pattern —
// verify exact API against the installed XRI package version before using.
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit.Interactables;
using UnityEngine.XR.Interaction.Toolkit.Interactors;

public class GrabFeedback : MonoBehaviour
{
    [SerializeField] private XRGrabInteractable grabInteractable;

    void OnEnable()
    {
        grabInteractable.selectEntered.AddListener(OnGrabbed);
        grabInteractable.selectExited.AddListener(OnReleased);
    }

    void OnDisable()
    {
        grabInteractable.selectEntered.RemoveListener(OnGrabbed);
        grabInteractable.selectExited.RemoveListener(OnReleased);
    }

    void OnGrabbed(SelectEnterEventArgs args) { /* haptic pulse, highlight, etc. */ }
    void OnReleased(SelectExitEventArgs args) { }
}
```

### AR Foundation Concepts (pretrained knowledge — local docs are landing-page-only)

The local Manual likewise only has a landing page for AR Foundation (`Manual/com.unity.xr.arfoundation.html`) plus landing pages for its two mainstream providers, ARCore (`Manual/com.unity.xr.arcore.html`) and ARKit (`Manual/com.unity.xr.arkit.html`) — those two pages do at least confirm the **feature lists**: ARCore supports background rendering, horizontal planes, depth data, anchors, hit testing, and occlusion; ARKit adds face tracking, environment probes, and meshing on top of the same base set. Full manager/subsystem API (`ARPlaneManager`, `ARAnchorManager`, `ARTrackedImageManager`, `ARRaycastManager`, `ARCameraManager`, etc.) lives only in the package's own docs — treat specifics below as pretrained knowledge to verify against the installed version.

AR Foundation is a **cross-platform abstraction** over ARKit/ARCore (and visionOS/OpenXR-based AR providers): write against its manager components and subsystem interfaces so the same C# runs unmodified on iOS and Android. The common pattern is a manager-per-feature, each raising `.trackablesChanged` (or similarly-named) events with added/updated/removed collections: `ARPlaneManager` for horizontal/vertical plane detection, `ARAnchorManager` for spatial anchors that persist a pose in the real world, `ARTrackedImageManager` for 2D image tracking against a reference-image library, `ARRaycastManager` for hit-testing against detected geometry, and `ARMeshManager`/`ARPointCloudManager` for scene reconstruction. **A scene without an active `ARSession` and an AR-capable origin (`XR Origin (AR)` / `XR Origin (Mobile AR)`, per `Manual/xr-origin.html`) will have these features silently no-op** — no errors, just empty event lists — which is the most common "AR isn't working" report.

```csharp
// Pretrained-knowledge sketch of the plane-detection manager pattern —
// verify exact API against the installed AR Foundation package version.
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class PlaneLogger : MonoBehaviour
{
    [SerializeField] private ARPlaneManager planeManager;

    void OnEnable() => planeManager.trackablesChanged.AddListener(OnPlanesChanged);
    void OnDisable() => planeManager.trackablesChanged.AddListener(OnPlanesChanged);

    void OnPlanesChanged(ARTrackablesChangedEventArgs<ARPlane> args)
    {
        foreach (var plane in args.added)
            Debug.Log($"New plane detected: {plane.trackableId}, alignment {plane.alignment}");
    }
}
```

`Manual/xr-run.html` also documents **XR Simulation**: AR Foundation ships pre-built test environments you can drive in Play Mode in the Editor, reporting simulated planes and trackables as if the device were moving through a real room — this is the fastest AR iteration loop and avoids constant device deploys.

### XR Graphics, Stereo Rendering & Performance Techniques

Rendering in XR follows the same principles as any Unity app but with two structural differences: every visible object is drawn twice (once per eye) unless single-pass techniques are used, and XR apps need very high, very *consistent* frame rates for comfort (`Manual/xr-graphics.html`). Stereo rendering has multiple modes selectable per-provider — multi-pass (simplest, most compatible, least efficient), single-pass instanced, and single-pass multiview — configured under XR Plug-in Management (`Manual/xr-stereo-rendering.html`); custom shaders may need updates to support single-pass instanced correctly. URP feature support in XR is uneven — check `Manual/xr-render-pipeline-compatibility.html` before relying on a specific post-processing effect (e.g., Lens Distortion and Spatial-Temporal Post-Processing are **not** supported in XR; Bloom, Motion Blur, Depth of Field, and GI are).

**Foveated rendering** (`Manual/xr-foveated-rendering.html`) trades peripheral resolution for GPU headroom by rendering the area outside the fovea at lower resolution. It has two device-dependent modes — fixed (always centers on the display center) and gaze-based (uses eye tracking, only on eye-tracking-capable hardware) — and must be enabled both in the provider's XR Plug-in Management settings *and* at runtime:

```csharp
// Manual/xr-foveated-rendering.html
public void SetFRLevel(XRDisplaySubsystem xrDisplaySubsystem, float strength)
{
    xrDisplaySubsystem.foveatedRenderingLevel = strength; // 0 = off, 1 = max
}

// Enable gaze-based foveation on devices that support it
xrDisplaySubsystem.foveatedRenderingFlags = XRDisplaySubsystem.FoveatedRenderingFlags.GazeAllowed;
```

Wait ~3 frames after the subsystem becomes available before touching its properties — it isn't fully initialized immediately.

**Multiview Render Regions** (`Manual/xr-multiview-render-regions.html`, Vulkan-only, Unity 6.1+) skips rendering the nasal region of the headset that's outside the user's view. In Unity 6.3+ it requires the render graph system (disable Compatibility Mode in Graphics settings), and only certain render-graph passes are marked `MultiviewRenderRegionsCompatible` — expect it to help mainly the final eye-texture pass unless you're on OpenXR 1.15+ with "All Passes" mode.

**Resolution scaling** (`Manual/xr-graphics-resolution-scaling.html`) is pipeline-specific: on URP without post-processing/HDR, prefer `XRSettings.renderViewportScale` — it can change every frame with no texture-reallocation cost and is compatible with fixed foveated rendering. With post-processing enabled, URP renders to intermediate textures instead, so enable **URP Dynamic Resolution** on the Main Camera and let `renderViewportScale` control the final blit viewport instead (not compatible with fixed foveation or TAA in that mode). Changing the URP asset's Render Scale value directly reallocates the eye texture — expensive, do not do this every frame. HDRP uses `XRSettings.eyeTextureResolutionScale` or the general dynamic-resolution system instead; `renderViewportScale` isn't supported there.

### Comfort & Performance for VR (Untethered/Mobile-GPU Devices)

`Manual/xr-untethered-device-optimization.html` is a concrete checklist for standalone (Quest, Android XR) devices, which use tile-based mobile GPUs: use **Vulkan** (not OpenGL ES) and **OpenXR**, use the render-graph system, use **Forward** rendering (Deferred's G-buffer requires multiple GMEM loads, expensive on tile GPUs), enable **on-tile post-processing** (Unity 6.3+) rather than leaving standard post-processing on (which forces intermediate textures), avoid **geometry shaders** entirely (they break the tiled binning pass and aren't supported on some devices), prefer **2x MSAA** over other AA (tile GPUs store extra samples cheaply), **disable depth priming** (no benefit with two-view XR rendering — use hardware LRZ/HSR instead), **disable Opaque/Depth Texture** properties unless required (extra GMEM copies), **disable SSAO** (needs a depth-priming pass plus blur passes — expensive), and **disable HDR** (most untethered headsets don't support it anyway). Combine with resolution scaling for a stable frame budget.

Offer **comfort options** regardless of how well-optimized the app is: teleport as an alternative to smooth/continuous locomotion, a vignette during movement or turning, and snap-turn as an alternative to smooth turning — these reduce the vestibular/visual mismatch that causes discomfort even at a stable frame rate. See **Advanced Notes** below for the frame-timing mechanics behind *why* dropped frames specifically cause motion sickness.

### Platform-Specific Workflows: Android XR & Meta Quest

Both platforms get a dedicated **Build Profile** in the Build Profiles window (`File > Build Profiles`, Unity 6.1+) that shares Player/Graphics/Quality settings with the underlying Android build platform by default, but lets you override them per-XR-target. Selecting the **Meta Quest** build platform auto-installs the OpenXR Plugin and, when you add the build profile, pre-configures Vulkan, IL2CPP, ARM64, and Instancing stereo rendering by default (`Manual/xr-meta-quest-build-profile.html`); you can additionally pull Meta's partner packages (Core/Audio/Haptics/Interaction/Platform/Voice/Simulator/MR-Utility-Kit SDKs) straight from the Platform Browser without touching the Asset Store. Selecting **Android XR** similarly auto-enables Android XR features and **Foveated Rendering** the first time you switch to that target (`Manual/xr-android-xr-build-profile.html`). Both are additive on top of core OpenXR: install the base **OpenXR Plugin** for cross-runtime portability, then layer the vendor extension package (**Unity OpenXR: Meta** or **Unity OpenXR: Android XR**) only for platform-specific features (e.g., Meta passthrough camera control) — write the shared logic against core OpenXR/AR Foundation APIs so it doesn't fork per platform.

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Headset not detected | No provider enabled under XR Plug-in Management for the active build target — enable it per-tab, per-platform |
| Motion sickness / judder reports | Frame drops below device refresh rate trigger reprojection; profile GPU/CPU cost (see Advanced Notes) and add comfort options |
| AR features never fire | Missing/inactive `ARSession` or an AR-capable XR Origin in the scene — these managers silently no-op with no error |
| Grab/interaction unresponsive | Interactor and Interactable aren't registered with the same Interaction Manager, or layer masks exclude each other |
| Works on one headset, not another | Code targets a vendor plug-in (Oculus API, Meta-specific extensions) directly instead of the OpenXR/AR Foundation abstraction |
| Legacy Input Manager code does nothing under OpenXR | OpenXR does not support the legacy Input Manager at all — must use the Input System (`TrackedPoseDriver`) or `XR.InputDevice` APIs |
| Multiple active XR Origins in one scene | Only one XR Origin should be enabled at a time; leftover duplicates from template scenes cause conflicting camera/controller tracking |
| Using "Convert Main Camera to XR Rig" / legacy XR Rig | Predates XR Origin; incompatible or partially compatible with newer XRI/AR Foundation features — use the XR Origin variant instead |
| AR Foundation / ARCore / ARKit version mismatch | These three package versions must match exactly (e.g., all 6.3.x) or the build/runtime breaks — check Package Manager versions together |
| Reallocating eye textures every frame | Changing the URP asset's Render Scale at runtime reallocates textures (expensive); use `XRSettings.renderViewportScale` instead for per-frame dynamic resolution |
| Post-processing left on for untethered targets | Forces intermediate textures on tile-based mobile GPUs, causing frame drops; use on-tile post-processing (6.3+) or disable it |
| Geometry shaders in an XR project targeting mobile/tile GPUs | Breaks the tiled binning pass and isn't supported on some devices — avoid entirely on Quest/Android XR |
| Expecting Multiview Render Regions to speed up post-processing passes | Unless configured for "All Passes" (OpenXR 1.15+) it only applies to the final eye-texture pass — profile before assuming a win |
| Ignoring yellow Project Validation warnings | They don't block the build but usually flag real compatibility/perf issues (e.g., missing render-graph setup for a feature) — fix before shipping |
| Assuming a code sample for `XRGrabInteractable`/`ARPlaneManager` etc. is exact | These types aren't in the local docs (landing pages only) — verify member names/signatures against the installed package version |

## Quick Reference

| Component / Concept | Purpose |
|---|---|
| XR Plug-in Management | Project Settings section: enable/configure provider plug-ins per build target, run Project Validation |
| XR provider plug-in | Package implementing subsystem interfaces for a device platform (OpenXR, Oculus, visionOS, ARCore, ARKit, …) |
| OpenXR Plugin | Cross-vendor Khronos standard provider; preferred over vendor-specific plug-ins for portability |
| Unity OpenXR: Meta / Android XR | Vendor extension layers on top of core OpenXR for Meta Quest / Android XR-specific features |
| XR Origin | GameObject hierarchy that transforms tracking-space data into scene world space; only one active per scene |
| XR Rig (legacy) | Predecessor to XR Origin via "Convert Main Camera"; less compatible with modern XRI/AR Foundation features |
| XR Interaction Toolkit (XRI) | Component-based interaction framework: Interactor, Interactable, Interaction Manager, locomotion, UI interaction |
| Interaction Manager | Scene-wide coordinator that brokers select/hover/activate events between Interactors and Interactables |
| Interactor / Interactable | Grab/point/select pattern for controllers or hands (`XRDirectInteractor`/`XRRayInteractor` ↔ `XRGrabInteractable`) |
| AR Foundation | Cross-platform abstraction over ARKit/ARCore/visionOS AR providers; manager-per-feature pattern |
| ARSession | AR Foundation lifecycle object; AR features no-op without an active session |
| XR Origin (AR) / (Mobile AR) | AR-specific XR Origin variants; scene-space tracking origin for handheld/passthrough AR |
| ARCore XR Plugin | Android AR provider: background rendering, planes, depth, anchors, hit testing, occlusion |
| ARKit XR Plugin | iOS/visionOS AR provider: adds face tracking, environment probes, meshing on top of ARCore's feature set |
| XR Hands | Cross-platform hand-tracking subsystem API; requires a hand-tracking-capable provider (e.g., OpenXR 1.12+) |
| XR Composition Layers | Submits sharper text/UI/video layers directly to the device compositor on supported OpenXR runtimes |
| `XRInputSubsystem` | Tracking-origin mode, boundary points, device recentering, per-subsystem device enumeration |
| `XRDisplaySubsystem` | Render pass/view management, foveation control, mirror-view blit modes, dropped-frame/GPU-time stats |
| `XRMeshSubsystem` | Async generation of meshes from spatial mapping (scene reconstruction for AR/MR) |
| `XR.InputDevice` / `InputDevices` | Device enumeration and per-device feature/haptics access |
| `XR.CommonUsages` | Canonical `InputFeatureUsage` names (trigger, grip, primaryButton, batteryLevel, handData, eyesData, …) |
| `XR.XRNode` | Enum of physical reference points (Head, LeftHand, RightHand, CenterEye, TrackingReference, …) |
| `XR.TrackingOriginModeFlags` | Device / Floor / TrackingReference / Unbounded tracking-origin modes |
| `XR.XRSettings` | `enabled`, `renderViewportScale`, `eyeTextureResolutionScale`, `stereoRenderingMode` |
| Foveated rendering | Lower peripheral resolution (fixed or gaze-based) to save GPU cost; `XRDisplaySubsystem.foveatedRenderingLevel` |
| Multiview Render Regions | Vulkan-only culling of the headset's unseen nasal region; requires render graph in Unity 6.3+ |
| Stereo rendering modes | Multi-pass, single-pass instanced, single-pass multiview — trade compatibility for efficiency |
| Mock HMD / XR Device Simulator | Simulate an HMD in Play Mode (Mock HMD) or drive interactions via keyboard/mouse (XRI Device Simulator) |
| XR Simulation | AR Foundation's in-Editor Play Mode test environments for AR without a physical device |
| Build Profiles (Android XR / Meta Quest) | Platform-specific settings overrides layered on the Android build platform |
| Project Validation | Per-platform rule engine in XR Plug-in Management flagging scene/project misconfiguration before build |

## Advanced Notes

### Frame rate and motion sickness

XR frame timing works like VSync-locked rendering, but Unity syncs to whichever VR SDK/runtime is active rather than the underlying 3D API's VSync (`Manual/VRFrameTiming.html`). There is no benefit to rendering faster than the display refresh — the runtime predicts the head pose **twice per frame** (once for the frame about to render, once for the following frame) to keep latency low, and Unity applies the first prediction to cameras/controllers for the current frame. **When a frame isn't ready in time for the next refresh, the runtime does not simply show a stale frame — it reprojects**: it takes the last submitted frame and warps it toward the newly predicted head pose (rotational reprojection is a crude approximation that looks wrong for anything animated or moving; positional/temporal reprojection tries to fill in more detail). This reprojection is what a user perceives as judder, and it is the direct mechanical link between dropped frames and motion sickness — the visual result no longer matches the vestibular signal from the inner ear. If an app *continually* drops frames, Unity falls back to rendering only every other frame, halving effective frame rate and making the mismatch worse.

Diagnose whether you're GPU- or CPU-bound with the Profiler's `XR.WaitForGPU` marker: if it exceeds one frame's time budget (`1 / refreshRate`, e.g., ~11.1 ms at 90 Hz) you're GPU-bound; if a frame takes longer than budget while `XR.WaitForGPU` stays under one frame, you're CPU-bound. `XRDisplaySubsystem.TryGetDroppedFrameCount`, `TryGetFramePresentCount`, and `TryGetAppGPUTimeLastFrame`/`TryGetCompositorGPUTimeLastFrame` (ScriptReference `XR.XRDisplaySubsystem.html`) give runtime access to the same signals for telemetry or adaptive-quality logic. Practical mitigation stacks in this order: fix the actual GPU/CPU cost first (tile-GPU checklist in `Manual/xr-untethered-device-optimization.html` — Vulkan, Forward rendering, on-tile post-processing, no geometry shaders, 2x MSAA, disabled depth priming/SSAO/HDR), then use resolution scaling (`XRSettings.renderViewportScale`, `Manual/xr-graphics-resolution-scaling.html`) to trade resolution for stable timing dynamically, then foveated rendering (`Manual/xr-foveated-rendering.html`) to spend the saved budget only where the fovea can perceive it, and only then rely on comfort UX (teleport, vignette, snap-turn) to soften whatever mismatch remains — comfort options manage *perception* of discomfort, they don't fix its root mechanical cause.

### Platform-specific XR provider setup (OpenXR)

**OpenXR is the default recommendation** for VR/MR: it's Khronos's open, royalty-free, cross-vendor standard, and `Manual/xr-support-packages.html` lists it as compatible with "any device with an OpenXR runtime" — Meta headsets, Vive/SteamVR, HoloLens, Windows Mixed Reality, and more — versus committing to a single vendor plug-in (Oculus XR Plugin) that only targets that vendor's hardware. The setup flow (`Manual/xr-configure-providers.html`) is uniform across platforms: XR Plug-in Management → per-build-target tab → enable **OpenXR** → configure OpenXR-specific settings (interaction profiles, render mode) in its own settings page → run Project Validation. Two structural facts to keep in mind: (1) **the OpenXR plug-in requires the Input System** — the legacy Input Manager is unsupported when OpenXR is active, so any code path assuming `Input.GetAxis` for XR controls will silently fail; and (2) core OpenXR only gets you the cross-runtime baseline — platform-specific features (Meta passthrough camera, Android XR extensions) require layering a **vendor extension package** (`Unity OpenXR: Meta` / `Unity OpenXR: Android XR`) on top, per `Manual/xr-android-xr-packages.html` and `Manual/xr-meta-quest-packages.html`. Both Meta Quest and Android XR now have first-class **Build Profiles** (`File > Build Profiles`, Unity 6.1+) that auto-install OpenXR and pre-set sensible defaults on selection — Meta Quest defaults to Vulkan + IL2CPP + ARM64 + Instancing stereo rendering (`Manual/xr-meta-quest-build-profile.html`); Android XR auto-enables Foveated Rendering the first time you switch to it (`Manual/xr-android-xr-build-profile.html`). For Windows Mixed Reality specifically, Unity 2021+ dropped the dedicated WMR provider package entirely in favor of OpenXR — enable OpenXR under the Windows/Mac/Linux tab and install Microsoft's Mixed Reality OpenXR plug-in via the Feature Tool referenced from that settings page. For Apple visionOS, windowed apps need only the visionOS platform module, but any real XR mode (VR/AR/MR) additionally requires the PolySpatial visionOS packages, which are gated behind a Unity Pro/Enterprise/Industry subscription (`Manual/xr-support-packages.html`).

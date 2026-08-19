# Unity 6.3 Skills

A set of 31 Claude Code Skills for Unity 6.3 LTS, grounded in Unity's official offline documentation (Manual + Scripting Reference) rather than pretrained knowledge alone. Each skill cites specific, verified documentation pages as its retrieval source, so answers stay accurate to Unity 6.3 rather than drifting to older or newer API versions.

## Install

Copy (or symlink) any `unity-*` folder into your Claude Code skills directory:

```bash
cp -r unity-*/ ~/.claude/skills/
```

Each skill auto-triggers when a task matches its description below — no manual invocation needed.

## Skills

| Skill | Use when |
|---|---|
| `unity-2d-physics-lowlevel` | Use when working with Unity's new low-level 2D physics API (Box2D-based rewrite) directly, as distinct from the classic Rigidbody2D/Collider2D component workflow. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-2d-tooling` | Use when building 2D Unity content beyond physics — Sprite Atlas, 2D Animation (bone rigging), or Tilemap. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-accessibility` | Use when implementing accessibility features in a Unity project — screen reader support, the Accessibility Hierarchy, or related APIs. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-addressables-assets` | Use when managing Unity asset loading/memory — the Addressables system, asset references, or legacy AssetBundles. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-ai-navigation` | Use when implementing Unity NPC pathfinding or navigation — NavMesh baking, NavMeshAgent, NavMeshObstacle, NavMeshSurface, or off-mesh links. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-animation-cinematics` | Use when working with Unity's Animator Controller, Timeline, Animation Rigging, or Cinemachine cameras. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-audio` | Use when implementing audio in Unity — AudioSource/AudioMixer setup, spatial audio, or mixer snapshots. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-build-deploy` | Use when configuring or troubleshooting a Unity build — Build Settings, per-platform Player Settings, IL2CPP vs Mono, or CI batch-mode builds. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-editor-scripting-extras` | Use when automating Unity Editor/tooling workflows beyond custom Inspectors — ScriptedImporter custom asset importers, the PackageManager Client scripting API, or CompilationPipeline/scripting define symbols. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-editor-tooling` | Use when building custom Unity Editor extensions — custom Inspectors, PropertyDrawers, EditorWindow tools, Gizmos, menu items, or AssetPostprocessors. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-input-system` | Use when handling player input in Unity via the Input System package — Action Maps, PlayerInput, control schemes, or device switching. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-networking-multiplayer` | Use when implementing Unity multiplayer — Netcode for GameObjects, Relay/Lobby services, or NetworkBehaviour/client-server patterns. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-performance-profiling` | Use when diagnosing or optimizing Unity performance — Profiler/Frame Debugger/Memory Profiler usage, GC allocation spikes, or Jobs/Burst/DOTS-ECS. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-physics` | Use when working with Unity's physics system — Rigidbody/Collider setup, CharacterController, raycasting, layers and the collision matrix, or choosing between 3D and 2D physics. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-playables-api` | Use when working with Unity's low-level Playables API directly — PlayableGraph, PlayableOutput, custom PlayableBehaviours, or blending/mixing playables outside the Timeline/Animator workflow layer. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-procedural-mesh` | Use when generating or modifying mesh geometry from code — the Mesh class, vertex/triangle/UV buffers, or the Mesh.MeshData advanced mesh API. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-project-structure` | Use when setting up a new Unity project's folder layout, configuring Assembly Definitions, managing packages via manifest.json, or setting up version control (.gitignore, meta files, Git LFS) for a Unity repo. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-render-pipeline-scripting` | Use when writing low-level rendering code — CommandBuffer, ScriptableRenderPass/Renderer Features, RenderPipelineManager callbacks, or compute shaders/ComputeBuffer/GraphicsBuffer. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-rendering-pipelines` | Use when choosing or configuring a Unity render pipeline (Built-in, URP, HDRP), authoring Shader Graph shaders, setting up materials/lighting, or configuring post-processing Volumes. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-scene-management` | Use when loading, unloading, or organizing Unity scenes — SceneManager, additive/async scene loading, or multi-scene workflows. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-scripting-fundamentals` | Use when writing or reviewing Unity C# scripts — MonoBehaviour lifecycle methods, ScriptableObjects, coroutines, events/delegates, serialization, or custom attributes. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-services` | Use when integrating Unity Gaming Services — Cloud Save, Cloud Code, Economy, Remote Config, Analytics, Authentication, In-App Purchasing, Ads, Vivox, or Cloud Build/DevOps. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-shaders-hlsl` | Use when hand-authoring Unity shaders in HLSL/ShaderLab rather than Shader Graph — writing .shader files, custom lighting models, or shader passes/variants. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-testing` | Use when writing automated tests for Unity code — the Unity Test Framework, Edit Mode vs Play Mode tests, or the Test Runner. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-text-tmp` | Use when displaying or styling text in Unity — TextMeshPro (TMP_Text/TMP_FontAsset), rich text tags, or TextCore font rendering. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-ui` | Use when building Unity UI — deciding between UI Toolkit (UXML/USS) and uGUI (Canvas/RectTransform), or implementing either. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-ui-toolkit-runtime` | Use when writing C# code against UI Toolkit's runtime API — building custom VisualElement controls, manipulators, data binding, or querying/mutating the UI Toolkit element tree from script. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-vfx-particles` | Use when creating particle/visual effects in Unity — choosing between the Shuriken Particle System and VFX Graph. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-video-playback` | Use when playing back or streaming video in Unity — VideoPlayer, VideoClip, or render-target/audio-output configuration for video. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-world-building` | Use when authoring Unity environments — the Terrain system, ProBuilder level geometry, or other world-building tools. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |
| `unity-xr` | Use when building Unity VR/AR experiences — the XR Interaction Toolkit or AR Foundation. Grounds answers in the local Unity 6.3 docs over pretrained knowledge. |

## Notes

- Skills bias toward retrieval from the local Unity 6.3 documentation over pretrained knowledge, and each one is explicit about the gaps where Unity's official offline docs bundle doesn't include full API coverage (e.g. Netcode for GameObjects, most Unity Gaming Services, XR Interaction Toolkit/AR Foundation deep API, DOTS/ECS, the TextMeshPro namespace, ProBuilder) rather than guessing.
- Built from Unity's official offline documentation download (`docs.unity3d.com/6000.3/Documentation/Manual/OfflineDocumentation.html`).

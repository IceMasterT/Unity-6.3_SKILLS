---
name: unity-animation-cinematics
description: Use when working with Unity's Animator Controller, Timeline, Animation Rigging, or Cinemachine cameras. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Animation & Cinematics

## Retrieval Sources

Docs root: `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/`

All paths below were verified to exist on disk at review time (`find`/`ls` against `Manual/` and `ScriptReference/`).

| Source | Path | Use for |
|--------|------|---------|
| Mecanim system introduction | `Manual/AnimationOverview.html` | Conceptual map of Animator/Avatar/Controller/Blend Tree pieces before diving into specifics |
| Animator Controller overview | `Manual/animation-animator-controller.html` | States, transitions, parameters, layers — the core controller model |
| Creating an Animator Controller | `Manual/AnimatorControllerCreation.html` | Step-by-step creation/wiring of a new controller asset |
| Animator window reference | `Manual/AnimatorWindow.html` | UI reference for the graph editor, parameters pane, layers pane |
| State machine basics | `Manual/StateMachineBasics.html` | States, default state, Any State, entry/exit nodes |
| State machine transitions | `Manual/StateMachineTransitions.html` | Conditions, exit time, transition duration/offset, interruption sources |
| State Machine Behaviours | `Manual/StateMachineBehaviours.html` | Scripting logic attached to individual states (OnStateEnter/Exit/Update/Move/IK) |
| Sub-state machines | `Manual/NestedStateMachines.html` | Nesting state machines for readability and reuse |
| Animation Layers | `Manual/AnimationLayers.html` | Layer weight, override vs additive blending, avatar mask per layer |
| Blend Trees | `Manual/animation-blend-trees.html` | 1D and 2D blend trees for locomotion and directional movement |
| Root Motion (how it works) | `Manual/RootMotion.html` | Root motion extraction from clips, applyRootMotion, deltaPosition/deltaRotation |
| Scripting Root Motion | `Manual/ScriptingRootMotion.html` | `OnAnimatorMove`, overriding root motion from code |
| Animation from external sources | `Manual/AnimationsImport.html` | Import pipeline for animation clips baked into FBX/other model files |
| Rig tab import settings | `Manual/FBXImporter-Rig.html` | Humanoid/Generic/Legacy animation type, Avatar Definition, configuring the Avatar |
| Creating models for animation | `Manual/UsingHumanoidChars.html` | Modeling/rigging requirements for a valid Humanoid Avatar |
| Avatar Mask window reference | `Manual/class-AvatarMask.html` | Body-part and transform-path masking, used by both layers and Animation Rigging |
| Constraint components introduction | `Manual/Constraints.html` | Built-in (non-package) constraint components conceptually |
| Constraint components reference | `Manual/constraint-components.html` | Position/Rotation/Aim/Parent/Scale/LookAt constraint component list |
| Animation Rigging package | `Manual/com.unity.animation.rigging.html` | Landing page only locally — points out to `docs.unity3d.com/Packages/com.unity.animation.rigging@1.4/manual/`; no local page-by-page rig/constraint docs |
| Timeline package | `Manual/com.unity.timeline.html` | Landing page only locally — points out to `docs.unity3d.com/Packages/com.unity.timeline@1.8/manual/`; no local track/clip/signal reference pages |
| PlayableDirector component | `Manual/class-PlayableDirector.html` | The component that plays a Timeline asset and binds tracks to scene objects |
| Cinemachine package | `Manual/com.unity.cinemachine.html` | Landing page only locally — points out to `docs.unity3d.com/Packages/com.unity.cinemachine@3.1/manual/`; no local vcam/blend/noise reference pages |
| `Animator` scripting API | `ScriptReference/Animator.html` | Runtime API: parameters, layers, IK, playback control, culling |
| `PlayableDirector` scripting API | `ScriptReference/Playables.PlayableDirector.html` | `Play()`, `Pause()`, `Stop()`, `time`, `playableAsset`, generic bindings |
| `AvatarMask` scripting API | `ScriptReference/AvatarMask.html` | Runtime/editor construction of masks for layers or rig constraints |
| `StateMachineBehaviour` scripting API | `ScriptReference/StateMachineBehaviour.html` | Base class for per-state scripted callbacks |
| `OnAnimatorMove` callback | `ScriptReference/MonoBehaviour.OnAnimatorMove.html` | Per-frame hook fired when root motion is applied |
| `OnAnimatorIK` callback | `ScriptReference/MonoBehaviour.OnAnimatorIK.html` | Per-frame hook fired during the Animator's IK pass |

**Honesty note on package docs**: locally, `com.unity.timeline.html`, `com.unity.cinemachine.html`, and `com.unity.animation.rigging.html` are thin package-landing stubs (~194–204 lines each, mostly site chrome) that link out to `docs.unity3d.com/Packages/...` for the actual manual. There is no local page for Timeline tracks/clips/signals, Cinemachine virtual camera/body/aim/noise components, or Animation Rigging's Rig Builder/constraint types beyond what's covered by the general (non-package) `Constraints.html`/`constraint-components.html` pages. Treat Timeline- and Cinemachine-specific and Animation-Rigging-specific claims below as pretrained knowledge, not doc-grounded, and say so if precision matters — verify against the live Unity package docs before asserting API specifics (method signatures, exact component field names) for those three packages.

## Key Guidelines

### Animator Controller & State Machines

The Animator Controller is a state machine asset: each state holds a Motion (a clip or a blend tree), transitions connect states with conditions evaluated against Animator parameters (bool/int/float/trigger), and "Any State" is a special node that can transition into any other state regardless of current state — reserve it for genuine interrupts (death, hit-stun, forced knockdown), not routine flow, or the graph becomes unreadable and transition priority order starts to matter unpredictably. Never assign a state or force a clip to play directly from gameplay code; drive parameters instead and let the graph's transition conditions decide what plays. Nested sub-state machines (`Manual/NestedStateMachines.html`) group related states (e.g. all "grounded locomotion" states) behind a single collapsible node for readability without behavior differences — transitions into/out of the sub-machine graph route through its entry/exit nodes.

```csharp
// Drive the state machine through parameters — never set clips directly.
public class LocomotionController : MonoBehaviour
{
    static readonly int Speed = Animator.StringToHash("Speed");
    static readonly int Jump = Animator.StringToHash("Jump");
    static readonly int IsGrounded = Animator.StringToHash("IsGrounded");

    Animator _animator;

    void Awake() => _animator = GetComponent<Animator>();

    void Update()
    {
        float speed01 = _inputSpeed / _maxSpeed; // normalize for the blend tree
        _animator.SetFloat(Speed, speed01, 0.1f, Time.deltaTime); // damped set, avoids snapping
        _animator.SetBool(IsGrounded, _characterController.isGrounded);

        if (Input.GetButtonDown("Jump") && _characterController.isGrounded)
            _animator.SetTrigger(Jump); // triggers auto-reset after consumption
    }
}
```

Attach `StateMachineBehaviour` subclasses directly to individual states for logic that must live and die with that state (enabling a hitbox only during an attack state, locking input during a knockdown state) — `OnStateEnter`/`OnStateExit`/`OnStateUpdate`/`OnStateMove`/`OnStateIK` fire only while that specific state is active, which is more robust than checking `AnimatorStateInfo` from an external script every frame.

```csharp
// Attach to the "Attack" state. Opens/closes a hit window without external polling.
public class AttackHitWindow : StateMachineBehaviour
{
    public override void OnStateEnter(Animator animator, AnimatorStateInfo info, int layerIndex)
        => animator.GetComponent<Combatant>().SetHitboxActive(true);

    public override void OnStateExit(Animator animator, AnimatorStateInfo info, int layerIndex)
        => animator.GetComponent<Combatant>().SetHitboxActive(false);
}
```

### Blend Trees (1D / 2D)

Use a 1D blend tree keyed on a single float (e.g. `Speed`) for locomotion along one axis (idle → walk → run) instead of authoring separate states with hand-tuned crossfade transitions between each speed tier — the blend tree interpolates continuously and its motion thresholds are set per-child clip in the tree editor. Use a 2D blend tree (Simple Directional, Freeform Directional, or Freeform Cartesian) keyed on two floats (e.g. `MoveX`/`MoveZ` or `Speed`/`Direction`) for strafing/directional locomotion where movement isn't confined to one axis; Simple Directional assumes clips are motion clips roughly evenly spaced around a circle, Freeform variants handle uneven or non-circular clip layouts at higher computational cost. Clamp or normalize the input parameters to the range the tree was authored for — feeding an unclamped value (e.g. raw `CharacterController.velocity.magnitude` past the run clip's threshold) causes the tree to keep sampling the extreme child clip, which reads as jitter or a stuck pose rather than a smooth cap.

```csharp
// Feed a 2D freeform-directional blend tree keyed on MoveX/MoveZ.
void Update()
{
    Vector2 move = Vector2.ClampMagnitude(_inputVector, 1f); // keep inside the tree's authored range
    _animator.SetFloat("MoveX", move.x);
    _animator.SetFloat("MoveZ", move.y);
}
```

### Animation Layers & Avatar Masks

Animator layers stack on top of the base layer, each with a weight (0–1) and a blend mode: Override replaces the underlying pose for masked bones, Additive adds delta motion on top of it (used for lean, breathing, recoil). Pair a layer with an `AvatarMask` (`Manual/class-AvatarMask.html`) to restrict it to specific humanoid body parts or explicit transform paths — e.g. an "Aim" layer masked to the upper body blends an aim-offset pose over locomotion running on the base layer, without the aim layer's arm motion overriding leg motion from the base layer. A layer with weight 0 still costs evaluation time unless disabled; set weight, don't just leave it at zero expecting it to skip work for free.

```csharp
// Fade an upper-body aim layer in/out based on whether the player is aiming.
static readonly int AimLayer = -1; // resolved once via GetLayerIndex
void Awake() => AimLayerIndex = _animator.GetLayerIndex("Aim");

void Update()
{
    float target = _isAiming ? 1f : 0f;
    float current = _animator.GetLayerWeight(AimLayerIndex);
    _animator.SetLayerWeight(AimLayerIndex, Mathf.MoveTowards(current, target, Time.deltaTime * 4f));
}
```

### Root Motion

Root motion is the translation/rotation baked into a clip's root bone, extracted per-frame as `Animator.deltaPosition`/`deltaRotation` and, when `Animator.applyRootMotion` is true, applied automatically to the GameObject's transform before physics runs. Pick exactly one authority for the object's position/rotation each frame: either root motion (`applyRootMotion = true`, no manual transform writes in `Update`) or code-driven movement (`applyRootMotion = false`, e.g. a `CharacterController.Move` call) — mixing both means two systems write the same transform in the same frame and the result drifts or stutters depending on execution order. To intercept and modify root motion before it's applied (e.g. to add lateral movement while keeping the clip's forward motion, or to redirect motion toward a target), implement `OnAnimatorMove` on a component on the same GameObject; Unity calls it instead of applying deltaPosition/deltaRotation automatically, and the callback is responsible for writing the transform (or `CharacterController.Move`) itself using `animator.deltaPosition`/`animator.deltaRotation`.

```csharp
// Intercept root motion instead of letting the Animator apply it automatically.
void OnAnimatorMove()
{
    Vector3 delta = _animator.deltaPosition;
    delta.y = 0f; // strip vertical root motion; gravity is handled separately
    _characterController.Move(delta);
    transform.rotation *= _animator.deltaRotation;
}
```

### Animation Import & Rig Configuration

Clips baked into an imported model (FBX, etc.) are configured on the model's Rig tab (`Manual/FBXImporter-Rig.html`): Animation Type is Humanoid (retargetable across skeletons sharing a compatible bone structure), Generic (non-humanoid skeletons, faster but not retargetable), or Legacy (old `Animation` component workflow, avoid for new work). Humanoid requires a valid Avatar definition — Unity attempts auto-mapping from the model's bone names to the Humanoid bone set, but complex or non-standard rigs (per `Manual/UsingHumanoidChars.html`) often need the mapping fixed manually in the Avatar configuration view before the Animator will play clips correctly. `AnimationClip` assets sliced from a single imported take (Splitting Animations) carry their own loop-time, root-transform-rotation/position, and curve settings independent of the source file's global settings — a clip that visibly "pops" at its loop point almost always has Loop Time on without Loop Pose/root-transform matching configured.

### Animation Rigging / IK

Animation Rigging (package, `Manual/com.unity.animation.rigging.html` — landing page only locally) layers procedural bone adjustment on top of baked Animator output using a `RigBuilder` component plus one or more `Rig` containers holding constraint components (Two Bone IK, Multi-Aim, Multi-Parent, Chain IK, Damped Transform, Override Transform). `RigBuilder` builds a Playable graph that evaluates *after* the Animator produces its base pose each frame, so constraints correct or override that pose rather than fighting it; each constraint has its own weight (0–1) for blending the procedural correction in and out at runtime, and `Rig` components themselves have a weight that scales every constraint inside them together. This is preferable to hand-writing bone `Transform` manipulation in `LateUpdate` because it composes with the existing Animator pose, supports weight-based blending, and is authored visually. The built-in (non-package) constraint components in `Manual/constraint-components.html` (Position/Rotation/Aim/Parent/Scale/LookAt Constraint) solve a related but distinct problem — constraining a plain `Transform` to follow one or more source transforms, independent of any Animator — useful for props, cameras, or non-animated rigs rather than for correcting a Humanoid's baked pose.

For per-frame IK driven directly from code rather than the Rigging package (foot placement against terrain, aiming a weapon bone at a target), implement `OnAnimatorIK` on a component on the Animator's GameObject and call `Animator.SetIKPosition`/`SetIKPositionWeight`/`SetIKRotation`/`SetIKRotationWeight` for the relevant `AvatarIKGoal` (LeftFoot, RightFoot, LeftHand, RightHand) — this only works on Humanoid rigs with IK Pass enabled on that Animator layer.

```csharp
// Runtime hand IK toward a target, using the Animator's built-in humanoid IK pass.
public Transform aimTarget;
[Range(0, 1)] public float ikWeight = 1f;

void OnAnimatorIK(int layerIndex)
{
    if (aimTarget == null) return;
    _animator.SetIKPositionWeight(AvatarIKGoal.RightHand, ikWeight);
    _animator.SetIKRotationWeight(AvatarIKGoal.RightHand, ikWeight);
    _animator.SetIKPosition(AvatarIKGoal.RightHand, aimTarget.position);
    _animator.SetIKRotation(AvatarIKGoal.RightHand, aimTarget.rotation);
}
```

### Timeline & Cutscenes

Timeline (package, `Manual/com.unity.timeline.html` — landing page only locally) authors a `TimelineAsset` containing tracks (Animation, Audio, Activation, Control, and others) bound at runtime to scene objects; a `PlayableDirector` component (`Manual/class-PlayableDirector.html`, `ScriptReference/Playables.PlayableDirector.html`) references the asset and holds the actual track-to-object bindings for a given scene instance, then drives playback. Use Timeline for authored, fixed-duration sequences — cutscenes, scripted multi-actor choreography, timed VFX/audio/camera-cut sequences — rather than coroutines chaining `yield return new WaitForSeconds(...)` calls, because Timeline gives you a visual, scrubbable, re-orderable representation and each track's evaluation is driven by a single graph time rather than independent per-effect timers that can drift out of sync with each other.

```csharp
// Play a cutscene Timeline asset and react when it finishes.
public PlayableDirector cutsceneDirector;

void PlayCutscene()
{
    cutsceneDirector.stopped += OnCutsceneStopped;
    cutsceneDirector.Play(); // uses the asset/bindings already set on the component
}

void OnCutsceneStopped(PlayableDirector director)
{
    director.stopped -= OnCutsceneStopped;
    ReenablePlayerControl();
}
```

`PlayableDirector.playOnAwake` controls whether the sequence starts automatically when the scene loads — leave it off for cutscenes triggered by gameplay events and call `Play()` explicitly. Generic (non-serialized) bindings can be set at runtime with `SetGenericBinding(trackAsset, target)` before calling `Play()`, useful when the bound object is instantiated or resolved dynamically rather than fixed in the scene.

### Cinemachine Virtual Cameras

Cinemachine (package, `Manual/com.unity.cinemachine.html` — landing page only locally) replaces hand-rolled camera-follow scripts with self-contained virtual camera components that each define Follow/LookAt targets and framing/composition rules; a `CinemachineBrain` component on the actual scene `Camera` continuously reads whichever virtual camera currently has the highest priority and blends the real camera toward it, smoothly cross-fading position/rotation/FOV when priority switches cause a different virtual camera to become active. Prefer raising/lowering a virtual camera's `Priority` (or enabling/disabling it) over writing custom camera-switch logic — the brain handles the blend curve and timing for you, including during gameplay-triggered camera changes (entering an aim mode, a cutscene camera taking over, a trigger-volume framing shot).

```csharp
// Switch active camera by priority; CinemachineBrain handles the blend.
public CinemachineCamera aimCamera; // highest priority while aiming
int _defaultPriority;

void OnAimStart()
{
    _defaultPriority = aimCamera.Priority;
    aimCamera.Priority = 20; // above the default follow camera's priority
}

void OnAimEnd() => aimCamera.Priority = _defaultPriority;
```

Because the package manual isn't available locally beyond the landing stub, verify exact component/property names (e.g. current-version class naming — Cinemachine 3.x renamed `CinemachineVirtualCamera` to `CinemachineCamera` and restructured the component-based body/aim/noise setup) against the live docs before writing code that depends on precise API surface.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Setting animation state every `Update()` | Bypasses the state machine; set parameters on change, drive transitions from them |
| Root motion fights manual movement code | Pick one authority for transform position/rotation per frame — either `applyRootMotion` or code, never both |
| Rigging constraints not applying | `RigBuilder` missing/disabled on the GameObject, or the constraint's/Rig's weight is 0 |
| Timeline not playing at runtime | Play On Awake off, or a track has no bound target in the scene's `PlayableDirector` bindings |
| Cinemachine cameras never blend | No `CinemachineBrain` on the real Camera, or two vcams share the same priority |
| Blend tree jitter at extremes | Input parameter exceeds the thresholds authored in the tree; clamp/normalize it before `SetFloat` |
| Layer has no visible effect | Layer weight is 0, or the layer's `AvatarMask` excludes the bones you expect it to affect |
| Additive layer causes exaggerated/broken poses | Source clip for the additive layer wasn't authored against the correct reference pose; re-author with the additive reference frame set correctly |
| Humanoid clip refuses to play or plays corrupted | Avatar auto-mapping failed on a non-standard rig; open the Avatar configuration view on the Rig tab and fix bone mapping manually |
| Clip pops at loop point | Loop Time enabled without matching root position/rotation curves (Loop Pose) configured on the clip |
| `SetTrigger` fires the transition twice or gets "stuck" | Trigger consumed on a frame the target transition's conditions weren't otherwise met yet; verify exit-time/condition combination, or call `ResetTrigger` defensively on state exit |
| `OnAnimatorIK` never fires | IK Pass checkbox not enabled on that Animator layer, or the rig isn't Humanoid |
| Animation Rigging pose "snaps" when a constraint's weight changes | Weight changed instantly instead of blended over time (e.g. `Mathf.MoveTowards`/tween the weight each frame) |
| PlayableDirector plays but nothing visibly changes | Generic bindings not set (`SetGenericBinding`) before `Play()` for tracks whose targets aren't wired in the Editor |
| Cinemachine follow camera lags or overshoots badly | Damping values tuned for a different target speed; re-tune Body damping against actual gameplay movement speed rather than editor-preview speed |
| Mixing Animator-driven root motion with Cinemachine follow damping | Camera lag amplifies visible root-motion pops; increase damping or smooth root motion input before both interact |

## Quick Reference

| Component/Class | Purpose |
|---|---|
| `Animator` | Runtime component driving an Animator Controller; exposes parameters, layers, IK, playback control |
| `AnimatorController` | Editor asset defining states, transitions, parameters, layers |
| `AnimatorControllerParameter` | Bool/Int/Float/Trigger definition read by transition conditions |
| `AnimatorStateInfo` / `AnimatorTransitionInfo` | Runtime info structs for the currently playing state / active transition |
| Blend Tree (1D/2D) | Motion node blending multiple clips by one or two float parameters |
| `StateMachineBehaviour` | Script base class attached to a state/sub-state-machine for scoped callbacks |
| `AnimationClip` | Keyframe/curve data asset; carries loop, root-motion, and event settings |
| `AnimatorOverrideController` | Swaps clips on an existing controller's states without duplicating the graph |
| `RuntimeAnimatorController` | Base class shared by `AnimatorController` and `AnimatorOverrideController` |
| `AvatarMask` | Body-part/transform-path mask used by Animator layers and by rig constraints |
| `Avatar` | Humanoid bone-mapping asset produced by the Rig import tab |
| Built-in Constraints (Position/Rotation/Aim/Parent/Scale/LookAt) | Non-package transform constraints, independent of the Animator |
| `Rig` / `RigBuilder` | Animation Rigging package container evaluated after the Animator's base pose |
| Rigging constraints (Two Bone IK, Multi-Aim, Multi-Parent, Chain IK, Damped Transform, Override Transform) | Procedural bone adjustment within a `Rig` |
| `PlayableDirector` | Plays a Timeline asset, holds/binds tracks to scene objects, exposes `Play`/`Pause`/`Stop`/`time` |
| `PlayableGraph` / `Playable` / `PlayableBehaviour` | Lower-level Playables API underlying both the Animator and Timeline |
| `TimelineAsset` / Tracks (Animation, Audio, Activation, Control) | Authored sequence data played back by a `PlayableDirector` |
| Cinemachine Virtual Camera (`CinemachineCamera` in Cinemachine 3.x) | Self-contained camera behavior: follow, look-at, composition |
| `CinemachineBrain` | Blends the real Camera between whichever virtual camera has top priority |
| `MonoBehaviour.OnAnimatorMove` | Callback for intercepting/redirecting root motion before it's applied |
| `MonoBehaviour.OnAnimatorIK` | Callback fired during the Animator's IK pass for scripted bone-goal IK |

## Advanced Notes

- **Rigging constraint stacking order matters.** Constraints inside a `Rig` (and across multiple `Rig` components under one `RigBuilder`) evaluate top-to-bottom in the order they're listed/laid out, each one operating on the pose the previous constraint left behind. A foot-IK constraint that must react to a look-at/aim constraint's output (or vice versa) needs to be ordered accordingly, and if two constraints drive the same bone, whichever evaluates later wins unless their weights are deliberately partial. Split unrelated corrections (e.g. left-arm aim vs foot placement) into separate `Rig` components when you need independent enable/disable or weight control over each without affecting the other.
- **Multiple `Rig` components let you group and gate corrections independently.** A `RigBuilder` can hold several `Rig` layers, each with its own weight — useful for turning off an entire category of procedural correction (e.g. all combat-related aim/look IK) with one weight change instead of toggling every constraint individually.
- **StateMachineBehaviour on sub-state machines.** `OnStateMachineEnter`/`OnStateMachineExit` fire on behaviours attached to a sub-state machine node itself (not an individual state) when execution enters/exits that nested graph — useful for setup/teardown shared by every state inside the sub-machine (e.g. locking a shared resource for the duration of a "Cutscene" sub-state-machine).
- **`Animator.Update(deltaTime)` for manual stepping.** Passing a controlled delta time (instead of relying on automatic per-frame update) lets you scrub or step the Animator deterministically — relevant for tools, replay systems, or netcode reconciliation where the Animator's state needs to be advanced to an exact simulated time rather than wall-clock frame time. Combine with `AnimatorUpdateMode.Manual` (`ScriptReference/AnimatorUpdateMode.html` locally lists Normal/Fixed/UnscaledTime, but Manual stepping is done by calling `Update()` yourself while update mode logic is handled at the call-site).
- **`AnimatorOverrideController` for clip variants without graph duplication.** Instead of duplicating an entire Animator Controller to swap a character's weapon-attack clips (sword vs axe), layer an `AnimatorOverrideController` on top of the base `RuntimeAnimatorController` and override just the clips that differ; assign the override controller to `Animator.runtimeAnimatorController` at runtime to swap the whole clip set in one call.
- **Timeline execution order relative to root motion and Rigging.** A Timeline Animation Track can drive the same Animator that also has Animation Rigging constraints and/or scripted root motion; because the Playables graph evaluates the Animator's output first and Rigging constraints after, a Timeline-driven animation still gets corrected by any active Rig — but if the Timeline track also carries root motion, make sure only one system (Timeline vs gameplay code) is authoritative over the transform for the sequence's duration, same rule as ordinary root motion.
- **Timeline Signal Emitters/Receivers and custom Marker/Track types are real Timeline package features** (emitting a `SignalAsset` from a point on a track to trigger a `SignalReceiver`'s UnityEvent, or authoring custom `PlayableAsset`/`PlayableBehaviour` tracks) but there is no local Manual page covering them — the docs root has no `Signal*.html` file under `Manual/`. Confirm exact API (`SignalEmitter`, `SignalReceiver`, `SignalAsset`) against the live package docs before writing code that depends on it.
- **PlayableGraph is the shared substrate.** Both `Animator` (internally) and `PlayableDirector`/Timeline build and evaluate a `PlayableGraph` of `Playable` nodes; `Animator.playableGraph` exposes the Animator's own graph for advanced use (e.g. injecting a custom `Playable` to blend in procedural animation below the Rigging layer). This is an advanced/rarely-needed integration point — most gameplay code should stay at the `Animator`/`PlayableDirector` level.

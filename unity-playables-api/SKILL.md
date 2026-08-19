---
name: unity-playables-api
description: Use when working with Unity's low-level Playables API directly — PlayableGraph, PlayableOutput, custom PlayableBehaviours, or blending/mixing playables outside the Timeline/Animator workflow layer. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Playables API

## Retrieval Sources

Docs root: `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/`

All paths below were verified to exist on disk (`ls`/`find` against `Manual/` and `ScriptReference/`; `ScriptReference/` alone holds 296 files matching `*Playable*`, `Manual/` holds a full 13-page Playables API chapter under `Manual/Playables.html`).

| Source | Path | Use for |
|---|---|---|
| Playables API landing page | `Manual/Playables.html` | What the API is for (dynamic blending, scripted playback, runtime-built graphs) and the chapter's table of contents |
| PlayableGraph, nodes, and output | `Manual/Playables-Graph.html` | Conceptual model: the graph, `Playable` nodes, `PlayableOutput` — the three core pieces |
| ScriptPlayable and PlayableBehaviour | `Manual/Playables-ScriptPlayable.html` | How to wrap a `PlayableBehaviour` in a `ScriptPlayable<T>` to build a custom playable |
| Playable Director component | `Manual/class-PlayableDirector.html` | How `PlayableDirector`/Timeline sits on top of a hand-built `PlayableGraph` |
| Playables API examples index | `Manual/Playables-Examples.html` | Table of contents for the seven worked examples below |
| PlayableGraph Visualizer note | `Manual/playables-visualizer.html` | Locally an empty/stub page — the Visualizer is a discontinued experimental package; don't assume it's installed |
| Example: play an animation clip | `Manual/playables-ex-play-clip.html` | Minimal graph: one `AnimationClipPlayable` into one `AnimationPlayableOutput` |
| Example: blend two animation clips | `Manual/playables-ex-blend-clips.html` | `AnimationMixerPlayable` with two clip inputs and `SetInputWeight` |
| Example: blend a clip with a controller | `Manual/playables-ex-blend-clip-controller.html` | Mixing an `AnimationClipPlayable` against an `AnimatorControllerPlayable` |
| Example: control the play state | `Manual/playables-ex-play-state.html` | `Playable.Pause()`/`Play()` semantics and how play-state propagates to children |
| Example: control playable timing | `Manual/playables-ex-control-timing.html` | `SetTime()` on a paused playable for manual scrubbing |
| Example: different output types | `Manual/playables-ex-different-outputs.html` | One graph driving both an `AnimationPlayableOutput` and an `AudioPlayableOutput` |
| Example: custom playable | `Manual/playables-ex-custom-playable.html` | Full `PlayableBehaviour` subclass (`PlayQueuePlayable`) overriding `PrepareFrame` to drive an `AnimationMixerPlayable` |
| `PlayableGraph` struct | `ScriptReference/Playables.PlayableGraph.html` | Full method list: `Create`, `Connect`, `Disconnect`, `Play`, `Stop`, `Evaluate`, `Destroy`, `DestroySubgraph`, `IsValid`, `IsPlaying`, `IsDone` |
| `PlayableGraph.Create` | `ScriptReference/Playables.PlayableGraph.Create.html` | Signature (`Create()` / `Create(string name)`); the name is only for editor tooling like the Visualizer |
| `PlayableGraph.Destroy` | `ScriptReference/Playables.PlayableGraph.Destroy.html` | Confirms destruction is deferred to later in the frame, not immediate |
| `PlayableGraph.Connect` | `ScriptReference/Playables.PlayableGraph.Connect.html` | Port semantics: `destinationInputPort = -1` auto-creates a new input port |
| `PlayableGraph.DestroySubgraph` | `ScriptReference/Playables.PlayableGraph.DestroySubgraph.html` | Recursive destroy of a playable and everything feeding into it |
| `PlayableGraph.Evaluate` | `ScriptReference/Playables.PlayableGraph.Evaluate.html` | Manual per-frame advance with an explicit `deltaTime` |
| `PlayableGraph.IsValid` | `ScriptReference/Playables.PlayableGraph.IsValid.html` | How to check a graph wasn't already destroyed before touching it |
| `PlayableBehaviour` base class | `ScriptReference/Playables.PlayableBehaviour.html` | Full worked example: a `BlenderPlayableBehaviour` driving mixer weights via `PingPong` |
| `PlayableBehaviour.OnGraphStart` | `ScriptReference/Playables.PlayableBehaviour.OnGraphStart.html` | Fires once when the owning graph starts playing or is first evaluated; paired with `OnGraphStop` |
| `PlayableBehaviour.OnGraphStop` | `ScriptReference/Playables.PlayableBehaviour.OnGraphStop.html` | Guaranteed to pair with a prior `OnGraphStart`; also fires before destroy for manually-evaluated graphs |
| `PlayableBehaviour.OnPlayableCreate` | `ScriptReference/Playables.PlayableBehaviour.OnPlayableCreate.html` | Fires when the owning `Playable` itself is created |
| `PlayableBehaviour.OnPlayableDestroy` | `ScriptReference/Playables.PlayableBehaviour.OnPlayableDestroy.html` | Fires when the owning `Playable` is destroyed (e.g. via `PlayableExtensions.Destroy`) |
| `PlayableBehaviour.OnBehaviourPlay`/`OnBehaviourPause` | `ScriptReference/Playables.PlayableBehaviour.OnBehaviourPlay.html`, `ScriptReference/Playables.PlayableBehaviour.OnBehaviourPause.html` | Per-playable play-state transition callbacks, distinct from graph-level start/stop |
| `PlayableBehaviour.OnBehaviourDelay` | `ScriptReference/Playables.PlayableBehaviour.OnBehaviourDelay.html` | Marked **Obsolete** in 6.3 — docs say to implement delay via a custom `ScriptPlayable` instead |
| `PlayableBehaviour.PrepareFrame` | `ScriptReference/Playables.PlayableBehaviour.PrepareFrame.html` | The phase for topology changes, weight changes, time changes — not actual output work |
| `PlayableBehaviour.ProcessFrame` | `ScriptReference/Playables.PlayableBehaviour.ProcessFrame.html` | The phase where a playable connected (directly or indirectly) to a `ScriptPlayableOutput` does its actual work |
| `PlayableBehaviour.PrepareData` | `ScriptReference/Playables.PlayableBehaviour.PrepareData.html` | Called only while a playable is in the (now-obsolete) delayed state |
| `FrameData` struct | `ScriptReference/Playables.FrameData.html` | Per-frame context passed into behaviour callbacks: `deltaTime`, `effectiveWeight`, `effectiveSpeed`, `effectivePlayState`, `seekOccurred`, `timeLooped`, `timeHeld` |
| `ScriptPlayable<T>` | `ScriptReference/Playables.ScriptPlayable_1.html` | The generic wrapper struct that gives a `PlayableBehaviour` a place in the graph; usable with `PlayableExtensions` like any other `IPlayable` |
| `ScriptPlayableOutput` | `ScriptReference/Playables.ScriptPlayableOutput.html` | A pure-script output type — no Animator/AudioSource target, useful for driving arbitrary game logic from `ProcessFrame` |
| `PlayableExtensions` static class | `ScriptReference/Playables.PlayableExtensions.html` | Every generic per-node operation: `SetInputWeight`, `ConnectInput`, `SetDuration`, `SetSpeed`, `SetTraversalMode`, `Pause`, `Play`, `SetTime`, `GetTime`, `Destroy`, `IsValid` |
| `PlayableExtensions.SetInputWeight` | `ScriptReference/Playables.PlayableExtensions.SetInputWeight.html` | Signature confirms weight is meant to stay in `[0, 1]` per input |
| `PlayableExtensions.ConnectInput` | `ScriptReference/Playables.PlayableExtensions.ConnectInput.html` | Convenience wrapper over `PlayableGraph.Connect` |
| `PlayableExtensions.SetTraversalMode` | `ScriptReference/Playables.PlayableExtensions.SetTraversalMode.html` | `Mix` vs `Passthrough` propagation for multi-output playables |
| `PlayableOutputExtensions` static class | `ScriptReference/Playables.PlayableOutputExtensions.html` | Output-side operations: `SetSourcePlayable`, `SetWeight`, `AddNotificationReceiver`, `PushNotification`, `SetUserData` |
| `INotificationReceiver` | `ScriptReference/Playables.INotificationReceiver.html` | Interface for receiving notifications pushed through a `PlayableOutput` |
| `PlayableAsset` | `ScriptReference/Playables.PlayableAsset.html` | Base class for authoring assets that inject a `Playable` subgraph via `CreatePlayable` — what Timeline's clip assets derive from |
| `PlayableTraversalMode` | `ScriptReference/Playables.PlayableTraversalMode.html` | `Mix` (normal) vs `Passthrough` (route only the connected output's matching input port) |
| `DirectorUpdateMode` | `ScriptReference/Playables.DirectorUpdateMode.html` | `GameTime`, `DSPClock`, `UnscaledGameTime`, `Manual` — governs what clock drives a graph forward |
| `PlayState` | `ScriptReference/Playables.PlayState.html` | `Playing` / `Paused` (the obsolete `Delayed` state still exists but is deprecated) |
| `AnimationPlayableUtilities` | `ScriptReference/Playables.AnimationPlayableUtilities.html` | High-level one-call helpers (`PlayClip`, `PlayMixer`, `PlayLayerMixer`, `PlayAnimatorController`) that build a minimal graph for you |
| `AnimationPlayableUtilities.PlayClip` | `ScriptReference/Playables.AnimationPlayableUtilities.PlayClip.html` | Exact signature: `PlayClip(Animator, AnimationClip, out PlayableGraph)` |
| `PlayableDirector.RebuildGraph` | `ScriptReference/Playables.PlayableDirector.RebuildGraph.html` | Discards and rebuilds the director's graph when the assigned `PlayableAsset` changed |
| `PlayableDirector.RebindPlayableGraphOutputs` | `ScriptReference/Playables.PlayableDirector.RebindPlayableGraphOutputs.html` | Re-resolves output bindings (new Animator/AudioSource/receiver) without a full rebuild |
| `AnimationMixerPlayable` | `ScriptReference/Animations.AnimationMixerPlayable.html` | Flat N-input animation mixer; works with `PlayableExtensions.SetInputWeight` |
| `AnimationLayerMixerPlayable` | `ScriptReference/Animations.AnimationLayerMixerPlayable.html` | Layered mixer with per-layer additive flag and avatar-mask support |
| `AnimationLayerMixerPlayable.Create` | `ScriptReference/Animations.AnimationLayerMixerPlayable.Create.html` | Signature includes `singleLayerOptimization` — a perf shortcut only valid for single-layer mixers |
| `AnimationClipPlayable` | `ScriptReference/Animations.AnimationClipPlayable.html` | Wraps a single `AnimationClip`; exposes `GetAnimationClip`, foot-IK/playable-IK flags |
| `AnimationPlayableOutput` | `ScriptReference/Animations.AnimationPlayableOutput.html` | Connects a graph to an `Animator` component in the scene |
| `AudioPlayableOutput` | `ScriptReference/Audio.AudioPlayableOutput.html` | Connects a graph to audio playback; `SetEvaluateOnSeek`/`GetEvaluateOnSeek` control seek behavior |
| `AnimationScriptPlayable` | `ScriptReference/Animations.AnimationScriptPlayable.html` | Runs a custom multi-threaded `IAnimationJob` with read/write access to the `AnimationStream` |
| `AnimatorControllerPlayable` | `ScriptReference/Animations.AnimatorControllerPlayable.html` | Wraps a `RuntimeAnimatorController` as a playable node — full Mecanim parameter/state API (`SetFloat`, `CrossFade`, `GetCurrentAnimatorStateInfo`, etc.) reachable from inside a graph |
| `UnityEditor.Playables.Utility` | `ScriptReference/Playables.Utility.html` | Editor-only introspection: `GetAllGraphs()`, `graphCreated`/`destroyingGraph` events |

## Key Guidelines

### PlayableGraph Fundamentals (creation/destruction/lifecycle)

A `PlayableGraph` is a lightweight C# struct handle to a native graph of time-synchronized nodes (`Manual/Playables-Graph.html`). Unlike the Animator's state-machine graph, which only understands animation, a `PlayableGraph` is data-agnostic — the same graph machinery drives animation, audio, or arbitrary scripted logic depending on what kind of `PlayableOutput` and `Playable` nodes you attach to it. You create one with the static `PlayableGraph.Create()` (optionally passing a name used only by editor tooling), and every playable/output you subsequently create must pass that same graph instance as its first `Create()` argument — the graph owns the native lifetime of everything built through it. Because `Playable`, `PlayableOutput`, and their subtypes are C# structs rather than classes (`Manual/Playables-Graph.html` calls this out explicitly), creating them allocates no managed garbage — but the *native* side they wrap is real unmanaged memory that the C# garbage collector knows nothing about. That's why `PlayableGraph.Destroy()` is mandatory: it schedules destruction of the graph and everything it owns (all playables, all outputs) for later in the same frame; skip it and Unity logs an error and the native graph leaks for the life of the domain. The idiomatic place to pair `Create()`/`Destroy()` is `Start()`/`OnDisable()` on the owning `MonoBehaviour`, mirroring every worked example in `Manual/playables-ex-*.html`.

```csharp
using UnityEngine;
using UnityEngine.Playables;
using UnityEngine.Animations;

[RequireComponent(typeof(Animator))]
public class MinimalGraphOwner : MonoBehaviour
{
    public AnimationClip clip;
    PlayableGraph graph;

    void Start()
    {
        // The graph owns every playable/output created from it below.
        graph = PlayableGraph.Create("MinimalGraphOwner");
        graph.SetTimeUpdateMode(DirectorUpdateMode.GameTime);

        var output = AnimationPlayableOutput.Create(graph, "Animation", GetComponent<Animator>());
        var clipPlayable = AnimationClipPlayable.Create(graph, clip);
        output.SetSourcePlayable(clipPlayable);

        graph.Play(); // starts advancing time; without this the graph sits frozen at t=0
    }

    void OnDisable()
    {
        // Mandatory: without this the native graph and every node/output it owns leaks.
        if (graph.IsValid())
            graph.Destroy();
    }
}
```

`graph.Play()`/`graph.Stop()` control whether the graph's clock advances at all; `graph.IsPlaying()` and `graph.IsDone()` let you query that state, and `graph.IsValid()` (`ScriptReference/Playables.PlayableGraph.IsValid.html`) tells you whether the graph has been created and not yet destroyed — check it before touching a graph reference held across frames, since using a destroyed graph throws. For advanced tooling, `UnityEditor.Playables.Utility.GetAllGraphs()` (`ScriptReference/Playables.Utility.html`) enumerates every currently-alive `PlayableGraph` in the editor process, and its `graphCreated`/`destroyingGraph` events let editor code track graph lifetime globally — useful for building custom debugging/visualization tools now that the official PlayableGraph Visualizer package is discontinued.

### Playable Outputs (Animation / Audio)

A `PlayableOutput` is the other endpoint of a graph — it defines *where* the result of evaluating a branch of playables actually goes (`Manual/Playables-Graph.html`). Like `Playable`, output types are structs, and non-abstract output types expose a static `Create(graph, ...)` method that the owning graph parents. An output only does something once you link it to a playable via `SetSourcePlayable()` (a `PlayableOutputExtensions` method, `ScriptReference/Playables.PlayableOutputExtensions.html`) — an output with no source playable evaluates to nothing every frame, which is one of the most common "the graph runs but nothing happens" bugs. The two output types you'll reach for constantly are `AnimationPlayableOutput` (targets an `Animator` component, `ScriptReference/Animations.AnimationPlayableOutput.html`) and `AudioPlayableOutput` (targets audio playback, `ScriptReference/Audio.AudioPlayableOutput.html`); a single graph can hold multiple outputs of different types simultaneously, each fed by its own (or a shared) playable subtree — `Manual/playables-ex-different-outputs.html` builds exactly this, one graph driving both an animated GameObject and an `AudioClip` at once.

```csharp
using UnityEngine;
using UnityEngine.Animations;
using UnityEngine.Audio;
using UnityEngine.Playables;

[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(AudioSource))]
public class DifferentOutputs : MonoBehaviour
{
    public AnimationClip animationClip;
    public AudioClip audioClip;
    PlayableGraph graph;

    void Start()
    {
        graph = PlayableGraph.Create("DifferentOutputs");

        var animationOutput = AnimationPlayableOutput.Create(graph, "Animation", GetComponent<Animator>());
        var audioOutput = AudioPlayableOutput.Create(graph, "Audio", GetComponent<AudioSource>());

        var animationClipPlayable = AnimationClipPlayable.Create(graph, animationClip);
        var audioClipPlayable = AudioClipPlayable.Create(graph, audioClip, true);

        animationOutput.SetSourcePlayable(animationClipPlayable);
        audioOutput.SetSourcePlayable(audioClipPlayable);

        graph.Play();
    }

    void OnDisable() => graph.Destroy();
}
```
Source: `Manual/playables-ex-different-outputs.html`.

For output types with no built-in scene target — when a playable needs to drive arbitrary gameplay code rather than an Animator or AudioSource — use `ScriptPlayableOutput` (`ScriptReference/Playables.ScriptPlayableOutput.html`). It's a "pure" output: connect a `ScriptPlayable<T>` as its source and every `ProcessFrame` call on that behaviour becomes your hook for doing whatever work the output is meant to represent. Outputs can also carry a weight (`PlayableOutputExtensions.SetWeight`/`GetWeight`) and a reference object (`SetReferenceObject`, used by Timeline to associate an output with the authoring track asset), and can register `INotificationReceiver`s (`AddNotificationReceiver`) to receive one-shot events pushed through `PushNotification` — this is the mechanism Timeline's Signal tracks build on.

### Writing a Custom PlayableBehaviour

`PlayableBehaviour` (`ScriptReference/Playables.PlayableBehaviour.html`, `Manual/Playables-ScriptPlayable.html`) is the base class every custom playable inherits from. On its own a `PlayableBehaviour` isn't a node in the graph — you wrap it in a `ScriptPlayable<T>` (`ScriptReference/Playables.ScriptPlayable_1.html`), which is the actual `IPlayable` struct that gets connected like any other node. Two creation patterns exist: `ScriptPlayable<T>.Create(graph)` builds a fresh default-constructed behaviour, while `ScriptPlayable<T>.Create(graph, existingInstance)` **clones** `existingInstance` and assigns the clone into the graph — useful when you want to pre-configure per-instance fields (e.g. from the Inspector) before the behaviour enters the graph. Retrieve the live behaviour back out of the wrapper with `scriptPlayable.GetBehaviour()`.

`PlayableBehaviour` exposes a lifecycle of overridable callbacks, and getting their order and purpose right matters:

- `OnPlayableCreate(Playable)` / `OnPlayableDestroy(Playable)` — fire once each, tied to the owning `Playable` node's own creation/destruction (`ScriptReference/Playables.PlayableBehaviour.OnPlayableCreate.html`, `...OnPlayableDestroy.html`), independent of whether the graph is playing.
- `OnGraphStart(Playable)` / `OnGraphStop(Playable)` — fire once each per graph play session: `OnGraphStart` when the *owning graph* starts playing or is first evaluated, `OnGraphStop` when it stops; the docs guarantee every `OnGraphStart` is paired with exactly one `OnGraphStop`, including a stop call before destruction of a manually-evaluated graph (`ScriptReference/Playables.PlayableBehaviour.OnGraphStart.html`, `...OnGraphStop.html`). Use these for graph-scoped setup/teardown (allocating a resource once for the whole playback session), not per-frame work.
- `OnBehaviourPlay(Playable, FrameData)` / `OnBehaviourPause(Playable, FrameData)` — fire on transitions of *this playable's* effective play state, distinct from the graph-level start/stop above (`ScriptReference/Playables.PlayableBehaviour.OnBehaviourPlay.html`, `...OnBehaviourPause.html`); `OnBehaviourPause` also fires if the whole graph stops while this playable was still marked playing.
- `PrepareFrame(Playable, FrameData)` — called every evaluated frame; this is where you should make **topological changes** (reconnecting inputs), change **input weights**, or otherwise change state that affects how the graph is structured or blended for this frame (`ScriptReference/Playables.PlayableBehaviour.PrepareFrame.html`). This is the phase the built-in queue example (below) uses to decide which clip is "current" and adjust mixer weights accordingly.
- `ProcessFrame(Playable, FrameData, object playerData)` — called every frame the playable is playing and connected (directly or indirectly) to a `ScriptPlayableOutput`; this is where the actual output-producing work happens, and `playerData` is the output's user data set via `SetUserData` (`ScriptReference/Playables.PlayableBehaviour.ProcessFrame.html`).
- `OnBehaviourDelay` / `PrepareData` — tied to the now-**obsolete** `PlayState.Delayed` state; Unity's own docs say to implement delay behavior via a custom `ScriptPlayable` instead of relying on this path (`ScriptReference/Playables.PlayableBehaviour.OnBehaviourDelay.html`).

The canonical worked example — a custom playable that cycles through a list of animation clips, one at a time, adjusting a mixer's weights each frame — comes straight from `Manual/playables-ex-custom-playable.html`:

```csharp
using UnityEngine;
using UnityEngine.Animations;
using UnityEngine.Playables;

public class PlayQueuePlayable : PlayableBehaviour
{
    int m_CurrentClipIndex = -1;
    float m_TimeToNextClip;
    Playable mixer;

    public void Initialize(AnimationClip[] clipsToPlay, Playable owner, PlayableGraph graph)
    {
        owner.SetInputCount(1);
        mixer = AnimationMixerPlayable.Create(graph, clipsToPlay.Length);
        graph.Connect(mixer, 0, owner, 0);
        owner.SetInputWeight(0, 1);

        for (int i = 0; i < mixer.GetInputCount(); ++i)
        {
            graph.Connect(AnimationClipPlayable.Create(graph, clipsToPlay[i]), 0, mixer, i);
            mixer.SetInputWeight(i, 1.0f);
        }
    }

    public override void PrepareFrame(Playable owner, FrameData info)
    {
        if (mixer.GetInputCount() == 0) return;

        m_TimeToNextClip -= (float)info.deltaTime; // use FrameData.deltaTime, not Time.deltaTime
        if (m_TimeToNextClip <= 0.0f)
        {
            m_CurrentClipIndex++;
            if (m_CurrentClipIndex >= mixer.GetInputCount())
                m_CurrentClipIndex = 0;

            var currentClip = (AnimationClipPlayable)mixer.GetInput(m_CurrentClipIndex);
            currentClip.SetTime(0); // restart the newly-active clip at its beginning
            m_TimeToNextClip = currentClip.GetAnimationClip().length;
        }

        for (int i = 0; i < mixer.GetInputCount(); ++i)
            mixer.SetInputWeight(i, i == m_CurrentClipIndex ? 1.0f : 0.0f);
    }
}

[RequireComponent(typeof(Animator))]
public class CustomPlayableExample : MonoBehaviour
{
    public AnimationClip[] clipsToPlay;
    PlayableGraph graph;

    void Start()
    {
        graph = PlayableGraph.Create("CustomPlayableExample");
        var queuePlayable = ScriptPlayable<PlayQueuePlayable>.Create(graph);
        queuePlayable.GetBehaviour().Initialize(clipsToPlay, queuePlayable, graph);

        var output = AnimationPlayableOutput.Create(graph, "Animation", GetComponent<Animator>());
        output.SetSourcePlayable(queuePlayable, 0);

        graph.Play();
    }

    void OnDisable() => graph.Destroy();
}
```

Note the pattern: `PlayQueuePlayable` doesn't play clips directly — it owns and manages a nested `AnimationMixerPlayable` internally (`owner.SetInputCount(1)` + `graph.Connect(mixer, 0, owner, 0)`), so from the outside it behaves like a single node while internally cycling which mixer input has full weight. This "playable that manages a sub-graph" pattern is exactly how more complex custom systems (state-machine-like queues, procedural blend trees) are typically built on top of the raw API.

### Mixing & Blending Playables

`AnimationMixerPlayable` (`ScriptReference/Animations.AnimationMixerPlayable.html`) is a flat N-input blend node: create it with an input count, connect N source playables to its inputs, and drive each input's contribution with `PlayableExtensions.SetInputWeight(index, weight)`. Unity does **not** auto-normalize weights for you — `Manual/playables-ex-blend-clips.html`'s pattern of `SetInputWeight(0, 1f - weight)` / `SetInputWeight(1, weight)` is a convention the calling code enforces, not a graph guarantee; feed it inputs that don't sum to 1 and you'll get a genuinely over- or under-weighted blend rather than an error. The same mixer works identically whether its inputs are `AnimationClipPlayable`s or an `AnimatorControllerPlayable` (`Manual/playables-ex-blend-clip-controller.html` blends a raw clip against a full Animator Controller by wrapping the `RuntimeAnimatorController` in `AnimatorControllerPlayable.Create(graph, controller)`) — the mixer only cares that its inputs are playables producing animation data, not what produced that data.

```csharp
using UnityEngine;
using UnityEngine.Playables;
using UnityEngine.Animations;

[RequireComponent(typeof(Animator))]
public class BlendAnimationClips : MonoBehaviour
{
    public AnimationClip clip0;
    public AnimationClip clip1;
    [Range(0, 1)] public float weight;
    PlayableGraph graph;
    AnimationMixerPlayable mixer;

    void Start()
    {
        graph = PlayableGraph.Create("BlendAnimationClips");
        mixer = AnimationMixerPlayable.Create(graph, 2);

        var output = AnimationPlayableOutput.Create(graph, "Animation", GetComponent<Animator>());
        output.SetSourcePlayable(mixer);

        graph.Connect(AnimationClipPlayable.Create(graph, clip0), 0, mixer, 0);
        graph.Connect(AnimationClipPlayable.Create(graph, clip1), 0, mixer, 1);

        graph.Play();
    }

    void Update()
    {
        weight = Mathf.Clamp01(weight);
        mixer.SetInputWeight(0, 1.0f - weight); // caller is responsible for the weights summing to 1
        mixer.SetInputWeight(1, weight);
    }

    void OnDisable() => graph.Destroy();
}
```
Source: `Manual/playables-ex-blend-clips.html`.

For layered rather than flat blending, `AnimationLayerMixerPlayable` (`ScriptReference/Animations.AnimationLayerMixerPlayable.html`) is the scripted equivalent of the Animator's built-in layer stack: each input is a layer, `SetLayerAdditive(layerIndex, true)` makes a layer add on top of layers below it instead of overriding them, and `SetLayerMaskFromAvatarMask(layerIndex, avatarMask)` restricts a layer to specific bones exactly like an Animator layer's avatar mask. `AnimationLayerMixerPlayable.Create(graph, inputCount, singleLayerOptimization)` (`ScriptReference/Animations.AnimationLayerMixerPlayable.Create.html`) takes a `singleLayerOptimization` flag that pins the first layer's weight to 1 and skips weight math — only safe when you genuinely have one layer; Unity forces it false automatically once you have more than one. You can control the play state of any playable, or a whole branch, via `Pause()`/`Play()` (`Manual/playables-ex-play-state.html`): pausing a node stops its own local time from advancing, but pause/play propagates downward to children regardless of *their* individual state, so pausing a mixer pauses everything feeding it even if an individual clip playable underneath was independently set to play.

### ScriptPlayable for Custom Logic

`ScriptPlayable<T>` is the generic struct wrapper that turns any `PlayableBehaviour`-derived type into a first-class graph node (`Manual/Playables-ScriptPlayable.html`, `ScriptReference/Playables.ScriptPlayable_1.html`). Because `ScriptPlayable<T>` is itself an `IPlayable`, every method on `PlayableExtensions` (weights, time, speed, duration, connections) works on it identically to a built-in playable type — nothing about it is second-class. This is the mechanism for injecting arbitrary C# logic *into* the graph's evaluation, as opposed to writing a `MonoBehaviour.Update()` that pokes at the graph from outside: logic inside `PrepareFrame`/`ProcessFrame` runs in lockstep with the graph's own traversal and sees consistent `FrameData` (deltaTime scaled correctly for the graph's time-update mode and any parent speed multipliers), which polling from `Update()` does not guarantee.

```csharp
using UnityEngine;
using UnityEngine.Animations;
using UnityEngine.Playables;

// A ScriptPlayable driving a mixer's weights every frame from inside the graph.
public class BlenderPlayableBehaviour : PlayableBehaviour
{
    public AnimationMixerPlayable mixerPlayable;

    public override void PrepareFrame(Playable playable, FrameData info)
    {
        float blend = Mathf.PingPong((float)playable.GetTime(), 1.0f);
        mixerPlayable.SetInputWeight(0, blend);
        mixerPlayable.SetInputWeight(1, 1.0f - blend);
    }
}

public class PlayableBehaviourSample : MonoBehaviour
{
    PlayableGraph m_Graph;
    public AnimationClip clipA;
    public AnimationClip clipB;

    void Start()
    {
        m_Graph = PlayableGraph.Create();
        var animOutput = AnimationPlayableOutput.Create(m_Graph, "AnimationOutput", GetComponent<Animator>());

        var mixerPlayable = AnimationMixerPlayable.Create(m_Graph, 2);
        var clipPlayableA = AnimationClipPlayable.Create(m_Graph, clipA);
        var clipPlayableB = AnimationClipPlayable.Create(m_Graph, clipB);

        var blenderPlayable = ScriptPlayable<BlenderPlayableBehaviour>.Create(m_Graph, 1);
        blenderPlayable.GetBehaviour().mixerPlayable = mixerPlayable;

        m_Graph.Connect(clipPlayableA, 0, mixerPlayable, 0);
        m_Graph.Connect(clipPlayableB, 0, mixerPlayable, 1);
        m_Graph.Connect(mixerPlayable, 0, blenderPlayable, 0);

        // blenderPlayable is a ScriptPlayable; SetSourcePlayable with an explicit
        // port makes the output pass through to the mixer beneath it.
        animOutput.SetSourcePlayable(blenderPlayable, 0);
        m_Graph.Play();
    }

    void OnDisable() => m_Graph.Destroy();
}
```
Source: `ScriptReference/Playables.PlayableBehaviour.html`.

For graphs that need to drive gameplay logic with no animation/audio target at all, pair a `ScriptPlayable<T>` with a `ScriptPlayableOutput` (`ScriptReference/Playables.ScriptPlayableOutput.html`) instead of an `AnimationPlayableOutput`/`AudioPlayableOutput` — `ProcessFrame` becomes the sole place work happens, useful for building custom sequencing/state-machine systems (dialogue queues, scripted event timelines) using the same graph-evaluation guarantees as animation playback.

### Integrating with Timeline/Animator

`PlayableAsset` (`ScriptReference/Playables.PlayableAsset.html`) is the authoring-time counterpart to a runtime `Playable`: it's a `ScriptableObject` whose `CreatePlayable(PlayableGraph, GameObject)` method injects a subgraph into a caller-owned graph, and its `outputs` property declares what kind of output(s) that subgraph expects to be connected to. This is exactly the base class Timeline's clip/track assets derive from — when Timeline builds its graph, each track effectively calls `CreatePlayable` on its asset(s) to populate the graph, then wires up bindings and outputs. You can write your own `PlayableAsset` subclasses to make custom logic authorable and droppable onto a Timeline track, bridging the low-level API covered here with the editor-facing Timeline workflow (out of scope for this skill — see `unity-animation-cinematics` for Timeline/Animator/Cinemachine at the workflow level).

`PlayableDirector` (`Manual/class-PlayableDirector.html`) is the component that builds and manages a `PlayableGraph` from a `PlayableAsset`, exposing `Play()`/`Pause()`/`Stop()`/`time`/`playableAsset` plus generic binding storage — it's "primarily used by Timeline" per the manual, but nothing stops you from driving a hand-authored `PlayableAsset` through a `PlayableDirector` outside of Timeline. Two methods matter when a director's underlying data changes at runtime: `RebuildGraph()` (`ScriptReference/Playables.PlayableDirector.RebuildGraph.html`) discards and reconstructs the whole graph — call it after swapping `playableAsset` — while `RebindPlayableGraphOutputs()` (`ScriptReference/Playables.PlayableDirector.RebindPlayableGraphOutputs.html`) is the cheaper option when only *bindings* changed (a new Animator/AudioSource/notification receiver appeared) and the graph topology itself is still valid.

`AnimationPlayableUtilities` (`ScriptReference/Playables.AnimationPlayableUtilities.html`) exists precisely because building a `PlayableGraph` by hand for the common cases above is repetitive: `PlayClip`, `PlayMixer`, `PlayLayerMixer`, and `PlayAnimatorController` each build a minimal graph and hand you back the graph plus the top-level playable in one call — e.g. `AnimationPlayableUtilities.PlayClip(animator, clip, out graph)` (`ScriptReference/Playables.AnimationPlayableUtilities.PlayClip.html`) is a one-line equivalent of the "play an animation clip" example above. Reach for these when you just need one of these standard shapes and don't need the full manual control of building each node yourself.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Never calling `graph.Destroy()` | Native memory leak — pair `Create()` in `Start()`/`OnEnable()` with `Destroy()` in `OnDisable()`/`OnDestroy()`, always |
| Building a graph but never calling `graph.Play()` | The graph sits frozen at its initial state; `Play()` is what starts the clock advancing |
| `DirectorUpdateMode.Manual` but never calling `PlayableGraph.Evaluate(deltaTime)` | Nothing advances — Manual mode means *you* are the clock |
| Mixer input weights that don't sum to 1 | Unity does not normalize for you; over/under-weighted blends are silent, not errors — normalize explicitly |
| Casting a `Playable` to a specialized type it doesn't actually hold | Throws `InvalidCastException`; only cast when you created/verified that input as that specific type |
| Forgetting `output.SetSourcePlayable(...)` | An output with no linked source playable does nothing every frame — this is the #1 "graph runs, nothing happens" bug |
| Connecting playables that belong to two different `PlayableGraph`s | Invalid — a playable can only be used within the graph that created it (`GetGraph()` to check) |
| Doing output-producing work in `PrepareFrame` instead of `ProcessFrame` | `PrepareFrame` is for topology/weight/time changes; `ProcessFrame` is the actual work phase, and only runs for playables connected to a `ScriptPlayableOutput` |
| Using `Time.deltaTime` inside a `PlayableBehaviour` instead of `FrameData.deltaTime` | Breaks under `DSPClock`/`Manual` update modes and under any parent speed multiplier — `FrameData.deltaTime` is already the graph-correct value |
| Reusing a `Playable`/`PlayableGraph` reference after it's been destroyed | Throws or silently no-ops depending on the call; check `IsValid()`/`IsNull()` before touching a long-lived reference |
| Relying on `OnBehaviourDelay`/`PrepareData` for delay logic | Obsolete in 6.3 — implement delay behavior via a custom `ScriptPlayable` instead |
| Two systems (e.g. hand-built graph + Timeline) both trying to drive the same `Animator` | Only one graph can own an `Animator`'s playable binding at a time; they will fight, not compose, unless deliberately layered |
| Assuming `AnimationLayerMixerPlayable`'s `singleLayerOptimization` is free with multiple layers | Unity forces it to `false` once `inputCount > 1`; don't rely on it as a general perf switch |
| Allocating resources in `OnGraphStart` without releasing them in `OnGraphStop` | The pairing is guaranteed by the docs — treat it as your graph-scoped setup/teardown symmetric pair, same discipline as `OnEnable`/`OnDisable` |
| Expecting `PlayableGraph.Destroy()` to free native memory immediately | Destruction is deferred to later in the same frame, not synchronous — don't assume the memory is gone the instruction after the call |

## Quick Reference

| Class / Method | Purpose |
|---|---|
| `PlayableGraph.Create([name])` | Creates an empty graph; owns every playable/output created from it |
| `PlayableGraph.Destroy()` | Schedules destruction of the graph and everything it owns, later this frame |
| `PlayableGraph.Play()` / `.Stop()` | Starts/stops the graph's clock from advancing |
| `PlayableGraph.Evaluate([deltaTime])` | Manually advances and evaluates the graph one step |
| `PlayableGraph.Connect(src, srcPort, dst, dstPort)` | Wires one playable's output into another's input |
| `PlayableGraph.Disconnect` / `DestroyPlayable` / `DestroySubgraph` | Remove a single connection / a single node / a node and everything feeding it |
| `PlayableGraph.IsValid()` / `IsPlaying()` / `IsDone()` | Lifecycle and playback-state queries |
| `PlayableGraph.SetTimeUpdateMode(DirectorUpdateMode)` | Chooses the clock source (GameTime/DSPClock/UnscaledGameTime/Manual) |
| `Playable` / `IPlayable` | Base struct/interface for every node type; implicit upcast, explicit (checked) downcast |
| `PlayableExtensions` (static) | Generic per-node ops: `SetInputWeight`, `ConnectInput`, `AddInput`, `SetDuration`, `SetSpeed`, `SetTime`/`GetTime`, `Pause`/`Play`, `Destroy`, `IsValid` |
| `PlayableOutput` / `IPlayableOutput` | Base struct/interface for every output type |
| `PlayableOutputExtensions` (static) | Output ops: `SetSourcePlayable`, `SetWeight`, `SetReferenceObject`, `AddNotificationReceiver`, `PushNotification`, `SetUserData` |
| `PlayableBehaviour` | Base class for custom playable logic; override lifecycle callbacks |
| `ScriptPlayable<T>` | Generic `IPlayable` wrapper hosting a `PlayableBehaviour` in the graph |
| `ScriptPlayableOutput` | Output type with no scene target; source for pure-script `ProcessFrame` logic |
| `FrameData` | Per-callback context: `deltaTime`, `effectiveWeight/Speed/PlayState`, `seekOccurred`, `timeLooped`, `timeHeld` |
| `AnimationClipPlayable` | Wraps a single `AnimationClip` as a playable node |
| `AnimationMixerPlayable` | Flat N-input animation blend node; weights via `SetInputWeight` |
| `AnimationLayerMixerPlayable` | Layered blend node; per-layer additive flag + avatar-mask support |
| `AnimatorControllerPlayable` | Wraps a `RuntimeAnimatorController` as a playable node; full Mecanim scripting API |
| `AnimationScriptPlayable` | Runs a custom `IAnimationJob` with direct `AnimationStream` access |
| `AnimationPlayableOutput` | Output targeting a scene `Animator` |
| `AudioClipPlayable` / `AudioMixerPlayable` / `AudioPlayableOutput` | Audio-side node and output types, structurally parallel to the animation ones |
| `AnimationPlayableUtilities` | One-call helpers: `PlayClip`, `PlayMixer`, `PlayLayerMixer`, `PlayAnimatorController` |
| `PlayableAsset` | `ScriptableObject` base for authoring assets that inject a subgraph via `CreatePlayable` |
| `PlayableDirector` | Component that builds/owns a graph from a `PlayableAsset`; `Play`/`Pause`/`Stop`/`time`/bindings |
| `PlayableDirector.RebuildGraph()` / `RebindPlayableGraphOutputs()` | Full graph rebuild vs. cheaper rebind-only-bindings update |
| `INotificationReceiver` / `Notification` | Interface + payload for one-shot events pushed through an output |
| `PlayableTraversalMode` | `Mix` (normal evaluation) vs `Passthrough` (route only the matching output port) |
| `DirectorUpdateMode` | `GameTime` / `DSPClock` / `UnscaledGameTime` / `Manual` clock sources |
| `PlayState` | `Playing` / `Paused` (per-playable play status) |
| `UnityEditor.Playables.Utility` | Editor-only: `GetAllGraphs()`, `graphCreated`/`destroyingGraph` events |

## Advanced Notes

- **Timeline and Animator are themselves built on `PlayableGraph`.** `Manual/class-PlayableDirector.html` states the Playable Director component is "primarily used by Unity's Timeline" to store the link between a Timeline instance and asset, hold track/binding data, and control playback — internally, Timeline's `TimelineAsset` and its tracks are `PlayableAsset`/`PlayableOutput`-producing types (`Manual/Playables.html`, `ScriptReference/Playables.PlayableAsset.html`) that a `PlayableDirector` assembles into a graph exactly the way this skill's examples do by hand. The Animator, similarly, exposes its own internal graph via `Animator.playableGraph` — Animation Rigging's `RigBuilder` (covered in `unity-animation-cinematics`) injects its constraint-evaluation playables into that same graph, downstream of the Animator's base pose. Understanding the raw API is therefore not just an alternative to Timeline/Animator — it's literally the substrate both are implemented on, which is why advanced integration (injecting a custom playable into an Animator's own graph, or writing a custom `PlayableAsset` that Timeline can host on a track) requires the vocabulary in this skill even when the end product still uses the Timeline/Animator editor UI.
- **When hand-rolling a graph beats using Timeline.** Reach for the raw Playables API when you need graph topology that changes dynamically at runtime in ways Timeline's fixed authored sequence can't express well — `Manual/Playables.html` calls out exactly this: "dynamically add or adjust playable nodes at runtime instead of creating a complex static graph that accounts for all possible outcomes," giving the example of a character with different interaction animations for a weapon, chest, and trap where the right clip should attach/detach based on proximity rather than being pre-authored into one giant state graph. Runtime procedural blending (the queue-cycling example, or weight-driven blends computed from gameplay state each frame) and pure-script `ScriptPlayableOutput`-driven systems (sequencers, dialogue queues) are also naturally graph-first problems that don't map cleanly onto Timeline's clip-on-a-track authoring model.
- **When Timeline still wins.** Anything an artist or designer needs to author, scrub, and re-order visually — fixed-duration cutscenes, scripted multi-track choreography — is better served by Timeline's editor even though it's "just" building the same kind of graph under the hood, because the authoring/iteration loop (drag clips, scrub the playhead, see markers) has no equivalent when you're writing `graph.Connect()` calls by hand. A common hybrid: author the coarse sequence in Timeline, but give one track a custom `PlayableAsset`/`PlayableBehaviour` pair whose `ProcessFrame` does dynamic, code-driven work (e.g. procedural aim-blending) that would be awkward to keyframe directly.
- **`AnimationScriptPlayable` and `IAnimationJob` are a distinct, job-system-based extension point** (`ScriptReference/Animations.AnimationScriptPlayable.html`) for cases needing direct, multi-threaded read/write access to the `AnimationStream` — lower-level than a `PlayableBehaviour`'s per-frame C# callbacks and intended for performance-critical procedural animation (custom IK solvers, physics-driven bone correction) rather than general graph orchestration; it's a different tool from the `PlayableBehaviour`/`ScriptPlayable<T>` path this skill focuses on, and its `IAnimationJob` contract deserves separate, focused treatment before writing production code against it.

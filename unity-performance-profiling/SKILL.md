---
name: unity-performance-profiling
description: Use when diagnosing or optimizing Unity performance — Profiler/Frame Debugger/Memory Profiler usage, GC allocation spikes, or Jobs/Burst/DOTS-ECS. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Performance & Profiling

## Retrieval Sources

All paths below were re-verified on disk under `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` on 2026-08-19 (`wc -l`, and for the two flagged package pages, a stripped-tag text dump). Manual pages listed as "full manual page" run 150-600+ lines of real prose; the two rows marked "package landing stub" are ~190-line auto-generated package-metadata pages (description, version, dependency list) with no authoring content.

| Source | Path | Use for |
|--------|------|---------|
| Profiler Window overview | `Manual/ProfilerWindow.html` (430 lines) | Window layout, modules list, timeline vs. hierarchy view, record/pause controls |
| Profiler introduction & navigation | `Manual/profiler-introduction.html`, `Manual/profiler-window-navigating.html` | Conceptual intro; how to read the frame timeline and select frames |
| Profiler modules overview | `Manual/profiler-modules-introduction.html`, `Manual/profiler-modules-activate.html`, `Manual/profiler-create-modules.html`, `Manual/profiler-customizing.html` | What a "module" is, enabling/disabling built-in modules, building custom modules |
| CPU module | `Manual/ProfilerCPU.html` (407 lines), `Manual/profiler-cpu.html`, `Manual/profiler-cpu-introduction.html`, `Manual/profiler-cpu-navigating.html` | Main-thread cost breakdown: Timeline view, Hierarchy view, per-thread rows |
| GPU module | `Manual/ProfilerGPU.html` (355 lines) | Render-thread cost per draw call/pass; requires a supported graphics API and GPU Profiler enabled on-device |
| Memory module (built-in Profiler tab) | `Manual/ProfilerMemory.html` (389 lines), `Manual/profiler-memory.html`, `Manual/profiler-memory-introduction.html` | Reserved/used memory trend line inside the Profiler window itself (not the separate package) |
| Memory Profiler package | `Manual/com.unity.memoryprofiler.html` (194 lines, real package doc with feature list, not a stub) | Point-in-time heap snapshots, snapshot comparison, object retention paths, managed vs. native breakdown |
| Rendering module | `Manual/ProfilerRendering.html`, `Manual/profile-rendering.html` | Batches, SetPass calls, triangles/vertices count per frame |
| Audio module | `Manual/ProfilerAudio.html` (377 lines) | DSP CPU usage, voice count, audio memory |
| Physics module (3D) | `Manual/ProfilerPhysics.html` | Physics step cost, active body/contact counts |
| Physics module (2D) | `Manual/2d-physics/physics-profiler/physics-2d-profiler-landing.html`, `.../physics-2d-profiler-module-reference.html` | Box2D-backed 2D physics step cost, separate from the 3D physics module |
| Global Illumination module | `Manual/ProfilerGI.html` | Realtime/baked GI system cost |
| Asset Loading / File Access / Virtual Texturing / Video modules | `Manual/profiler-asset-loading-module.html`, `Manual/profiler-file-access-module.html`, `Manual/profiler-virtual-texturing-module.html`, `Manual/profiler-video-profiler-module.html` | Streaming and I/O bottleneck isolation, separate from CPU/GPU compute cost |
| Deep Profiling | `Manual/profiler-deep-profiling.html` (185 lines) | Instrumenting every managed function call (not just markers); cost and when to use it |
| Play mode / Edit mode profiling scope | `Manual/profiling-play-mode.html`, `Manual/profiling-edit-mode.html`, `Manual/profiler-play-edit-samples.html` | What gets captured in each mode; Editor-only overhead to discount |
| Target-device / build profiling | `Manual/profiling-target-device.html`, `Manual/profiling-collect-data-introduction.html`, `Manual/profiler-standalone-process.html`, `Manual/profiler-command-line-arguments.html` | Attaching the Profiler to a standalone Development Build; `-deepprofiling`, autoconnect flags |
| Android on-device profiling | `Manual/android-profile-on-an-android-device.html` (213 lines) | ADB-based capture workflow, Autoconnect Profiler, on-device thermal/CPU caveats |
| iOS on-device profiling | `Manual/ios-profile-device.html` (179 lines), `Manual/iphone-performance.html`, `Manual/iphone-InternalProfiler.html`, `Manual/XcodeFrameDebuggerIntegration.html` | Xcode-side capture, Instruments handoff, internal profiler log flags |
| WebGL / Windows Store profiling | `Manual/webgl-performance.html`, `Manual/webgl-memory.html`, `Manual/windowsstore-profiler.html` | Platform-specific memory ceilings and capture limitations |
| Frame Debugger | `Manual/FrameDebugger.html` (185 lines), `Manual/FrameDebugger-landing.html`, `Manual/FrameDebugger-attach.html`, `Manual/FrameDebugger-debug.html`, `Manual/FrameDebugger-share-event-information.html` | Step-through draw-call inspection: event list, per-event state (shader, textures, mesh) |
| Draw-call batching & reduction | `Manual/DrawCallBatching.html`, `Manual/DrawCallBatching-landing.html`, `Manual/DrawCallBatching-Enable.html`, `Manual/DrawCallBatching-SetUp.html`, `Manual/DrawCallBatching-Properties.html`, `Manual/optimizing-draw-calls.html`, `Manual/optimizing-draw-calls-choose-method.html`, `Manual/reduce-draw-calls-landing.html`, `Manual/static-batching-enable.html` | Static/dynamic/GPU instancing batching mechanics, when each applies |
| SRP Batcher | `Manual/SRPBatcher-Profile.html` | Reading SRP Batcher compatibility/break info in the Frame Debugger |
| GPU/graphics performance overview | `Manual/graphics-performance-profiling.html`, `Manual/graphics-performance-birp.html`, `Manual/graphics-performance-and-profiling-in-urp.html`, `Manual/OptimizingGraphicsPerformance.html` | Pipeline-specific (Built-in vs URP) GPU profiling workflow differences |
| Garbage collector overview | `Manual/performance-garbage-collector.html` (178 lines) | .NET/Boehm/incremental GC model Unity uses, generational behavior notes |
| Incremental GC modes | `Manual/performance-incremental-garbage-collection.html` (243 lines) | Incremental vs. non-incremental mode, per-frame time-slicing budget |
| Configuring / disabling GC | `Manual/performance-disabling-garbage-collection.html` (212 lines) | `GarbageCollector.GCMode`, manual `System.GC.Collect`, when disabling is (rarely) appropriate |
| Tracking GC allocations | `Manual/performance-track-garbage-collection.html` (205 lines) | Reading the GC.Alloc column, `Profiler.GetAllocatedMemoryForFrame`, allocation callstacks |
| Managed memory model | `Manual/performance-managed-memory.html`, `Manual/performance-managed-memory-introduction.html`, `Manual/performance-optimizing-code-managed-memory.html`, `Manual/performance-reference-types.html`, `Manual/performance-optimizing-arrays.html`, `Manual/performance-gc-avoid-reflection.html`, `Manual/performance-reusable-code.html` | Boxing, reference vs. value type allocation, array/collection alloc patterns, reflection cost |
| Native memory model & allocators | `Manual/performance-native-memory.html`, `Manual/performance-native-memory-introduction.html`, `Manual/performance-native-allocators.html`, `Manual/performance-bucket-allocator.html`, `Manual/performance-dynamic-heap-allocator.html`, `Manual/performance-dual-thread-allocator.html`, `Manual/performance-threadsafe-linear-allocator.html`, `Manual/performance-tls-stack-allocator.html`, `Manual/performance-native-memory-allocator-reference.html`, `Manual/performance-native-memory-allocator-examples.html` | Unity's C++-side allocator families; relevant when the Memory Profiler shows native, not managed, growth |
| Memory overview / unmanaged memory | `Manual/performance-memory.html`, `Manual/performance-memory-overview.html`, `Manual/performance-unmanaged-memory.html` | Top-level map of managed vs. native vs. unmanaged (NativeArray/Job) memory categories |
| Job System overview | `Manual/job-system-overview.html` (192 lines) | What the Job System is for, worker-thread model, why it exists vs. raw `System.Threading` |
| Job System full manual tree | `Manual/job-system.html` (194 lines, TOC page), `Manual/job-system-jobs.html`, `Manual/job-system-creating-jobs.html` (303 lines), `Manual/job-system-parallel-for-jobs.html` (245 lines), `Manual/job-system-job-dependencies.html` | `IJob` implementation steps, `Schedule`/`Complete`, `IJobParallelFor`, dependency chaining via `JobHandle` |
| NativeContainer / thread safety | `Manual/job-system-native-container.html`, `Manual/job-system-thread-safe-types.html`, `Manual/job-system-copy-nativecontainer.html`, `Manual/job-system-custom-nativecontainer.html`, `Manual/job-system-custom-nativecontainer-example.html` | `NativeArray<T>` and friends, safety-check system, writing a custom native container |
| Burst package page | `Manual/com.unity.burst.html` — **re-verified 2026-08-19: 190-line package landing stub** (description "IL/.NET bytecode → LLVM native code", version info, dependency list only; no authoring/usage content) | Confirms package presence/version only; do not expect conceptual Burst guidance here |
| Entities (DOTS-ECS) package page | `Manual/com.unity.entities.html` — **re-verified 2026-08-19: 194-line package landing stub** (description "modern ECS implementation", version 1.4.8 for Editor 6000.3, dependency list only; no authoring/usage content) | Confirms package presence/version only; this docs copy has no ECS authoring manual |
| Entities Graphics / DOTS instancing | `Manual/com.unity.entities.graphics.html` (also a landing stub), `Manual/dots-instancing-shaders*.html` (10 pages) | Shader-side DOTS instancing hookup only — not general ECS authoring |
| Adaptive Performance package | `Manual/com.unity.adaptiveperformance.html`, `Manual/adaptive-performance/*.html` (8 pages: overview, CPU/GPU control, metrics, optimization strategies, profiler integration, scaler profiles, boosts) | Mobile thermal/power-aware dynamic quality scaling — distinct from the core Profiler |
| Profile Analyzer package | `Manual/com.unity.performance.profile-analyzer.html` | Multi-frame CPU data-set comparison tool, layered on top of raw Profiler captures |
| Custom markers & counters (code) | `Manual/profiler-markers.html` (608 lines), `Manual/profiler-add-markers-code.html` (299 lines), `Manual/profiler-add-counters-code.html`, `Manual/profiler-adding-information-code.html`, `Manual/profiler-adding-information-code-intro.html`, `Manual/profiler-creating-custom-counters.html` | `ProfilerMarker`/`ProfilerCounter` authoring, custom Profiler module data sources |
| Profiler ScriptReference: sampling API | `ScriptReference/Profiling.Profiler.html`, `Profiling.Profiler.BeginSample.html`, `Profiling.Profiler.EndSample.html`, `Profiling.Profiler.BeginThreadProfiling.html`, `Profiling.Profiler.EndThreadProfiling.html`, `Profiling.Profiler-usedHeapSizeLong.html` | Manual instrumentation API surface for `Profiler.BeginSample`/`EndSample` |
| Profiler ScriptReference: markers/recorders | `ScriptReference/Unity.Profiling.ProfilerMarker.html`, `Unity.Profiling.ProfilerMarker.Auto.html`, `Unity.Profiling.ProfilerMarker.AutoScope.html`, `Unity.Profiling.ProfilerRecorder.html`, `Unity.Profiling.ProfilerRecorder.StartNew.html` (and ~15 related member pages) | `ProfilerMarker` struct API, `ProfilerRecorder` for reading built-in counters from code |
| GC ScriptReference | `ScriptReference/Scripting.GarbageCollector.html`, `Scripting.GarbageCollector.GCMode.html`, `Scripting.GarbageCollector.CollectIncremental.html`, `Scripting.GarbageCollector-incrementalTimeSliceNanoseconds.html`, `Scripting.GarbageCollector-isIncremental.html` | `GarbageCollector.GCMode` (Enabled/Disabled/Manual), incremental time-slicing control from code |
| Object pooling ScriptReference | `ScriptReference/Pool.ObjectPool_1.html`, `Pool.ObjectPool_1-ctor.html`, `Pool.ObjectPool_1.Get.html`, `Pool.ObjectPool_1.Release.html`, `Pool.ObjectPool_1.Clear.html`, `Pool.IObjectPool_1.html` | `UnityEngine.Pool.ObjectPool<T>` full constructor/member API |
| Memory snapshot ScriptReference | `ScriptReference/Unity.Profiling.Memory.MemoryProfiler.html`, `.TakeSnapshot.html`, `.TakeTempSnapshot.html`, plus the legacy `MemoryProfiler.PackedMemorySnapshot.html` family (~50 member pages) | Programmatic snapshot capture and the raw packed-snapshot data model the Memory Profiler package reads |
| Job System ScriptReference | `ScriptReference/Unity.Jobs.IJob.html`, `IJob.Execute.html`, `IJobExtensions.Schedule.html`, `IJobExtensions.Run.html`, `Unity.Jobs.IJobFor.html`, `Unity.Jobs.IJobParallelFor.html`, `IJobParallelForExtensions.Schedule.html`, `Unity.Jobs.JobHandle.CombineDependencies.html` | Exact method signatures for `IJob`/`IJobFor`/`IJobParallelFor`, `Schedule`/`Run`/`ScheduleParallel` overloads, `JobHandle` combination |
| Burst attribute ScriptReference | `ScriptReference/Unity.Burst.BurstAuthorizedExternalMethodAttribute.html`, `Unity.Burst.BurstDiscardAttribute.html` | The only local Burst API surface found; **no `[BurstCompile]` page located locally** — treat core Burst attribute/compiler usage as pretrained knowledge (flagged below) |

**Local coverage note (re-confirmed 2026-08-19):** `Manual/com.unity.entities.html` and `Manual/com.unity.burst.html` are both auto-generated ~190-line package-landing pages — title, one-paragraph description, version table, dependency list, nothing else. There is no local DOTS-ECS authoring manual (no archetypes/chunks/systems/SystemBase content) and no conceptual Burst manual (no `[BurstCompile]` walkthrough, no supported-subset-of-C# reference). The Job System itself, by contrast, has a full 7-page manual tree (`job-system*.html`, 192-303 lines each) plus deep ScriptReference coverage, so Job System guidance below is doc-grounded; Burst/DOTS-ECS guidance is explicitly labeled pretrained knowledge in Key Guidelines. Do not present Burst compiler internals or ECS architecture as sourced from this docs copy.

## Key Guidelines

### Using the Profiler Window
The Profiler window (`Manual/ProfilerWindow.html`) is a stack of independent modules — CPU, GPU, Memory, Rendering, Audio, Physics, GI, and more — each showing a per-frame value on the shared timeline at the top and a detail view below for whichever module is selected. Enable only the modules you need (`Manual/profiler-modules-activate.html`); every active module adds capture overhead. Two capture modes matter: the live Editor session (cheap to start, but Editor overhead pollutes CPU/memory numbers) and a Development Build with "Autoconnect Profiler" checked, which attaches to a standalone process over the network (`Manual/profiling-target-device.html`, `Manual/profiler-standalone-process.html`) — the latter is the only trustworthy source for real performance numbers. Records are frame-indexed: click any frame in the timeline strip to inspect it in the detail pane below, and use the frame-selection keyboard shortcuts to step frame-by-frame around a spike.

```csharp
// Attaching from code is rarely needed — Autoconnect Profiler in Build Settings covers
// the common case. For scripted CI capture of a specific window, use command-line args
// documented in Manual/profiler-command-line-arguments.html, e.g.:
//   MyGame.exe -deepprofiling -connectProfiler
// combined with EditorUserBuildSettings.connectProfiler = true before a batch build.
```

### CPU vs GPU vs Memory Profiling
These three modules answer different questions and none of them substitutes for another. The CPU module (`Manual/ProfilerCPU.html`) has two views: Timeline (per-thread, time-ordered, shows waits and thread handoffs) and Hierarchy (call-tree, sortable by Total/Self time) — use Timeline to see *why* a frame stalled (e.g. main thread blocked waiting on a Job) and Hierarchy to see *what* is expensive in aggregate. The GPU module (`Manual/ProfilerGPU.html`) requires a graphics API that supports GPU timing and, on-device, an explicit GPU Profiler enable step; it reports cost per draw call/pass on the render thread, which runs one or more frames behind the CPU thread — so a CPU spike and the GPU spike it caused can appear on different frame indices. The built-in Memory module (`Manual/ProfilerMemory.html`) only plots reserved/used totals over time — good for spotting a leak trend (a line that climbs and never comes back down after a scene unload) but useless for finding *which object* is responsible; that's the separate Memory Profiler package's job (`Manual/com.unity.memoryprofiler.html`), which captures a full point-in-time snapshot you can inspect object-by-object and diff against an earlier snapshot to find exactly what grew. Diagnostic order: watch CPU+GPU modules together first to classify CPU-bound vs GPU-bound; if the CPU module shows GC.Alloc or a memory trend looks wrong, switch to the Memory Profiler package for root-causing.

```csharp
// Reading the same counters the Memory module plots, from code, via ProfilerRecorder
// (ScriptReference/Unity.Profiling.ProfilerRecorder.html):
using Unity.Profiling;

ProfilerRecorder totalReservedRecorder =
    ProfilerRecorder.StartNew(ProfilerCategory.Memory, "Total Reserved Memory");

void LogMemory()
{
    if (totalReservedRecorder.Valid)
        Debug.Log($"Reserved: {totalReservedRecorder.LastValue / (1024 * 1024)} MB");
}

void OnDestroy() => totalReservedRecorder.Dispose(); // ProfilerRecorder must be disposed
```

### Frame Debugger for Draw Calls
The Frame Debugger (`Manual/FrameDebugger.html`) freezes a frame and lets you step through its render events one draw call at a time, in submission order, showing per-event state: which shader pass, which textures/buffers were bound, and the mesh/vertex count. This is a *state and ordering* tool, not a timing tool — it will tell you that a draw call broke SRP batching or wasn't statically batched (`Manual/SRPBatcher-Profile.html`, `Manual/DrawCallBatching.html`), but not how many milliseconds it cost; pair it with the GPU module for cost. Typical workflow: spot an unexpectedly high draw-call/SetPass count in the Rendering module (`Manual/ProfilerRendering.html`), open the Frame Debugger on that frame, and step through events looking for consecutive draws using the same material that failed to batch — usually because of a per-renderer `MaterialPropertyBlock` override, a non-uniform scale breaking static batching eligibility, or two materials that are instance-identical but were loaded/instanced separately so Unity treats them as different (`Manual/optimizing-draw-calls-choose-method.html`).

```csharp
// Common batching-break fix: use MaterialPropertyBlock instead of unique Material
// instances per-object, which keeps objects on the same batched material.
// BEFORE — breaks SRP Batcher / dynamic batching, one material instance per unit:
public class UnitBad : MonoBehaviour
{
    public Color tint;
    void Start()
    {
        // Instantiating .material creates a unique material per object.
        GetComponent<Renderer>().material.color = tint;
    }
}

// AFTER — all units share one material asset; per-instance color goes through a block.
public class UnitGood : MonoBehaviour
{
    public Color tint;
    static readonly int TintId = Shader.PropertyToID("_BaseColor");
    MaterialPropertyBlock _block;
    Renderer _renderer;

    void Awake()
    {
        _renderer = GetComponent<Renderer>();
        _block = new MaterialPropertyBlock();
    }

    void Start()
    {
        _renderer.GetPropertyBlock(_block);
        _block.SetColor(TintId, tint);
        _renderer.SetPropertyBlock(_block);
    }
}
```

### Deep Profiling
Deep Profiling (`Manual/profiler-deep-profiling.html`) instruments every managed method call, not just calls wrapped in a `ProfilerMarker` — normally the Profiler only sees Unity's built-in markers plus any you added by hand. This gives full call-tree visibility into third-party or legacy code with no markers, at the cost of instrumenting *every* function call, which can slow a frame by an order of magnitude and will itself distort the very timings you're trying to read. Enable it only for a short, targeted capture on a narrow repro case, then turn it off; never leave it on as a default profiling mode, and never trust its absolute millisecond values — use it to find *which* function is hot, then re-measure that function's real cost with Deep Profiling off (via a manual `ProfilerMarker` around it if needed).

### Avoiding GC Allocations
Per-frame managed allocations are the most common source of stutter, and a low CPU-ms reading can still hide a GC problem — always check the GC.Alloc column in the CPU module's Hierarchy view alongside time, not instead of it (`Manual/performance-track-garbage-collection.html`). Common per-frame allocators: `foreach` over a non-generic/`IEnumerable`-typed collection (boxes the enumerator), string concatenation and interpolation in a hot path, boxing a value type into `object`/an interface, LINQ (nearly every operator allocates an iterator or closure), and closures capturing local variables in a delegate created every frame (`Manual/performance-optimizing-code-managed-memory.html`, `Manual/performance-reference-types.html`). Once garbage exists, Unity's incremental GC (`Manual/performance-incremental-garbage-collection.html`) spreads collection across frames rather than pausing the whole application, which controls *when* the stall happens, not whether garbage was produced — the fix is always to stop allocating in the hot path, not to lean on incremental collection to hide it.

```csharp
// BEFORE — allocates every frame: closure, LINQ, and string concat all box/allocate.
void Update()
{
    var nearby = enemies.Where(e => Vector3.Distance(e.transform.position, transform.position) < range)
                         .OrderBy(e => e.priority)
                         .ToList();
    Debug.Log("Nearby count: " + nearby.Count + " at frame " + Time.frameCount);
}

// AFTER — zero per-frame managed allocation: manual loop, no LINQ, no closure, no concat.
List<Enemy> _nearbyBuffer = new List<Enemy>(32); // allocated once, reused

void Update()
{
    _nearbyBuffer.Clear();
    float sqrRange = range * range;
    for (int i = 0; i < enemies.Count; i++)
    {
        Enemy e = enemies[i];
        if ((e.transform.position - transform.position).sqrMagnitude < sqrRange)
            _nearbyBuffer.Add(e);
    }
    _nearbyBuffer.Sort((a, b) => a.priority.CompareTo(b.priority)); // in-place, no alloc
    // avoid Debug.Log/string building in a hot Update at all in shipping code
}
```

### Object Pooling
Frequent `Instantiate`/`Destroy` churn is expensive for two independent reasons: `Instantiate` does real allocation and initialization work (component construction, native object creation), and `Destroy` triggers cleanup plus eventually GC of any managed garbage left behind — both compound with spawn-heavy systems like projectiles or particles. `UnityEngine.Pool.ObjectPool<T>` (`ScriptReference/Pool.ObjectPool_1.html`) is the built-in reuse pattern: it wraps create/get/release/destroy callbacks and an internal stack, with an optional max-size to avoid unbounded pool growth. Prefer it over hand-rolled pools unless you need pooling semantics it doesn't offer (e.g. cross-scene persistence with custom eviction).

```csharp
using UnityEngine;
using UnityEngine.Pool;

public class BulletPool : MonoBehaviour
{
    [SerializeField] Bullet bulletPrefab;
    ObjectPool<Bullet> _pool;

    void Awake()
    {
        _pool = new ObjectPool<Bullet>(
            createFunc: () => Instantiate(bulletPrefab),
            actionOnGet: b => b.gameObject.SetActive(true),
            actionOnRelease: b => b.gameObject.SetActive(false),
            actionOnDestroy: b => Destroy(b.gameObject),
            collectionCheck: true,   // catches double-release bugs in dev builds
            defaultCapacity: 32,
            maxSize: 256);
    }

    public Bullet Spawn(Vector3 pos, Quaternion rot)
    {
        Bullet b = _pool.Get();
        b.transform.SetPositionAndRotation(pos, rot);
        return b;
    }

    public void Despawn(Bullet b) => _pool.Release(b);

    void OnDestroy() => _pool.Dispose();
}
```

### Caching Component Lookups
`GetComponent` (and `Find`/`FindObjectOfType` family calls) walk the object's component list or the scene graph and are meaningfully expensive to call every frame. Resolve references once in `Awake`/`Start` and cache them in a field; if a reference is needed across objects, wire it via inspector serialization or a lookup built once, not `GameObject.Find` in `Update`.

```csharp
// BEFORE
void Update() { GetComponent<Rigidbody>().AddForce(Vector3.up); }

// AFTER
Rigidbody _rb;
void Awake() => _rb = GetComponent<Rigidbody>();
void Update() => _rb.AddForce(Vector3.up);
```

### Jobs / Burst Overview (mixed: Job System is doc-grounded, Burst/DOTS-ECS is pretrained knowledge)
The C# Job System (`Manual/job-system-overview.html`, doc-grounded) lets you write structured multithreaded code that Unity's own scheduler distributes across worker threads, using `NativeContainer` types (`NativeArray<T>` etc., `Manual/job-system-native-container.html`) to share data safely between the main thread and jobs — the safety system detects and errors on unsynchronized concurrent access rather than silently corrupting data. To run work: implement `IJob` (single unit of work, `Manual/job-system-creating-jobs.html`) or `IJobParallelFor`/`IJobFor` (data-parallel over a `NativeArray`, `Manual/job-system-parallel-for-jobs.html`), call `.Schedule()` to enqueue it (or `.Run()` to execute immediately on the main thread, useful for debugging), and call `.Complete()` on the returned `JobHandle` before touching the data it wrote — when one job depends on another's output, pass the earlier `JobHandle` into the later `.Schedule(dependency)` call so the scheduler orders them correctly (`Manual/job-system-job-dependencies.html`). **Burst and DOTS-ECS are only landing-page-stubbed locally** (see coverage note above), so the following is pretrained knowledge, not sourced from this docs copy — verify specifics against Unity's online package docs before committing to an architecture: the `[BurstCompile]` attribute ahead-of-time-compiles a job's `Execute` method (and static burst-compiled functions) from IL to native code via LLVM, giving near-C++ throughput on math-heavy, branch-light code operating over blittable/native-container data; it does not support arbitrary managed C# (no managed allocations, no exceptions in release, restricted API subset) so a job with unsupported code silently falls back to Mono/IL2CPP interpretation unless you enable compile-time errors for that case. DOTS-ECS (the Entities package) is a separate, opt-in architectural layer built on top of the Job System/Burst — entities/components/systems replace GameObject/MonoBehaviour for the subset of the project adopting it; adopting Jobs+Burst does not require adopting ECS, and the two should not be conflated when scoping optimization work.

```csharp
using Unity.Collections;
using Unity.Jobs;
using UnityEngine;
// [Unity.Burst.BurstCompile] would ahead-of-time-compile Execute() — attribute confirmed to
// exist locally (ScriptReference/Unity.Burst.BurstDiscardAttribute.html implies BurstCompile's
// presence in the package), but its usage/behavior details here are pretrained knowledge.

public struct DistanceJob : IJobParallelFor
{
    [ReadOnly] public NativeArray<Vector3> positions;
    public NativeArray<float> results; // written in parallel, one index per thread call
    public Vector3 origin;

    public void Execute(int index)
    {
        results[index] = Vector3.Distance(positions[index], origin);
    }
}

public class DistanceJobRunner : MonoBehaviour
{
    public void RunJob(Vector3[] points, Vector3 origin)
    {
        var positions = new NativeArray<Vector3>(points, Allocator.TempJob);
        var results = new NativeArray<float>(points.Length, Allocator.TempJob);

        var job = new DistanceJob { positions = positions, results = results, origin = origin };
        JobHandle handle = job.Schedule(points.Length, 64); // 64 = inner batch count
        handle.Complete(); // blocks main thread until done — schedule earlier in the frame
                            // and Complete() later to overlap with other work instead

        // ... read results ...

        positions.Dispose();
        results.Dispose();
    }
}
```

### Garbage Collector Configuration
Unity's default GC mode is incremental (`Manual/performance-incremental-garbage-collection.html`): collection work is time-sliced across multiple frames (budget controlled by `GarbageCollector.incrementalTimeSliceNanoseconds`, `ScriptReference/Scripting.GarbageCollector-incrementalTimeSliceNanoseconds.html`) instead of a single long stop-the-world pause. You can also disable incremental mode in Player Settings, or take manual control from code via `GarbageCollector.GCMode` (`Disabled`/`Enabled`/`Manual`, `ScriptReference/Scripting.GarbageCollector.GCMode.html`) and force a full synchronous collection with `System.GC.Collect()` at a controlled point (e.g. a loading-screen frame) rather than letting it land mid-gameplay (`Manual/performance-disabling-garbage-collection.html`).

```csharp
using UnityEngine.Scripting;

// During a loading screen: force a full collection at a moment the player won't feel it,
// so gameplay frames are less likely to hit a GC pause from accumulated garbage.
void OnLoadingScreenShown()
{
    System.GC.Collect();
}

// Temporarily pausing GC during a latency-critical stretch (use sparingly — garbage still
// accumulates and must be collected eventually, ideally at the next safe point):
void BeginLatencyCriticalSection() => GarbageCollector.GCMode = GarbageCollector.Mode.Disabled;
void EndLatencyCriticalSection()   => GarbageCollector.GCMode = GarbageCollector.Mode.Enabled;
```

### Profiling on Target Hardware
Editor playmode profiling always includes Editor-only overhead (extra rendering, IDE-attached debugger costs, non-shipping code paths) that skews both CPU and memory numbers relative to a real build. Always validate with a Development Build, Autoconnect Profiler enabled, running on the actual target device — mobile CPUs in particular throttle under sustained thermal load in ways the Editor on a desktop workstation never will (`Manual/android-profile-on-an-android-device.html`, `Manual/ios-profile-device.html`, `Manual/profiling-target-device.html`).

## Common Mistakes

| Mistake | Fix |
|---|---|
| Profiling only in the Editor | Build a Development Build with Autoconnect Profiler, test on target hardware (`Manual/profiling-target-device.html`). |
| Ignoring the GC.Alloc column | Low CPU ms can still hide per-frame allocations causing stutter (`Manual/performance-track-garbage-collection.html`). |
| `GetComponent`/`Find` in `Update` | Cache the reference once in `Awake`. |
| Reading the Frame Debugger as timing data | It's draw-call order/state; pair with the GPU module for actual cost (`Manual/FrameDebugger.html`). |
| Deep Profiling left on by default | Massively distorts timings; enable only for a targeted capture (`Manual/profiler-deep-profiling.html`). |
| Assuming Jobs/Burst implies full DOTS-ECS | Both work standalone with MonoBehaviours; ECS is a separate architectural commitment, and this docs copy has no ECS authoring manual to lean on. |
| Trusting the built-in Memory module for leak root-causing | It only shows a trend line; use the Memory Profiler package snapshot diff to find the responsible object (`Manual/com.unity.memoryprofiler.html`). |
| Calling `.Complete()` immediately after `.Schedule()` | Defeats the purpose of scheduling — schedule early in the frame, do other work, `Complete()` later to actually overlap. |
| Creating a unique `Material` instance per renderer (`renderer.material`) | Breaks SRP Batcher/static/dynamic batching; use a shared material plus `MaterialPropertyBlock` for per-instance values. |
| Using LINQ / `foreach` over boxed enumerators in hot paths | Both allocate; replace with manual `for` loops and pre-sized reusable buffers in `Update`/`FixedUpdate`. |
| Leaving `NativeArray`/`NativeContainer` undisposed | Throws a safety-system error and leaks native memory; always pair allocation with `Dispose()`, ideally via `using` or a guaranteed cleanup path. |
| One-off `Instantiate`/`Destroy` for frequently spawned objects (bullets, VFX, pickups) | Use `ObjectPool<T>` (`ScriptReference/Pool.ObjectPool_1.html`) to reuse instances instead. |
| Assuming GPU module numbers are same-frame as CPU numbers | The render thread trails the CPU thread by one or more frames; a CPU spike and its GPU consequence can show on different frame indices. |
| Treating incremental GC as "fixing" allocation problems | It only spreads the *pause*, not the amount of garbage; still eliminate hot-path allocations. |
| Forgetting to `Dispose()` a `ProfilerRecorder` | Leaks the recorder handle; dispose in `OnDestroy` or an equivalent teardown. |
| Citing Burst/ECS internals as if sourced from local docs | `com.unity.burst.html` and `com.unity.entities.html` are landing-page stubs only — label such guidance as pretrained knowledge and verify online before relying on it architecturally. |

## Quick Reference

| Tool / API | Purpose |
|---|---|
| Profiler Window — CPU module | Main-thread frame cost: scripts, physics, animation, rendering submission (`Manual/ProfilerCPU.html`) |
| Profiler Window — GPU module | Render-thread frame cost, requires supported graphics API (`Manual/ProfilerGPU.html`) |
| Profiler Window — Memory module | Reserved/used memory trend over time, not object-level (`Manual/ProfilerMemory.html`) |
| Profiler Window — Rendering module | Batches, SetPass calls, triangle/vertex counts (`Manual/ProfilerRendering.html`) |
| Profiler Window — Audio module | DSP CPU cost, voice count, audio memory (`Manual/ProfilerAudio.html`) |
| Profiler Window — Physics/Physics2D modules | Physics step cost, active bodies/contacts |
| Deep Profiling mode | Instruments every managed call; huge overhead, use briefly and narrowly |
| Frame Debugger | Step-through draw-call inspection for batching/state issues, not timing |
| Memory Profiler package | Point-in-time heap snapshots, diffing, object retention/leak analysis |
| Profile Analyzer package | Multi-frame CPU data-set comparison across two captures |
| Adaptive Performance package | Runtime thermal/power-aware quality scaling on supported mobile devices |
| `Profiler.BeginSample`/`EndSample` | Manual instrumentation of a code region (string-keyed) |
| `Unity.Profiling.ProfilerMarker` | Struct-based manual marker, lower overhead than string-keyed `BeginSample` |
| `Unity.Profiling.ProfilerRecorder` | Read built-in or custom counter values from code at runtime |
| `GarbageCollector.GCMode` | `Enabled`/`Disabled`/`Manual` control of the collector from code |
| `GarbageCollector.incrementalTimeSliceNanoseconds` | Per-frame time budget for incremental GC |
| `System.GC.Collect()` | Force a full synchronous collection at a controlled point |
| `UnityEngine.Pool.ObjectPool<T>` | Built-in reuse pattern to avoid alloc/GC churn from frequent instantiate/destroy |
| C# Job System — `IJob` | Single scheduled unit of work off the main thread |
| C# Job System — `IJobFor`/`IJobParallelFor` | Data-parallel work over a `NativeArray`, batched across worker threads |
| `JobHandle` / `.Schedule()` / `.Complete()` / `CombineDependencies` | Job scheduling, dependency chaining, and synchronization points |
| `NativeArray<T>` / `NativeContainer` | Thread-safe unmanaged data shared between main thread and jobs; must be `Dispose()`d |
| `[BurstCompile]` (pretrained knowledge — local docs are stub-only) | Ahead-of-time LLVM compilation of job/static methods for near-native throughput |
| Autoconnect Profiler + Development Build | Only reliable way to get real on-device CPU/GPU/memory numbers |
| `Manual/profiler-command-line-arguments.html` flags (e.g. `-deepprofiling`) | Scripted/CI capture configuration |

## Advanced Notes

### Mobile-Specific Optimization Checklist
Mobile devices differ from desktop/console in ways that change what "optimized" means: thermal throttling means a device can pass a short benchmark and still degrade over a 20-minute play session as the SoC downclocks, so profile sustained sessions on-device, not just a few captured frames (`Manual/android-profile-on-an-android-device.html`, `Manual/ios-profile-device.html`). Practical checklist:
- Always profile via Autoconnect Profiler on the actual target device class you ship to, including a low/mid-tier device, not just your dev phone — thermal and memory ceilings vary widely across the Android install base in particular.
- Watch for thermal throttling specifically: capture a long session and compare frame times at minute 1 vs. minute 15-20; a steady climb in CPU/GPU ms with no code change is throttling, not a regression.
- Consider the Adaptive Performance package (`Manual/com.unity.adaptiveperformance.html`, `Manual/adaptive-performance/adaptive-performance.html`) to read device thermal/power state at runtime and scale quality (resolution, effect density, LOD bias) proactively rather than reactively via manual QualitySettings tuning.
- Memory ceilings are much tighter and OS-enforced — the Memory Profiler package snapshot workflow matters more here than on desktop, since an OOM kill on mobile is silent (no exception, the process just dies).
- Draw-call/SetPass count matters more per-platform: mobile GPUs are typically bandwidth- and overdraw-sensitive rather than raw-shader-throughput-bound, so batching wins (`Manual/DrawCallBatching.html`, `Manual/optimizing-draw-calls.html`) usually pay off more on mobile than equivalent desktop scenes.
- GC pauses are proportionally worse on mobile's slower CPUs; the incremental GC's per-frame time slice (`GarbageCollector.incrementalTimeSliceNanoseconds`) may need tuning down from default so a single frame's slice doesn't itself cause a visible hitch on weaker hardware.
- Audio module (`Manual/ProfilerAudio.html`) DSP cost is worth checking explicitly on mobile — voice counts that are free on desktop can meaningfully compete with CPU budget on a phone.
- Validate build size and texture memory budget per-platform; WebGL and mobile both have hard practical or OS-enforced ceilings documented separately (`Manual/webgl-memory.html`).

### The Incremental Garbage Collector, In Depth
Unity's incremental GC mode (default-on; `Manual/performance-incremental-garbage-collection.html`) spreads the mark-and-sweep work of a collection cycle across multiple frames instead of pausing the whole application for one long stop-the-world sweep. Each frame, the collector is allowed to do up to `GarbageCollector.incrementalTimeSliceNanoseconds` worth of work before yielding back to the game loop; a full collection cycle may therefore span many frames, each contributing a small, bounded amount of GC time rather than one unpredictable spike. This changes *when* collection pauses land and how big any single pause is — it does not reduce the total amount of garbage collected or the total CPU time spent collecting over the run of the program. Two practical consequences: first, a scene that allocates heavily every frame can still accumulate GC-related frame-time cost that shows up as a steady tax across many frames rather than one visible stutter, which is easy to miss if you're only looking for spikes — check the CPU module's GC.Alloc column and cumulative allocation trend, not just frame-time outliers. Second, disabling incremental GC (`Manual/performance-disabling-garbage-collection.html`, via the Player Setting or `GarbageCollector.GCMode = GarbageCollector.Mode.Disabled`) reverts to the older stop-the-world behavior, trading many small pauses for fewer, larger, more disruptive ones — this is essentially never the right default and should only be used narrowly (e.g. combined with a manually-triggered `System.GC.Collect()` at a controlled point such as a loading screen, per `Manual/performance-track-garbage-collection.html`) rather than left on globally. The correct fix for GC-related stutter is almost always to reduce or eliminate hot-path allocations (see the Avoiding GC Allocations and Object Pooling sections above) — incremental GC is a mitigation for unavoidable garbage, not a substitute for not producing it.

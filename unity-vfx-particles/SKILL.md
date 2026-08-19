---
name: unity-vfx-particles
description: Use when creating particle/visual effects in Unity — choosing between the Shuriken Particle System and VFX Graph. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity VFX & Particles

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Choosing your particle system | `Manual/ChoosingYourParticleSystem.html` | Shuriken vs. VFX Graph comparison table (render pipeline support, feasible particle counts, physics interaction, script interaction, frame buffer access) |
| Particle Systems overview | `Manual/ParticleSystems.html` | Shuriken component landing page, general concepts, links into every sub-topic |
| Particle System modules reference | `Manual/ParticleSystemModules.html` | Full module list (Emission, Shape, Velocity over Lifetime, Color over Lifetime, Size over Lifetime, Collision, Sub Emitters, Trails, Lights, Noise, Triggers, Texture Sheet Animation) |
| ParticleSystem component | `Manual/class-ParticleSystem.html` | Component properties, Inspector layout, Main module settings |
| Particle emissions and emitters | `Manual/particle-emissions-emitters.html` | Emission module (Rate over Time vs. Rate over Distance, bursts) and Shape module (emission volume/surface, start-velocity direction) |
| Particle color | `Manual/particle-color.html` | Color over Lifetime and Color by Speed modules |
| Particle size | `Manual/particle-size.html` | Size over Lifetime and Size by Speed modules |
| Particle rotation | `Manual/particle-rotation.html` | Rotation over Lifetime and Rotation by Speed modules |
| Particle velocity | `Manual/particle-velocity.html` | Velocity over Lifetime, Limit Velocity over Lifetime, Inherit Velocity |
| Particle physics/forces | `Manual/particle-physics-forces.html` | Force over Lifetime, External Forces module, gravity modifier |
| Particle collisions | `Manual/particle-collisions.html` | Collision module — World mode (real Colliders) vs. Planes mode (lower overhead, no Collider needed), Collides With layer mask, `sendCollisionMessages` |
| Particle triggers | `Manual/particle-triggers.html` | Trigger module — script callbacks (`OnParticleTrigger`) when particles enter/exit defined colliders, without full collision physics |
| Particle lights and trails | `Manual/particle-lights-trails.html` | Lights module (real-time lights per particle, `Maximum Lights` cap) and Trails module (per-particle trail ribbons, shares properties with Trail Renderer) |
| Varying properties over time | `Manual/varying-particle-system-properties-over-time.html` | Curve/random-between-curves authoring pattern used across most modules |
| Particle System + C# Job System | `Manual/particle-system-job-system-integration.html` | `IJobParticleSystem`, `IJobParticleSystemParallelFor`, `IJobParticleSystemParallelForBatch` — Burst-compiled per-particle access, faster than `GetParticles()`/`SetParticles()` |
| Particle System optimization (Built-in RP) | `Manual/particle-system-optimization.html` | GPU instancing for particles (render Meshes instead of billboards) in the Built-in Render Pipeline |
| VFX Graph overview | `Manual/VFXGraph.html` | GPU-simulated node-graph VFX package, requirements, links to VFX asset/component/Property Binder reference |
| VFX Project Settings | `Manual/class-VFXManager.html` | Project-wide VFX tab settings (only visible once the VFX Graph package is installed) |
| ParticleSystem scripting API | `ScriptReference/ParticleSystem.html` | Runtime control (Play/Stop/Pause/Emit/Clear, `particleCount`, module struct accessors) |
| ParticleSystem.MainModule | `ScriptReference/ParticleSystem.MainModule.html` | `duration`, `loop`, `startLifetime`, `startSpeed`, `startSize`, `simulationSpace`, `maxParticles` |
| ParticleSystem.EmissionModule | `ScriptReference/ParticleSystem.EmissionModule.html` | `rateOverTime`, `rateOverDistance`, `SetBursts`/`GetBursts` |
| ParticleSystem.ShapeModule | `ScriptReference/ParticleSystem.ShapeModule.html` | `shapeType`, `radius`, `arc`, `randomDirectionAmount` |
| ParticleSystem.VelocityOverLifetimeModule | `ScriptReference/ParticleSystem.VelocityOverLifetimeModule.html` | Per-axis velocity curves, `space` (local/world) |
| ParticleSystem.ColorOverLifetimeModule | `ScriptReference/ParticleSystem.ColorOverLifetimeModule.html` | `color` (MinMaxGradient) applied across normalized lifetime |
| ParticleSystem.SizeOverLifetimeModule | `ScriptReference/ParticleSystem.SizeOverLifetimeModule.html` | `size`/`sizeMultiplier` curve, `separateAxes` for non-uniform scale |
| ParticleSystem.CollisionModule | `ScriptReference/ParticleSystem.CollisionModule.html` | `type` (Planes/World), `mode`, `quality`, `AddPlane`/`SetPlane`, `bounce`, `dampen`, `lifetimeLoss` |
| ParticleSystem.SubEmittersModule | `ScriptReference/ParticleSystem.SubEmittersModule.html` | Sub-emitter slots keyed to Birth/Collision/Death/Trigger/Manual events |
| ParticleSystem.TrailModule | `ScriptReference/ParticleSystem.TrailModule.html` | `ratio`, `lifetime`, `minVertexDistance`, `textureMode` for per-particle trails |
| VisualEffect component API | `ScriptReference/VFX.VisualEffect.html` | Runtime control of VFX Graph instances — `Play`/`Stop`/`SendEvent`, `SetFloat`/`SetVector3`/`SetTexture`/`SetGraphicsBuffer`, `pause`, `playRate` |
| VFXManager (VFX Graph global settings) | `ScriptReference/VFX.VFXManager.html` | `fixedTimeStep`, `maxDeltaTime`, `GetBatchedEffectInfos` (GPU/CPU memory and instance-batching stats) |

## Key Guidelines

### Shuriken Particle System Modules

The Shuriken system is the `ParticleSystem` component: a fixed pipeline of toggleable modules, each simulated on the CPU (with optional Burst/Job System acceleration — see below) and configured either in the Inspector or through the matching module struct in script. The Main module (always on) sets system-wide defaults — `duration`, `loop`, `startLifetime`, `startSpeed`, `startSize`, `simulationSpace`, `maxParticles`. Emission controls spawn rate (`rateOverTime` for continuous emission independent of motion, `rateOverDistance` for motion-driven emission like tire dust) plus discrete bursts. Shape defines the emission volume/surface (cone, sphere, box, mesh, edge, etc.) and therefore the initial position and direction of new particles. From there, "over Lifetime" and "by Speed" modules (Color, Size, Rotation, Velocity, Limit Velocity, Force, Noise) apply curves or gradients evaluated against a particle's normalized age or current speed — the "varying properties over time" authoring pattern (curve, random-between-two-curves, or random-between-two-constants) is shared across nearly all of them. Every module is independently toggled per system, so a system pays simulation cost only for the modules it has enabled.

```csharp
using UnityEngine;

public class ConfigureBurstEffect : MonoBehaviour
{
    [SerializeField] private ParticleSystem ps;

    void Start()
    {
        var main = ps.main;
        main.startLifetime = 2f;
        main.startSpeed = 5f;
        main.maxParticles = 500;

        var emission = ps.emission;
        emission.rateOverTime = 0f; // no continuous emission
        emission.SetBursts(new[]
        {
            new ParticleSystem.Burst(0f, 30, 50, cycleCount: 1, repeatInterval: 0f)
        });

        var shape = ps.shape;
        shape.shapeType = ParticleSystemShapeType.Sphere;
        shape.radius = 0.5f;

        var colorOverLifetime = ps.colorOverLifetime;
        colorOverLifetime.enabled = true;
        var grad = new Gradient();
        grad.SetKeys(
            new[] { new GradientColorKey(Color.white, 0f), new GradientColorKey(Color.red, 1f) },
            new[] { new GradientAlphaKey(1f, 0f), new GradientAlphaKey(0f, 1f) });
        colorOverLifetime.color = new ParticleSystem.MinMaxGradient(grad);

        ps.Play();
    }
}
```

### Sub-Emitters & Bursts

Bursts (part of the Emission module) fire a batch of particles at a specific time — or, with `cycleCount`/`repeatInterval`, repeatedly — making them the tool for punctual effects like muzzle flashes or explosions rather than driving them through a sustained `rateOverTime`. Sub Emitters attach a second `ParticleSystem` to specific lifecycle events of the parent's particles — Birth, Collision, Death, Trigger, or Manual (fired from script) — so a single system can spawn a layered secondary effect, e.g. a smoke trail on birth and a burst of sparks on death. Sub-emitters inherit spatial context from the parent particle they're attached to but are configured as independent `ParticleSystem` assets/children, so their own modules (Emission, Shape, etc.) apply normally once triggered.

```csharp
using UnityEngine;

public class ConfigureSubEmitter : MonoBehaviour
{
    [SerializeField] private ParticleSystem parentPs;
    [SerializeField] private ParticleSystem deathBurstPs;

    void Start()
    {
        var subEmitters = parentPs.subEmitters;
        subEmitters.enabled = true;
        // Attach deathBurstPs to fire when parent particles die.
        subEmitters.AddSubEmitter(deathBurstPs, ParticleSystemSubEmitterType.Death,
            ParticleSystemSubEmitterProperties.InheritEverything);
    }
}
```

### Particle Collision & Triggers

The Collision module has two distinct modes with different cost profiles. World mode collides particles against real Colliders in the Scene — flexible (any layer, dynamic obstacles) but the most expensive option, with a `quality` setting (High/Medium/Low) trading accuracy for CPU cost. Planes mode instead collides against a manually authored list of infinite planes (defined by empty-GameObject transforms) that need no Collider component at all — this is explicitly documented as lower processor overhead and is the right default for simple floors/walls. `sendCollisionMessages` opts into `OnParticleCollision` callbacks on scripts for gameplay reactions (spawning decals, damage, sound). The separate Trigger module is cheaper still: it defines a set of collider regions and fires `OnParticleTrigger` when particles enter/exit/inside/outside them, without running full collision physics (bounce, dampen, lifetime loss) — use it purely to detect particle presence in a volume, not to make particles physically bounce.

```csharp
using UnityEngine;

public class ParticleCollisionHandler : MonoBehaviour
{
    private ParticleSystem ps;
    private readonly System.Collections.Generic.List<ParticleCollisionEvent> collisionEvents = new();

    void Awake()
    {
        ps = GetComponent<ParticleSystem>();
        var collision = ps.collision;
        collision.enabled = true;
        collision.type = ParticleSystemCollisionType.World;
        collision.mode = ParticleSystemCollisionMode.Collision3D;
        collision.quality = ParticleSystemCollisionQuality.Medium;
        collision.bounce = 0.3f;
        collision.sendCollisionMessages = true;
    }

    void OnParticleCollision(GameObject other)
    {
        int count = ps.GetCollisionEvents(other, collisionEvents);
        for (int i = 0; i < count; i++)
        {
            // e.g. spawn a decal/impact effect at collisionEvents[i].intersection
        }
    }
}
```

### VFX Graph (GPU-Simulated)

VFX Graph is a separate package built around the `VisualEffectAsset` (a node-graph you author in a dedicated graph editor) and the `VisualEffect` component that plays an instance of it in a scene. Unlike Shuriken's fixed module pipeline, simulation logic itself is authored as a graph and runs on the GPU via compute shaders, which is what lets it scale to millions of particles versus Shuriken's thousands (per the docs' own comparison table). It only supports the Universal and High Definition Render Pipelines (not the Built-in Render Pipeline), and in HDRP it additionally exposes camera color/depth buffer sampling that Shuriken has no equivalent for. Script interaction is event-driven rather than direct-particle-access: you expose named properties on the graph and drive them from C# via `SetFloat`/`SetVector3`/`SetTexture`/`SetGraphicsBuffer`/`SetGradient`, and send named events (with optional attached `VFXEventAttribute` payload data) via `SendEvent` instead of reading/writing individual particles.

```csharp
using UnityEngine;
using UnityEngine.VFX;

public class DriveVfxGraph : MonoBehaviour
{
    [SerializeField] private VisualEffect vfx;
    private static readonly int SpawnRateID = Shader.PropertyToID("SpawnRate");
    private static readonly int ColorID = Shader.PropertyToID("Color");

    void Start()
    {
        vfx.SetFloat(SpawnRateID, 200f);
        vfx.SetVector4(ColorID, Color.cyan);
        vfx.playRate = 1f;
        vfx.Play();
    }

    public void TriggerImpact(Vector3 worldPos)
    {
        var attr = vfx.CreateVFXEventAttribute();
        attr.SetVector3("position", worldPos);
        vfx.SendEvent("OnImpact", attr);
    }
}
```

### Choosing Between Them

The docs' own comparison table (`ChoosingYourParticleSystem.html`) is the canonical decision reference: Shuriken supports Built-in/URP/HDRP and full C# read/write access to every particle plus physics-engine collision, and is authored via Inspector modules; VFX Graph supports only URP/HDRP, scales to millions of particles via GPU simulation, and is authored via a visual node graph with an event/exposed-property scripting surface rather than per-particle access. Note that on platforms that support compute shaders, Unity explicitly allows using both solutions simultaneously in one project — this isn't an either/or choice at the project level, only per-effect. Default to Shuriken for gameplay-driven effects that need per-particle script logic (collision response, custom job-system behaviors), for projects still on the Built-in Render Pipeline, or for moderate particle counts. Reach for VFX Graph when an effect needs particle counts CPU simulation can't sustain, when you need HDRP camera-buffer sampling (refraction, depth-aware effects), or when the effect is authored primarily by a technical artist working in a graph rather than by a programmer.

### Performance / Overdraw

Shuriken's CPU cost scales with `maxParticles`, active module count, and the Collision module's `quality`/`type` setting — profile with the Profiler's CPU timeline, not guesswork. The C# Job System integration (`IJobParticleSystem`, `IJobParticleSystemParallelFor`, `IJobParticleSystemParallelForBatch`) lets custom per-particle behavior run Burst-compiled across worker threads, which the docs call out as strictly faster than the main-thread `GetParticles()`/`SetParticles()` pair — prefer it for any per-frame custom particle logic at scale. Independently of CPU sim cost, GPU instancing (`Manual/particle-system-optimization.html`, Built-in RP) renders particles as instanced Meshes instead of billboards, which is a rendering-side optimization orthogonal to simulation cost. The Lights module is called out explicitly in the docs as high performance cost — "especially in Forward Rendering mode," worse still with shadow-casting lights — which is why it has a dedicated `Maximum Lights` safety cap to prevent an emission-rate tweak from spawning thousands of real-time lights. VFX Graph cost shows up as GPU time (compute simulation + fill-rate for translucent particle rendering) rather than CPU time, so profile it with the GPU frame-timing tools/Frame Debugger, not the CPU profiler.

```csharp
using Unity.Collections;
using Unity.Jobs;
using UnityEngine;
using UnityEngine.ParticleSystemJobs;

public struct FadeAndDriftJob : IJobParticleSystemParallelFor
{
    public float deltaTime;

    public void Execute(ParticleSystemJobData jobData, int index)
    {
        var velocities = jobData.velocities;
        velocities[index] = new Vector3(
            velocities[index].x,
            velocities[index].y - 0.5f * deltaTime, // extra gravity, Burst-compiled
            velocities[index].z);
    }
}

public class JobDrivenParticles : MonoBehaviour
{
    [SerializeField] private ParticleSystem ps;

    void LateUpdate()
    {
        new FadeAndDriftJob { deltaTime = Time.deltaTime }
            .Schedule(ps)
            .Complete();
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Using Shuriken for hundreds of thousands+ particles | The docs' own comparison rates Shuriken's feasible count as "thousands" vs. VFX Graph's "millions"; CPU simulation bottlenecks well before GPU-simulated VFX Graph would — move large-scale effects to VFX Graph |
| VFX Graph effect renders on desktop but not on a target device | Platform/API lacks compute shader support, or the project is on the Built-in Render Pipeline (VFX Graph only supports URP/HDRP) — check both before authoring in VFX Graph |
| Leftover particles on pooled/reused GameObjects | `Stop()`/`Clear()` isn't called before reuse; call `ParticleSystem.Clear()` (and `Stop()` with `ParticleSystemStopBehavior.StopEmittingAndClear` if needed) when returning an object to a pool |
| Assuming VFX Graph is a drop-in replacement for every Shuriken effect | The authoring model differs fundamentally (graph/GPU/event-driven vs. modules/CPU/direct-particle-access); simple gameplay-feedback effects with per-particle script logic are often cheaper to build and run in Shuriken |
| Enabling World-mode Collision when Planes mode would do | World mode queries real Colliders and costs more CPU; the docs call out Planes mode as explicitly lower overhead for simple floors/walls that don't need a real Collider |
| Using the Collision module just to detect particle presence in a region | Collision module runs full physics response (bounce, dampen, lifetime loss) at collision cost; the Trigger module does cheap enter/exit/inside/outside detection via `OnParticleTrigger` without simulating a bounce |
| Driving per-particle custom behavior through `GetParticles()`/`SetParticles()` every frame | These run on the main thread and can't use the Burst Compiler; `IJobParticleSystem`/`IJobParticleSystemParallelFor`/`IJobParticleSystemParallelForBatch` distribute the same work across worker threads with Burst compilation |
| Over-using the Lights module for per-particle real-time lights | Explicitly flagged in the docs as high performance cost, worse in Forward Rendering and with shadows; uncapped `Maximum Lights` after an emission-rate tweak can silently spawn thousands of lights — always set a sane cap |
| Reading/writing `VisualEffect` particle data like a Shuriken system | VFX Graph has no per-particle C# read/write API; interact only through exposed properties (`SetFloat`/`SetVector3`/etc.) and events (`SendEvent`), never direct particle access |
| Forgetting `subEmitters.enabled = true` after calling `AddSubEmitter` | The module (like every Shuriken module) has its own `enabled` flag separate from having sub-emitter slots populated; a configured-but-disabled module does nothing |
| Assuming particle systems must pick either Shuriken or VFX Graph project-wide | The docs state platforms with compute shader support can use both simultaneously — the choice is per-effect, not a project-wide constraint |
| Ignoring GPU instancing for Mesh-based Shuriken particles in the Built-in RP | `particle-system-optimization.html` documents applying GPU instancing to render Mesh particles instead of billboards for a straightforward performance win, but it must be explicitly activated |
| Treating `rateOverDistance` and `rateOverTime` as interchangeable | `rateOverDistance` only emits as the emitter's transform moves (e.g. tire dust); a stationary object with only `rateOverDistance` configured will emit nothing — use `rateOverTime` for motion-independent continuous emission |
| Not setting `cycleCount`/`repeatInterval` correctly on Bursts | A default `Burst` fires once; repeated punctual effects (e.g. a chimney puffing every few seconds) need explicit `cycleCount` (or `-1` semantics per the scripting reference) and `repeatInterval`, not a `rateOverTime` workaround |
| Blaming VFX Graph GPU cost on the wrong subsystem | GPU cost surfaces as compute-shader simulation time plus translucent-particle fill-rate/overdraw; profiling with the CPU Profiler alone hides the real bottleneck — use GPU frame timing/Frame Debugger |

## Quick Reference

| Concept | Purpose |
|---------|---------|
| Main module | System-wide defaults: `duration`, `loop`, `startLifetime`, `startSpeed`, `startSize`, `simulationSpace`, `maxParticles` |
| Emission module | Controls spawn rate (`rateOverTime`, `rateOverDistance`) and bursts |
| Shape module | Defines the volume/surface particles emit from and initial velocity direction |
| Velocity over Lifetime | Adds velocity that changes across a particle's life, in local or world space |
| Limit Velocity over Lifetime | Clamps/dampens particle speed over lifetime (e.g. air drag) |
| Inherit Velocity | Particles inherit some or all of the emitter Transform's velocity |
| Force over Lifetime / External Forces | Applies scripted or scene wind-zone forces to particles over time |
| Color over Lifetime / Color by Speed | Animates color/alpha across a particle's normalized life or current speed |
| Size over Lifetime / Size by Speed | Animates uniform or per-axis size across life or speed |
| Rotation over Lifetime / Rotation by Speed | Animates particle rotation across life or speed |
| Collision module | Physical particle interaction with Colliders (World mode) or authored infinite planes (Planes mode, lower overhead) |
| Trigger module | Cheap enter/exit/inside/outside detection against collider regions via `OnParticleTrigger`, no physics response |
| Sub Emitters module | Spawns a secondary `ParticleSystem` on Birth/Collision/Death/Trigger/Manual events |
| Trails module | Adds per-particle trail ribbons; shares properties with the Trail Renderer component |
| Lights module | Attaches real-time lights to particles; high performance cost, guarded by `Maximum Lights` |
| Noise module | Perturbs particle motion with procedural turbulence |
| Texture Sheet Animation module | Plays a flipbook of frames from a texture sheet across a particle's life |
| `IJobParticleSystem` / `…ParallelFor` / `…ParallelForBatch` | Burst-compiled, worker-thread particle access; faster than main-thread `GetParticles()`/`SetParticles()` |
| `VisualEffectAsset` | The VFX Graph asset compiled into a GPU simulation (URP/HDRP only) |
| `VisualEffect` component | Runtime instance/controller for a `VisualEffectAsset` — `Play`/`Stop`/`SendEvent`/exposed-property setters |
| `VFXManager` | Project-wide VFX Graph settings (`fixedTimeStep`, `maxDeltaTime`) and batching diagnostics (`GetBatchedEffectInfos`) |
| GPU instancing (Built-in RP) | Renders Mesh particles instead of billboards for a rendering-side performance win |

## Advanced Notes

Mobile GPUs are bandwidth- and fill-rate-constrained far more than desktop/console GPUs, which makes particle-heavy effects one of the fastest ways to tank frame time on mobile even when CPU simulation cost looks cheap in the Profiler. Most particle materials are alpha-blended and unlit-additive, which means every overlapping particle quad is a full-screen-resolution shader invocation stacked on top of whatever's already been drawn — large, overlapping, screen-filling particle systems (explosions, smoke columns, weather) create severe overdraw where the same pixel is shaded many times per frame. On tile-based mobile GPUs this is punished especially hard because blended geometry can't use early-Z/hidden-surface removal the way opaque geometry can, so overdraw cost is close to linear in the number of overlapping translucent layers.

Practical mitigations, in rough order of impact: keep individual particle quads small relative to the screen and cap `maxParticles` aggressively for mobile-targeted systems, since a handful of huge, screen-filling particles cost far more fill-rate than many small ones covering the same visual area; prefer fewer, larger bursts of short-lived particles over sustained high `rateOverTime` emission so the number of simultaneously alive (and therefore simultaneously overdrawing) particles stays low; avoid stacking multiple semi-transparent particle systems in the same screen-space region (e.g. layering separate smoke, embers, and glow systems on one explosion) — collapse layers into fewer systems or a single custom shader where possible; disable or heavily cap the Lights module on mobile, since real-time per-particle lights are explicitly documented as high-cost, worse in Forward Rendering, and worst with shadows enabled, all of which are common mobile constraints already; use the World/Planes Collision-mode distinction to avoid physics-query overhead on lower-end CPUs, defaulting to Planes wherever the interaction is against static level geometry; and use the Texture Sheet Animation module or lower-resolution/simpler particle textures rather than driving visual complexity purely through overlapping particle count. For VFX Graph specifically, remember it requires compute shader support — verify target mobile hardware/API actually supports it before committing an effect to it, since a compute-shader-driven graph that silently fails to render on a subset of Android/iOS devices is a common, hard-to-catch regression. Regardless of which system you use, validate overdraw hotspots empirically with the Rendering Debugger's overdraw view (or a platform-specific GPU profiler/frame capture on device) rather than assuming from particle count alone — actual fill-rate cost depends on final on-screen size and blend-layer depth, not raw particle count.

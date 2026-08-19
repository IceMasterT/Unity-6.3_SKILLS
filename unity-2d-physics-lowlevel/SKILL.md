---
name: unity-2d-physics-lowlevel
description: Use when working with Unity's new low-level 2D physics API (Box2D-based rewrite) directly, as distinct from the classic Rigidbody2D/Collider2D component workflow. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Low-Level 2D Physics

## Retrieval Sources

This system lives in the `UnityEngine.LowLevelPhysics2D` namespace (1581 ScriptReference pages locally) and, unlike many newer/preview-ish Unity APIs, it has a genuinely substantial Manual tree explaining it: `Manual/2d-physics-api/` contains 34 real explanatory pages (confirmed on disk, not just a bare ScriptReference dump), each carrying working code samples. So this skill is NOT purely signature-only — there is real conceptual documentation, and it is cited below alongside the exact ScriptReference member pages that were verified to exist. Every page in this table was confirmed present on disk before being cited (via `ls`, `find`, or direct `Read`). The official Unity example project referenced repeatedly across the Manual pages is `github.com/Unity-Technologies/PhysicsExamples2D` (an external resource, not verified locally, but named verbatim in the docs).

| Source | Path | Use for |
|--------|------|---------|
| Namespace landing & TOC | `Manual/2d-physics-api/2d-physics-api-landing.html` | Entry point into the whole low-level 2D physics Manual tree; explicitly states this API does not use the Rigidbody 2D/Collider 2D component workflow |
| Introduction & rationale | `Manual/2d-physics-api/2d-physics-api-introduction.html` | Box2D v3 origin, no-GameObject-required model, up to 64 CPU cores, 64 collision layers, struct-based/DOTS-friendly return types, compute-shader platform requirement |
| Get started / workflow | `Manual/2d-physics-api/2d-physics-api-get-started-landing.html`, `Manual/2d-physics-api/2d-physics-api-workflow.html` | The canonical 5-step "create world → create body → create shape → configure via definitions → attach a script" workflow, with a full runnable example |
| World creation & concept | `Manual/2d-physics-api/2d-physics-api-world.html`, `ScriptReference/LowLevelPhysics2D.PhysicsWorld.html`, `ScriptReference/LowLevelPhysics2D.PhysicsWorldDefinition.html` | `PhysicsWorld.defaultWorld`, `PhysicsWorld.Create(definition)`, default gravity (-9.81 Y), pausing a world |
| World reference (Inspector-facing properties) | `Manual/2d-physics-api/2d-physics-api-reference-world.html` | Full property table for a world definition: simulation type/sub-steps/workers, transform write mode/plane/tweening, sleeping, continuous collision, contact tuning, draw options |
| Multithreading model | `Manual/2d-physics-api/2d-physics-api-multithreading.html` | "Write Once, Read Many" (WORM) per-world locking; which calls are and aren't thread-/Job-safe; `simulationWorkers` tuning |
| Global low-level settings | `Manual/2d-physics-api/2d-physics-api-global-settings.html`, `Manual/class-PhysicsLowLevelSettings2D.html`, `ScriptReference/LowLevelPhysics2D.PhysicsLowLevelSettings2D.html` | The `Physics Low Level Settings 2D` asset (Assets > Create > 2D), wired into Project Settings > Physics 2D > Low Level tab; default definitions, 64-layer names, concurrent simulations, `bypassLowLevel` kill switch |
| Classic 2D Physics settings (cross-link only) | `Manual/class-Physics2DSettings.html` | The ordinary Physics 2D project settings page; relevant here only for its single "Low Level" tab field that assigns the settings asset above — everything else on this page is `unity-physics` territory |
| Physics object concept | `Manual/2d-physics-api/2d-physics-api-physics-object.html`, `Manual/2d-physics-api/2d-physics-api-create-objects-landing.html` | Body vs shape distinction: a body alone has no area, a shape gives it collidable geometry; multiple shapes per body |
| Definition-object pattern | `Manual/2d-physics-api/2d-physics-api-definitions.html` | The struct-fill-then-`Create` pattern used everywhere in this API, and exposing a definition as a public field for Inspector-style configuration |
| Body reference | `Manual/2d-physics-api/2d-physics-api-reference-body.html`, `ScriptReference/LowLevelPhysics2D.PhysicsBody.html`, `ScriptReference/LowLevelPhysics2D.PhysicsBodyDefinition.html` | Full body property table: type, constraints, damping, sleep, mass configuration, transform bridge |
| Body type & constraints enums | `ScriptReference/LowLevelPhysics2D.PhysicsBody.BodyType.html`, `ScriptReference/LowLevelPhysics2D.PhysicsBody.BodyConstraints.html` | Verified enum members and per-member semantics (`Dynamic`/`Kinematic`/`Static`; `None`/`PositionX`/`PositionY`/`Rotation`/`Position`/`All`) |
| Shape reference | `Manual/2d-physics-api/2d-physics-api-reference-shape.html`, `ScriptReference/LowLevelPhysics2D.PhysicsShape.html`, `ScriptReference/LowLevelPhysics2D.PhysicsShapeDefinition.html` | Shape property table: surface material, contact filter, trigger flag, density, mass |
| Properties landing | `Manual/2d-physics-api/2d-physics-api-properties-landing.html` | Index page linking body/shape/world property references |
| Geometry types | `ScriptReference/LowLevelPhysics2D.CircleGeometry.html`, `ScriptReference/LowLevelPhysics2D.CapsuleGeometry.html`, `ScriptReference/LowLevelPhysics2D.PolygonGeometry.html`, `ScriptReference/LowLevelPhysics2D.SegmentGeometry.html` | The four shape-geometry structs passed into `PhysicsBody.CreateShape`; each exposes `CastRay`/`CastShape`/`ClosestPoint`/`OverlapPoint`/`CalculateAABB`/`Intersect` |
| Chain geometry types | `ScriptReference/LowLevelPhysics2D.ChainGeometry.html`, `ScriptReference/LowLevelPhysics2D.ChainSegmentGeometry.html` | Edge-loop/edge-strip geometry consumed by `PhysicsBody.CreateChain`, distinct from the four regular shape geometries |
| GameObject/sprite bridge | `Manual/2d-physics-api/2d-physics-api-add-sprite.html`, `Manual/2d-physics-api/2d-physics-api-move-gameobject.html` | How to visually represent a physics body with a SpriteRenderer, and how `PhysicsBody.transformObject`/`transformWriteMode` writes simulation results back onto a real Transform |
| Custom user data | `Manual/2d-physics-api/2d-physics-api-custom-data.html`, `ScriptReference/LowLevelPhysics2D.PhysicsUserData.html` | Attaching arbitrary payload (`bool`/`float`/`int`/`int64`/managed object/`PhysicsMask`) to bodies/shapes/joints/chains via `userData`/`ownerUserData` |
| Joints overview | `Manual/2d-physics-api/2d-physics-api-joints.html`, `ScriptReference/LowLevelPhysics2D.PhysicsJoint.html` | The definition-struct → `world.CreateJoint(definition)` pattern shared by every joint type; common fields (thresholds, tuning, `collideConnected`) |
| Distance joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-distance.html`, `ScriptReference/LowLevelPhysics2D.PhysicsDistanceJointDefinition.html` | Keeps two bodies a target `distance` apart, with optional spring/motor/limit (motor and limit both spring-gated) |
| Fixed joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-fixed.html`, `ScriptReference/LowLevelPhysics2D.PhysicsFixedJointDefinition.html` | Spring-welds two bodies to the same position and rotation (not a rigid weld — has `linearFrequency`/`angularFrequency`) |
| Hinge joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-hinge.html`, `ScriptReference/LowLevelPhysics2D.PhysicsHingeJointDefinition.html` | Coincident-pivot rotation constraint with spring-to-angle, motor (speed+torque), and angle limits |
| Slider joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-slider.html`, `ScriptReference/LowLevelPhysics2D.PhysicsSliderJointDefinition.html` | Restricts a body to slide along an axis set by the other body's rotation; spring/motor/translation limits |
| Wheel joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-wheel.html`, `ScriptReference/LowLevelPhysics2D.PhysicsWheelJointDefinition.html` | Suspension-style joint combining translation along an axis with free rotation; spring/motor/translation limits |
| Relative joint | `Manual/2d-physics-api/2d-physics-api-reference-joint-relative.html`, `ScriptReference/LowLevelPhysics2D.PhysicsRelativeJointDefinition.html` | Drives a target relative linear/angular *velocity* between two bodies rather than a positional constraint |
| Ignore joint | `ScriptReference/LowLevelPhysics2D.PhysicsIgnoreJointDefinition.html`, `ScriptReference/LowLevelPhysics2D.PhysicsIgnoreJoint.html` | No dedicated Manual page found for this one (ScriptReference-only); links only `bodyA`/`bodyB` to suppress collision between them with no other constraint — treat as signature-verified, not conceptually documented |
| Interactions concept | `Manual/2d-physics-api/2d-physics-api-interactions-landing.html`, `Manual/2d-physics-api/2d-physics-api-interactions-introduction.html` | Collider-vs-trigger shape reaction modes; collisions enabled by default |
| Layers & filtering | `Manual/2d-physics-api/2d-physics-api-collisions-enable.html`, `ScriptReference/LowLevelPhysics2D.PhysicsMask.html`, `ScriptReference/LowLevelPhysics2D.PhysicsLayers.html` | `PhysicsLayers.GetLayerMask`, `PhysicsMask.All`/`None`, the opt-in 64-layer system vs. the default 32 GameObject layers, `ContactFilter.categories`/`contacts` |
| Collision/trigger callback access | `Manual/2d-physics-api/2d-physics-api-collision-handle.html`, `ScriptReference/LowLevelPhysics2D.PhysicsCallbacks.html`, `ScriptReference/LowLevelPhysics2D.PhysicsEvents.html` | The two access patterns: `IContactCallback`/`ITriggerCallback` interfaces vs. polling `PhysicsWorld.contactBeginEvents`/`triggerBeginEvents` spans |
| Shape composition (CSG) | `Manual/2d-physics-api/2d-physics-api-connect-combine-shapes.html`, `ScriptReference/LowLevelPhysics2D.PhysicsComposer.html` | `PhysicsComposer.Create`/`AddLayer`/`CreatePolygonGeometry` with `Operation.OR/AND/NOT/XOR`, requires `Unity.Collections` allocator management |
| Chain object reference | `Manual/2d-physics-api/2d-physics-api-reference-chain.html`, `ScriptReference/LowLevelPhysics2D.PhysicsChain.html`, `ScriptReference/LowLevelPhysics2D.PhysicsChainDefinition.html` | Edge-loop collider (tilemap-boundary equivalent): surface material, contact filter with `groupIndex` override, `isLoop` |
| Queries: raycasts & casts | `Manual/2d-physics-api/2d-physics-api-raycasting.html`, `ScriptReference/LowLevelPhysics2D.PhysicsQuery.html` | `PhysicsWorld.CastRay`/`CastGeometry`/`OverlapCircle`/`TestOverlapAABB`; `PhysicsQuery` static shape-pair test methods; `QueryFilter`, `WorldCastMode` |
| Destruction & memory | `Manual/2d-physics-api/2d-physics-api-destroy.html` | Cascading `Destroy()` semantics, `isValid` guard, batch destroy, `SetOwner` deletion-protection key, mandatory disposal of `NativeArray`/`ReadOnlySpan` query results |
| Debug visualization | `Manual/2d-physics-api/2d-physics-api-debug-drawing.html` | Automatic in-editor drawing, `DrawOptions`/`DrawColors` config, manual immediate-mode `world.DrawLine`/`DrawCircle`/`DrawPoint`/`DrawGeometry`, `Draw In Build` + compute-shader requirement |
| Using 2D physics on a 3D plane | `Manual/2d-physics-api/2d-physics-api-3d-planes.html` | `PhysicsWorld.TransformPlane` (`XY`/`XZ`/`ZY`) remaps 2D simulation math onto an arbitrary plane in 3D space for rendering |
| Reference index | `Manual/2d-physics-api/2d-physics-api-reference.html` | Landing/TOC page linking the 10 per-object reference pages above; no additional API surface |
| Destructible geometry | `ScriptReference/LowLevelPhysics2D.PhysicsDestructor.html` | `Slice`/`Fragment` static utilities for runtime-breaking geometry into pieces (ScriptReference-verified; no dedicated Manual conceptual page found under `2d-physics-api/`) |
| Math/transform utility types | `ScriptReference/LowLevelPhysics2D.PhysicsTransform.html`, `ScriptReference/LowLevelPhysics2D.PhysicsRotate.html`, `ScriptReference/LowLevelPhysics2D.PhysicsMath.html`, `ScriptReference/LowLevelPhysics2D.PhysicsPlane.html`, `ScriptReference/LowLevelPhysics2D.PhysicsAABB.html` | Position+rotation composition, Vector2⇄Vector3 conversion helpers, AABB math — all struct-based, no dedicated Manual prose beyond their use inside other pages |
| Constants | `ScriptReference/LowLevelPhysics2D.PhysicsConstants.html` | `MaxPolygonVertices`, `MaxWorkers`, `MaxWorlds` hard limits |
| Origin & version context | `Manual/WhatsNewUnity63.html` (search "LowLevelPhysics2D") | Confirms this is a Unity 6.3 addition, a Box2D v3 integration, explicitly framed as an alternative to Rigidbody2D/Collider2D for developers who want direct object management or custom components |

## Key Guidelines

### Physics World Management

A `PhysicsWorld` is the simulation container — nothing exists in this API outside of one. `PhysicsWorld.defaultWorld` is a ready-made world Unity auto-creates (and recreates on Editor start, Play mode enter/exit, and application start), and is the fastest way to get moving; for anything with custom settings, fill a `PhysicsWorldDefinition` struct and call `PhysicsWorld.Create(definition)`. The definition's most important field is `gravity` (`Vector2`, defaults to `(0, -9.81)`), but it also carries `simulationType` (see Simulation Stepping below), `simulationSubSteps` (default 4), `simulationWorkers`, `transformWriteMode`/`transformPlane`/`transformTweening` (how/whether simulation results get written back to real Transforms — see the GameObject-bridge subsection), `sleepingAllowed`, `continuousAllowed`, and a large block of `draw*` fields controlling the automatic Scene/Game-view debug visualization. Unlike `Physics2D`, this is not a single global static system — you can have multiple independent `PhysicsWorld` instances (up to `PhysicsConstants.MaxWorlds`) simulated concurrently on separate threads, each fully isolated from the others (`Manual/2d-physics-api/2d-physics-api-multithreading.html`). A world can be paused via `paused = true`, and every object created in it must be explicitly destroyed or is cascade-destroyed when the world itself is destroyed (see Debug Visualization & Object Lifetime).

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class CustomWorldExample : MonoBehaviour
{
    private PhysicsWorld world;

    void Awake()
    {
        PhysicsWorldDefinition worldDefinition = new PhysicsWorldDefinition
        {
            gravity = new Vector2(0f, -20f),      // heavier gravity than the -9.81 default
            simulationType = PhysicsWorld.SimulationType.FixedUpdate,
            simulationSubSteps = 4
        };
        world = PhysicsWorld.Create(worldDefinition);
    }

    void OnDestroy() => world.Destroy(); // worlds are never auto-cleaned up except the default world
}
```

### Body Creation & Configuration

A `PhysicsBody` holds position, rotation, and velocity but has no shape/area of its own — you attach one or more `PhysicsShape`s to it to give it collidable geometry (`Manual/2d-physics-api/2d-physics-api-physics-object.html`). Bodies are created from a `PhysicsBodyDefinition` via `world.CreateBody(definition)` (or `world.CreateBody()` for an all-defaults body). The `type` field is `PhysicsBody.BodyType`, a 3-value enum with real solver semantics, not just an on/off kinematic flag: `Dynamic` (positive mass, velocity driven by forces, moved by the solver), `Kinematic` (zero mass, velocity set directly by you, still moved by the solver — collides with and pushes Dynamic bodies), and `Static` (zero mass, zero velocity, may only be moved manually) — verified verbatim from `ScriptReference/LowLevelPhysics2D.PhysicsBody.BodyType.html`. `BodyConstraints` (`None`/`PositionX`/`PositionY`/`Rotation`/`Position`/`All`) is the low-level equivalent of Rigidbody's freeze-position/rotation checkboxes. Mass (`massConfiguration`: `mass`, `center`, `rotationalInertia`) is normally recalculated automatically every time a shape is added/removed/modified; for bulk shape addition, set `PhysicsShapeDefinition.startMassUpdate = false` per shape while adding many, then call `body.ApplyMassFromShapes()` once at the end to avoid redundant recalculation.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class DynamicBodyExample : MonoBehaviour
{
    void Start()
    {
        PhysicsWorld world = PhysicsWorld.defaultWorld;

        PhysicsBody body = world.CreateBody(new PhysicsBodyDefinition
        {
            position = new Vector2(0f, 8f),
            type = PhysicsBody.BodyType.Dynamic,
            constraints = PhysicsBody.BodyConstraints.Rotation, // free to move, can't spin
            linearDamping = 0.05f
        });

        body.CreateShape(new CircleGeometry { radius = 0.5f });
    }
}
```

### Shape Definition & Surface Materials

`PhysicsShape` is the collidable-area component, attached to a body via one of `PhysicsBody.CreateShape`'s overloads — one per geometry type: `CircleGeometry`, `CapsuleGeometry`, `PolygonGeometry`, `SegmentGeometry` (verified declarations, e.g. `public PhysicsShape CreateShape(CircleGeometry geometry)` and the overload taking an additional `PhysicsShapeDefinition`). A `PhysicsShapeDefinition` configures `isTrigger`, `density` (affects mass, not size), `contactFilter` (layer/category filtering — see Collision & Trigger Interactions), and `surfaceMaterial`. `PhysicsShape.SurfaceMaterial` bundles `friction`, `bounciness`, `rollingResistance`, `tangentSpeed` (conveyor-belt-style surface motion), plus a `MixingMode` enum (`Average`/`Mean`/`Multiply`/`Minimum`/`Maximum`) and a `*Priority` field per property so that when two touching shapes disagree on friction/bounciness mixing mode, the higher-priority (and on ties, higher-enum-value) shape's mode wins. Density can be read/written after creation via `GetDensity()`/`SetDensity(density, updateBodyMass)` — the `updateBodyMass` flag lets you skip the mass recalculation for speed when setting many shapes' densities in a row.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class ShapeSurfaceExample : MonoBehaviour
{
    void Start()
    {
        PhysicsWorld world = PhysicsWorld.defaultWorld;
        PhysicsBody body = world.CreateBody(new PhysicsBodyDefinition { type = PhysicsBody.BodyType.Dynamic });

        PhysicsShapeDefinition bouncyDefinition = new PhysicsShapeDefinition
        {
            density = 2f,
            surfaceMaterial = new PhysicsShape.SurfaceMaterial { friction = 0.1f, bounciness = 0.9f }
        };
        body.CreateShape(new CircleGeometry { radius = 0.5f }, bouncyDefinition);
    }
}
```

### Joints

Every joint follows the same pattern as bodies/shapes: fill a `PhysicsXxxJointDefinition` struct (set `bodyA`/`bodyB` plus joint-specific fields), then call `world.CreateJoint(definition)`, which returns the matching `PhysicsXxxJoint` (each overload of `PhysicsWorld.CreateJoint` is strongly typed per definition — verified via `ScriptReference/LowLevelPhysics2D.PhysicsWorld.CreateJoint.html`). Six constraint kinds exist, each with its own semantics: **Distance** keeps two bodies a target `distance` apart (with optional spring/motor/limit, all spring-gated); **Fixed** spring-welds position and rotation together (not perfectly rigid — has `linearFrequency`/`angularFrequency`); **Hinge** pins a shared pivot point so the second body rotates freely about it (spring-to-angle, motor with speed+torque, angle limits); **Slider** restricts the second body to translate along an axis fixed to the first body's rotation; **Wheel** combines Slider-style translation (suspension) with free rotation — the vehicle-suspension joint; **Relative** is different from the rest — it drives a target relative linear/angular *velocity* rather than a positional constraint. `PhysicsIgnoreJoint` is a degenerate case: it links `bodyA`/`bodyB` purely to suppress collision between them, with no positional or velocity constraint at all. Fields common to every joint definition: `forceThreshold`/`torqueThreshold` (fire `OnJointThreshold2D` when exceeded), `tuningFrequency`/`tuningDamping` (overall joint stiffness), `collideConnected` (whether the two connected bodies can still collide with each other), and `drawScale` for debug visualization. Destroying either connected body automatically destroys the joint.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class HingeJointExample : MonoBehaviour
{
    void Start()
    {
        PhysicsWorld world = PhysicsWorld.defaultWorld;

        PhysicsBody anchor = world.CreateBody(new PhysicsBodyDefinition { type = PhysicsBody.BodyType.Static });
        PhysicsBody door = world.CreateBody(new PhysicsBodyDefinition
        {
            position = new Vector2(1f, 0f),
            type = PhysicsBody.BodyType.Dynamic
        });
        door.CreateShape(PolygonGeometry.CreateBox(new Vector2(2f, 0.2f)));

        PhysicsHingeJointDefinition hingeDefinition = new PhysicsHingeJointDefinition
        {
            bodyA = anchor,
            bodyB = door,
            enableMotor = true,
            motorSpeed = 90f,
            maxMotorTorque = 50f,
            enableLimit = true,
            lowerAngleLimit = 0f,
            upperAngleLimit = 90f
        };
        PhysicsJoint hinge = world.CreateJoint(hingeDefinition);
    }
}
```

### Collision & Trigger Interactions

Each `PhysicsShape` behaves as either a solid collider (default) or a trigger, controlled by `PhysicsShapeDefinition.isTrigger`. Filtering happens through `PhysicsShape.ContactFilter { categories, contacts, groupIndex }` assigned into the shape definition — this API defaults to the standard 32 GameObject layers, but can opt into its own 64-layer system by enabling "Use Full Layers" on the `PhysicsLowLevelSettings2D` asset; `PhysicsLayers.GetLayerMask("Name")` resolves a `PhysicsMask` correctly under either mode, and `PhysicsMask.All`/`PhysicsMask.None` are shortcuts. There are two distinct, verified ways to observe collisions/triggers: (1) implement `PhysicsCallbacks.IContactCallback` (`OnContactBegin2D`/`OnContactEnd2D`) or `PhysicsCallbacks.ITriggerCallback` (`OnTriggerBegin2D`/`OnTriggerEnd2D`) on any object, then wire it up — this requires the world's `autoContactCallbacks`/`autoTriggerCallbacks` to be enabled, the shape's `contactEvents`/`triggerEvents` set `true`, and the shape's `callbackTarget` pointed at your callback object; or (2) poll `PhysicsWorld.contactBeginEvents`/`triggerBeginEvents` (and the matching `*EndEvents`), which return `ReadOnlySpan`s directly over engine memory — faster but explicitly documented as less safe to hold onto past the current frame. Fast-moving dynamic-vs-dynamic tunneling isn't prevented automatically (continuous collision is automatic only for dynamic-vs-static); opt a fast mover in with `PhysicsBody.fastCollisionsAllowed = true`.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class ContactListener : MonoBehaviour, PhysicsCallbacks.IContactCallback
{
    void Start()
    {
        PhysicsWorld world = PhysicsWorld.defaultWorld;
        world.autoContactCallbacks = true; // must be enabled on the world

        PhysicsBody body = world.CreateBody(new PhysicsBodyDefinition { type = PhysicsBody.BodyType.Dynamic });
        PhysicsShape shape = body.CreateShape(new CircleGeometry { radius = 0.5f });
        shape.contactEvents = true;
        shape.callbackTarget = this; // must implement IContactCallback
    }

    public void OnContactBegin2D(PhysicsEvents.ContactBeginEvent contactEvent)
    {
        Debug.Log($"Contact started between {contactEvent.contactId.contact.shapeA} and {contactEvent.contactId.contact.shapeB}");
    }

    public void OnContactEnd2D(PhysicsEvents.ContactEndEvent contactEvent) { }
}
```

### Queries (Raycasts, Overlaps & Casts)

Query entry points live as instance methods on `PhysicsWorld` — `CastRay` (a bounded line segment, not an infinite ray), `CastGeometry`/`CastShape` (sweep a shape through the world), `OverlapPoint`/`OverlapAABB`/`OverlapShape` (static-volume tests), plus boolean-only `TestOverlap*` variants for a cheaper yes/no answer. All are declared thread-safe, unlike object creation/destruction. Every cast/overlap call takes a `PhysicsQuery.QueryFilter` (layer/category filtering, mirroring `ContactFilter`) and an explicit `Unity.Collections.Allocator` (`Temp`, `TempJob`, or `Persistent` only), and returns a `NativeArray<T>` of results (`WorldCastResult` for casts, `WorldOverlapResult` for overlaps) that **must be disposed** — the docs explicitly call out leaks otherwise (the sole exception being an empty result array). `PhysicsQuery.WorldCastMode` controls how many/which order results come back (`Closest` confirmed; `All`/`AllSorted` also present per the enum's ScriptReference page). Separately, `PhysicsQuery` also exposes narrow-phase, static, shape-pair-vs-shape-pair test methods (e.g. `CircleAndCircle`, `PolygonAndCapsule`, `SegmentAndPolygon`) and each geometry struct (`CircleGeometry`, `CapsuleGeometry`, etc.) carries its own `Intersect`/`CastRay`/`CastShape`/`ClosestPoint`/`OverlapPoint` instance methods for lower-level geometry math without going through a world at all.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;
using Unity.Collections;

public class RaycastExample : MonoBehaviour
{
    void CheckGround(Vector2 origin)
    {
        PhysicsWorld world = PhysicsWorld.defaultWorld;
        var input = new PhysicsQuery.CastRayInput
        {
            origin = origin,
            translation = Vector2.down * 2f
        };
        var filter = new PhysicsQuery.QueryFilter();

        using NativeArray<PhysicsQuery.WorldCastResult> results =
            world.CastRay(input, filter, PhysicsQuery.WorldCastMode.Closest, Allocator.Temp);

        if (results.Length > 0)
            Debug.Log($"Hit {results[0].shape} at {results[0].point}");
        // 'using' disposes the NativeArray automatically at scope exit
    }
}
```

### Simulation Stepping & Multithreading

`PhysicsWorldDefinition.simulationType` (a `PhysicsWorld.SimulationType` enum) picks when a world advances: `FixedUpdate` (default, mirrors classic physics timing), `Update` (steps once per rendered frame instead), or `Script` — in `Script` mode the world does nothing until you explicitly call `world.Simulate(deltaTime)`, which the docs state only works when `simulationType == Script`; there's also a static batch overload, `PhysicsWorld.Simulate(ReadOnlySpan<PhysicsWorld> worlds, deltaTime)`, that can advance several worlds concurrently depending on `PhysicsLowLevelSettings2D.concurrentSimulations`. Internally the solver can use up to `simulationWorkers` threads (capped by `PhysicsConstants.MaxWorkers`) per world, using a "Write Once, Read Many" (WORM) lock: any number of threads can read a world simultaneously, but only one can write, and separate worlds are fully independent so each can be written from a different thread concurrently. Crucially, not everything is safe to call from a job or worker thread — `Create`, `CreateBatch`, `Destroy`, and `DestroyBatch` (on bodies, shapes, joints, chains) are explicitly documented as not thread-safe; only reads and the per-instance simulation/query methods are.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;

public class ManualStepExample : MonoBehaviour
{
    private PhysicsWorld world;

    void Awake()
    {
        world = PhysicsWorld.Create(new PhysicsWorldDefinition
        {
            simulationType = PhysicsWorld.SimulationType.Script // required for manual Simulate() to have any effect
        });
    }

    void FixedUpdate()
    {
        world.Simulate(Time.fixedDeltaTime); // deterministic, lockstep-friendly stepping (e.g. for networked physics/replays)
    }
}
```

### Chains, Shape Composition & Destructible Geometry

`PhysicsChain` (created via `body.CreateChain(ChainGeometry, PhysicsChainDefinition)`) is the low-level equivalent of an edge-loop collider — a strip or closed loop of line segments, the natural fit for tilemap boundaries or terrain outlines, configured with the same `surfaceMaterial`/`contactFilter` shape as regular shapes plus `isLoop` to close the strip into a loop, and a `groupIndex` override on its contact filter that always/never forces collision with same-group shapes regardless of category/mask (mirroring classic Physics2D's collision-group override). `PhysicsComposer` performs CSG-style boolean composition of geometry — `AddLayer` stacks `CircleGeometry`/`CapsuleGeometry`/`PolygonGeometry`/`PhysicsShape`/raw point loops together with an `Operation` (`OR`/`AND`/`NOT`/`XOR`), then `CreatePolygonGeometry`/`CreateChainGeometry` bakes the result into geometry you can feed straight into `CreateShapeBatch`/`CreateChain`. Composer work happens through `Unity.Collections` (`NativeArray`/`NativeList` with an explicit `Allocator`), so it is Burst/job-adjacent, unmanaged-memory territory rather than ordinary GC'd C#. `PhysicsDestructor` provides `Slice` and `Fragment` static utilities for runtime geometry breaking (e.g. shattering a shape into pieces along a cut line or fracture pattern) — confirmed to exist in ScriptReference, but no dedicated Manual conceptual page was found for it under `2d-physics-api/`, so treat its exact intended workflow as signature-level only.

```csharp
using UnityEngine;
using UnityEngine.LowLevelPhysics2D;
using Unity.Collections;

public class ComposedShapeExample : MonoBehaviour
{
    void Start()
    {
        PhysicsComposer composer = PhysicsComposer.Create(Allocator.Temp);
        composer.AddLayer(new CircleGeometry { radius = 1f }, PhysicsTransform.identity, PhysicsComposer.Operation.OR);
        composer.AddLayer(new CircleGeometry { radius = 0.6f, center = new Vector2(0.8f, 0f) }, PhysicsTransform.identity, PhysicsComposer.Operation.OR);

        using NativeArray<PolygonGeometry> combined = composer.CreatePolygonGeometry(Vector2.one, Allocator.Temp);

        PhysicsBody body = PhysicsWorld.defaultWorld.CreateBody(new PhysicsBodyDefinition { type = PhysicsBody.BodyType.Dynamic });
        body.CreateShapeBatch(combined, PhysicsShapeDefinition.defaultDefinition);

        composer.Destroy();
    }
}
```

### Debug Visualization & Object Lifetime Management

Every shape created through this API is drawn automatically in the Scene view, Game view, and Play mode with no opt-in required in-editor — controlled by `Draw*`-prefixed fields on `PhysicsWorldDefinition` (`drawOptions` picks which categories draw — bodies, shapes, joints, contacts, solver islands; `drawColors` sets per-category colors) and a per-shape `customColor` override on `PhysicsShapeDefinition`. To see this in an actual Player build (not just the Editor), the target platform must support compute shaders and the `PhysicsLowLevelSettings2D` asset must have "Draw In Build" enabled. Beyond the automatic draw, `PhysicsWorld` exposes immediate-mode manual draw calls — `DrawLine`, `DrawCircle`, `DrawPoint`, `DrawGeometry` — each taking an explicit `Color` and, optionally, a `lifeTime` (default one frame). On lifetime: every major object type (`PhysicsWorld`, `PhysicsBody`, `PhysicsShape`, `PhysicsJoint`, `PhysicsChain`) has an instance `Destroy()`, and destruction cascades — destroying a body destroys its attached shapes/chains/joints, destroying a world destroys everything in it. Calling `Destroy()` on an already-destroyed object logs a console error, so check `isValid` first when destruction order isn't guaranteed. Batch variants (`CreateBodyBatch`/`DestroyBodyBatch`, `CreateShapeBatch`/`DestroyShapeBatch`, `DestroyJointBatch`) exist for bulk work. `SetOwner()` returns a unique integer key that subsequent `Destroy()` calls must supply to succeed — documented explicitly as "a deterrent, not cryptographically secure" ownership protection, not a real security boundary.

### Relationship to Classic Rigidbody2D/GameObject Workflow

This API and the classic `Rigidbody2D`/`Collider2D` component system are completely separate simulations that never interact — a `PhysicsBody` never collides with a `Rigidbody2D`, and there is no shared collision matrix (`Manual/2d-physics-api/2d-physics-api-introduction.html`: "The API doesn't interact with or affect the built-in Unity 2D physics components"). There are no built-in Inspector components in this system at all — every object is created directly from code, though a `PhysicsBodyDefinition`/`PhysicsShapeDefinition` field exposed as `public` on a `MonoBehaviour` does get an Inspector-editable struct view, giving back some of the classic component-editing convenience. Physics objects are not automatically tied to any GameObject or Transform — the sole bridge is `PhysicsBody.transformObject` (a `Transform` reference) combined with `PhysicsBody.transformWriteMode` (`Off`/`Current`/`Interpolate`/`Extrapolate`) *and* the world's own `transformWriteMode` (`Off`/`Fast2D`/`Slow3D`) — both must be non-`Off` for simulation results to actually be written onto a real Transform each step; `Fast2D` is cheaper but forces rotation onto a single axis (zeroing any other-axis/3D rotation), while `Slow3D` writes a full 3D rotation. A `PhysicsWorld.TransformPlane` (`XY`/`XZ`/`ZY`) setting controls which plane that 2D simulation gets remapped onto in 3D space — letting a Box2D-only, `Vector2`-internal simulation drive objects lying flat on the ground (`XZ`) or standing upright side-on (`ZY`), not just facing the camera. Because objects need no GameObject at all, this API is equally usable for fully headless/off-screen simulation (e.g. background particle-like physics, AI spatial reasoning) with nothing ever rendered.

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Assuming a `PhysicsBody`/`PhysicsShape` will collide with a scene's `Rigidbody2D`/`Collider2D` objects | The two systems are entirely separate simulations with no shared collision matrix — verified explicitly in the introduction Manual page; nothing bridges them except identical (but independent) world-space coordinates |
| Creating a `PhysicsBody` and expecting it to have collision geometry | A body alone has no shape/area; you must call `body.CreateShape(geometry)` (or `CreateChain`) at least once before it can collide with anything |
| Leaking `NativeArray`/`ReadOnlySpan` query results | Every `CastRay`/`CastGeometry`/`OverlapPoint`/etc. call returns a `NativeArray` that must be disposed (`using` or `.Dispose()`) except when empty; the Manual explicitly warns leaks occur otherwise |
| Calling `Create`/`CreateBatch`/`Destroy`/`DestroyBatch` from a Job or worker thread | These are explicitly documented as not thread-safe, unlike most of the rest of the API; only run object creation/destruction on the main thread |
| Implementing `IContactCallback`/`ITriggerCallback` but callbacks never fire | Requires all of: the world's `autoContactCallbacks`/`autoTriggerCallbacks` enabled, the shape's `contactEvents`/`triggerEvents` set `true`, and the shape's `callbackTarget` assigned — missing any one silently disables the callback |
| Expecting `ITriggerCallback` to fire on a shape with `isTrigger = false` | Trigger events only fire for shapes explicitly marked `PhysicsShapeDefinition.isTrigger = true`; a solid collider only ever raises contact events |
| Expecting a `PhysicsBody`'s position to visually move a GameObject automatically | It won't unless `PhysicsBody.transformObject` is assigned to a real `Transform` *and* both `PhysicsBody.transformWriteMode` and `PhysicsWorld.transformWriteMode` are set to something other than `Off` |
| Calling `world.Simulate(deltaTime)` and nothing happens | `Simulate` only has an effect when the world's `simulationType` is `PhysicsWorld.SimulationType.Script`; under `FixedUpdate`/`Update` the world already steps itself automatically and manual calls are ignored |
| Calling `Destroy()` twice on the same object (e.g. destroying a body, then separately destroying one of its now-cascaded-away shapes) | Logs a console error rather than crashing; guard destruction order-sensitive code with `if (obj.isValid) obj.Destroy();` |
| Confusing `density` with shape size | `density` only feeds mass calculation (`mass = density × area`), not geometry — shape size is entirely controlled by the geometry struct (`radius`, `vertices`, etc.) |
| Assuming the standard `LayerMask`/32-layer collision matrix applies here | This API defaults to the same 32 GameObject layers but can opt into an independent 64-layer system (`PhysicsMask`) via "Use Full Layers" on the `PhysicsLowLevelSettings2D` asset — the two layer systems and their masks are not interchangeable types |
| Reaching for this API to build a normal small-scene platformer/top-down game | It trades away Inspector components, automatic GameObject sync, and Rigidbody2D-workflow familiarity for raw performance and manual struct/lifetime plumbing; for a few dozen ordinary dynamic objects the classic `Rigidbody2D` workflow (see `unity-physics`) is simpler and just as fast in practice |
| Using `PhysicsComposer` without importing `Unity.Collections` or without an explicit `Allocator` | Composer results are `NativeArray`/`NativeList`-based, unmanaged-memory constructs, not ordinary C# collections — they need the same allocator/disposal discipline as query results |
| Assuming fast-moving dynamic-vs-dynamic bodies never tunnel through each other by default | Continuous collision detection is automatic only for dynamic-vs-static pairs; enable `PhysicsBody.fastCollisionsAllowed = true` on fast movers to also get CCD against other dynamic bodies |

## Quick Reference

| Item | Purpose |
|------|---------|
| `PhysicsWorld` / `PhysicsWorldDefinition` | The simulation container; `PhysicsWorld.defaultWorld` for the auto-managed default, `PhysicsWorld.Create(definition)` for a custom world |
| `PhysicsBody` / `PhysicsBodyDefinition` | Position/rotation/velocity object; `BodyType` (`Dynamic`/`Kinematic`/`Static`), `BodyConstraints` (freeze axes), created via `world.CreateBody(definition)` |
| `PhysicsShape` / `PhysicsShapeDefinition` | Collidable geometry attached to a body via `body.CreateShape(geometry[, definition])`; carries `isTrigger`, `density`, `contactFilter`, `surfaceMaterial` |
| `CircleGeometry` / `CapsuleGeometry` / `PolygonGeometry` / `SegmentGeometry` | The four geometry structs a `PhysicsShape` can be built from; each has `CastRay`/`CastShape`/`ClosestPoint`/`OverlapPoint`/`CalculateAABB` |
| `ChainGeometry` / `ChainSegmentGeometry` / `PhysicsChain` / `PhysicsChainDefinition` | Edge-loop/edge-strip collider (tilemap-boundary equivalent), created via `body.CreateChain(geometry, definition)` |
| `PhysicsShape.SurfaceMaterial` | `friction`, `bounciness`, `rollingResistance`, `tangentSpeed`, plus `MixingMode`/`*Priority` for resolving disagreements between touching shapes |
| `PhysicsJoint` / `PhysicsXxxJointDefinition` | Distance/Fixed/Hinge/Slider/Wheel/Relative/Ignore joint types; fill a definition, call `world.CreateJoint(definition)` |
| `PhysicsCallbacks.IContactCallback` / `ITriggerCallback` | Callback interfaces for `OnContactBegin2D`/`OnContactEnd2D` and `OnTriggerBegin2D`/`OnTriggerEnd2D`, wired via `world.autoContactCallbacks`/`autoTriggerCallbacks` + shape `contactEvents`/`triggerEvents` + `callbackTarget` |
| `PhysicsWorld.contactBeginEvents` / `triggerBeginEvents` (etc.) | Polling alternative to callbacks — `ReadOnlySpan`s over engine memory, faster but only valid for the current frame |
| `PhysicsMask` / `PhysicsLayers` | This API's own layer-mask type and helper (`GetLayerMask`), supporting up to 64 layers when "Use Full Layers" is enabled |
| `PhysicsQuery` / `PhysicsWorld.CastRay` / `CastGeometry` / `OverlapPoint` / `OverlapAABB` / `TestOverlapAABB` | Raycast/shape-cast/overlap queries; results are `NativeArray<T>` requiring explicit `Allocator` + disposal |
| `PhysicsWorld.SimulationType` (`FixedUpdate` / `Update` / `Script`) + `PhysicsWorld.Simulate(deltaTime)` | Controls when/whether the world steps automatically vs. manually |
| `PhysicsWorld.simulationWorkers` / multithreading "WORM" model | Per-world worker-thread count; many concurrent readers, one writer, per world; `Create`/`Destroy`(`Batch`) calls are not thread-safe |
| `PhysicsBody.transformObject` / `transformWriteMode` / `PhysicsWorld.transformWriteMode` | The only bridge writing simulation results back onto a real `Transform`; both body- and world-level write modes must be non-`Off` |
| `PhysicsWorld.TransformPlane` (`XY` / `XZ` / `ZY`) | Remaps this 2D (`Vector2`-internal) simulation onto an arbitrary plane in 3D space |
| `PhysicsComposer` | CSG-style shape composition (`Operation.OR`/`AND`/`NOT`/`XOR`) producing combined `PolygonGeometry`/`ChainGeometry`, via `Unity.Collections` allocators |
| `PhysicsDestructor` (`Slice` / `Fragment`) | Runtime geometry-breaking utilities (ScriptReference-verified; no Manual conceptual page found) |
| `PhysicsTransform` / `PhysicsRotate` / `PhysicsMath` / `PhysicsPlane` / `PhysicsAABB` | Low-level math/utility structs: position+rotation composition, angle math, Vector2⇄Vector3 conversion, AABB operations |
| `PhysicsUserData` / `userData` / `ownerUserData` | Attach arbitrary payload (primitive, managed object, or `PhysicsMask`) to any body/shape/joint/chain |
| `PhysicsLowLevelSettings2D` (asset) | Global config: 64-layer names, default definitions, `concurrentSimulations`, `Draw In Build`, `bypassLowLevel` kill switch; wired into Project Settings > Physics 2D > Low Level |
| `isValid` / `Destroy()` / `SetOwner()` | Object-lifetime guard, cascading manual destruction, and an ownership-key deterrent against unauthorized `Destroy()` calls |

## Advanced Notes

**When to reach for this API vs. classic `Rigidbody2D`.** This is explicitly a performance- and control-oriented alternative, not a replacement for everyday 2D gameplay physics — reach for it when a project needs large numbers of simulated bodies (the introduction page advertises effectively unlimited object counts via batch creation and contiguous-memory layout), deterministic reproducible simulation (same input → same output every run, useful for replays/lockstep networking), physics without any backing GameObject at all (headless simulation, procedural destruction, background spatial queries), or direct Job System integration (struct-based objects, explicit `NativeArray` results, a documented WORM threading model letting you parallelize your own query code safely). For a typical small-to-medium scene — a platformer's dozen enemies, a top-down game's pickups and hazards — the classic `Rigidbody2D`/`Collider2D` component workflow (see `unity-physics`) remains the pragmatic choice: it comes with Inspector components, automatic GameObject sync, and none of this API's manual struct/definition/lifetime bookkeeping, at no meaningful performance cost until object counts get genuinely large.

**Relationship to DOTS/ECS.** This is *not* Unity's separate `Unity.Physics` DOTS package (see the `com.unity.physics` cross-reference in `unity-physics`'s Advanced Notes) — it ships as part of the built-in engine (`Implemented in: UnityEngine.Physics2DModule`, confirmed on the `MassConfiguration` ScriptReference page), works with ordinary `MonoBehaviour`s, and requires no ECS/Entities package. It is, however, deliberately designed to be *ECS-and-Job-friendly*: the introduction page states most returned types are structs specifically so they can be used from DOTS-style code, and the documented WORM multithreading model plus thread-safety notes on `Create`/`Destroy` exist precisely because this API expects to be driven from Job-parallelized code. Treat it as a bridge technology — GameObject-workflow-compatible (via `transformObject`) but architected with the same struct-of-data, explicit-memory-management discipline that DOTS/ECS code uses, rather than as either the classic component system or the DOTS physics package proper.

**Box2D v3 lineage and version scope.** `Manual/2d-physics-api/2d-physics-api-introduction.html` states plainly that "the API is based on version 3 of the Box2D physics system," and `Manual/WhatsNewUnity63.html` frames its introduction in Unity 6.3 around multithreaded performance, enhanced determinism, and improved debug visualization/gizmos over both classic `Physics2D` and Box2D v2-era integrations. Because it targets platforms that support compute shaders (for its debug-drawing pipeline) and is a comparatively new addition (Unity 6.3), verify platform/version compatibility before committing a project to it, especially for console or older mobile targets.

**Coverage caveat for this skill.** The bulk of this system has real, substantive Manual prose (34 pages under `Manual/2d-physics-api/`, each with working code samples) — this is not one of the ScriptReference-signature-only APIs the prompt warned might be encountered. The one confirmed gap is `PhysicsIgnoreJoint`/`PhysicsIgnoreJointDefinition` and `PhysicsDestructor`, which exist and are documented in ScriptReference but have no dedicated conceptual Manual page found under `2d-physics-api/` — those two are called out explicitly above rather than having invented usage narratives attached to them.

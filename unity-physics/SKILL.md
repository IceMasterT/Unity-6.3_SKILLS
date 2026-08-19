---
name: unity-physics
description: Use when working with Unity's physics system — Rigidbody/Collider setup, CharacterController, raycasting, layers and the collision matrix, or choosing between 3D and 2D physics. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Physics

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Physics overview | `Manual/PhysicsSection.html`, `Manual/PhysicsOverview.html` | Landing pages for the whole 3D physics system; navigation into every subtopic below |
| Rigidbody | `Manual/class-Rigidbody.html`, `Manual/rigidbody-physics-section.html`, `Manual/rigidbody-configure-colliders.html` | Rigidbody component fields, mass/drag, attaching colliders to a body |
| Rigidbody scripting | `ScriptReference/Rigidbody.html`, `ScriptReference/Rigidbody.AddForce.html`, `ScriptReference/Rigidbody.AddForceAtPosition.html`, `ScriptReference/Rigidbody.AddTorque.html`, `ScriptReference/Rigidbody.MovePosition.html`, `ScriptReference/Rigidbody.MoveRotation.html` | Exact method signatures and ForceMode semantics for driving a Rigidbody from code |
| Rigidbody interpolation & sleep | `ScriptReference/Rigidbody-interpolation.html`, `ScriptReference/Rigidbody-sleepThreshold.html`, `ScriptReference/Rigidbody.Sleep.html`, `Manual/physics-optimization-cpu-rigidbody-sleeping.html` | Smoothing visuals across fixed-timestep steps; when/why bodies go to sleep and how to force it |
| Continuous collision detection | `ScriptReference/Rigidbody-collisionDetectionMode.html`, `Manual/physics-optimization-cpu-rigidbody-collision-modes.html` | Discrete vs Continuous vs ContinuousDynamic vs ContinuousSpeculative, tunneling tradeoffs |
| Collider overview & types | `Manual/CollidersOverview.html`, `Manual/collider-shapes.html`, `Manual/collider-shapes-introduction.html`, `Manual/collider-types-introduction.html` | Choosing a collider shape, how colliders relate to Rigidbody and to each other |
| Primitive colliders | `Manual/primitive-colliders.html`, `Manual/primitive-colliders-introduction.html`, `Manual/class-BoxCollider.html`, `Manual/class-SphereCollider.html`, `Manual/class-CapsuleCollider.html`, `ScriptReference/BoxCollider.html`, `ScriptReference/SphereCollider.html`, `ScriptReference/CapsuleCollider.html` | Box/Sphere/Capsule collider fields and cheapest-shape guidance |
| Mesh colliders | `Manual/mesh-colliders.html`, `Manual/mesh-colliders-introduction.html`, `Manual/class-MeshCollider.html`, `Manual/prepare-mesh-for-mesh-collider.html`, `ScriptReference/MeshCollider.html`, `ScriptReference/MeshCollider-convex.html` | Convex vs non-convex MeshCollider rules, cooking a mesh for collision use |
| Wheel colliders | `Manual/wheel-colliders.html`, `Manual/wheel-colliders-introduction.html`, `Manual/wheel-colliders-friction.html`, `Manual/wheel-colliders-suspension.html`, `Manual/class-WheelCollider.html`, `Manual/WheelColliderTutorial.html`, `ScriptReference/WheelCollider.html` | Vehicle wheel simulation: suspension, friction curves, motor/brake torque |
| Terrain collider | `Manual/terrain-colliders.html`, `Manual/terrain-colliders-introduction.html`, `Manual/terrain-Tree-Colliders.html`, `Manual/class-TerrainCollider.html`, `ScriptReference/TerrainCollider.html` | Heightmap-based terrain collision and per-tree colliders |
| Compound colliders | `Manual/compound-colliders.html`, `Manual/compound-colliders-introduction.html`, `Manual/create-compound-collider.html` | Building complex shapes from multiple primitive colliders on child GameObjects |
| Collider interaction events | `Manual/collider-interactions.html`, `Manual/collider-interactions-oncollision.html`, `Manual/collider-interactions-ontrigger.html`, `Manual/collider-interactions-other-events.html`, `Manual/collider-interactions-create-trigger.html`, `Manual/collider-interactions-example-scripts.html`, `Manual/collider-types-interaction.html` | `OnCollisionEnter/Stay/Exit` vs `OnTriggerEnter/Stay/Exit`, what Rigidbody/isTrigger combination each needs |
| CharacterController | `Manual/CharacterControllers.html`, `Manual/class-CharacterController.html`, `Manual/com.unity.charactercontroller.html`, `ScriptReference/CharacterController.html`, `ScriptReference/CharacterController.Move.html`, `ScriptReference/CharacterController.SimpleMove.html` | Kinematic character movement API, step offset, slope limit, `isGrounded`, collision flags |
| Physics.Raycast & queries | `ScriptReference/Physics.Raycast.html`, `ScriptReference/Physics.RaycastAll.html`, `ScriptReference/Physics.Linecast.html`, `ScriptReference/Physics.SphereCast.html`, `ScriptReference/Physics.BoxCast.html`, `ScriptReference/Physics.CapsuleCast.html`, `ScriptReference/Physics.OverlapSphere.html`, `ScriptReference/Physics.OverlapBox.html`, `ScriptReference/Physics.OverlapCapsule.html`, `ScriptReference/RaycastHit.html`, `Manual/physics-optimization-raycasts-queries.html` | Every non-2D query overload, LayerMask/QueryTriggerInteraction args, RaycastHit fields, query cost |
| PhysicsMaterial | `Manual/class-PhysicsMaterial.html`, `Manual/create-apply-physics-material.html`, `Manual/collider-surface-friction.html`, `Manual/collider-surface-bounce.html`, `Manual/collider-surfaces-combine.html`, `Manual/collider-surfaces.html`, `ScriptReference/PhysicsMaterial.html`, `ScriptReference/PhysicsMaterialCombine.html` | Friction/bounciness values and the Average/Minimum/Maximum/Multiply combine modes (class is `PhysicsMaterial` in Unity 6, not the legacy `PhysicMaterial` name) |
| Joints (3D) | `Manual/Joints.html`, `Manual/joints-section.html`, `Manual/class-FixedJoint.html`, `Manual/class-HingeJoint.html`, `Manual/class-SpringJoint.html`, `Manual/class-ConfigurableJoint.html`, `Manual/class-CharacterJoint.html`, `Manual/create-configurable-joint.html`, `Manual/configurable-joints-movement-constraint.html`, `Manual/configurable-joints-driving-forces.html` | Fixed/Hinge/Spring/Configurable/Character joint setup and ConfigurableJoint's per-axis motion/drive model |
| Joints (3D) scripting | `ScriptReference/Joint.html`, `ScriptReference/FixedJoint.html`, `ScriptReference/HingeJoint.html`, `ScriptReference/SpringJoint.html`, `ScriptReference/ConfigurableJoint.html`, `ScriptReference/CharacterJoint.html` | Joint base class members (`connectedBody`, `breakForce`) and per-joint scripting fields |
| Ragdolls & articulations | `Manual/ragdoll-physics-section.html`, `Manual/RagdollStability.html`, `Manual/wizard-RagdollWizard.html`, `Manual/physics-articulations.html` | Ragdoll Wizard workflow, stability tuning, `ArticulationBody` for robotics-style joint chains (alternative to Joint-based ragdolls) |
| Layers & collision matrix | `Manual/LayerBasedCollision.html`, `Manual/layers-and-layermasks.html`, `Manual/create-layers.html`, `Manual/use-layers.html`, `Manual/Layers.html`, `Manual/physics-optimization-cpu-collision-layers.html`, `ScriptReference/LayerMask.html`, `ScriptReference/LayerMask.GetMask.html` | Assigning physics layers, Project Settings collision matrix, building `LayerMask` bitmasks in code |
| Physics project settings | `Manual/class-PhysicsManager.html`, `Manual/class-TimeManager.html` | Project Settings > Physics panel (solver iterations, default material, bounce/contact thresholds) and Time panel (`Fixed Timestep`, `Maximum Allowed Timestep`) |
| Physics settings scripting | `ScriptReference/Physics-gravity.html`, `ScriptReference/Physics-defaultSolverIterations.html`, `ScriptReference/Physics-defaultSolverVelocityIterations.html`, `ScriptReference/Physics-bounceThreshold.html`, `ScriptReference/Physics-sleepThreshold.html`, `ScriptReference/Physics-defaultContactOffset.html`, `ScriptReference/Physics-queriesHitTriggers.html`, `ScriptReference/Physics-queriesHitBackfaces.html`, `ScriptReference/Physics-simulationMode.html`, `ScriptReference/Time-fixedDeltaTime.html` | Global physics tunables reachable from code, and the `Physics.simulationMode` enum (FixedUpdate/Update/Script) |
| Manual physics stepping | `Manual/physics-optimization-cpu-manual-simulation.html`, `Manual/physics-optimization-cpu-frequency.html`, `ScriptReference/Physics.Simulate.html` | Driving the physics step yourself with `Physics.Simulate` when `simulationMode` is `Script` |
| Physics performance/optimization | `Manual/physics-optimization.html`, `Manual/physics-optimization-cpu.html`, `Manual/physics-optimization-cpu-broad-phase.html`, `Manual/physics-optimization-cpu-collider-types.html`, `Manual/physics-optimization-cpu-static-colliders.html`, `Manual/physics-optimization-cpu-transform-sync.html`, `Manual/physics-optimization-cpu-mesh-cooking-options.html`, `Manual/physics-optimization-cpu-query-only.html`, `Manual/physics-optimization-collision-callbacks.html`, `Manual/physics-optimization-memory.html`, `Manual/physics-performance-issues.html`, `Manual/ProfilerPhysics.html`, `Manual/PhysicsDebugVisualization.html` | CPU cost breakdown by subsystem, static vs dynamic collider cost, transform-sync cost, Physics Debug window, Profiler physics module |
| Multi-scene & backend | `Manual/physics-multi-scene.html`, `Manual/physics-integrations.html`, `Manual/physics-remove-backend.html` | Per-scene physics worlds (`PhysicsScene`), swapping/removing the physics backend module |
| 2D physics overview | `Manual/2d-physics/2d-physics.html` | Landing page for the entire 2D physics manual tree (verified correct path — do not use a bare `Manual/2d-physics.html`) |
| 2D Rigidbody | `Manual/2d-physics/rigidbody/rigidbody-2d-landing.html`, `Manual/2d-physics/rigidbody/introduction-to-rigidbody-2d.html`, `Manual/2d-physics/rigidbody/body-types/rigidbody-2d-body-types-landing.html`, `Manual/2d-physics/rigidbody/body-types/introduction-to-rigidbody-2d-body-types.html`, `Manual/2d-physics/rigidbody/rigidbody-2d-simulated-property.html`, `ScriptReference/Rigidbody2D.html` | Rigidbody2D body types (Dynamic/Kinematic/Static), the `simulated` toggle |
| 2D Colliders | `Manual/2d-physics/collider/collider-2d-landing.html`, `Manual/2d-physics/collider/box-collider-2d-reference.html`, `Manual/2d-physics/collider/circle-collider-2d-reference.html`, `Manual/2d-physics/collider/edge-collider-2d-reference.html`, `Manual/2d-physics/collider/polygon-collider-2d-reference.html`, `Manual/2d-physics/collider/capsule-collider/capsule-collider-2d-landing.html`, `Manual/2d-physics/collider/composite-collider/composite-collider-2d-landing.html`, `Manual/2d-physics/collider/composite-collider/combine-colliders-composite-collider-2d.html`, `Manual/2d-physics/collider/custom-collider/custom-collider-2d-landing.html`, `Manual/2d-physics/collider/edit-collider-geometry.html`, `ScriptReference/Collider2D.html` | Box/Circle/Capsule/Edge/Polygon/Composite/Custom Collider2D types and geometry editing |
| 2D Joints | `Manual/2d-physics/joints/2d-joints-landing.html`, `Manual/2d-physics/joints/introduction-to-2d-joints.html`, `Manual/2d-physics/joints/2d-joint-constraints.html`, `Manual/2d-physics/joints/distance-joint-2d-landing.html`, `Manual/2d-physics/joints/fixed-joint-2d-landing.html`, `Manual/2d-physics/joints/hinge-joint-2d-landing.html`, `Manual/2d-physics/joints/slider-joint-2d-landing.html`, `Manual/2d-physics/joints/spring-joint-2d-landing.html`, `Manual/2d-physics/joints/wheel-joint-2d-landing.html`, `Manual/2d-physics/joints/relative-joint-2d-landing.html`, `Manual/2d-physics/joints/friction-joint-2d-landing.html`, `Manual/2d-physics/joints/target-joint-2d-landing.html` | Every Joint2D type's fundamentals + reference sub-pages |
| 2D Effectors | `Manual/2d-physics/effectors/effectors-2d-landing.html`, `Manual/2d-physics/effectors/area-effector-2d-reference.html`, `Manual/2d-physics/effectors/point-effector-2d-reference.html`, `Manual/2d-physics/effectors/platform-effector-2d-reference.html`, `Manual/2d-physics/effectors/surface-effector-2d-reference.html`, `Manual/2d-physics/effectors/buoyancy-effector-2d-reference.html` | Area/Point/Platform/Surface/Buoyancy effector fields, always paired with a Collider2D flagged `Used By Effector` |
| 2D Physics Material & profiler | `Manual/2d-physics/physics-material-2d-reference.html`, `Manual/2d-physics/constant-force-2d-reference.html`, `Manual/2d-physics/physics-profiler/physics-2d-profiler-landing.html`, `Manual/2d-physics/physics-profiler/physics-2d-profiler-module-reference.html` | PhysicsMaterial2D friction/bounciness, ConstantForce2D, the dedicated 2D Physics Profiler module |
| 2D Physics API (low-level) | `Manual/2d-physics-api/2d-physics-api-introduction.html`, `Manual/2d-physics-api/2d-physics-api-world.html`, `Manual/2d-physics-api/2d-physics-api-physics-object.html`, `Manual/2d-physics-api/2d-physics-api-multithreading.html`, `Manual/2d-physics-api/2d-physics-api-raycasting.html`, `Manual/2d-physics-api/2d-physics-api-joints.html` | The newer explicit `PhysicsWorld2D`/`PhysicsBody2D` job-friendly API layered under the classic 2D physics components |
| 2D global settings | `Manual/class-Physics2DSettings.html`, `Manual/class-PhysicsLowLevelSettings2D.html`, `ScriptReference/Physics2D.Raycast.html`, `ScriptReference/Physics2D.OverlapCircle.html`, `ScriptReference/Physics2D.BoxCast.html`, `ScriptReference/Physics2D-autoSimulation.html` | Project Settings > Physics 2D panel, `Physics2D.*` static query/settings API (separate namespace from 3D `Physics`) |
| DOTS Unity Physics (alt. stack) | `Manual/com.unity.physics.html`, `Manual/com.unity.modules.physics.html`, `Manual/com.unity.modules.physics2d.html`, `Manual/com.unity.modules.terrainphysics.html` | Package landing pages for the DOTS-based `Unity.Physics` package (ECS/burst-compiled, deterministic) vs the built-in GameObject `PhysX`-backed system covered by the rest of this table |

## Key Guidelines

### Rigidbody & Movement

A Rigidbody hands a GameObject over to the physics solver: once attached (and not `isKinematic`), gravity, drag, and collision response act on it automatically, and the solver — not your script — is the source of truth for `transform.position`/`rotation` after each physics step. All physics-driven writes (`AddForce`, `AddTorque`, `MovePosition`, `MoveRotation`, reading/writing `linearVelocity`) belong in `FixedUpdate`, because the solver advances on a fixed timestep (`Time.fixedDeltaTime`, default 0.02s / 50Hz) that is decoupled from the variable per-frame `Update` rate — doing force math in `Update` makes motion framerate-dependent and non-reproducible across machines. Never assign `transform.position` directly to a non-kinematic Rigidbody; that snaps the body without going through the solver's velocity integration, causing jitter, tunneling, and missed collision events on the next step. If a Rigidbody must be positioned by external logic (e.g. an animation or a moving platform script) rather than by forces, mark it `isKinematic = true` and drive it with `MovePosition`/`MoveRotation`, which still generates correct collision/trigger events against non-kinematic bodies. `Rigidbody.interpolation` (`None`/`Interpolate`/`Extrapolate`) smooths the *visual* transform between fixed-timestep steps for cameras/rendering without affecting the simulation itself — use `Interpolate` on the player-controlled or camera-followed body and leave everything else `None` to save cost.

```csharp
public class ForceMover : MonoBehaviour
{
    [SerializeField] private float thrust = 10f;
    private Rigidbody rb;

    void Awake()
    {
        rb = GetComponent<Rigidbody>();
        rb.interpolation = RigidbodyInterpolation.Interpolate; // smooth visuals only
        rb.collisionDetectionMode = CollisionDetectionMode.ContinuousDynamic; // fast mover vs fast/slow others
    }

    void FixedUpdate()
    {
        Vector3 input = new Vector3(Input.GetAxis("Horizontal"), 0f, Input.GetAxis("Vertical"));
        rb.AddForce(input * thrust, ForceMode.Force); // continuous force, mass-dependent
        // rb.MovePosition(rb.position + input * speed * Time.fixedDeltaTime); // alternative: kinematic-style displacement
    }
}
```

### Colliders & Triggers

A Collider defines the shape used for both physical collision response and trigger detection; it can exist with or without a Rigidbody, but at least one of the two colliding GameObjects in a pair must have a non-kinematic (or kinematic) Rigidbody for Unity to generate any callback at all — two static colliders (no Rigidbody on either) never fire `OnCollisionEnter`/`OnTriggerEnter`. `isTrigger` decides the behavior: with it on, the collider raises `OnTriggerEnter/Stay/Exit` and never generates a physical response (objects pass through it); with it off, the collider is solid, blocks motion, and raises `OnCollisionEnter/Stay/Exit` with contact-point/impulse data available via the `Collision` parameter. Prefer primitive colliders (Box/Sphere/Capsule) for anything that moves — they use analytic collision math and are far cheaper than Mesh Colliders. A `MeshCollider` is non-convex by default, which means it can only collide with primitive colliders and other convex mesh colliders, never with another non-convex mesh collider, and non-convex mesh colliders cannot be attached to a non-kinematic Rigidbody at all — check `convex = true` (capped at 255 triangles) if the shape must move or must collide with other meshes. Build complex static or moving shapes out of several primitive colliders on child GameObjects (compound colliders) rather than reaching for a Mesh Collider by default.

```csharp
public class HazardTrigger : MonoBehaviour
{
    void Awake()
    {
        GetComponent<Collider>().isTrigger = true; // must be true to get OnTrigger* callbacks
    }

    void OnTriggerEnter(Collider other)
    {
        if (other.attachedRigidbody != null && other.CompareTag("Player"))
            Debug.Log("Player entered hazard zone");
    }

    void OnCollisionEnter(Collision collision) // only fires on a non-trigger collider pair
    {
        ContactPoint contact = collision.GetContact(0);
        Debug.Log($"Hit {collision.collider.name} at {contact.point} with impulse {collision.impulse}");
    }
}
```

### Raycasting & Queries

`Physics.Raycast` and its siblings (`RaycastAll`, `Linecast`, `SphereCast`, `BoxCast`, `CapsuleCast`, `OverlapSphere`, `OverlapBox`, `OverlapCapsule`) are the query API for asking "what's out there" without waiting for a collision callback — used for ground checks, line-of-sight, weapon hit detection, and proximity checks. Every overload accepts an optional `LayerMask` and `QueryTriggerInteraction`; always pass an explicit LayerMask on any query run every frame (e.g. a ground check) to skip irrelevant colliders (UI hitboxes, VFX-only triggers, etc.) and cut query cost — an unfiltered raycast walks every collider layer in the scene's broad phase. `Physics.Raycast` returns a `bool` and fills a `RaycastHit` only on hit; `RaycastAll` allocates an array every call and is more expensive, so prefer the non-allocating `Physics.RaycastNonAlloc` variant in hot loops. Shape casts (`SphereCast`/`BoxCast`/`CapsuleCast`) sweep a volume instead of an infinitely thin line and are the right tool for fast-moving projectiles or characters where a thin raycast could miss thin geometry between frames — they cost more than a plain raycast, so reserve them for cases where tunneling is a real risk. `OverlapSphere`/`OverlapBox` test a static volume for overlapping colliders (no direction/distance), useful for "what's near me right now" checks like an explosion radius.

```csharp
public class GroundCheck : MonoBehaviour
{
    [SerializeField] private LayerMask groundMask; // set in Inspector to only the "Ground" layer
    [SerializeField] private float checkDistance = 0.2f;

    bool IsGrounded()
    {
        Ray ray = new Ray(transform.position, Vector3.down);
        return Physics.Raycast(ray, out RaycastHit hit, checkDistance, groundMask, QueryTriggerInteraction.Ignore);
    }

    void FireHitscanWeapon(Vector3 origin, Vector3 direction, float range)
    {
        if (Physics.SphereCast(origin, 0.1f, direction, out RaycastHit hit, range, groundMask))
            Debug.Log($"Hit {hit.collider.name} at distance {hit.distance}");
    }
}
```

### Layers & Collision Matrix

Physics layers (Project Settings > Tags and Layers) combined with the collision matrix (Project Settings > Physics, the "Layer Collision Matrix" grid) let Unity skip broad-phase collision checks entirely between layer pairs that are unchecked — this is cheaper and more centralized than filtering collisions in `OnCollisionEnter`/`OnTriggerEnter` code, because the excluded pairs never even reach the narrow phase. Leaving everything on the `Default` layer means the matrix can't selectively exclude anything, so assign meaningful layers (Player, Enemy, Ground, Projectile, Trigger-only, etc.) early in a project rather than retrofitting them later. `LayerMask` values used in scripts are bitmasks, not layer indices — build them with `LayerMask.GetMask("Ground", "Wall")` rather than hardcoding an index, since layer indices can be reordered by whoever edits the Tags and Layers list. `Physics.IgnoreLayerCollision(layerA, layerB, ignore)` and `Physics.IgnoreCollision(colliderA, colliderB, ignore)` can flip collision ignoring at runtime for cases the static matrix can't express (per-instance rather than per-layer), such as temporarily letting a player fall through a specific platform.

```csharp
public class LayerSetup : MonoBehaviour
{
    [SerializeField] private LayerMask groundAndWallMask;

    void Start()
    {
        // equivalent runtime construction if the mask isn't set in the Inspector
        int mask = LayerMask.GetMask("Ground", "Wall");

        // runtime override of the static collision matrix for one specific pair of colliders
        Collider player = GetComponent<Collider>();
        Collider oneWayPlatform = GameObject.Find("OneWayPlatform").GetComponent<Collider>();
        Physics.IgnoreCollision(player, oneWayPlatform, true);
    }
}
```

### Joints

Joints connect two Rigidbodies (or one Rigidbody to a fixed point in world space when `connectedBody` is left `None`) and constrain their relative motion instead of letting collision response do it. `FixedJoint` welds two bodies together but still lets both react individually to `breakForce`/`breakTorque` thresholds — useful for objects that should detach when hit hard enough. `HingeJoint` constrains motion to rotation about a single axis (doors, wheels, pendulums) and exposes a `JointMotor` for driving that rotation with a target velocity/force, plus optional `JointLimits`. `SpringJoint` pulls two bodies toward a target distance with spring/damper forces rather than a hard constraint — good for tethers and soft connections. `ConfigurableJoint` is the general case: every one of the 6 degrees of freedom (3 linear, 3 angular) is independently set to `Free`/`Limited`/`Locked`, with drive forces (`xDrive`/`angularXDrive`/etc.) able to actively motor an axis toward a target — this is how to build anything from a rigid weld (all locked) up to a custom vehicle suspension or a ragdoll limb, and it subsumes what the other joint types do as special cases. `CharacterJoint` is a ConfigurableJoint preset specialized for ragdoll limbs (twist + swing1/swing2 limits). Use the Ragdoll Wizard for a quick humanoid ragdoll rig; tune `RagdollStability` guidance (higher solver iterations, mass distribution) if limbs jitter. `ArticulationBody`/`physics-articulations.html` is a separate, more numerically stable alternative to Joint-chains for kinematic chains like robot arms or multi-segment ragdolls, and is generally preferred over stacked Joints for long chains because it doesn't accumulate the same soft-constraint drift.

```csharp
public class DoorHinge : MonoBehaviour
{
    void Awake()
    {
        HingeJoint hinge = gameObject.AddComponent<HingeJoint>();
        hinge.connectedBody = null; // hinges to world space at this GameObject's anchor
        hinge.axis = Vector3.up;
        hinge.useLimits = true;
        hinge.limits = new JointLimits { min = 0f, max = 90f };
        hinge.useMotor = true;
        hinge.motor = new JointMotor { targetVelocity = 90f, force = 50f };
    }
}
```

### 2D Physics

2D physics is a separate simulation engine from 3D physics — `Rigidbody2D`/`Collider2D` run on Box2D-derived solver, have their own Project Settings > Physics 2D panel and their own collision matrix, and do not interact with `Rigidbody`/`Collider` at all. Decide 3D vs 2D per *project*, not per object. `Rigidbody2D.bodyType` (Dynamic/Kinematic/Static) replaces the 3D `isKinematic` bool with an explicit enum. Collider2D shapes cover Box/Circle/Capsule/Edge/Polygon, plus a `CompositeCollider2D` that merges multiple child colliders (each flagged `usedByComposite`) into one optimized outline — the standard pattern for tilemap collision. Joint2D types (Distance/Fixed/Hinge/Slider/Spring/Wheel/Relative/Friction/Target) mirror the 3D joint concepts but each ships as its own component rather than one configurable joint. 2D-only effectors (`AreaEffector2D`, `PointEffector2D`, `PlatformEffector2D`, `SurfaceEffector2D`, `BuoyancyEffector2D`) apply forces to any Rigidbody2D whose Collider2D has `usedByEffector` checked — no 3D equivalent exists; `PlatformEffector2D` in particular implements one-way platforms without runtime layer-ignoring tricks. A lower-level `PhysicsWorld2D`/`PhysicsBody2D` job-friendly API sits underneath the classic components for performance-critical, multithreaded 2D physics work (`Manual/2d-physics-api/`).

```csharp
public class OneWayPlatform : MonoBehaviour
{
    void Awake()
    {
        var collider = GetComponent<Collider2D>();
        collider.usedByEffector = true;
        var effector = gameObject.AddComponent<PlatformEffector2D>();
        effector.useOneWay = true;
        effector.surfaceArc = 180f; // only collide from above
    }
}
```

### CharacterController vs Rigidbody

`CharacterController` is a kinematic capsule moved explicitly via `Move(Vector3)` or `SimpleMove(Vector3)`; it applies its own built-in step-offset and slope-limit handling, reports `isGrounded` and per-call `CollisionFlags`, and is not affected by incoming physics forces or pushed by other Rigidbodies — you get precise, jitter-free control at the cost of having to hand-code any push/knockback/platform-riding behavior yourself (typically inside `OnControllerColliderHit`). A Rigidbody-driven capsule (non-kinematic Rigidbody + CapsuleCollider, moved with `AddForce`/`MovePosition` in `FixedUpdate`) fully integrates with the solver — it can be pushed by explosions, can push other Rigidbodies, and interacts naturally with joints — but is harder to tune for the snappy, deterministic feel players expect from platformer/FPS controllers, since gravity, friction, and drag all fight the intended input. Choose `CharacterController` for traditional first/third-person controllers where precise, predictable movement matters most; choose a Rigidbody capsule when the character must be part of the physics simulation (rammed by vehicles, knocked back by explosions, ragdolls on death).

```csharp
public class KinematicPlayer : MonoBehaviour
{
    private CharacterController controller;
    [SerializeField] private float speed = 5f;
    [SerializeField] private float gravity = -9.81f;
    private Vector3 velocity;

    void Awake() => controller = GetComponent<CharacterController>();

    void Update()
    {
        if (controller.isGrounded && velocity.y < 0) velocity.y = -2f; // small stick-to-ground bias
        Vector3 input = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
        controller.Move(input * speed * Time.deltaTime);
        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);
    }

    void OnControllerColliderHit(ControllerColliderHit hit)
    {
        if (hit.rigidbody != null && !hit.rigidbody.isKinematic)
            hit.rigidbody.AddForceAtPosition(controller.velocity * 0.1f, hit.point, ForceMode.Impulse); // manual push
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Moving a Rigidbody via `transform.position` | Bypasses the solver's velocity integration, causing jitter and missed collisions; use `Rigidbody.MovePosition`/`AddForce` in `FixedUpdate` instead |
| Physics code in `Update` instead of `FixedUpdate` | Forces/velocity changes applied at a variable frame rate produce inconsistent, framerate-dependent motion; move all force/velocity/`MovePosition` calls into `FixedUpdate` |
| No Rigidbody on either side of a moving trigger/collider pair | Two static colliders (neither has a Rigidbody) never generate `OnTriggerEnter`/`OnCollisionEnter`; add a (often kinematic) Rigidbody to the moving object |
| Confusing `OnTriggerEnter` and `OnCollisionEnter` | `isTrigger=true` gives callbacks but no physical response; `isTrigger=false` blocks motion and fires collision callbacks; using the wrong flag means either no blocking or no event ever fires |
| Everything left on the `Default` layer | The collision matrix can't selectively ignore pairs when nothing is differentiated; assign meaningful physics layers (Player/Enemy/Ground/Projectile) early |
| Non-convex Mesh Collider on a moving/non-kinematic Rigidbody | Non-convex `MeshCollider` can't be attached to a non-kinematic body and can't collide with another non-convex mesh; set `convex = true` (255-tri cap) or swap to primitive colliders |
| Reading `Rigidbody.linearVelocity`/position mid-`Update` and expecting it reflects the latest physics step | The solver only advances during `FixedUpdate`; values read in `Update` are from the last completed physics step, not "now" — this is usually fine but surprises people expecting sub-step precision |
| Fast/small objects tunneling through thin colliders (bullets, tiny balls) | Default `CollisionDetectionMode.Discrete` only checks positions at each fixed step, missing objects that fully cross a thin collider between steps; set `CollisionDetectionMode.Continuous` (against static geometry) or `ContinuousDynamic`/`ContinuousSpeculative` (against other moving bodies) on the fast object |
| Unfiltered `Physics.Raycast` called every frame with no `LayerMask` | Walks every collider layer in the broad phase, wasting CPU on irrelevant hits (UI colliders, VFX triggers); always pass an explicit `LayerMask` built via `LayerMask.GetMask` for per-frame queries like ground checks |
| Using `Physics.RaycastAll` in a hot loop | Allocates a new array on every call, creating GC pressure; use `Physics.RaycastNonAlloc` with a reusable buffer for anything called repeatedly |
| Pushing a `CharacterController` player with a Rigidbody explosion and expecting it to just work | `CharacterController` ignores incoming physics forces entirely by design; apply knockback manually (e.g. add to a velocity field consumed by your own `Move` call) instead of `AddForce` |
| Two Rigidbodies welded with `FixedJoint` snapping apart unexpectedly | `breakForce`/`breakTorque` default to `Mathf.Infinity` only if left alone, but many templates set finite values; check both fields if a joint is breaking under normal gameplay forces |
| Ragdoll limbs jittering or exploding | Usually too few solver iterations for the mass ratio between connected bodies, or colliders overlapping at rest; increase `Physics.defaultSolverIterations`/`defaultSolverVelocityIterations` or the Rigidbody's own `solverIterations`, and check `RagdollStability` guidance for mass/collider sizing |
| Mixing 3D and 2D physics components on the same object expecting them to interact | `Rigidbody`/`Collider` and `Rigidbody2D`/`Collider2D` are entirely separate simulations with separate collision matrices; a `Rigidbody2D` never collides with a 3D `Collider`, and vice versa |
| Assuming `PhysicMaterial` is the Unity 6 class name | The physics material class was renamed to `PhysicsMaterial` (and `PhysicsMaterial2D` for 2D) in current Unity versions; scripts/tutorials referencing `PhysicMaterial` are targeting an older API surface |

## Quick Reference

| Item | Purpose |
|------|---------|
| `Rigidbody` / `Rigidbody2D` | Adds mass, gravity, and solver-driven motion to a GameObject; `isKinematic` (3D) / `bodyType` (2D) switches between physics-driven and script-driven |
| `Collider` / `Collider2D` | Defines the physical shape used for collision/trigger detection; `isTrigger` switches between physical blocking and trigger-only callbacks |
| `BoxCollider` / `SphereCollider` / `CapsuleCollider` | Cheapest, analytic-math primitive collider shapes; prefer these over Mesh Colliders for anything that moves |
| `MeshCollider` | Arbitrary-mesh collision shape; non-convex by default (static-only, can't collide with other non-convex meshes), or `convex = true` (255-tri cap, works on moving bodies) |
| `WheelCollider` | Vehicle wheel simulation with suspension, friction curves, motor/brake torque — not a physical shape by itself |
| `TerrainCollider` | Heightmap-based collision matching a Terrain object, plus separate colliders for individual trees |
| `CharacterController` | Kinematic capsule movement via `Move`/`SimpleMove`, with built-in step offset, slope limit, `isGrounded`, and `CollisionFlags` |
| `PhysicsMaterial` / `PhysicsMaterial2D` | Per-collider friction and bounciness, with `PhysicsMaterialCombine` modes (Average/Minimum/Maximum/Multiply) controlling how two materials combine on contact |
| `Physics.Raycast` / `RaycastAll` / `RaycastNonAlloc` | Cast a thin ray against colliders, optionally filtered by `LayerMask` and `QueryTriggerInteraction` |
| `Physics.SphereCast` / `BoxCast` / `CapsuleCast` | Sweep a volume rather than a line — catches thin geometry a raycast could miss between frames |
| `Physics.OverlapSphere` / `OverlapBox` / `OverlapCapsule` | Test a static volume for currently-overlapping colliders (no direction/distance) |
| `Physics.IgnoreCollision` / `IgnoreLayerCollision` | Runtime override of collision ignoring for a specific collider pair or layer pair |
| `Rigidbody.MovePosition` / `MoveRotation` | Physics-safe ways to move/rotate a kinematic Rigidbody, generating correct collision events |
| `Rigidbody.AddForce` / `AddForceAtPosition` / `AddTorque` | Physics-safe ways to accelerate a non-kinematic Rigidbody; `ForceMode` (Force/Impulse/VelocityChange/Acceleration) controls mass-dependence and instant vs continuous application |
| `Rigidbody.interpolation` | Smooths rendered transform between fixed-timestep steps (`Interpolate`/`Extrapolate`) without affecting simulation |
| `Rigidbody.collisionDetectionMode` | `Discrete` (cheap, default) vs `Continuous`/`ContinuousDynamic`/`ContinuousSpeculative` (costlier, prevents tunneling) |
| `FixedJoint` / `HingeJoint` / `SpringJoint` / `ConfigurableJoint` / `CharacterJoint` | Constrain relative motion between two Rigidbodies; `ConfigurableJoint` is the general 6-DOF case the others specialize |
| `ArticulationBody` | Alternative to Joint chains for kinematic chains (robot arms, long ragdoll chains); more numerically stable than stacked Joints |
| `LayerMask` / Project Settings collision matrix | Assign physics layers and precompute which layer pairs the broad phase should even test, cheaper than per-callback filtering in code |
| `Physics.simulationMode` / `Physics.Simulate` | Switch between automatic `FixedUpdate`-driven stepping and manually stepping the solver with `Physics.Simulate(deltaTime)` |
| `Time.fixedDeltaTime` / Project Settings > Time | Controls the physics solver's fixed timestep (default 0.02s / 50Hz) |
| `AreaEffector2D` / `PointEffector2D` / `PlatformEffector2D` / `SurfaceEffector2D` / `BuoyancyEffector2D` | 2D-only force fields that act on any Rigidbody2D whose Collider2D has `usedByEffector` checked |
| `CompositeCollider2D` | Merges multiple child Collider2D shapes (flagged `usedByComposite`) into one optimized outline, the standard pattern for tilemap collision |

## Advanced Notes

**Fixed timestep tuning.** `Time.fixedDeltaTime` (Project Settings > Time > Fixed Timestep) sets how often the solver steps; smaller values give a more accurate simulation at higher CPU cost, larger values are cheaper but coarser (visible as choppier physics motion, especially under `interpolation = None`). `Maximum Allowed Timestep` caps how much simulated time a single frame is allowed to catch up on a slow frame — if a frame takes longer than this, Unity runs multiple `FixedUpdate` steps in a row (a "physics spiral of death" risk if the cap is too high and steps can't keep up); lowering the cap instead skips catching up fully and lets visible time slip. `Physics.defaultSolverIterations`/`Physics-defaultSolverVelocityIterations` (or per-Rigidbody `solverIterations`/`solverVelocityIterations`) trade CPU for constraint accuracy — raise them for stacks of joints/ragdolls that jitter, lower them for scenes with many simple non-interacting bodies. Set `Physics.simulationMode` to `Script` and drive stepping yourself with `Physics.Simulate(step)` for deterministic lockstep simulation (networked physics, replay systems) instead of relying on Unity's automatic `FixedUpdate` cadence — see `Manual/physics-optimization-cpu-manual-simulation.html`.

**Static vs dynamic collider cost.** Colliders on GameObjects with no Rigidbody are treated as static by the broad phase and are cheap to have many of, but moving a "static" collider's transform at runtime forces an expensive broad-phase rebuild — if a wall or platform needs to move, give it a `kinematic` Rigidbody rather than just moving its transform, so Unity treats it as dynamic and updates it cheaply instead of re-baking static state every frame (`Manual/physics-optimization-cpu-static-colliders.html`, `Manual/physics-optimization-cpu-transform-sync.html`).

**Unity Physics / DOTS (alternative stack).** Alongside the built-in GameObject/PhysX-backed system this skill otherwise documents, Unity ships a separate `Unity.Physics` package (`Manual/com.unity.physics.html`) built for the Entity Component System — burst-compiled, job-safe, and deterministic, aimed at large-scale simulations (thousands of bodies) and DOTS-based projects. It does not share components with `Rigidbody`/`Collider` (uses `PhysicsVelocity`, `PhysicsCollider`, etc. as ECS components instead) and is a project-level architecture choice, not a drop-in swap — reach for it only if the project is already ECS/DOTS-based or physics performance at GameObject scale has been profiled and found wanting.

**Physics Debug window & Profiler.** `Manual/PhysicsDebugVisualization.html` exposes a Physics Debug window (Window > Analysis > Physics Debug) to visualize colliders, contacts, and queries directly in-editor, independent of Gizmos code. The Profiler's dedicated Physics module (`Manual/ProfilerPhysics.html`) and the 2D-specific Physics 2D Profiler module (`Manual/2d-physics/physics-profiler/physics-2d-profiler-module-reference.html`) break down solver cost by phase (broad phase, narrow phase, solve, sync-transforms) — use these before guessing which optimization (layer matrix, collider type, solver iterations, sleep thresholds) will actually move the needle.

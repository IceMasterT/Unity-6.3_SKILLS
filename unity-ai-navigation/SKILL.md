---
name: unity-ai-navigation
description: Use when implementing Unity NPC pathfinding or navigation — NavMesh baking, NavMeshAgent, NavMeshObstacle, NavMeshSurface, or off-mesh links. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity AI Navigation

## Retrieval Sources

Local coverage is lopsided and it is worth being precise about the shape of that imbalance. The **AI Navigation package Manual** page (`Manual/com.unity.ai.navigation.html`, package `com.unity.ai.navigation` version 2.0.14 for Unity Editor 6000.3) is a single 194-line landing-page stub: it contains only the package description ("high-level components that allow you to use navmeshes to incorporate navigation and pathfinding in your game... build and use navmeshes at runtime and at edit time, create dynamic obstacles, and use links..."), version/compatibility metadata, and a keyword index (carving, navmesh agent, navmesh link, navmeshlink, navmesh modifier, navmeshmodifier, navmesh modifier volume, navmeshmodifiervolume, navmesh obstacle, navmesh surface, navmeshsurface, offmesh link, pathfinding). A directory search of `Manual/` for `*navmesh*` returns **zero** additional files — there are no local component reference pages for `NavMeshSurface`, `NavMeshModifier`, `NavMeshModifierVolume`, or `NavMeshLink` (the actual AI Navigation package components you drag onto GameObjects in the Editor). Any guidance on those four components in this skill is pretrained knowledge, flagged as such, not sourced from local docs.

In sharp contrast, the built-in `UnityEngine.AI` **ScriptReference** namespace is exhaustively documented locally: 252 files matching `AI.*.html` under `ScriptReference/`, covering every public class, struct, enum, method, and property of the legacy/low-level navigation API that the package's high-level components are built on top of. This is the namespace to lean on. The table below cites every class/member group actually found on disk (verified 2026-08-19 against `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/`), organized by subsystem.

| # | Source | Path(s) | Use for |
|---|--------|---------|---------|
| 1 | AI Navigation package overview | `Manual/com.unity.ai.navigation.html` | Package description, version (2.0.14 / Unity 6000.3), keyword index only — confirmed stub, no deep component pages locally |
| 2 | `NavMeshAgent` core | `ScriptReference/AI.NavMeshAgent.html` | Class overview: component that moves a GameObject across a navmesh |
| 3 | `NavMeshAgent` movement API | `AI.NavMeshAgent.SetDestination.html`, `AI.NavMeshAgent.Move.html`, `AI.NavMeshAgent.Warp.html`, `AI.NavMeshAgent.Stop.html`, `AI.NavMeshAgent.Resume.html`, `AI.NavMeshAgent.ResetPath.html` | Setting/clearing destinations, relative movement, teleporting, pausing/resuming |
| 4 | `NavMeshAgent` path query API | `AI.NavMeshAgent.CalculatePath.html`, `AI.NavMeshAgent.SamplePathPosition.html`, `AI.NavMeshAgent.FindClosestEdge.html`, `AI.NavMeshAgent.Raycast.html`, `AI.NavMeshAgent.SetPath.html` | Synchronous path pre-computation, edge/point lookups against the agent's own filter |
| 5 | `NavMeshAgent` off-mesh link API | `AI.NavMeshAgent.CompleteOffMeshLink.html`, `AI.NavMeshAgent.ActivateCurrentOffMeshLink.html`, `AI.NavMeshAgent-currentOffMeshLinkData.html`, `AI.NavMeshAgent-nextOffMeshLinkData.html`, `AI.NavMeshAgent-isOnOffMeshLink.html`, `AI.NavMeshAgent-autoTraverseOffMeshLink.html` | Manual vs. automatic off-mesh link traversal |
| 6 | `NavMeshAgent` area/cost API | `AI.NavMeshAgent.GetAreaCost.html`, `AI.NavMeshAgent.SetAreaCost.html`, `AI.NavMeshAgent.GetLayerCost.html`, `AI.NavMeshAgent.SetLayerCost.html`, `AI.NavMeshAgent-areaMask.html`, `AI.NavMeshAgent-walkableMask.html` | Per-agent area cost overrides and traversable-area bitmasks |
| 7 | `NavMeshAgent` body/tuning fields | `AI.NavMeshAgent-radius.html`, `AI.NavMeshAgent-height.html`, `AI.NavMeshAgent-speed.html`, `AI.NavMeshAgent-acceleration.html`, `AI.NavMeshAgent-angularSpeed.html`, `AI.NavMeshAgent-stoppingDistance.html`, `AI.NavMeshAgent-baseOffset.html`, `AI.NavMeshAgent-autoBraking.html`, `AI.NavMeshAgent-autoRepath.html` | Agent capsule size and movement tuning |
| 8 | `NavMeshAgent` avoidance fields | `AI.NavMeshAgent-obstacleAvoidanceType.html`, `AI.NavMeshAgent-avoidancePriority.html`, `AI.ObstacleAvoidanceType.html` (and its 5 quality-level members) | Local-avoidance quality/performance tradeoff, per-agent priority (0=highest, 99=lowest, default 50) |
| 9 | `NavMeshAgent` state/telemetry fields | `AI.NavMeshAgent-hasPath.html`, `AI.NavMeshAgent-pathPending.html`, `AI.NavMeshAgent-pathStatus.html`, `AI.NavMeshAgent-isPathStale.html`, `AI.NavMeshAgent-isOnNavMesh.html`, `AI.NavMeshAgent-isStopped.html`, `AI.NavMeshAgent-remainingDistance.html`, `AI.NavMeshAgent-desiredVelocity.html`, `AI.NavMeshAgent-velocity.html`, `AI.NavMeshAgent-steeringTarget.html`, `AI.NavMeshAgent-nextPosition.html`, `AI.NavMeshAgent-path.html`, `AI.NavMeshAgent-destination.html` | Reading current path/movement state each frame |
| 10 | `NavMeshAgent` transform-sync fields | `AI.NavMeshAgent-updatePosition.html`, `AI.NavMeshAgent-updateRotation.html`, `AI.NavMeshAgent-updateUpAxis.html`, `AI.NavMeshAgent-agentTypeID.html`, `AI.NavMeshAgent-navMeshOwner.html` | Decoupling agent-driven transform updates from Transform (root-motion, custom controllers) |
| 11 | `NavMeshObstacle` | `AI.NavMeshObstacle.html`, `-radius.html`, `-height.html`, `-center.html`, `-size.html`, `-shape.html`, `AI.NavMeshObstacleShape.html` (Box/Capsule members), `-velocity.html` | Static/dynamic blocker shape and dimensions |
| 12 | `NavMeshObstacle` carving fields | `AI.NavMeshObstacle-carving.html`, `-carveOnlyStationary.html`, `-carvingMoveThreshold.html`, `-carvingTimeToStationary.html` | Whether/when the obstacle cuts a real hole in the navmesh |
| 13 | `NavMesh` static sampling/tracing | `AI.NavMesh.SamplePosition.html`, `AI.NavMesh.Raycast.html`, `AI.NavMesh.FindClosestEdge.html`, `AI.NavMesh.CalculatePath.html`, `AI.NavMesh.CalculateTriangulation.html` | Snapping arbitrary points to the mesh, straight-line NavMesh raycasts, standalone path queries, full-mesh triangulation dump |
| 14 | `NavMesh` area/layer cost statics | `AI.NavMesh.SetAreaCost.html`, `AI.NavMesh.GetAreaCost.html`, `AI.NavMesh.SetLayerCost.html`, `AI.NavMesh.GetLayerCost.html`, `AI.NavMesh.GetAreaFromName.html`, `AI.NavMesh.GetAreaNames.html`, `AI.NavMesh.GetNavMeshLayerFromName.html`, `AI.NavMesh.AllAreas.html` | Global default area costs (applies to newly-created agents), area name↔index lookup |
| 15 | `NavMesh` build-settings statics | `AI.NavMesh.CreateSettings.html`, `AI.NavMesh.GetSettingsByID.html`, `AI.NavMesh.GetSettingsByIndex.html`, `AI.NavMesh.GetSettingsCount.html`, `AI.NavMesh.GetSettingsNameFromID.html`, `AI.NavMesh.RemoveSettings.html` | Managing the set of registered `NavMeshBuildSettings` (agent types) at runtime |
| 16 | `NavMesh` data-instance statics | `AI.NavMesh.AddNavMeshData.html`, `AI.NavMesh.RemoveNavMeshData.html`, `AI.NavMesh.RemoveAllNavMeshData.html` | Registering/unregistering baked `NavMeshData` blocks into the active navigation world |
| 17 | `NavMesh` scripted-link statics | `AI.NavMesh.AddLink.html`, `AI.NavMesh.RemoveLink.html`, `AI.NavMesh.IsLinkValid.html`, `AI.NavMesh.IsLinkActive.html`, `AI.NavMesh.SetLinkActive.html`, `AI.NavMesh.IsLinkOccupied.html`, `AI.NavMesh.GetLinkOwner.html`, `AI.NavMesh.SetLinkOwner.html` | Adding/removing off-mesh links purely from code, without the `OffMeshLink`/`NavMeshLink` component |
| 18 | `NavMesh` global avoidance/pathfinder tuning | `AI.NavMesh-avoidancePredictionTime.html` (static float, default 2.0s, tuning range 0.5–5.0), `AI.NavMesh-pathfindingIterationsPerFrame.html` (static int, caps nodes expanded per frame during async pathfinding) | Global crowd-avoidance lookahead and async pathfinder frame-budget knobs |
| 19 | `NavMesh` misc statics | `AI.NavMesh.OnNavMeshPreUpdate.html`, `AI.NavMesh-onPreUpdate.html` | Delegate/event fired once per navmesh update tick, before agents move |
| 20 | `NavMeshPath` | `AI.NavMeshPath.html`, `-corners.html`, `-status.html`, `AI.NavMeshPath.ClearCorners.html`, `AI.NavMeshPath.GetCornersNonAlloc.html`, `AI.NavMeshPath-ctor.html` | Reusable path container returned by `CalculatePath`; corner-point polyline and completeness status |
| 21 | `NavMeshPathStatus` | `AI.NavMeshPathStatus.html` and its 3 members: `PathComplete.html`, `PathPartial.html`, `PathInvalid.html` | Enum for whether a computed path fully, partially, or fails to reach the target |
| 22 | `NavMeshHit` | `AI.NavMeshHit.html`, `-position.html`, `-normal.html`, `-distance.html`, `-mask.html`, `-hit.html` | Out-parameter struct populated by `SamplePosition`/`Raycast` |
| 23 | `NavMeshQueryFilter` | `AI.NavMeshQueryFilter.html`, `-agentTypeID.html`, `-areaMask.html`, `AI.NavMeshQueryFilter.GetAreaCost.html`, `AI.NavMeshQueryFilter.SetAreaCost.html` | Per-query override struct for agent type + per-area costs, used with the filtered overloads of `CalculatePath`/`Raycast`/`FindClosestEdge`/`SamplePosition` |
| 24 | `NavMeshTriangulation` | `AI.NavMeshTriangulation.html`, `-vertices.html`, `-indices.html`, `-areas.html`, `-layers.html` | Raw triangle mesh + per-triangle area indices, from `NavMesh.CalculateTriangulation` (debug draw, custom visualization, external tooling) |
| 25 | `NavMeshBuilder` (low-level runtime bake) | `AI.NavMeshBuilder.html`, `.BuildNavMeshData.html`, `.BuildNavMesh.html`, `.BuildNavMeshAsync.html`, `.UpdateNavMeshData.html`, `.UpdateNavMeshDataAsync.html`, `.BuildNavMeshForMultipleScenes.html`, `.Cancel.html`, `.ClearAllNavMeshes.html`, `-isRunning.html` | Scripted synchronous/async NavMesh baking and incremental rebakes without the package's `NavMeshSurface` wrapper |
| 26 | `NavMeshBuilder` source collection | `AI.NavMeshBuilder.CollectSources.html`, `.CollectSourcesInStage.html` | Gathering `NavMeshBuildSource` lists (renderers/colliders/terrain) from a scene or prefab stage for `BuildNavMeshData` |
| 27 | `NavMeshBuildSettings` | `AI.NavMeshBuildSettings.html` + fields `-agentRadius.html`, `-agentHeight.html`, `-agentSlope.html`, `-agentClimb.html`, `-agentTypeID.html`, `-minRegionArea.html`, `-voxelSize.html`, `-overrideVoxelSize.html`, `-tileSize.html`, `-overrideTileSize.html`, `-ledgeDropHeight.html`, `-maxJumpAcrossDistance.html`, `-buildHeightMesh.html`, `-maxJobWorkers.html`, `-preserveTilesOutsideBounds.html`, `-debug.html`, `.ValidationReport.html` | Full struct describing one agent-type's bake parameters (footprint, voxelization precision, ledge/jump auto-links) |
| 28 | `NavMeshBuildSource` | `AI.NavMeshBuildSource.html` + `-shape.html`, `-area.html`, `-component.html`, `-transform.html`, `-size.html`, `-sourceObject.html`, `-generateLinks.html`, `AI.NavMeshBuildSourceShape.html` (Mesh/Terrain/Box/Sphere/Capsule/ModifierBox members) | One piece of input geometry (or modifier volume) fed into a bake |
| 29 | `NavMeshBuildMarkup` | `AI.NavMeshBuildMarkup.html`, `-root.html`, `-area.html`, `-overrideArea.html`, `-ignoreFromBuild.html`, `-overrideIgnore.html`, `-generateLinks.html`, `-overrideGenerateLinks.html`, `-applyToChildren.html` | Per-subtree area override / exclude-from-bake rules used during `CollectSources` (scripted equivalent of the package's `NavMeshModifier` component) |
| 30 | `NavMeshBuildDebugFlags` / `NavMeshBuildDebugSettings` | `AI.NavMeshBuildDebugFlags.html` (All/None/InputGeometry/PolygonMeshes/PolygonMeshesDetail/RawContours/Regions/SimplifiedContours/Voxels), `AI.NavMeshBuildDebugSettings.html`, `-flags.html` | Enabling intermediate-stage debug visualization during a bake |
| 31 | `NavMeshData` / `NavMeshDataInstance` | `AI.NavMeshData.html`, `-ctor.html`, `-position.html`, `-rotation.html`, `-sourceBounds.html`, `AI.NavMeshDataInstance.html`, `-owner.html`, `-valid.html`, `.Remove.html` | The baked-mesh asset object returned by `BuildNavMeshData`, and the handle returned by `NavMesh.AddNavMeshData` used to remove it later |
| 32 | `NavMeshLinkData` / `NavMeshLinkInstance` | `AI.NavMeshLinkData.html` + `-startPosition.html`, `-endPosition.html`, `-width.html`, `-costModifier.html`, `-bidirectional.html`, `-area.html`, `-agentTypeID.html`, `AI.NavMeshLinkInstance.html`, `-owner.html`, `-valid.html`, `.Remove.html` | Struct/handle pair for `NavMesh.AddLink` — the scripted-code path to off-mesh links (max 65535 active links; adding fails silently-returns-invalid if the area is Not Walkable or the cap is hit) |
| 33 | `OffMeshLink` component | `AI.OffMeshLink.html` + `-startTransform.html`, `-endTransform.html`, `-biDirectional.html`, `-costOverride.html`, `-activated.html`, `-occupied.html`, `-area.html`, `-navMeshLayer.html`, `-autoUpdatePositions.html`, `.UpdatePositions.html` | The Inspector-facing off-mesh link component (manually placed jump/drop connectors) |
| 34 | `OffMeshLinkData` / `OffMeshLinkType` | `AI.OffMeshLinkData.html` + `-startPos.html`, `-endPos.html`, `-linkType.html`, `-owner.html`, `-offMeshLink.html`, `-activated.html`, `-valid.html`, `AI.OffMeshLinkType.html` (LinkTypeManual/LinkTypeDropDown/LinkTypeJumpAcross) | Read-only snapshot of the link an agent is currently/next crossing, and whether it was manually placed vs. auto-generated from a ledge drop or jump-across gap |
| 35 | `NavMeshEditorHelpers` | `AI.NavMeshEditorHelpers.html`, `.CollectSourcesInStage.html`, `.DrawBuildDebug.html` | Editor-only helpers for custom bake tooling / debug gizmo drawing |
| 36 | `NavMeshVisualizationSettings` | `AI.NavMeshVisualizationSettings.html`, `-showNavigation.html` | Scene-view navmesh visualization toggle |
| 37 | `NavMeshCollectGeometry` | `AI.NavMeshCollectGeometry.html` (PhysicsColliders / RenderMeshes members) | Whether a bake reads collider shapes or render mesh geometry as source input |
| 38 | Module overview | `ScriptReference/UnityEngine.AIModule.html` | Assembly-level entry point listing everything implemented in `UnityEngine.AIModule` |
| 39 | Legacy area-name helpers (outside `AI.*`) | `ScriptReference/GameObjectUtility.GetNavMeshAreaFromName.html`, `GameObjectUtility.GetNavMeshAreaNames.html` | Pre-`NavMesh.GetAreaFromName` legacy equivalents, still present and occasionally referenced in older sample code |

38 substantive rows (39 counting the module overview) covering all 252 on-disk `AI.*` pages plus the 2 `GameObjectUtility` legacy helpers and the module-overview page — every path above was opened and its content verified on disk during this pass (2026-08-19), not inferred from filenames alone.

## Key Guidelines

### NavMesh Baking

Unity has two separate baking paths and conflating them causes real confusion. The classic **Navigation window** (Window > AI > Navigation) bakes a single navmesh for the whole open scene at edit time, driven by one `NavMeshBuildSettings` per agent type and the **Navigation Static** flag on each GameObject (set via the Inspector's Static dropdown or the Navigation window's Object tab) — only flagged geometry contributes. The AI Navigation package's `NavMeshSurface` component (pretrained knowledge — no local Manual page exists to cite) instead bakes per-GameObject, either at edit time via its Inspector "Bake" button or at runtime via `NavMeshSurface.BuildNavMesh()`, and supports multiple independent surfaces in one scene (e.g. a ground surface for walking agents and a separate surface at a different agent radius/height for flying or small agents). Under the hood both paths ultimately go through the same `AI.NavMeshBuilder`/`AI.NavMeshBuildSettings`/`AI.NavMeshBuildSource` machinery documented in ScriptReference. For fully scripted control — no `NavMeshSurface` component at all — call `NavMeshBuilder.CollectSources` to gather geometry into a `List<NavMeshBuildSource>`, then `NavMeshBuilder.BuildNavMeshData` to bake it into a `NavMeshData` asset, then `NavMesh.AddNavMeshData` to register it into the live navigation world.

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class RuntimeBake : MonoBehaviour
{
    NavMeshData navMeshData;
    NavMeshDataInstance instance;

    void Start()
    {
        var settings = NavMesh.CreateSettings();
        settings.agentRadius = 0.5f;
        settings.agentHeight = 2.0f;
        settings.agentSlope = 45f;
        settings.agentClimb = 0.4f;

        var sources = new List<NavMeshBuildSource>();
        var bounds = new Bounds(Vector3.zero, new Vector3(100f, 20f, 100f));
        NavMeshBuilder.CollectSources(
            bounds, ~0, NavMeshCollectGeometry.RenderMeshes,
            0, new List<NavMeshBuildMarkup>(), sources);

        navMeshData = NavMeshBuilder.BuildNavMeshData(
            settings, sources, bounds, Vector3.zero, Quaternion.identity);

        instance = NavMesh.AddNavMeshData(navMeshData);
    }

    // Call after procedural geometry changes (destructible walls, spawned terrain)
    public void Rebake()
    {
        var sources = new List<NavMeshBuildSource>();
        var bounds = navMeshData.sourceBounds;
        NavMeshBuilder.CollectSources(
            bounds, ~0, NavMeshCollectGeometry.RenderMeshes,
            0, new List<NavMeshBuildMarkup>(), sources);
        NavMeshBuilder.UpdateNavMeshData(navMeshData, NavMesh.GetSettingsByID(0), sources, bounds);
    }

    void OnDestroy() => instance.Remove();
}
```

`NavMeshBuildSettings` also carries voxelization/tiling precision knobs (`voxelSize`, `tileSize`, `overrideVoxelSize`/`overrideTileSize`) and auto-link generation distances (`ledgeDropHeight`, `maxJumpAcrossDistance`) — leave these at their defaults unless a bake is either too coarse (agents clip corners, thin gaps aren't traversable) or too slow (large open scenes with tiny voxel size).

### NavMeshAgent Movement & Tuning

`NavMeshAgent` autonomously drives its GameObject's Transform every frame once given a destination via `SetDestination(Vector3)`. `SetDestination` returns `true` if the request was accepted and is asynchronous — the path is not necessarily ready the same frame; check `pathPending` (true while the path is still being computed) and `pathStatus` (`PathComplete`/`PathPartial`/`PathInvalid`) before assuming the agent is en route. Because the agent moves the Transform directly, a non-kinematic `Rigidbody` on the same GameObject fights it every frame (physics and navigation both trying to own position) — omit the Rigidbody or set `isKinematic = true`. `radius`, `height`, `speed`, `acceleration`, `angularSpeed`, and `stoppingDistance` should match the character's actual capsule and desired feel; `baseOffset` lifts the visual model relative to the navmesh plane without changing the collision capsule. For cases where you don't want the agent to own the Transform (root-motion animation, a custom CharacterController), set `updatePosition`/`updateRotation` to `false` and read `nextPosition`/`desiredVelocity` each frame to drive the object yourself while the agent still does pathfinding and steering math.

```csharp
using UnityEngine;
using UnityEngine.AI;

public class ClickToMove : MonoBehaviour
{
    NavMeshAgent agent;

    void Start() => agent = GetComponent<NavMeshAgent>();

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out RaycastHit hit))
            {
                agent.SetDestination(hit.point);
            }
        }

        // Poll movement state instead of assuming the agent arrived
        if (!agent.pathPending && agent.remainingDistance <= agent.stoppingDistance
            && (!agent.hasPath || agent.velocity.sqrMagnitude == 0f))
        {
            // Arrived / idle
        }
    }
}
```

Use `CalculatePath` (either `NavMeshAgent.CalculatePath` or the static `NavMesh.CalculatePath`) to synchronously pre-check reachability or plan ahead without moving the agent — it's synchronous and can hurt frame rate on long paths, so budget it to a few calls per frame if evaluating many candidate destinations (e.g. patrol point scoring). `Warp(Vector3)` teleports the agent (and resets its path) without playing movement; use it for spawn placement or cutscene repositioning instead of setting `transform.position` directly, which desyncs the agent's internal navmesh location.

### NavMeshObstacle & Carving

`NavMeshObstacle` marks a GameObject as a blocker other agents should route around. Its `shape` is either `Box` or `Capsule`, sized via `center`/`size`/`radius`/`height`. The critical property is `carving` (bool): when `true`, the obstacle cuts an actual hole in the navmesh so agents path around it in advance rather than only reacting at close range via local avoidance; when `false`, the obstacle only affects `NavMeshAgent` local avoidance steering (cheap, but agents can bunch up against it or clip through pure-obstacle-avoidance limits). Carving recalculation is not free, so Unity gives two throttling knobs: `carveOnlyStationary` (default-equivalent "carve when stationary" behavior — the hole is only cut in when the obstacle isn't moving, avoiding a re-carve every frame for something in constant motion) and, when carving-while-moving is wanted anyway, `carvingMoveThreshold`/`carvingTimeToStationary` control how far/how long the obstacle must be still before it's considered "stationary" and gets re-carved.

```csharp
using UnityEngine;
using UnityEngine.AI;

[RequireComponent(typeof(NavMeshObstacle))]
public class Door : MonoBehaviour
{
    NavMeshObstacle obstacle;

    void Awake()
    {
        obstacle = GetComponent<NavMeshObstacle>();
        obstacle.shape = NavMeshObstacleShape.Box;
        obstacle.carving = true;
        obstacle.carveOnlyStationary = true; // cheap: only re-cuts the hole once the door stops
    }

    public void Open()
    {
        // Animate/move the door; while moving, no re-carve happens (carveOnlyStationary=true),
        // so agents keep routing around the door's last-stationary footprint until it settles.
    }
}
```

For obstacles that move constantly (a swarm of physics-driven crates, another agent's collider used as an obstacle proxy), consider disabling carving entirely and relying purely on `NavMeshAgent` local avoidance (`obstacleAvoidanceType`) instead — carving every frame for many fast-moving obstacles is a common performance cliff (see Advanced Notes).

### Off-Mesh Links

Off-mesh links connect two points that aren't continuously walkable between (a gap to jump, a ledge to drop from, a ladder) so pathfinding can route across the disconnection. There are three ways to create one, all converging on the same runtime `OffMeshLinkData`: (1) the Inspector-facing `OffMeshLink` component with manually assigned `startTransform`/`endTransform`, optionally `biDirectional` and a `costOverride`; (2) auto-generated links created during baking when geometry has a drop within `NavMeshBuildSettings.ledgeDropHeight` or a gap within `maxJumpAcrossDistance` (these surface as `OffMeshLinkType.LinkTypeDropDown` / `LinkTypeJumpAcross` vs. the component's `LinkTypeManual`); (3) fully scripted via `NavMesh.AddLink(NavMeshLinkData)`, which returns a `NavMeshLinkInstance` handle used later to `Remove()` the link — useful for procedurally generated connections with no Inspector object backing them. `NavMeshLinkData` costModifier: a positive value overrides the default cost-by-distance for traversing the link (e.g. make a long jump artificially "cheap" to encourage agents to prefer it, or expensive to discourage it as a last resort).

Whether an agent crosses a link automatically or the game code drives it is controlled by `NavMeshAgent.autoTraverseOffMeshLink`. When `true` (default), the agent animates straight across the link itself. When `false`, the agent stops at the link start and your code must read `currentOffMeshLinkData` to know the link, animate/reposition however you like (jump arc, ladder climb animation), and call `CompleteOffMeshLink()` when done — `ActivateCurrentOffMeshLink(bool)` can also gate whether the agent is allowed to enter a pending link at all.

```csharp
using UnityEngine;
using UnityEngine.AI;

public class ManualLinkTraversal : MonoBehaviour
{
    NavMeshAgent agent;

    void Start()
    {
        agent = GetComponent<NavMeshAgent>();
        agent.autoTraverseOffMeshLink = false;
    }

    void Update()
    {
        if (agent.isOnOffMeshLink)
        {
            OffMeshLinkData data = agent.currentOffMeshLinkData;
            // Custom jump/climb animation driving transform.position from data.startPos to data.endPos
            if (JumpAnimationFinished())
                agent.CompleteOffMeshLink();
        }
    }

    bool JumpAnimationFinished() => true; // placeholder
}
```

```csharp
// Fully scripted link, no OffMeshLink component
var link = new NavMeshLinkData
{
    startPosition = ledgeTop,
    endPosition = ledgeBottom,
    width = 1.0f,
    costModifier = -1f,   // negative = use default distance-based cost
    bidirectional = false,
    area = 0,
    agentTypeID = 0
};
NavMeshLinkInstance handle = NavMesh.AddLink(link);
// later: handle.Remove();
```

### Runtime NavMesh Building (NavMeshSurface — pretrained knowledge, package Manual is thin)

The AI Navigation package's `NavMeshSurface` component is the recommended way to bake at runtime or maintain multiple independent surfaces, but — as noted in Retrieval Sources — there is no local Manual or ScriptReference page for it; everything in this subsection is pretrained knowledge and should be verified against Unity's online package docs before shipping. `NavMeshSurface` wraps the low-level `NavMeshBuilder`/`NavMeshBuildSettings`/`NavMeshBuildSource` API: add one per agent type (e.g. one surface with a small-radius `NavMeshAgent` profile for a cat NPC, a separate surface with a larger radius for a human NPC), set its collect-geometry mode (Render Meshes vs. Physics Colliders) and included layers, and call `.BuildNavMesh()` from code whenever the underlying geometry changes (procedural level generation, destructible terrain, spawned obstacles that should be baked in rather than carved by `NavMeshObstacle`). The companion `NavMeshModifier` component overrides the navigation area for a subtree (equivalent to setting `NavMeshBuildSource.area`/`NavMeshBuildMarkup.overrideArea` in the scripted API), and `NavMeshModifierVolume` applies an area override to everything inside a box volume regardless of what geometry is there (useful for a "danger zone" that should cost more to cross without modeling separate collision geometry). `NavMeshLink` is the package's Inspector-friendly counterpart to `OffMeshLink`/`NavMeshLinkData`, supporting the same start/end/width/bidirectional/cost fields.

```csharp
// Pretrained knowledge — verify against Unity's online AI Navigation package docs
using UnityEngine;
using Unity.AI.Navigation;

public class ProceduralLevelBake : MonoBehaviour
{
    [SerializeField] NavMeshSurface surface;

    public void OnLevelGenerated()
    {
        surface.BuildNavMesh(); // full rebuild; consider async variants for large levels
    }
}
```

### Area Costs & Layers

Navigation **areas** (Default, Not Walkable, Jump, plus up to 29 custom user-defined areas configured in the Navigation window's Areas tab) let you bias pathfinding toward or away from terrain types instead of hard-blocking them, by weighting the relative cost of crossing each area. Costs can be set at three scopes, checked in order of specificity by the pathfinder: a per-query `NavMeshQueryFilter.SetAreaCost`, a per-agent `NavMeshAgent.SetAreaCost` (only affects that one agent and any new agents created afterward inherit the *global* default, not this override), or the global `NavMesh.SetAreaCost` (replaces the default cost used by all agents and by any agent created after the call — cost must be `> 1.0`). Use `NavMesh.GetAreaFromName("water")` to resolve an area's index by its Inspector-configured name rather than hardcoding indices, which shift if areas are reordered.

```csharp
using UnityEngine;
using UnityEngine.AI;

public class ToggleWaterCost : MonoBehaviour
{
    void Update()
    {
        if (Input.anyKeyDown)
        {
            // Make the water area 10x more costly to traverse for all agents.
            NavMesh.SetAreaCost(NavMesh.GetAreaFromName("water"), 10.0f);
        }
    }
}
```

```csharp
// Per-agent override: this one agent avoids lava-tagged area entirely via areaMask,
// while treating "road" as cheaper than its walkable default.
agent.areaMask &= ~(1 << NavMesh.GetAreaFromName("lava"));
agent.SetAreaCost(NavMesh.GetAreaFromName("road"), 0.5f);
```

For query-scoped filtering without touching any agent's persistent state, build a `NavMeshQueryFilter` and pass it to the filtered overloads of `NavMesh.CalculatePath`/`SamplePosition`/`Raycast`/`FindClosestEdge` — this is the right tool for "what if this hypothetical agent type pathed here" checks (e.g. AI planning/utility scoring across multiple candidate agent profiles) without mutating any live agent.

### Path Queries

`NavMeshPath` is a reusable container populated by `CalculatePath` — reuse one instance per caller instead of allocating a new one per query to avoid GC churn in hot loops (e.g. per-frame reachability checks for several AI agents). `NavMeshPath.corners` is the polyline of waypoints; `status` reports `PathComplete` (fully reaches target), `PathPartial` (best-effort path toward an unreachable target — common when the target is off the navmesh or in a disconnected/blocked area), or `PathInvalid` (no path at all). `NavMesh.SamplePosition(sourcePosition, out NavMeshHit hit, maxDistance, areaMask)` is the standard way to validate/snap an arbitrary world point (a raycast hit, a spawn point, a click position) onto the navmesh before using it as a destination — note `hit.normal` is documented as never computed (always `(0,0,0)`), only `hit.position`, `hit.distance`, `hit.mask`, `hit.hit` are meaningful. `NavMesh.Raycast` traces a straight line across the mesh and reports the first point where the mesh becomes untraversable (area mask mismatch or mesh edge) — useful for line-of-sight-along-ground checks distinct from a 3D physics raycast. `NavMesh.CalculateTriangulation()` dumps the entire baked mesh as a flat `NavMeshTriangulation` (vertices/indices/areas/layers) — heavy, intended for debug visualization or external tooling, not per-frame use.

```csharp
using UnityEngine;
using UnityEngine.AI;

public class ReachabilityCheck : MonoBehaviour
{
    NavMeshPath scratchPath = new NavMeshPath(); // reused, not reallocated per call

    public bool CanReach(Vector3 from, Vector3 to)
    {
        if (!NavMesh.SamplePosition(to, out NavMeshHit hit, 2.0f, NavMesh.AllAreas))
            return false; // target isn't near any navmesh

        NavMesh.CalculatePath(from, hit.position, NavMesh.AllAreas, scratchPath);
        return scratchPath.status == NavMeshPathStatus.PathComplete;
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Agents walk through walls after a scene edit | NavMesh wasn't re-baked after geometry changed; re-bake (Navigation window or `NavMeshSurface.BuildNavMesh()` / `NavMeshBuilder.UpdateNavMeshData`) whenever static geometry moves |
| NavMeshAgent jitters or fights physics | A non-kinematic Rigidbody on the same GameObject is also driving position; remove it or set `isKinematic = true` |
| Obstacle carving tanks frame rate | Many `NavMeshObstacle`s carving every frame is expensive; enable `carveOnlyStationary` for frequently-moving obstacles, tune `carvingMoveThreshold`/`carvingTimeToStationary`, or disable carving and rely on agent local avoidance instead |
| Agent refuses to reach a destination | Destination point isn't on the navmesh (off the baked area, or in an area excluded by the agent's `areaMask`); use `NavMesh.SamplePosition` to snap to the nearest valid point first, and check `pathStatus` for `PathPartial`/`PathInvalid` rather than assuming success |
| Off-mesh link never triggers | `autoTraverseOffMeshLink` is false and no code calls `CompleteOffMeshLink`, or link endpoints aren't close enough to navmesh edges for an auto-generated connection |
| Single scene-wide bake used for both ground and flying/swimming agents | The legacy Navigation window bakes one mesh; use separate `NavMeshSurface` components (or separate `NavMeshBuildSettings`/agent-type IDs) per agent profile instead |
| Setting `transform.position` directly instead of `Warp()` | Desyncs the agent's internal navmesh location from its actual world position, causing `isOnNavMesh` to go false or path queries to silently fail; always use `agent.Warp(pos)` for teleports |
| Assuming `SetDestination` moves the agent immediately | Path computation is asynchronous; `pathPending` stays true for one or more frames, and the caller must poll it (or `pathStatus`) rather than assume the path is ready the same frame |
| Calling `CalculatePath`/`NavMesh.CalculatePath` on many candidates every frame | Both are synchronous and expensive on long paths; batch/throttle path-feasibility checks (e.g. a few per frame) instead of scoring dozens of waypoints in one frame |
| Reusing a global `NavMesh.SetAreaCost` call and expecting only one agent to change | `NavMesh.SetAreaCost` replaces the cost for *all* agents (and the default for future ones); use `NavMeshAgent.SetAreaCost` (per-agent) or a `NavMeshQueryFilter` (per-query) for scoped overrides |
| Forgetting off-mesh link cap | `NavMesh.AddLink` silently fails (returns an invalid instance) past 65535 active links or when the target area is Not Walkable; check `NavMeshLinkInstance.valid` after adding |
| Allocating a new `NavMeshPath` every query | `NavMeshPath` is a reusable container; allocate once and reuse via `CalculatePath(pos, path)` to avoid per-call GC pressure in hot AI loops |
| Assuming `NavMeshHit.normal` is populated after `SamplePosition` | Documented as always `(0,0,0)` for that call — only `position`/`distance`/`mask`/`hit` carry real data; don't rely on `normal` from `SamplePosition` (it is populated by other queries like physics raycasts, not this one) |
| Treating `NavMeshModifier`/`NavMeshModifierVolume`/`NavMeshLink` guidance as doc-verified | These are AI Navigation package components with no local Manual/ScriptReference pages found; the guidance here is pretrained knowledge — cross-check Unity's online package manual before relying on exact field names |
| Baking with default voxel/tile size on a very large or very detailed scene | Slow bakes or missed thin geometry; tune `NavMeshBuildSettings.voxelSize`/`tileSize` (via `overrideVoxelSize`/`overrideTileSize`) rather than accepting whatever the agent-radius-derived default produces |

## Quick Reference

| Item | Purpose |
|------|---------|
| `NavMeshAgent` | Component that autonomously moves a GameObject along the navmesh toward a destination |
| `NavMeshAgent.SetDestination(Vector3)` | Requests an async path to a point; returns bool accepted, poll `pathPending`/`pathStatus` |
| `NavMeshAgent.CalculatePath` / `NavMesh.CalculatePath` | Synchronous path pre-check into a reusable `NavMeshPath`; throttle — expensive on long paths |
| `NavMeshAgent.Warp(Vector3)` | Teleports the agent and resyncs its internal navmesh location; use instead of `transform.position =` |
| `NavMeshAgent.Move(Vector3)` | Applies relative movement, adjusting the current path if one exists |
| `NavMeshAgent.Stop()` / `.Resume()` / `.ResetPath()` | Pause, resume, or clear the current path |
| `NavMeshAgent.radius/height/speed/acceleration/angularSpeed/stoppingDistance/baseOffset` | Core body and movement tuning fields |
| `NavMeshAgent.obstacleAvoidanceType` / `avoidancePriority` | Local-avoidance quality (5 levels) vs. performance; per-agent priority 0 (highest) – 99 (lowest), default 50 |
| `NavMeshAgent.updatePosition/updateRotation/updateUpAxis` | Decouple agent from driving Transform directly (root motion, custom controllers) |
| `NavMeshAgent.areaMask` / `.SetAreaCost` / `.GetAreaCost` | Per-agent traversable-area bitmask and cost overrides |
| `NavMeshObstacle` | Dynamic blocker; optionally carves a hole in the navmesh |
| `NavMeshObstacle.carving` / `carveOnlyStationary` / `carvingMoveThreshold` / `carvingTimeToStationary` | Whether/when the obstacle cuts navmesh geometry, throttled for performance |
| `NavMeshSurface` (AI Navigation package, pretrained) | Per-GameObject runtime/edit-time bake target, supports multiple surfaces |
| `NavMeshModifier` / `NavMeshModifierVolume` (package, pretrained) | Per-subtree / per-volume area override for baking |
| `NavMeshLink` (package, pretrained) | Inspector component wrapping `NavMeshLinkData`/`NavMesh.AddLink` |
| `NavMeshBuilder.BuildNavMeshData` / `.UpdateNavMeshData` / async variants | Low-level scripted API for building/updating `NavMeshData` at runtime |
| `NavMeshBuilder.CollectSources` | Gathers `NavMeshBuildSource` list from scene geometry for a manual bake |
| `NavMeshBuildSettings` | Per-agent-type bake parameters: radius, height, slope, climb, voxel/tile size, ledge/jump link distances |
| `NavMeshData` / `NavMeshDataInstance` | Baked mesh asset, and the handle used to add/remove it from the live nav world via `NavMesh.AddNavMeshData` |
| `OffMeshLink` / `NavMeshLinkData` / `NavMesh.AddLink` | Manual (component), data (struct), and scripted (static call) paths to connect a navmesh gap |
| `NavMeshAgent.autoTraverseOffMeshLink` / `.CompleteOffMeshLink()` | Automatic vs. manually-driven crossing of an off-mesh link |
| `NavMesh.SamplePosition` | Finds the nearest point on the navmesh to an arbitrary world position (note: `hit.normal` always zero) |
| `NavMesh.Raycast` | Straight-line trace across the navmesh, stopping at the mesh edge or an excluded area |
| `NavMesh.SetAreaCost` / `GetAreaFromName` / `GetAreaNames` | Global area cost and name↔index resolution |
| `NavMeshQueryFilter` | Per-query agentTypeID + area-cost override, doesn't mutate any agent |
| `NavMeshPath` / `NavMeshPathStatus` | Reusable path container; status is Complete/Partial/Invalid |
| `NavMeshTriangulation` | Full baked-mesh dump (vertices/indices/areas/layers) via `NavMesh.CalculateTriangulation` |
| `NavMesh.avoidancePredictionTime` | Global static float (default 2.0s) — how far ahead agents predict collisions for local avoidance |
| `NavMesh.pathfindingIterationsPerFrame` | Global static int — caps nodes expanded per frame for async pathfinding, smooths frame time on long/many paths |
| Navigation Static flag | Marks geometry to be included in a legacy Navigation-window bake |

## Advanced Notes

**Multi-agent crowd movement.** `NavMeshAgent` local avoidance (steering agents around each other, not around baked-in obstacles) is governed by `obstacleAvoidanceType` (`NoObstacleAvoidance`, `LowQualityObstacleAvoidance`, `MedQualityObstacleAvoidance`, `GoodQualityObstacleAvoidance`, `HighQualityObstacleAvoidance`) — this is a per-agent tradeoff between steering realism and CPU cost, and it does not scale linearly: a scene with a few dozen `HighQualityObstacleAvoidance` agents can cost noticeably more than the same count at `LowQualityObstacleAvoidance` or `NoObstacleAvoidance`, because avoidance quality controls how many neighboring agents/obstacles are considered and how far ahead collisions are predicted. `avoidancePriority` (0–99, default 50) lets higher-priority agents (e.g. the player, key NPCs) push through crowds of lower-priority agents that yield instead of mutually contesting space — assign priority bands rather than leaving everything at 50, or dense crowds visibly jostle without resolving. `NavMesh.avoidancePredictionTime` (global static, default 2.0s, documented tuning range 0.5–5.0s) is the single biggest lever for crowd behavior at scale: shorter prediction windows make avoidance cheaper and more reactive/twitchy, longer windows make it smoother but pricier since more of each agent's future path must be checked against neighbors each tick. For genuinely large crowds (hundreds of agents), mixing avoidance quality by relevance — full quality only for agents near the camera/player, `NoObstacleAvoidance` or `LowQualityObstacleAvoidance` for background/offscreen agents — is a practical way to keep total avoidance cost bounded; this is a pattern to implement yourself (e.g. LOD-style distance checks each agent's script performs on itself) since the API has no built-in distance-based LOD for avoidance quality. `NavMesh.pathfindingIterationsPerFrame` bounds how many navmesh-polygon nodes the *asynchronous* pathfinder expands per frame — raising it makes individual async path requests resolve faster (fewer frames until `pathPending` goes false) at the cost of a frame-time spike when many agents request paths simultaneously (e.g. a wave-spawn of enemies all calling `SetDestination` the same frame); lowering it spreads that cost across more frames but delays path availability. Because it only affects the async path (the one behind `SetDestination`), it does not throttle the synchronous `CalculatePath`/`NavMesh.CalculatePath` calls, which always resolve within the calling frame regardless of this setting — that's precisely why those synchronous calls need their own manual per-frame throttling (see Common Mistakes) rather than relying on this knob.

**Dynamic obstacle performance.** Carving (`NavMeshObstacle.carving = true`) is the most expensive per-obstacle cost in the navigation system because it triggers a real navmesh polygon regeneration in the obstacle's vicinity, not just a steering adjustment — this is why `carveOnlyStationary` defaults to the recommended behavior of only re-cutting the hole once an obstacle stops moving, rather than re-carving every frame for something in constant motion. When an obstacle must carve while moving (a slow-moving elevator platform, a rolling boulder that should block path planning ahead of time, not just steer around reactively), `carvingMoveThreshold` (how far it must move to be considered "moving" again after settling) and `carvingTimeToStationary` (how long it must stay still before being re-carved as stationary) should be tuned together: a low threshold plus long stationary-time produces frequent flapping between carved/uncarved states (worst case for cost), while a high threshold plus short stationary-time produces stale carve holes that lag noticeably behind the obstacle's actual position. For scenes with many simultaneously-moving obstacles (a physics-driven debris field, a crowd of NPCs each also acting as obstacles for others), the practical ceiling is usually reached well before the agent-count ceiling — profile with the Unity Profiler's AI/Navigation markers specifically, since generic CPU profiling can misattribute carving cost to "physics" or "scripts" depending on what triggers the recarve. A common escape hatch when carving cost dominates: turn carving off for that category of obstacle entirely and let `NavMeshAgent` local avoidance (obstacleAvoidanceType) handle it reactively instead — this trades "agents plan routes around the obstacle in advance" for "agents steer around it at close range," which is often an acceptable and much cheaper tradeoff for obstacles that are numerous, small, or short-lived (dropped items, thrown projectiles, temporary hazards) rather than large fixed-duration blockers (a closed door, a parked vehicle) where advance route planning actually matters to believability.

---
name: unity-world-building
description: Use when authoring Unity environments — the Terrain system, ProBuilder level geometry, or other world-building tools. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity World Building

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Terrain overview & module | `Manual/script-Terrain.html`, `Manual/terrain-UsingTerrains.html`, `Manual/com.unity.modules.terrain.html`, `Manual/com.unity.modules.terrainphysics.html` | Terrain GameObject basics, what the Terrain/TerrainPhysics built-in modules provide |
| Terrain Tools package & overlay toolbox | `Manual/TerrainTools.html`, `Manual/com.unity.terrain-tools.html` | Terrain Tools overlay layout, brush categories (Sculpt/Materials/Foliage/Neighbor Terrains), package vs. built-in tool differences |
| Height fundamentals | `Manual/terrain-Intro-to-Height.html`, `Manual/terrain-Height-Landing.html` | Max height/depth clamp per tile, heightmap resolution tradeoffs, "before you start" sculpting checklist |
| Heightmap data & import/export | `Manual/terrain-Heightmaps.html` | What a heightmap is (grayscale texture, one value per grid point), importing/exporting `.raw`/`.png` heightmaps |
| Basic sculpting workflow | `Manual/terrain-Basic-Sculpting.html` | End-to-end sculpting tutorial flow tying the primary tools together |
| Primary sculpt tools | `Manual/terrain-Sculpt.html`, `Manual/terrain-RaiseLowerTerrain.html`, `Manual/terrain-SetHeight.html`, `Manual/terrain-SmoothHeight.html` | Raise/Lower, Set Height (absolute height stamping), Smooth Height brush behavior |
| Advanced sculpt tools | `Manual/terrain-StampTerrain.html`, `Manual/terrain-Sculpt-Bridge.html`, `Manual/terrain-Sculpt-Clone.html`, `Manual/terrain-Sculpt-Terrace.html` | Stamp (brush-shape height stamping), Bridge (connect two points), Clone (sample-and-paint height), Terrace |
| Noise-based sculpting | `Manual/terrain-Sculpt-Noise-Landing.html`, `Manual/terrain-Sculpt-Noise-Tool.html`, `Manual/terrain-Noise-Ref.html`, `Manual/terrain-Noise-Types.html`, `Manual/terrain-Noise-Other-Uses.html` | Procedural noise brush, noise type reference (Perlin/Voronoi/ridge etc.), using noise for masks beyond height |
| Transform brushes | `Manual/terrain-Transform.html`, `Manual/terrain-Transform-Pinch.html`, `Manual/terrain-Transform-Smudge.html`, `Manual/terrain-Transform-Twist.html` | Pinch/Smudge/Twist brushes for reshaping existing sculpted features |
| Erosion simulation | `Manual/terrain-Erosion.html`, `Manual/terrain-Erosion-Hydraulic.html`, `Manual/terrain-Erosion-Thermal.html`, `Manual/terrain-Erosion-Wind.html`, `Manual/terrain-Erosion-Considerations.html` | Hydraulic (sediment carried by water), Thermal (angle-of-repose settling), Wind (dune-forming) erosion tools and prep considerations |
| Height effects filters | `Manual/terrain-Effects.html`, `Manual/terrain-Effects-Contrast.html`, `Manual/terrain-Effects-Flatten.html`, `Manual/terrain-Effects-Sharpen.html` | Contrast/Flatten/Sharpen post-process filters applied to sculpted height data |
| Terrain holes | `Manual/terrain-PaintHoles.html`, `ScriptReference/TerrainData.SetHoles.html`, `ScriptReference/TerrainData.GetHoles.html`, `ScriptReference/TerrainData-holesResolution.html`, `ScriptReference/TerrainData-holesTexture.html`, `ScriptReference/TerrainData-enableHolesTextureCompression.html`, `ScriptReference/TerrainData.SetHolesDelayLOD.html` | Cutting caves/overhangs/tunnel mouths into terrain via the holes mask |
| Texture painting workflow | `Manual/terrain-PaintTexture.html`, `Manual/terrain-Textures-Landing.html` | Paint Texture tool usage, flood-fill-on-first-layer behavior, Materials Mode overlay |
| Terrain Layers (Manual + API) | `Manual/class-TerrainLayer.html`, `ScriptReference/TerrainLayer.html`, `ScriptReference/TerrainLayer-diffuseTexture.html`, `ScriptReference/TerrainLayer-normalMapTexture.html`, `ScriptReference/TerrainLayer-maskMapTexture.html`, `ScriptReference/TerrainLayer-tileSize.html`, `ScriptReference/TerrainLayer-tileOffset.html`, `ScriptReference/TerrainLayer-smoothness.html`, `ScriptReference/TerrainLayer-metallic.html`, `ScriptReference/TerrainLayer-normalScale.html` | Terrain Layer asset structure: diffuse/normal/mask maps, tiling, PBR surface params |
| Alphamap/splatmap scripting | `ScriptReference/TerrainData.SetAlphamaps.html`, `ScriptReference/TerrainData.GetAlphamaps.html`, `ScriptReference/TerrainData-alphamapResolution.html`, `ScriptReference/TerrainData-alphamapLayers.html`, `ScriptReference/TerrainData-alphamapTextureCount.html`, `ScriptReference/TerrainData.GetAlphamapTexture.html` | Reading/writing the per-layer blend weights (splatmap) that drive texture painting at runtime or in editor tools |
| Tree placement & prototypes | `Manual/terrain-Trees.html`, `Manual/terrain-Trees-Landing.html`, `Manual/terrain-Tree-From-Mesh.html`, `Manual/terrain-Tree-Hierarchy-UI.html` | Paint Trees tool, tree prototype setup, building tree assets from arbitrary meshes |
| Tree collision & wind | `Manual/terrain-Tree-Colliders.html`, `Manual/terrain-trees-wind-zones.html`, `Manual/terrain-Wind-Reference.html`, `Manual/terrain-Trees-Mat-Shaders.html` | Adding capsule colliders to painted trees, Wind Zone interaction, tree shader/material requirements |
| Tree LOD & performance | `Manual/terrain-Tree-LOD.html`, `Manual/terrain-Tree-Performance.html` | Billboard crossfade distance, polygon-count guidance, per-tile tree draw settings |
| Grass & detail meshes | `Manual/terrain-Grass.html` | Detail render modes: Instanced Mesh (GPU instancing, recommended), Vertex Lit mesh (combined mesh, no instancing), Grass mesh (up-facing, wind-reactive), Grass Texture (billboard quads) |
| Detail scripting | `ScriptReference/TerrainData.SetDetailLayer.html`, `ScriptReference/TerrainData.GetDetailLayer.html`, `ScriptReference/TerrainData-detailPrototypes.html`, `ScriptReference/TerrainData-detailResolution.html`, `ScriptReference/TerrainData-detailResolutionPerPatch.html`, `ScriptReference/TerrainData-detailScatterMode.html`, `ScriptReference/TerrainData.SetDetailScatterMode.html`, `ScriptReference/TerrainData.ComputeDetailCoverage.html`, `ScriptReference/TerrainData.ComputeDetailInstanceTransforms.html` | Per-pixel density maps driving procedural detail placement, patch-based culling resolution |
| Multi-tile / neighbor terrains | `Manual/terrain-CreateNeighborTerrains.html`, `ScriptReference/Terrain.SetNeighbors.html`, `ScriptReference/Terrain-allowAutoConnect.html`, `ScriptReference/Terrain-groupingID.html`, `ScriptReference/Terrain-topNeighbor.html`, `ScriptReference/Terrain-bottomNeighbor.html`, `ScriptReference/Terrain-leftNeighbor.html`, `ScriptReference/Terrain-rightNeighbor.html`, `ScriptReference/TerrainUtils.TerrainMap.html`, `ScriptReference/TerrainUtils.TerrainUtility.AutoConnect.html` | Creating adjacent tiles, wiring neighbor references so edges stitch seamlessly, grouping ID for selective auto-connect |
| Terrain Settings reference | `Manual/terrain-OtherSettings.html` | Per-tile inspector settings: Grouping ID, Auto Connect, Draw, Base/Detail/Tree distances, mesh resolution, lighting options |
| Runtime terrain usage | `Manual/terrain-Runtime.html` | Build-time resource inclusion caveat: keep at least one Terrain instance/placeholder in a build-profile scene or runtime-only Terrain creation silently omits engine resources |
| Shader Graph terrain rendering | `Manual/terrain-shader-graph.html`, `Manual/urp/shader-terrain-lit.html`, `Manual/urp/prebuilt-shader-graphs-urp-terrain-lit.html` | Authoring a custom terrain shader in Shader Graph, URP Terrain Lit prebuilt graph |
| Terrain quality settings (Manual concept + ScriptReference knobs) | `ScriptReference/QualitySettings-terrainPixelError.html`, `ScriptReference/QualitySettings-terrainBasemapDistance.html`, `ScriptReference/QualitySettings-terrainDetailDistance.html`, `ScriptReference/QualitySettings-terrainDetailDensityScale.html`, `ScriptReference/QualitySettings-terrainTreeDistance.html`, `ScriptReference/QualitySettings-terrainBillboardStart.html`, `ScriptReference/QualitySettings-terrainFadeLength.html`, `ScriptReference/QualitySettings-terrainMaxTrees.html`, `ScriptReference/QualitySettings-terrainQualityOverrides.html` | Global quality-tier knobs for LOD/draw distance/billboarding, and per-Terrain override flags (`TerrainQualityOverrides`) that opt individual tiles out of the global values |
| `Terrain` component API | `ScriptReference/Terrain.html`, `ScriptReference/Terrain-terrainData.html`, `ScriptReference/Terrain-heightmapPixelError.html`, `ScriptReference/Terrain-basemapDistance.html`, `ScriptReference/Terrain-treeDistance.html`, `ScriptReference/Terrain-treeBillboardDistance.html`, `ScriptReference/Terrain-treeCrossFadeLength.html`, `ScriptReference/Terrain-treeMaximumFullLODCount.html`, `ScriptReference/Terrain-detailObjectDistance.html`, `ScriptReference/Terrain-detailObjectDensity.html`, `ScriptReference/Terrain.SampleHeight.html`, `ScriptReference/Terrain.GetPosition.html`, `ScriptReference/Terrain.Flush.html`, `ScriptReference/Terrain-activeTerrain.html`, `ScriptReference/Terrain-activeTerrains.html`, `ScriptReference/Terrain.GetActiveTerrains.html` | Per-instance rendering/LOD tuning, sampling world-space height, iterating all live Terrain instances |
| `TerrainData` API core | `ScriptReference/TerrainData.html`, `ScriptReference/TerrainData-size.html`, `ScriptReference/TerrainData-heightmapResolution.html`, `ScriptReference/TerrainData-heightmapScale.html`, `ScriptReference/TerrainData-bounds.html`, `ScriptReference/TerrainData-terrainLayers.html`, `ScriptReference/TerrainData-treeInstances.html`, `ScriptReference/TerrainData-treePrototypes.html` | Core data asset: size, resolution, bounds, and collection accessors backing everything painted on the Terrain |
| `TerrainData` height read/write methods | `ScriptReference/TerrainData.SetHeights.html`, `ScriptReference/TerrainData.GetHeights.html`, `ScriptReference/TerrainData.SetHeightsDelayLOD.html`, `ScriptReference/TerrainData.SyncHeightmap.html`, `ScriptReference/TerrainData.GetHeight.html`, `ScriptReference/TerrainData.GetInterpolatedHeight.html`, `ScriptReference/TerrainData.GetInterpolatedHeights.html`, `ScriptReference/TerrainData.GetInterpolatedNormal.html`, `ScriptReference/TerrainData.GetSteepness.html`, `ScriptReference/TerrainData.DirtyHeightmapRegion.html`, `ScriptReference/TerrainData.CopyActiveRenderTextureToHeightmap.html` | `SetHeights` (expensive, full LOD/vegetation recompute) vs. `SetHeightsDelayLOD` + `SyncHeightmap` (cheap per-edit, sync once) — the load-bearing distinction for runtime/editor sculpting tools |
| `TerrainData` tree instance methods | `ScriptReference/TerrainData.SetTreeInstances.html`, `ScriptReference/TerrainData.SetTreeInstance.html`, `ScriptReference/TerrainData.GetTreeInstance.html`, `ScriptReference/TerrainData-treeInstanceCount.html`, `ScriptReference/Terrain.AddTreeInstance.html` | Bulk vs. single tree-instance writes, snapping trees to the heightmap on write |
| Terrain colliders (Manual + API) | `Manual/terrain-colliders-introduction.html`, `Manual/terrain-colliders.html`, `Manual/class-TerrainCollider.html`, `ScriptReference/TerrainCollider.html`, `ScriptReference/TerrainCollider-terrainData.html`, `ScriptReference/PhysicsVisualizationSettings-terrainTilesMax.html`, `ScriptReference/LowLevelPhysics.TerrainGeometry.html` | Auto-generated collision mesh matching the heightmap, low-level physics terrain geometry type, gizmo tile-count cap for visualizing terrain colliders |
| Terrain change callbacks | `ScriptReference/TerrainCallbacks.html`, `ScriptReference/TerrainCallbacks-heightmapChanged.html`, `ScriptReference/TerrainCallbacks.HeightmapChangedCallback.html`, `ScriptReference/TerrainCallbacks-textureChanged.html`, `ScriptReference/TerrainChangedFlags.html` | Subscribing to editor/runtime terrain-edit events instead of polling, `TerrainChangedFlags` bitmask for filtering which change fired |
| Custom terrain brush/tool authoring (`UnityEditor.TerrainTools` namespace) | `ScriptReference/TerrainTools.TerrainPaintTool_1.html`, `ScriptReference/TerrainTools.PaintContext.html`, `ScriptReference/TerrainTools.BrushTransform.html`, `ScriptReference/TerrainTools.IOnPaint.html`, `ScriptReference/TerrainTools.IOnSceneGUI.html`, `ScriptReference/TerrainTools.TerrainPaintUtility.html` | Writing a custom Editor brush/tool that plugs into the Terrain Tools overlay (advanced/editor-tooling use only) |
| ProBuilder (package stub only) | `Manual/com.unity.probuilder.html` | Package description/keywords only — see note below |

**ProBuilder coverage note**: local docs have only the package landing-page stub (description: "Build, edit, and texture custom geometry in Unity... in-scene level design, prototyping, collision meshes"; keywords: 3d, model, mesh, modeling, geometry, shape, cube, blender, max; no tutorials, no ScriptReference). Answer ProBuilder questions from general Unity 6 pretrained knowledge, explicitly say retrieval coverage is thin, and point to `docs.unity3d.com/Packages/com.unity.probuilder@latest` for authoritative API/tutorial details.

## Key Guidelines

### Terrain Heightmap Sculpting

A Terrain's height is not per-vertex mesh geometry — it is a grayscale **heightmap** texture where each texel stores a height value from 0 to 1, scaled by the tile's `TerrainData.size.y`. Two hard constraints from `terrain-Intro-to-Height.html` shape every sculpting decision: each tile has a maximum height (Terrain Settings > Mesh Resolution > Terrain Height) beyond which the top of a mountain simply flattens with no further detail possible, and you cannot sculpt *below* the tile's GameObject y-position — for sunken features (lakes, riverbeds, canyons) you must first use Set Height to lower a starting plateau, then sculpt within it. Heightmap resolution (`TerrainData.heightmapResolution`) is clamped by Unity to one of 33, 65, 129, 257, 513, 1025, 2049, or 4097 texels; picking this value is a one-time-cheap, change-later-expensive decision (per `terrain-Heightmaps.html`) because resampling an existing heightmap to a new resolution can distort or lose sculpted detail, so lock in a target resolution before investing sculpting time. Above raw Raise/Lower/Set Height/Smooth, Unity 6 ships Stamp (brush-shaped height stamping), Bridge (connects two picked points with a ramp), Clone (samples height from one area and paints it elsewhere), Terrace, and a full noise-brush suite (Perlin/ridge/Voronoi et al. via `terrain-Sculpt-Noise-Tool.html`) plus Transform brushes (Pinch/Smudge/Twist) for reshaping already-sculpted terrain, and a three-pass erosion simulation (Hydraulic — sediment travels with water and can move long distances, carving riverbanks; Thermal — sediment settles at its angle of repose, limiting how steep cliffs/mountains can form and not traveling far; Wind — sediment travels far and builds dune-like features at the end of its path) for physically-plausible large-scale landforms.

Runtime/editor scripting against the heightmap has a critical performance fork: `TerrainData.SetHeights` recomputes all LOD and vegetation placement information on every call, which is expensive enough to stutter interactive editing or per-frame runtime modification. `TerrainData.SetHeightsDelayLOD` writes the same height data without that recompute (the terrain may transiently render at the wrong LOD), and you call `TerrainData.SyncHeightmap()` once after a batch of edits (e.g., on mouse-up, or once per streaming-tile load) to flush the deferred LOD/vegetation update.

```csharp
using UnityEngine;

public class CraterStamper : MonoBehaviour
{
    public Terrain terrain;
    public Vector2Int centerTexel = new Vector2Int(256, 256);
    public int radius = 40;
    public float depth = 0.05f; // normalized height units (0..1)

    public void StampCrater()
    {
        TerrainData data = terrain.terrainData;
        int size = radius * 2;
        int xBase = Mathf.Clamp(centerTexel.x - radius, 0, data.heightmapResolution - size);
        int yBase = Mathf.Clamp(centerTexel.y - radius, 0, data.heightmapResolution - size);

        // heights array is indexed [y, x], values 0..1
        float[,] heights = data.GetHeights(xBase, yBase, size, size);
        for (int y = 0; y < size; y++)
        {
            for (int x = 0; x < size; x++)
            {
                float dist = Vector2.Distance(new Vector2(x, y), new Vector2(radius, radius));
                float falloff = Mathf.Clamp01(1f - dist / radius);
                heights[y, x] = Mathf.Max(0f, heights[y, x] - depth * falloff * falloff);
            }
        }

        // Interactive/per-frame path: cheap write, no LOD recompute yet.
        data.SetHeightsDelayLOD(xBase, yBase, heights);
    }

    void OnMouseUp() => terrain.terrainData.SyncHeightmap(); // flush LOD/vegetation once, on release
}
```

### Terrain Texture Painting & Layers

Texture painting is driven by **Terrain Layers** (`TerrainLayer` assets: a diffuse texture, optional normal map, optional mask map bundling smoothness/metallic/AO channels, plus tile size/offset), blended per-pixel via an alphamap (splatmap) baked into `TerrainData`. Per `terrain-PaintTexture.html`, the very first Terrain Layer you add flood-fills the entire tile — there is no "default/no layer" state, so plan your base layer (the tiling ground texture that shows through everywhere nothing else is painted) before adding detail layers. Terrain Layers are standalone Assets, reusable across every tile in a scene, so a shared layer library (rock, grass, sand, snow, dirt-path) keeps texel-level identity consistent across a multi-tile world and lets you re-tune one texture globally. Each additional layer costs one more alphamap sample per rendered terrain pixel, so layer count is a direct fill-rate cost, not just an authoring convenience — consolidate similar surfaces (e.g., one "rock" layer with height/slope-based procedural variation via a custom shader) rather than adding a new layer for every visual variant.

Alphamap scripting reads/writes a `float[,,]` array indexed `[y, x, layerIndex]` where the last axis sums to 1 per texel across all layers on that tile:

```csharp
using UnityEngine;

public class SlopePainter : MonoBehaviour
{
    public Terrain terrain; // Layer 0 = grass, Layer 1 = rock

    void Start()
    {
        TerrainData data = terrain.terrainData;
        float[,,] map = new float[data.alphamapWidth, data.alphamapHeight, data.alphamapLayers];

        for (int y = 0; y < data.alphamapHeight; y++)
        {
            for (int x = 0; x < data.alphamapWidth; x++)
            {
                float normX = x / (float)(data.alphamapWidth - 1);
                float normY = y / (float)(data.alphamapHeight - 1);
                float angle = data.GetSteepness(normX, normY); // 0..90 degrees
                float rockWeight = Mathf.Clamp01(angle / 45f);  // steep => more rock

                // array is [y, x, layer]
                map[y, x, 0] = 1f - rockWeight; // grass
                map[y, x, 1] = rockWeight;      // rock
            }
        }
        data.SetAlphamaps(0, 0, map);
    }
}
```

### Tree & Detail Placement

Trees and details (grass/foliage) paint onto the terrain through **prototypes**: `TreePrototype` entries reference a prefab, `DetailPrototype` entries reference either a mesh or a texture. Trees render as real instanced GameObjects at close range and crossfade to camera-facing billboards at distance (`treeBillboardDistance`/`treeCrossFadeLength` on `Terrain`, or the global `QualitySettings.terrainBillboardStart`); per `terrain-Tree-Performance.html`, tree performance is dominated by polygon count of the tree model itself, so simplify meshes/reduce leaf-and-branch counts before relying on distance culling to hide the cost, and always verify the visible LOD/billboard transition distance doesn't create a "pop" the player notices. Detail rendering (`terrain-Grass.html`) offers four render modes with real cost differences: **Instanced Mesh** (GPU-instanced, Unity's recommended default for most cases), **Vertex Lit mesh** (all instances combined into one static mesh — no GPU instancing, so instance-count scaling is worse, and shading is simplified), **Grass mesh** (like Vertex Lit but normals are forced upward and the mesh reacts to wind), and **Grass Texture** (billboard quads generated directly from a texture with optional camera-facing). Detail density maps (`TerrainData.SetDetailLayer`) are per-pixel grayscale layers where each texel's value is the number of detail instances to scatter in that cell, and `TerrainData.detailScatterMode` controls whether that count is interpreted deterministically or randomized — `detailResolutionPerPatch` governs the culling-patch granularity, trading CPU culling overhead against per-patch draw-call batching.

```csharp
using UnityEngine;

public class DetailThinner : MonoBehaviour
{
    public Terrain terrain;
    public int detailLayer = 0;
    public float threshold = 3f;

    void ThinSparseGrass()
    {
        TerrainData data = terrain.terrainData;
        int[,] map = data.GetDetailLayer(0, 0, data.detailWidth, data.detailHeight, detailLayer);

        for (int y = 0; y < data.detailHeight; y++)
            for (int x = 0; x < data.detailWidth; x++)
                if (map[x, y] < threshold) map[x, y] = 0; // cull sparse/isolated grass instances

        data.SetDetailLayer(0, 0, detailLayer, map);
    }
}
```

### Terrain Performance (Pixel Error, LOD, Draw Distance)

Terrain LOD and draw-distance behavior is controlled by a mix of global `QualitySettings` terrain knobs and per-`Terrain` instance overrides gated by `Terrain.ignoreQualitySettings`/`TerrainData`'s `TerrainQualityOverrides` flags — this lets most tiles follow the project's quality tier while a hero/close-up tile pins tighter settings. `terrainPixelError` (mirrored per-instance as `Terrain.heightmapPixelError`) is the single highest-leverage LOD knob: it caps the allowed screen-space error (in pixels) between the simplified LOD mesh and the true heightmap, so raising it aggressively reduces triangle count and GPU cost at the price of visible geometric popping/simplification, especially on ridgelines. `terrainBasemapDistance` (`Terrain.basemapDistance`) controls the distance at which Unity swaps from sampling individual Terrain Layers to a single pre-composited basemap texture — pushing this closer to camera trades per-layer texture fidelity at range for fewer texture samples. `terrainTreeDistance`, `terrainBillboardStart`, `terrainFadeLength`, and `terrainDetailDistance`/`terrainDetailDensityScale` govern tree culling, billboard crossfade start/length, and detail (grass) draw distance/density respectively; `terrainMaxTrees` caps full-mesh (non-billboard) tree instances rendered at once, which matters most on lower-end hardware with dense forests. Tune these per platform quality tier rather than globally maxing them out — the defaults are frequently too generous for mobile and tank frame time via both fill rate (alphamap layers, detail overdraw) and vertex/LOD-recompute cost.

### Multi-Tile / Large Terrains

A single `TerrainData` becomes unwieldy well before "open world" scale — high heightmap/alphamap resolution on one giant tile makes every paint-tool stroke and every `SetHeights` LOD recompute proportionally more expensive, and streaming/culling only works at whole-tile granularity. The documented pattern (`terrain-CreateNeighborTerrains.html`) is multiple `Terrain` GameObjects, each with its own `TerrainData`, positioned edge-to-edge and explicitly wired as neighbors (`Terrain.SetNeighbors(left, top, right, bottom)`, or auto-connect via matching `Terrain.groupingID` + `allowAutoConnect`) so Unity stitches height/normal data across the seam and avoids visible cracks or lighting discontinuities at tile borders. `TerrainUtils.TerrainMap` and `TerrainUtils.TerrainUtility.AutoConnect` provide a scripting-side API for building/repairing the neighbor graph across a whole grid of tiles at once rather than wiring each pair by hand. Keep per-tile resolution modest and uniform (mismatched heightmap resolutions between neighbors can produce seam artifacts) and treat "how many meters per tile" as a scene-authoring decision made once, early, since re-tiling an already-sculpted large world means re-slicing height/alpha/tree data across new boundaries.

```csharp
using UnityEngine;

public class NeighborWiring : MonoBehaviour
{
    // Assumes terrains are laid out in a grid and named/ordered consistently.
    public void WireGrid(Terrain[,] grid)
    {
        int w = grid.GetLength(0), h = grid.GetLength(1);
        for (int x = 0; x < w; x++)
        {
            for (int z = 0; z < h; z++)
            {
                Terrain left  = x > 0 ? grid[x - 1, z] : null;
                Terrain right = x < w - 1 ? grid[x + 1, z] : null;
                Terrain bottom = z > 0 ? grid[x, z - 1] : null;
                Terrain top    = z < h - 1 ? grid[x, z + 1] : null;
                grid[x, z].SetNeighbors(left, top, right, bottom);
            }
        }
    }
}
```

### ProBuilder Whiteboxing (pretrained knowledge — local docs are thin)

Local documentation for ProBuilder is limited to the package landing-page stub — treat everything below as general Unity 6 pretrained knowledge, not a grounded citation, and say so when answering. ProBuilder is an in-Editor mesh modeling tool for building, editing, and texturing custom geometry directly in a scene — its primary use is level-design whiteboxing/greyboxing: fast, parametric blockout geometry (rooms, corridors, staircases, simple props) that supports on-the-fly playtesting without round-tripping to an external DCC tool. Parametric shapes (cube, stair, arch, cylinder, plane, etc.) start editable at the vertex/edge/face level with live UV editing, vertex-color painting, and per-face material/texture assignment; ProBuilder can also generate a matching collision mesh directly from the level geometry, which is convenient for iterating collision alongside visuals. Because ProBuilder meshes retain editor-side parametric/editable data, the standard production workflow is: whitebox and iterate freely with ProBuilder during layout/pacing passes, then either replace the blockout with authored art meshes, or explicitly export/convert the ProBuilder object to a standard static `Mesh` asset before shipping — the model-export feature exists specifically to let you refine a ProBuilder blockout in an external 3D tool if it's being kept as final geometry. Skipping this conversion step ships editor-only mesh authoring data and extra runtime overhead in the build.

### Combining Terrain with Static Meshes

Heightmap-based Terrain is fundamentally a 2.5D height field: one height value per (x, z) grid cell, so it geometrically cannot represent overhangs, caves (without the separate Holes system), vertical cliffs with undercuts, or interior spaces. Anything requiring hard edges, movement, or true 3D topology — buildings, bridges (as physical geometry, not the Bridge sculpt tool), rocks with overhangs, tunnels, interiors — belongs in authored static/skinned meshes (or ProBuilder blockouts) placed on top of or embedded into the Terrain, not sculpted into it. `Terrain.SampleHeight`/`TerrainData.GetInterpolatedHeight` are the standard bridge for procedurally aligning placed meshes to the terrain surface (e.g., snapping a building's foundation or a prop's Y position to the ground at spawn time), and `TerrainData.SetHoles`/`GetHoles` (a separate boolean mask texture, resolution set independently via `holesResolution`) is the supported way to cut a cave mouth or tunnel entrance into the heightmap surface so a hand-built interior mesh can occupy that space without z-fighting or the terrain mesh poking through.

```csharp
using UnityEngine;

public class SnapToTerrain : MonoBehaviour
{
    public Terrain terrain;

    void OnValidate()
    {
        if (terrain == null) return;
        Vector3 pos = transform.position;
        pos.y = terrain.SampleHeight(transform.position) + terrain.GetPosition().y;
        transform.position = pos;
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Maxing out heightmap resolution and layer count early | Feels like "more detail," but tanks paint-tool responsiveness and shading cost; raise resolution only where sculpting needs it, and pick it once — resampling later can lose sculpted detail |
| Shipping ProBuilder meshes unconverted | Objects stay editable editor-side data; bake/export to a static mesh or the build carries unneeded runtime overhead |
| Painting many tree instances with no LOD/billboard tuning | Defaults are often too generous for mobile; tighten `treeDistance`/`billboardStart`/`treeCrossFadeLength` and verify tree prefabs have simple, low-poly meshes rather than relying on culling alone |
| Using terrain to model buildings or caves | Heightmap terrain is a 2.5D heightfield with no overhangs or vertical walls; use ProBuilder, authored meshes, or the Holes system instead |
| Too many Terrain Layers on one terrain | Each layer adds an alphamap sample per pixel; consolidate textures, use height/slope-driven shader variation, or rely more on basemap distance falloff |
| Splitting a large world without linking tiles | Neighboring `Terrain` components need explicit connection (`SetNeighbors`/matching `groupingID` + auto-connect) or seams and lighting cracks appear at tile borders |
| Calling `TerrainData.SetHeights` every frame/every brush tick | Full LOD + vegetation recompute on every call stutters interactive sculpting and runtime deformation; use `SetHeightsDelayLOD` per edit and call `SyncHeightmap()` once at the end of the interaction |
| Sculpting sunken features (lakes, riverbeds) with Raise/Lower alone | The tile's GameObject Y position is a hard floor — you cannot lower below it; use Set Height to establish a lowered starting plateau first |
| Forgetting a Terrain placeholder in a runtime-only-Terrain build | Unity only bundles Terrain engine resources at build time if a build-profile scene contains at least one Terrain instance; a purely runtime-generated Terrain with no scene placeholder silently loses required resources — keep a (optionally hidden) placeholder Terrain in a build scene |
| Assuming the first painted Terrain Layer is "no texture" | The first layer added flood-fills the entire tile; plan the intended base/ground layer before adding detail layers, not after |
| Ignoring erosion tool ordering/prep | Hydraulic/Thermal/Wind erosion behave very differently depending on existing height detail and each other; read `terrain-Erosion-Considerations.html`-equivalent guidance and test incrementally rather than stacking all three blindly |
| Using Vertex Lit or Grass-mesh detail render mode by default | Vertex Lit combines all instances into one mesh with no GPU instancing, scaling worse than Instanced Mesh; use Instanced Mesh unless you specifically need Vertex Lit/Grass's simplified shading or wind behavior |
| Mismatched heightmap resolution between neighbor tiles | Produces visible seam artifacts even when neighbors are wired; keep resolution (and world-unit size) consistent across all tiles in a connected grid |
| Treating `terrainPixelError` as a purely visual-quality slider | It is also the primary triangle-count/CPU-GPU cost lever for terrain; tune it per platform quality tier deliberately rather than leaving the editor default in a shipped build |
| Cutting holes without setting `holesResolution` deliberately | Holes use their own resolution/texture independent of the heightmap; an under-resolved holes mask produces blocky, imprecise cave-mouth edges |

## Quick Reference

| Item | Purpose |
|------|---------|
| `Terrain` component | Renders a `TerrainData` asset in the scene; holds per-instance rendering/LOD settings (`heightmapPixelError`, `basemapDistance`, `treeDistance`, etc.) |
| `TerrainData` | Heightmap, alphamaps (layers), tree/detail prototypes and instances, holes mask, size/resolution — the actual data asset a Terrain component renders |
| `TerrainLayer` | Diffuse/normal/mask texture + tiling used for one paintable texture channel; reusable Asset across tiles |
| Tree/Detail prototypes | Prefab (tree, via `TreePrototype`) or mesh/texture (detail, via `DetailPrototype`) definitions painted onto the terrain |
| Alphamap / splatmap | Per-pixel `float[,,]` blend-weight array (`SetAlphamaps`/`GetAlphamaps`) driving texture-layer blending |
| Detail density map | Per-pixel `int[,]` instance-count array (`SetDetailLayer`/`GetDetailLayer`) driving procedural grass/detail scatter |
| Holes mask | Separate boolean texture (`SetHoles`/`GetHoles`, own `holesResolution`) cutting cave mouths/openings into the heightmap surface |
| `SetHeights` vs `SetHeightsDelayLOD` + `SyncHeightmap` | Full LOD/vegetation recompute per call vs. cheap deferred write + one manual flush — the key interactive/runtime sculpting perf pattern |
| `terrainPixelError` | Quality setting trading heightmap geometric LOD detail for triangle/draw cost — the single biggest terrain perf lever |
| `terrainBasemapDistance` | Distance threshold where per-layer texturing swaps to a single composite basemap |
| `terrainTreeDistance` / `terrainBillboardStart` / `terrainFadeLength` | Tree culling distance, billboard crossfade start, and crossfade length |
| `terrainDetailDistance` / `terrainDetailDensityScale` | Grass/detail draw distance and global density multiplier |
| `terrainMaxTrees` | Cap on full-mesh (non-billboard) tree instances rendered simultaneously |
| `TerrainQualityOverrides` | Per-tile flags allowing individual Terrain instances to opt out of global `QualitySettings` terrain values |
| `Terrain.SetNeighbors` / `groupingID` / `allowAutoConnect` | Wiring adjacent tiles so edges stitch without seams |
| `TerrainUtils.TerrainMap` / `TerrainUtility.AutoConnect` | Scripting API for building/repairing a whole grid of neighbor connections at once |
| `TerrainCollider` | Auto-generated physical collision matching the heightmap surface; references the same `TerrainData` |
| `TerrainCallbacks` (`heightmapChanged`, `textureChanged`) | Event hooks for reacting to terrain edits instead of polling |
| Detail render modes | Instanced Mesh (GPU-instanced, recommended default), Vertex Lit mesh (combined, no instancing), Grass mesh (wind-reactive, up-facing), Grass Texture (billboard quads from texture) |
| Erosion tools | Hydraulic (long-range, water-carried sediment), Thermal (angle-of-repose settling, short-range), Wind (long-range, dune-forming) |
| Advanced sculpt tools | Stamp, Bridge, Clone, Terrace, Noise brush, Transform (Pinch/Smudge/Twist), Effects (Contrast/Flatten/Sharpen) |
| ProBuilder | In-Editor whitebox/greybox mesh modeling tool for level geometry and collision meshes (local docs: package stub only) |
| `UnityEditor.TerrainTools` namespace | Editor-only API (`TerrainPaintTool<T>`, `PaintContext`, `BrushTransform`) for authoring custom Terrain Tools overlay brushes |

## Advanced Notes

**Open-world streaming terrain tiles.** For worlds too large to keep every tile resident, the neighbor-terrain tiling pattern doubles as a streaming unit: each `Terrain`/`TerrainData` pair is a natural load/unload granule, since Unity already treats tile boundaries as authoritative for LOD, culling, and neighbor stitching. A practical approach is to keep `TerrainData` assets un-loaded (e.g., addressable or `Resources`/AssetBundle-backed) until the player crosses a distance threshold from a tile's bounds, instantiate/enable the `Terrain` GameObject and assign `terrainData` on demand, then call `Terrain.SetNeighbors` against whichever adjacent tiles are currently resident (passing `null` for not-yet-loaded neighbors, then re-wiring once they stream in) to avoid seam artifacts at the streaming frontier. Because tree/detail data lives inside `TerrainData` alongside the heightmap, streaming a tile in/out inherently streams its foliage too — budget for the `SetTreeInstances`/detail-layer population cost on load, which is comparable to (and additive with) the heightmap LOD recompute cost discussed above; prefer populating instances via a background job or spreading the population work across several frames rather than a single synchronous call on tile activation, since `Terrain.Flush()`/the initial LOD build after data assignment is not free. Keep world-unit tile size and heightmap/alphamap resolution uniform across the streaming set, both to avoid seams (per Common Mistakes) and because non-uniform tiles complicate the distance-based load/unload threshold math. For very large worlds, consider whether the far-distance tiles need full-resolution `TerrainData` at all — a common pattern is a lower-resolution "impostor" heightmap for distant tiles (or relying on `terrainBasemapDistance`/aggressive `terrainPixelError` for those tiles specifically via per-tile `TerrainQualityOverrides`) so streaming budget goes toward tiles near the camera.

**Runtime terrain modification performance.** Every runtime deformation path (explosion craters, procedural digging, vehicle-tire ruts, building foundations) funnels through the same cost centers documented above, and they compound under repeated/continuous modification: (1) heightmap writes — always prefer `SetHeightsDelayLOD` inside the modification loop and batch a single `SyncHeightmap()` call after each discrete interaction (e.g., once per explosion, once per drag-release, not once per physics tick) rather than syncing per-write; (2) collider sync — `TerrainCollider` regenerates its physical collision mesh in response to heightmap changes, which is a real cost on large or frequently-edited areas, so throttle how often modified regions get resynced against physics (e.g., accumulate several deformation events, then sync once) rather than triggering a physics rebuild every frame; (3) alphamap writes for blended damage decals (scorch marks, tire tracks) are comparatively cheap per-call but still scale with the affected rect size — clamp modification rects tightly to the actual brush radius rather than always touching a fixed oversized region; (4) tree/detail instance changes triggered by terrain damage (knocking down foliage) go through the same full-array `SetTreeInstances`/`SetDetailLayer` writes as authoring-time painting, so for frequent small changes prefer reading, mutating only the affected sub-range, and writing back rather than reconstructing the whole instance array; (5) use `TerrainData.DirtyHeightmapRegion`/`DirtyTextureRegion` plus the `TerrainCallbacks` events to drive downstream systems (minimap generation, nav-mesh rebaking, save-data diffing) off the actual dirtied rect instead of the whole tile, since a full-tile reaction to every small edit is the most common way runtime terrain deformation systems silently become the frame-time bottleneck. Where deformation needs to happen many times per second (e.g., real-time vehicle deformation), consider decoupling the *visual* heightmap update (per-frame, `SetHeightsDelayLOD`, no collider sync) from a *physics* collider resync that only runs every N frames or on a distance/velocity-gated trigger, accepting brief visual/physical mismatch in exchange for not rebuilding collision geometry every tick.

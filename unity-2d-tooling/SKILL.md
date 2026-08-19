---
name: unity-2d-tooling
description: Use when building 2D Unity content beyond physics — Sprite Atlas, 2D Animation (bone rigging), or Tilemap. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity 2D Tooling

This skill covers the non-physics half of Unity's 2D toolset: sprite import and slicing, Sprite Atlas packing (including V2 format and platform variants), 9-slice sprites, sprite sorting (Sorting Layers, Sorting Group, Sprite Mask), the full Tilemap/Tile Palette authoring pipeline, Tilemap collision components, 2D lighting in URP, and the 2D Animation (Sprite Skin/bone rigging) package. 2D physics bodies, colliders, joints, and effectors are owned by the sibling `unity-physics` skill — do not duplicate that material here; this skill only touches collision components (TilemapCollider2D, sprite auto-collider generation) as they relate to authoring, not physics simulation behavior.

## Retrieval Sources

All paths below were re-verified present on disk under `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` as of this pass (Unity 6.3 LTS / 6000.3 docs, built 2026-08-10). Paths are relative to that root.

| Source | Path | Use for |
|--------|------|---------|
| Sprite import & Texture Type | `Manual/sprite/import-images-sprites/import-images-sprites-landing.html`, `Manual/texture-type-sprite.html` | Texture Type = Sprite, Sprite Mode (Single/Multiple), pixels-per-unit, pivot, mesh type |
| Sprite Editor window | `Manual/sprite/sprite-editor/sprite-editor-window-reference-landing.html`, `Manual/sprite/sprite-editor/sprite-editor-window-reference.html`, `Manual/sprite/sprite-editor/use-editor.html` | Slicing spritesheets into sub-sprites, editor workflow and toolbar |
| Sprite Editor advanced tabs | `Manual/sprite/sprite-editor/generate-outline.html`, `Manual/sprite/sprite-editor/custom-outline-editor-reference.html`, `Manual/sprite/sprite-editor/custom-physics-shape-editor-reference.html`, `Manual/sprite/sprite-editor/secondary-textures-editor-reference.html` | Custom Outline tab (crop/silhouette), Custom Physics Shape tab, Secondary Textures tab (normal/mask maps) |
| Sprite auto collision geometry | `Manual/sprite/create-collision-geometry.html` | Generating collider outlines automatically from sprite alpha, tolerance settings |
| Sprite landing & placeholder sprites | `Manual/sprite/sprite-landing.html`, `Manual/sprite/placeholder/placeholder-landing.html` | 2D sprite feature overview; built-in placeholder sprites for prototyping |
| Sprite Atlas overview & creation | `Manual/sprite/atlas/atlas-landing.html`, `Manual/sprite/atlas/atlas-introduction.html`, `Manual/sprite/atlas/create-sprite-atlas.html` | Why/when to pack atlases (draw-call batching), creating a `.spriteatlasv2` asset, Objects for Packing workflow |
| Sprite Atlas Inspector reference | `Manual/sprite/atlas/sprite-atlas-reference.html` | Full property reference: Type (Master/Variant), Packing (Allow Rotation, Tight Packing, Alpha Dilation, Padding), Texture (Read/Write, Mip Maps, Filter Mode), platform-specific override tabs |
| Sprite Atlas V2 format | `Manual/sprite/atlas/v2/sprite-atlas-v2.html` | `.spriteatlasv2` format details, upgrading legacy V1 sprite atlases |
| Sprite Atlas master/variant atlases | `Manual/sprite/atlas/master-variant/master-variant-sprite-atlases.html` | Creating lower-resolution Variant atlases from a Master atlas for different platforms/quality tiers |
| Load Sprite Atlas at runtime | `Manual/sprite/atlas/distribution/load-sprite-atlas-spriteatlasmanageratlasrequested.html` | Late-binding atlases via `SpriteAtlasManager.atlasRequested`, loading from Resources or AssetBundles, excluding an atlas from the build |
| 9-slice sprites | `Manual/sprite/9-slice/9-slice-landing.html`, `Manual/sprite/9-slice/9-slicing.html`, `Manual/sprite/9-slice/set-sprite-9slicing.html` | Border-sliced sprites, Mesh Type = Full Rect requirement, Draw Mode Sliced/Tiled, Tile Mode (Continuous/Adaptive), collider Auto Tiling |
| Sprite sorting / 2D rendering order | `Manual/sprite/sort-sprites/sort-sprites-landing.html`, `Manual/sprite/sort-sprites/sort-sprites.html` | The 5-criteria 2D draw-order algorithm: Sorting Layer, Order in Layer, Render Queue, camera distance, shader/material grouping |
| Sprite Renderer component reference | `Manual/sprite/renderer/sprite-renderer-reference.html` | SpriteRenderer Inspector fields: Drawing Mode, Mask Interaction, Sprite Sort Point, Sorting Layer/Order in Layer |
| Sorting Group | `Manual/sprite/sorting-group/sorting-group-reference.html`, `Manual/sprite/sorting-group/use-sorting-groups.html` | Preventing prefab-instance sort-order mixing, nested sorting groups, Sort At Root |
| Sprite Mask | `Manual/sprite/mask/mask-landing.html`, `Manual/sprite/mask/sprite-mask-reference.html`, `Manual/sprite/mask/hide-reveal-parts-sprite-mask.html` | Hiding/revealing sprite parts, Mask Source (Sprite/Supported Renderer), Alpha Cutoff, Custom Range (front/back sorting layer) |
| Tilemap landing & grid concepts | `Manual/tilemaps/tilemaps-landing.html`, `Manual/tilemaps/tilemaps.html`, `Manual/tilemaps/reference.html`, `Manual/tilemaps/grid-reference.html` | Grid component, Cell Layout types (Rectangle/Hexagon/Isometric), overall Tilemap system overview |
| Create & paint tilemaps | `Manual/tilemaps/work-with-tilemaps/create-tilemap-landing.html`, `Manual/tilemaps/work-with-tilemaps/create-tilemap.html`, `Manual/tilemaps/work-with-tilemaps/tilemap-reference.html`, `Manual/tilemaps/work-with-tilemaps/tilemap-renderer-reference.html` | Creating a Grid + Tilemap GameObject, painting/erasing/moving tiles, `Tilemap`/`TilemapRenderer` component reference |
| Hexagonal & isometric tilemaps | `Manual/tilemaps/work-with-tilemaps/hexagonal-tilemaps/hexagonal-tilemap-landing.html`, `Manual/tilemaps/work-with-tilemaps/isometric-tilemaps/isometric-tilemap-landing.html`, `Manual/tilemaps/work-with-tilemaps/isometric-tilemaps/create-isometric-tilemap.html` | Point Top vs. Flat Top hex grids, isometric grid setup and cell size/axis conventions |
| Tile assets & scriptable tiles | `Manual/tilemaps/custom-tiles-brushes.html`, `Manual/tilemaps/tiles-for-tilemaps/create-tile-palette-landing.html`, `Manual/tilemaps/tiles-for-tilemaps/create-tile-assets.html`, `Manual/tilemaps/tiles-for-tilemaps/tile-asset-reference.html`, `Manual/tilemaps/tiles-for-tilemaps/scriptable-tiles/scriptable-tiles.html` | Creating Tile assets, custom scripted tiles via `TileBase`/`GetTileData`, prebuilt scriptable tiles (2D Tilemap Extras) |
| Tile Palette editor | `Manual/tilemaps/tile-palettes/tile-palette-editor-reference.html`, `Manual/tilemaps/tile-palettes/tile-palette-preferences-reference.html`, `Manual/tilemaps/tile-palettes/tile-palette-clipboard-overlay.html`, `Manual/tilemaps/tile-palettes/tile-set-properties.html`, `Manual/tilemaps/tile-palettes/tile-template-asset.html`, `Manual/tilemaps/tile-palettes/tools/tile-palette-tools-landing.html`, `Manual/tilemaps/tile-palettes/brushes/create-scriptable-brush.html` | Tile Palette window layout, painting tools, clipboard overlay, custom scriptable brushes |
| Tilemap collision | `Manual/tilemaps/work-with-tilemaps/tilemap-collider-2d-landing.html`, `Manual/tilemaps/work-with-tilemaps/tilemap-collider-2d.html`, `Manual/tilemaps/work-with-tilemaps/tilemap-collider-2d-reference.html` | Enabling collision on a Tilemap, full `TilemapCollider2D` property reference (Max Tile Change Count, Extrusion Factor, Use Delaunay Mesh, Composite Operation) |
| 2D Animation package | `Manual/com.unity.2d.animation.html` | Package landing page only — a version/description stub (package `com.unity.2d.animation@13.0.5`), not a feature manual. Local coverage of Sprite Skin / bone rigging is thin; lean on pretrained package knowledge and flag that when answering |
| 2D lighting in URP — landing & concepts | `Manual/urp/2d-index.html`, `Manual/urp/Lights-2D-intro.html` | 2D Renderer feature landing, the four 2D light types (Spot, Freeform, Sprite, Global), the 3-stage render order (shadows → lights → scene) |
| Light 2D component reference | `Manual/urp/2DLightProperties.html`, `Manual/urp/2d-light-properties-explained.html` | Full `Light2D` Inspector reference: per-type properties (Radius/Angle for Spot, Falloff for Freeform, Sprite for Sprite lights), Blend Style, Light Order, Overlap Operation, Shadows, Volumetric, Normal Map Quality |
| 2D light optimization & shadows | `Manual/urp/2d-light-batching-debugger.html`, `Manual/urp/2d-light-blend-modes.html`, `Manual/urp/2d-shadows-landing.html`, `Manual/urp/2DShadows.html`, `Manual/urp/ShadowCaster2D.html` | Light batching debugger tool, custom blend styles, `ShadowCaster2D` component for casting/self-shadowing sprites |
| 2D Renderer sorting, sprite mask interaction & Pixel Perfect | `Manual/2d-renderer-sorting.html`, `Manual/urp/2d-renderer-urp-sorting-workflows.html`, `Manual/urp/2d-renderer-urp-sprite-mask-interaction.html`, `Manual/urp/2DRendererData-overview.html`, `Manual/urp/2d-renderer-urp-features-landing.html`, `Manual/urp/2d-pixelperfect.html`, `Manual/urp/2d-pixelperfect-configure.html`, `Manual/urp/2d-pixelperfect-ref.html` | 2D Renderer Data asset (Light Blend Styles, Transparency Sort settings), how sorting layers gate light targeting, Sprite Mask + 2D light interaction, Pixel Perfect Camera setup |
| `SpriteRenderer` scripting API | `ScriptReference/SpriteRenderer.html`, `ScriptReference/SpriteRenderer-sprite.html`, `ScriptReference/SpriteRenderer-drawMode.html`, `ScriptReference/SpriteRenderer-size.html`, `ScriptReference/SpriteRenderer-color.html`, `ScriptReference/SpriteRenderer-flipX.html`, `ScriptReference/SpriteRenderer-flipY.html`, `ScriptReference/SpriteRenderer-maskInteraction.html`, `ScriptReference/Renderer-sortingOrder.html`, `ScriptReference/Renderer-sortingLayerName.html`, `ScriptReference/Renderer-sortingLayerID.html` | Swapping sprites/9-slice state/tint/flip at runtime; `sortingOrder`/`sortingLayerName` are inherited from the base `Renderer` class, not declared on `SpriteRenderer` itself |
| `Sprite` and `SpriteMask` scripting API | `ScriptReference/Sprite.html`, `ScriptReference/Sprite.Create.html`, `ScriptReference/Sprite.GetPhysicsShapeCount.html`, `ScriptReference/Sprite.GetPhysicsShape.html`, `ScriptReference/SpriteMask.html`, `ScriptReference/SpriteMask-maskSource.html`, `ScriptReference/SpriteMask-isCustomRangeActive.html`, `ScriptReference/SpriteMask-frontSortingOrder.html`, `ScriptReference/SpriteMask-backSortingOrder.html` | Creating sprites from a `Texture2D` at runtime, reading the auto-generated physics-shape outline, scripting `SpriteMask` custom range/mask source |
| `Tilemap` scripting API | `ScriptReference/Tilemaps.Tilemap.html`, `ScriptReference/Tilemaps.Tilemap.SetTile.html`, `ScriptReference/Tilemaps.Tilemap.GetTile.html`, `ScriptReference/Tilemaps.Tilemap.SetTiles.html`, `ScriptReference/Tilemaps.Tilemap.ContainsTile.html`, `ScriptReference/Tilemaps.Tilemap.ClearAllTiles.html`, `ScriptReference/Tilemaps.Tilemap.BoxFill.html`, `ScriptReference/Tilemaps.Tilemap-cellBounds.html`, `ScriptReference/Tilemaps.Tilemap.CompressBounds.html`, `ScriptReference/Tilemaps.TileBase.html`, `ScriptReference/Tilemaps.Tile.html`, `ScriptReference/Tilemaps.TilemapCollider2D.html` | Programmatic tile placement/removal/bulk-fill, custom `TileBase` subclasses, reading collider state from script |
| `SpriteAtlas` runtime/scripting API | `ScriptReference/U2D.SpriteAtlas.html`, `ScriptReference/U2D.SpriteAtlas.GetSprite.html`, `ScriptReference/U2D.SpriteAtlasManager.html`, `ScriptReference/U2D.SpriteAtlasManager-atlasRequested.html`, `ScriptReference/U2D.SpriteAtlasManager-atlasRegistered.html`, `ScriptReference/U2D.SpriteAtlasManager.RequestAtlasCallback.html`, `ScriptReference/U2D.SpriteAtlasAsset.html`, `ScriptReference/U2D.SpriteAtlasAsset.Load.html` | Looking up a packed sprite by name at runtime, late-binding atlas delivery via the `atlasRequested` event (the API backing `Manual/sprite/atlas/distribution/...` above) |

## Key Guidelines

### Sprite Import Modes

Every sprite starts as a texture with **Texture Type** set to **Sprite (2D and UI)**. Use **Sprite Mode = Single** for a texture that holds exactly one sprite, and **Sprite Mode = Multiple** for a spritesheet — after switching to Multiple, open the Sprite Editor and slice the sheet into individual sub-sprites (by grid, by automatic alpha detection, or manually). Keep **Pixels Per Unit** and **Pivot** consistent across a related sprite set: PPU determines how large the sprite appears relative to `Transform` scale 1, and an inconsistent pivot breaks animation anchoring and physics-shape alignment when sprites are swapped at runtime. **Mesh Type** matters beyond rendering cost — Full Rect (a simple quad) is *required* for 9-slicing to work correctly, while Tight (a mesh hugging the sprite's alpha silhouette) reduces overdraw for non-sliced sprites. Import settings can also be scripted for consistency across a large asset set via `AssetPostprocessor`:

```csharp
using UnityEditor;
using UnityEngine;

public class SpriteImportSettings : AssetPostprocessor
{
    void OnPreprocessTexture()
    {
        // Only touch textures under an Art/Sprites folder
        if (!assetPath.Contains("Art/Sprites")) return;

        var importer = (TextureImporter)assetImporter;
        importer.textureType = TextureImporterType.Sprite;
        importer.spriteImportMode = SpriteImportMode.Multiple;
        importer.spritePixelsPerUnit = 32f;
        importer.mipmapEnabled = false;
        importer.filterMode = FilterMode.Point; // crisp pixel art

        TextureImporterSettings settings = new TextureImporterSettings();
        importer.ReadTextureSettings(settings);
        settings.spriteMeshType = SpriteMeshType.Tight;
        settings.spriteAlignment = (int)SpriteAlignment.BottomCenter;
        importer.SetTextureSettings(settings);
    }
}
```

### Sprite Atlas Packing

A separate texture per sprite means a separate GPU draw call per texture (unless SRP Batcher is compensating). Pack related sprites into a **Sprite Atlas** asset (`Assets > Create > 2D > Sprite Atlas`, producing a `.spriteatlasv2` file) so Unity combines them into one texture and one draw call. Add sprites, textures, or whole folders to the atlas's **Objects for Packing** list, then **Pack Preview** to build it. Before packing, disable **Read/Write** on the source textures unless you touch pixel data from script, and enable **Tight Packing** to shrink transparent padding. Split atlases along real usage boundaries — one atlas per scene/level, one per compression requirement (detailed character sprites vs. flat environment tiles), and one for frequently- vs. infrequently-used sprites — rather than one giant atlas, so unrelated sprites aren't forced into memory together. For platform-specific sizing use **Master/Variant** atlases (see Advanced Notes) instead of hand-maintaining separate atlas assets per platform. When a sprite belongs to more than one atlas and both are `Include in Build`, Unity picks one at random at runtime — avoid this by disabling `Include in Build` on all but one atlas and late-binding the rest:

```csharp
using UnityEngine;
using UnityEngine.U2D;

public class RuntimeAtlasLoader : MonoBehaviour
{
    void OnEnable()
    {
        // Fires whenever a sprite requests an atlas that isn't loaded yet
        SpriteAtlasManager.atlasRequested += LoadAtlasByName;
    }

    void OnDisable()
    {
        SpriteAtlasManager.atlasRequested -= LoadAtlasByName;
    }

    void LoadAtlasByName(string atlasName, System.Action<SpriteAtlas> callback)
    {
        // Atlas asset must live under a Resources/ folder for this API
        var atlas = Resources.Load<SpriteAtlas>(atlasName);
        callback(atlas);
    }
}
```

### 9-Slice Sprites

9-slicing divides a sprite into a 3x3 grid — four fixed corners, four stretchable/tileable edges, and a stretchable/tileable center — so the same source art scales cleanly to any size without smearing or distorting the corners. Requirements: the sprite's **Mesh Type** must be **Full Rect** (Tight breaks slicing), and border values (L/R/T/B) are set by dragging the green handles in the Sprite Editor or typing values directly. On the `SpriteRenderer`, set **Draw Mode** to **Sliced** (edges/center stretch) or **Tiled** (edges/center repeat, controlled further by **Tile Mode**: Continuous crops at the edges, Adaptive stretches until a **Stretch Value** threshold then repeats). Once Draw Mode is Sliced or Tiled, the `SpriteRenderer` drives any attached `Collider2D` automatically — don't hand-edit that collider. Only `BoxCollider2D` and `PolygonCollider2D` support **Auto Tiling** for 9-sliced sprites.

```csharp
using UnityEngine;

[RequireComponent(typeof(SpriteRenderer))]
public class NineSliceResize : MonoBehaviour
{
    [SerializeField] SpriteRenderer panelRenderer;

    void Start()
    {
        panelRenderer.drawMode = SpriteDrawMode.Sliced;
        // .size only takes effect once drawMode is Sliced or Tiled;
        // this resizes the panel without distorting the 9-slice borders
        panelRenderer.size = new Vector2(6f, 3f);
    }

    public void ResizeTo(Vector2 worldSize)
    {
        panelRenderer.size = worldSize;
    }
}
```

### Sorting Layers & Order

Unity decides 2D draw order with five criteria, evaluated in this priority: (1) **Sorting Layer** — higher in the Tags and Layers list draws later (in front); (2) **Order in Layer** — lower value draws first (behind) within the same Sorting Layer; (3) material **Render Queue**; (4) **distance to camera** (only the deciding factor once the first three are tied — this is why untouched sprites, which all share the Default layer/order/queue, sort purely by camera distance); (5) shader/material grouping, which is unordered. Control draw order with the `SpriteRenderer`'s Sorting Layer and Order in Layer fields — never by nudging Z position as a substitute, since Z only feeds into criterion 4 and is unreliable once any Sorting Layer/Order differs. Use a **Sorting Group** component on a parent GameObject to make a whole hierarchy (e.g. a multi-part character prefab) sort as one atomic unit on one layer/sublayer — without it, two instances of the same prefab can visually interleave because their child parts share sorting layers and orders.

```csharp
using UnityEngine;

public class DepthSort : MonoBehaviour
{
    [SerializeField] SpriteRenderer spriteRenderer;
    [SerializeField] string sortingLayer = "Characters";

    void LateUpdate()
    {
        spriteRenderer.sortingLayerName = sortingLayer;
        // Classic "sort by Y" for top-down 2D: further up the screen = further back
        spriteRenderer.sortingOrder = -(int)(transform.position.y * 100f);
    }
}
```

### Sprite Masking

A `SpriteMask` component uses opaque pixels of a source texture as a stencil, hiding or revealing parts of other sprites that opt in via their `SpriteRenderer`'s **Mask Interaction** field (`VisibleInsideMask` or `VisibleOutsideMask`; `None` ignores masks entirely). **Mask Source** is either a **Sprite** (a dedicated mask shape) or a **Supported Renderer** (reusing the texture already on a `SpriteRenderer`, `SpriteShapeRenderer`, or `TilemapRenderer` on the same GameObject). **Alpha Cutoff** sets the minimum alpha counted as "inside" the mask. By default a mask affects every sorting layer; enable **Custom Range** to restrict it to a Front/Back sorting-layer-and-order window — useful for a mask that should only clip objects in one specific layer band (e.g. a water-surface mask that shouldn't affect UI).

```csharp
using UnityEngine;

[RequireComponent(typeof(SpriteMask))]
public class WaterMaskRange : MonoBehaviour
{
    void Start()
    {
        var mask = GetComponent<SpriteMask>();
        mask.isCustomRangeActive = true;
        mask.frontSortingLayerID = SortingLayer.NameToID("Water");
        mask.backSortingLayerID  = SortingLayer.NameToID("Underwater");
        mask.alphaCutoff = 0.5f;
    }

    public void SetVisibleThroughMask(SpriteRenderer target, bool insideOnly)
    {
        target.maskInteraction = insideOnly
            ? SpriteMaskInteraction.VisibleInsideMask
            : SpriteMaskInteraction.VisibleOutsideMask;
    }
}
```

### Tilemap & Tile Palette

Grid-based levels use a **Grid** GameObject (the infinite layout guide — pick its **Cell Layout**: Rectangle, Hexagon, or Isometric — up front, since changing it later re-warps every painted tile) with one or more child **Tilemap** GameObjects (the paintable layers, each rendered by a `TilemapRenderer`). Author content by opening the **Tile Palette** window, selecting a palette, setting **Active Target** to the tilemap to paint onto, and using the Paint/Erase/Select/Move tools; multiple Tilemap children on one Grid let you separate ground, decoration, and collision-only layers. Tiles themselves are `Tile` assets (`Assets > Create > 2D > Tiles`) or custom **scriptable tiles** subclassing `TileBase` and overriding `GetTileData` for tiles that change appearance based on neighbors or state (part of the 2D Tilemap Extras package). At runtime, read and write tiles through the `Tilemap` API directly rather than only through the editor:

```csharp
using UnityEngine;
using UnityEngine.Tilemaps;

public class ProceduralFloor : MonoBehaviour
{
    [SerializeField] Tilemap tilemap;
    [SerializeField] TileBase floorTile;
    [SerializeField] TileBase wallTile;

    public void CarveRoom(BoundsInt area)
    {
        var floorPositions = new Vector3Int[area.size.x * area.size.y];
        var floorTiles = new TileBase[floorPositions.Length];
        int i = 0;
        foreach (var pos in area.allPositionsWithin)
        {
            floorPositions[i] = pos;
            floorTiles[i] = floorTile;
            i++;
        }
        // Batched write is far cheaper than one SetTile call per cell
        tilemap.SetTiles(floorPositions, floorTiles);
    }

    public void PlaceWall(Vector3 worldPosition)
    {
        Vector3Int cell = tilemap.WorldToCell(worldPosition);
        tilemap.SetTile(cell, wallTile);
    }

    public bool IsWalkable(Vector3Int cell) => tilemap.GetTile(cell) != wallTile;
}
```

### Tilemap Collision

Add a `TilemapCollider2D` to a tile layer to generate a collision shape per non-empty tile. On its own this creates one collider shape per tile, which is expensive for large levels — pair it with a `CompositeCollider2D` (set **Composite Operation** to **Merge**) so Unity fuses adjacent tile shapes into a small number of larger polygon colliders. **Max Tile Change Count** (default 1000) throttles how many edits accumulate before Unity regenerates all shapes — lower it if you mutate the tilemap heavily at runtime and need colliders to stay current sooner. **Extrusion Factor** nudges per-tile shapes to overlap slightly so the Composite Collider can actually merge them (only relevant when a `CompositeCollider2D` with a non-None Composite Operation is present). **Use Delaunay Mesh** improves shape accuracy for irregular tile art at a performance cost. When a `CompositeCollider2D` is attached with Composite Operation set, it takes over the `Material`, `Is Trigger`, and `Used By Effector` properties from the `TilemapCollider2D`.

```csharp
using UnityEngine;
using UnityEngine.Tilemaps;

[RequireComponent(typeof(Tilemap))]
public class LevelCollisionSetup : MonoBehaviour
{
    void Awake()
    {
        var tilemapCollider = gameObject.AddComponent<TilemapCollider2D>();
        var composite = gameObject.AddComponent<CompositeCollider2D>();
        var body = gameObject.AddComponent<Rigidbody2D>();

        body.bodyType = RigidbodyType2D.Static;
        tilemapCollider.compositeOperation = Collider2D.CompositeOperation.Merge;
        tilemapCollider.usedByComposite = true; // required for TilemapCollider2D + CompositeCollider2D pairing
        composite.geometryType = CompositeCollider2D.GeometryType.Polygons;
    }
}
```

Note: `Rigidbody2D`/`Collider2D` simulation behavior (mass, layers, physics materials, joints, effectors) is owned by `unity-physics` — this guideline only covers wiring the components up for a tile-authored level.

### 2D Skeletal Animation (pretrained knowledge — local docs are thin)

The local Unity 6.3 manual page for the 2D Animation package (`Manual/com.unity.2d.animation.html`) is a bare package-version stub — description, version number (`com.unity.2d.animation@13.0.5`), and nothing else. The following is pretrained knowledge, not grounded in local docs; verify against the in-Editor package documentation or `docs.unity3d.com` for the installed package version before relying on specifics. The 2D Animation package's **Sprite Skin** does bone-based skeletal deformation of a single sprite's mesh, distinct from traditional sprite-sheet animation (which swaps whole sprite frames via `AnimationClip`s and needs no rig). Workflow: import a multi-part character texture, use the **Skinning Editor** (opened from the Sprite Editor once the package is installed) to create a bone hierarchy, generate/edit the mesh geometry, and paint per-vertex bone weights; the resulting `SpriteSkin` component deforms the mesh at runtime as its bone `Transform`s move — typically driven by a regular `Animator`/`AnimationClip` animating the bone transforms, or by IK/procedural code.

```csharp
using UnityEngine;
using UnityEngine.U2D.Animation;

public class ProceduralLimbSway : MonoBehaviour
{
    [SerializeField] SpriteSkin spriteSkin;
    [SerializeField] string boneName = "UpperArm_R";
    [SerializeField] float swayDegrees = 12f;
    [SerializeField] float swaySpeed = 2f;

    Transform bone;

    void Start()
    {
        // SpriteSkin exposes the deforming bone Transforms it was rigged with
        foreach (var b in spriteSkin.boneTransforms)
        {
            if (b.name == boneName) { bone = b; break; }
        }
    }

    void Update()
    {
        if (bone == null) return;
        float angle = Mathf.Sin(Time.time * swaySpeed) * swayDegrees;
        bone.localRotation = Quaternion.Euler(0, 0, angle);
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| No Sprite Atlas for many small sprites | Each unbatched sprite texture can add a draw call; pack related sprites into a Sprite Atlas via Objects for Packing |
| Wrong Sorting Layer/Order in Layer | Sprite renders behind what it should be in front of; fix via SpriteRenderer sorting fields, not Transform Z depth |
| Sprite Mode left on Single for a spritesheet | Sub-sprites never get sliced; switch to Multiple and slice in the Sprite Editor |
| No TilemapCollider2D on a tile level | Tiles are visual only — actors walk through them; add TilemapCollider2D (+ CompositeCollider2D) |
| Missing/zero 9-slice border, or Mesh Type left on Tight | Scaling a 9-sliced sprite stretches or smears the corners, or slicing silently fails; set the border in the Sprite Editor and confirm Mesh Type = Full Rect before scaling |
| Sprite added to multiple Include-in-Build atlases | Unity picks an atlas at random at runtime, causing inconsistent texture/compression per run; disable Include in Build on all but one atlas or late-bind via `SpriteAtlasManager.atlasRequested` |
| Read/Write left enabled on atlas source textures | Doubles texture memory for no benefit unless C# actually reads/writes pixel data; disable it before packing |
| TilemapCollider2D without `usedByComposite` set, or without a CompositeCollider2D | One discrete collider shape is generated per tile — expensive for large levels and can produce seams between adjacent tiles; pair with CompositeCollider2D and Merge |
| Small Max Texture Size on a Variant sprite atlas (below 0.25 of the Master) | Unity compresses the sprites twice (once for Master, once for the already-downscaled Variant), degrading quality more than expected |
| Forgetting a Sorting Group on a multi-part prefab | Multiple prefab instances with the same child Sorting Layer/Order values interleave incorrectly when several are on screen at once |
| Editing a Collider2D manually on a Sliced/Tiled 9-slice sprite | SpriteRenderer drives that collider automatically once Draw Mode is Sliced or Tiled; manual edits get overwritten |
| Assuming Z position controls 2D draw order | Z only enters the 5-criteria sort algorithm as camera-distance (criterion 4), and only once Sorting Layer, Order in Layer, and Render Queue are all tied |
| Painting an isometric or hexagonal tilemap with a Rectangle-configured Grid | Tiles render in the wrong positions/skew; set the Grid's Cell Layout to match the tile art before painting, since changing it later re-warps existing tiles |
| Treating the 2D Animation manual page as a full feature reference | It's a package-version stub with no workflow detail; rely on pretrained knowledge or the in-Editor package docs, and say so explicitly |
| No Shadow Caster 2D on occluders in a lit 2D scene | Sprites meant to block 2D light (walls, pillars) cast no shadow without an explicit `ShadowCaster2D` component — 2D lighting doesn't infer shadow casters from colliders or renderers |

## Quick Reference

| Item | Purpose |
|------|---------|
| `SpriteRenderer` | Renders a Sprite; carries Sorting Layer, Order in Layer, Draw Mode, Mask Interaction, Sprite Sort Point |
| `SpriteMask` | Stencils sprite/tilemap rendering to hide/reveal parts; Mask Source, Alpha Cutoff, optional Custom Range |
| `SortingGroup` | Forces a GameObject hierarchy to sort as one atomic unit on one layer/sublayer; supports nesting and Sort At Root |
| `SpriteAtlas` | Packs multiple sprite textures into fewer atlas textures for batching; Master/Variant types for platform-specific resolution |
| `SpriteAtlasManager.atlasRequested` | Callback for manually (late-)binding a Sprite Atlas at runtime instead of auto-loading it at startup |
| Sprite Editor window | Slices spritesheets, edits pivot/border/outline/physics-shape/secondary textures |
| `Tile` / `TileBase` | Data asset painted onto a Tilemap; subclass `TileBase` + override `GetTileData` for scriptable/dynamic tiles |
| `Tilemap` | Grid data layer; scripting entry point (`SetTile`, `SetTiles`, `GetTile`, `WorldToCell`, `CellToWorld`) for procedural tile placement |
| `TilemapRenderer` | Renders a Tilemap's tiles; carries sorting and chunking/culling settings |
| Tile Palette window | Editor window for painting/erasing/selecting Tile assets onto a Tilemap; hosts brushes and painting tools |
| `TilemapCollider2D` | Generates per-tile collision shapes for a Tilemap; Max Tile Change Count, Extrusion Factor, Use Delaunay Mesh, Composite Operation |
| `CompositeCollider2D` | Merges per-tile (or per-sprite) collider shapes into fewer, cheaper composite colliders |
| Grid (Cell Layout) | Rectangle / Hexagon (Point Top or Flat Top) / Isometric layout mode for a Grid + its child Tilemaps |
| `SpriteSkin` (2D Animation) | Deforms a sprite mesh via a bone `Transform` hierarchy for skeletal animation (pretrained knowledge — see 2D Animation section) |
| `Light2D` (URP) | 2D light component: Spot, Freeform, Sprite, or Global type; Blend Style, Light Order, Overlap Operation, Shadows, Volumetric |
| `ShadowCaster2D` (URP) | Marks a GameObject as blocking/self-shadowing 2D light in the URP 2D Renderer |
| 2D Renderer Data asset | URP asset defining Light Blend Styles and 2D-specific renderer settings (`2DRendererData-overview.html`) |
| Pixel Perfect Camera (URP) | Snaps rendering to a fixed reference resolution/PPU grid to keep pixel art crisp and stable |

## Advanced Notes

### 2D Lighting (2D Renderer / URP 2D Lights)

Local coverage is real and detailed under `Manual/urp/`. Unity's URP 2D Renderer supports four **2D light types** on a `Light2D` component: **Spot** (point-sourced, cone-shaped, with Inner/Outer Radius and Inner/Outer Spot Angle plus Falloff Strength), **Freeform** (a hand-edited polygon shape with its own Falloff/Falloff Strength), **Sprite** (light shaped by an arbitrary sprite asset), and **Global** (uniform, non-attenuated light across all 2D GameObjects on its target sorting layers — only one Global light is allowed per Blend Style + sorting layer combination). 2D lighting is explicitly **not physically based**, unlike 3D URP/HDRP lighting.

Rendering happens in three passes each frame: (1) draw 2D shadow shapes into shadow textures, (2) draw each light's color/shape into light textures, (3) composite the lit scene to screen using those textures — Unity tries to finish all light/shadow texture passes before touching the screen target, to avoid render-target thrashing. Key `Light2D` fields beyond type-specific shape controls: **Blend Style** (Multiply, Additive, Multiply with Mask (R), Additive with Mask (R) — customizable per-project on the 2D Renderer Data asset), **Light Order** (numeric render order among lights sharing a sorting layer; most visible when **Overlap Operation** is Alpha Blend rather than the default Additive), **Target Sorting Layers** (which sorting layers the light affects at all), **Shadows** (Strength/Softness/Falloff Strength, only meaningful once `ShadowCaster2D` components exist on occluders), **Volumetric** (Volumetric Intensity/Shadow Strength for light visible in empty space, e.g. fog-style beams), and **Normal Map Quality** (Disabled/Fast/Accurate — trades lighting accuracy from a sprite's normal map for performance). A **Light Batching Debugger** window helps diagnose why 2D lights aren't batching together. Sprite Mask components interact with 2D lights too (`2d-renderer-urp-sprite-mask-interaction.html`) — mask geometry can occlude light the same way it occludes sprites, which matters when using masks for reveal effects in a lit scene.

```csharp
using UnityEngine;
using UnityEngine.Rendering.Universal;

public class FlickeringTorch : MonoBehaviour
{
    [SerializeField] Light2D torchLight; // Light Type = Point (Spot) in the Inspector
    [SerializeField] float baseIntensity = 1.2f;
    [SerializeField] float flickerAmount = 0.2f;
    [SerializeField] float flickerSpeed = 8f;

    void Update()
    {
        float noise = Mathf.PerlinNoise(Time.time * flickerSpeed, 0f) - 0.5f;
        torchLight.intensity = baseIntensity + noise * flickerAmount;
    }
}
```

### Sprite Atlas Variants for Different Platforms

Rather than maintaining separate hand-packed atlases per platform, create one **Master** sprite atlas (the default `Type`, holding the real Objects for Packing list) and one or more **Variant** atlases that reference it via **Master Atlas**. A Variant has no packing list of its own — it repacks the Master's sprites at a lower resolution, controlled by its **Scale** property (a multiple of the Master's resolution, max 1.0; e.g. `Scale = 0.5` for a half-resolution mobile variant). Don't set a Variant's **Max Texture Size** below 0.25x the Master's — Unity would compress the sprites twice (once building the Master, once building the already-downscaled Variant), losing more quality than intended. Because Unity includes both Master and Variant in a build by default and picks between them at random unless told otherwise, resolve the ambiguity with one of: `SpriteAtlas.GetSprite()` to explicitly pick an atlas at runtime, `SpriteAtlasManager.atlasRequested` late-binding (see the Sprite Atlas Packing code sample above) to load the platform-appropriate variant on demand, or disabling **Include in Build** on the atlas(es) you don't want auto-included for a given platform build (via a per-platform build script, since the Inspector checkbox itself isn't platform-conditional). If Include in Build ends up disabled on *both* Master and Variant with no runtime binder attached, the sprites have no backing texture and simply don't render — this is a common silent failure after adding a mobile variant without wiring up loading.

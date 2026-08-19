---
name: unity-procedural-mesh
description: Use when generating or modifying mesh geometry from code — the Mesh class, vertex/triangle/UV buffers, or the Mesh.MeshData advanced mesh API. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Procedural Mesh Generation

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Meshes landing & terminology | `Manual/mesh.html`, `Manual/get-started-with-meshes.html`, `Manual/mesh-introduction.html` | Entry points into the whole mesh manual tree; defines mesh/vertex/index/topology vocabulary used everywhere else |
| Select a mesh asset | `Manual/mesh-select-mesh-asset.html` | How to open the Mesh Inspector on a GameObject's mesh asset to eyeball generated data |
| Mesh data landing page | `Manual/AnatomyofaMesh.html` | Index of the four mesh-data sub-pages (vertex/topology/index/deformable) below |
| Mesh vertex data | `Manual/mesh-vertex-data.html` | Per-vertex attributes (position, normal, tangent, color, up to 8 UV channels, bone weights) and which `Mesh.Get*/Set*` pair backs each one |
| Mesh topology data | `Manual/mesh-topology-data.html` | `MeshTopology` enum (Triangle/Quad/Lines/LineStrip/Points) and how topology determines index-group size |
| Mesh index data | `Manual/mesh-index-data.html` | Index buffer semantics, shared vs. duplicated vertices, and Unity's clockwise winding-order rule for front-facing triangles |
| Mesh data for deformable meshes | `Manual/mesh-data-deformable-meshes.html` | Blend shape vertex deltas and bind poses — the two deformable-mesh-only data types |
| View mesh data visualizations | `Manual/view-mesh-data-visualizations.html` | Mesh Preview window modes (Wireframe, UV Checker, Normals, Tangents, Blendshapes) for debugging generated geometry visually |
| Creating/accessing meshes via script (landing) | `Manual/creating-meshes.html`, `Manual/create-mesh.html` | Overview of the three ways to get a mesh into a project (import, in-Editor tools, code) and links into the scripting sub-pages |
| Access meshes via the Mesh API | `Manual/UsingtheMeshClass.html` | Core scripting-workflow page: MeshFilter access, vertex/triangle array ordering rules, pointer to the advanced buffer API |
| Create a quad mesh via script (tutorial) | `Manual/Example-CreatingaBillboardPlane.html` | Worked step-by-step example building a simple quad mesh from script |
| Combine meshes manually | `Manual/combining-meshes.html` | When/why to use `Mesh.CombineMeshes` as a draw-call optimization vs. static batching |
| Mesh asset Inspector reference | `Manual/class-Mesh.html` | Mesh Inspector fields (Vertices/Indices/Skin/Other sections) — useful for interpreting a generated mesh's stats |
| Mesh Filter component reference | `Manual/class-MeshFilter.html` | `MeshFilter.mesh` vs `MeshFilter.sharedMesh` and how a mesh asset reaches the renderer |
| Mesh Renderer component reference | `Manual/class-MeshRenderer.html` | Companion renderer component that draws whatever `MeshFilter` references |
| Skinned Mesh Renderer reference | `Manual/class-SkinnedMeshRenderer.html`, `Manual/troubleshooting-skinned-mesh-renderer-visibility.html` | Renderer used for deformable/skinned procedural meshes instead of MeshFilter+MeshRenderer |
| Mesh compression | `Manual/compressing-mesh-data-optimization.html`, `Manual/types-of-mesh-data-compression.html`, `Manual/configure-mesh-compression.html` | Vertex Compression (FP32→FP16 per-channel) vs. Mesh Compression (on-disk, decompressed at load) — both apply to imported/authored mesh assets, including ones you save from generated data |
| Mesh Collider & cooking | `Manual/mesh-colliders.html`, `Manual/mesh-colliders-introduction.html`, `Manual/prepare-mesh-for-mesh-collider.html`, `Manual/physics-optimization-cpu-mesh-cooking-options.html`, `Manual/class-MeshCollider.html` | Cost of cooking a procedurally generated mesh for physics use — full collider/physics detail lives in `unity-physics`, cited here only for the mesh-generation-time cost tradeoff |
| Mesh components reference (index) | `Manual/mesh-components-reference.html` | Manual index page linking MeshFilter/MeshRenderer/MeshCollider/SkinnedMeshRenderer references |
| `Mesh` class scripting reference | `ScriptReference/Mesh.html` | The full member list, the "simple vs advanced API" distinction, and three canonical usage patterns (build-from-scratch, per-frame vertex animation, clear-and-rebuild) with worked examples |
| `Mesh` constructor & `Clear` | `ScriptReference/Mesh-ctor.html`, `ScriptReference/Mesh.Clear.html` | Creating an empty `Mesh` instance; wiping vertex/index data before a full rebuild to avoid stale out-of-bounds indices |
| Vertices (simple API) | `ScriptReference/Mesh-vertices.html`, `ScriptReference/Mesh.SetVertices.html`, `ScriptReference/Mesh.GetVertices.html` | Legacy array property vs. the `List<Vector3>`-friendly `Set/GetVertices` overloads |
| Triangles / indices (simple API) | `ScriptReference/Mesh-triangles.html`, `ScriptReference/Mesh.SetTriangles.html`, `ScriptReference/Mesh.SetIndices.html` | Per-submesh triangle assignment, `baseVertex` for exceeding 65535 vertices under 16-bit indices, the `SetIndices(topology, submesh)` overload for non-triangle topologies |
| Normals (simple API) | `ScriptReference/Mesh-normals.html`, `ScriptReference/Mesh.SetNormals.html`, `ScriptReference/Mesh.RecalculateNormals.html` | Manually supplying normals vs. auto-computing them from triangle/vertex data; smoothing behavior across shared vertices |
| Tangents (simple API) | `ScriptReference/Mesh-tangents.html`, `ScriptReference/Mesh.SetTangents.html`, `ScriptReference/Mesh.RecalculateTangents.html` | 4-component tangent format (xyz + w-sign for binormal), dependency on normals/UV0 being present first |
| UVs (simple API) | `ScriptReference/Mesh-uv.html`, `ScriptReference/Mesh.SetUVs.html`, `ScriptReference/Mesh.GetUVs.html` | Up to 8 UV channels (`uv`..`uv8`), `Vector2`/`Vector3`/`Vector4` UV data, `TEXCOORD0`-`7` shader semantic mapping |
| Vertex colors (simple API) | `ScriptReference/Mesh-colors.html`, `ScriptReference/Mesh-colors32.html`, `ScriptReference/Mesh.SetColors.html` | Float `Color` vs. byte-packed `Color32` vertex color storage |
| Bounds | `ScriptReference/Mesh-bounds.html`, `ScriptReference/Mesh.RecalculateBounds.html` | Bounding volume used for culling; must be recalculated manually after buffer-API vertex writes |
| Submeshes (simple API) | `ScriptReference/Mesh-subMeshCount.html`, `ScriptReference/Mesh.SetSubMeshes.html`, `ScriptReference/Mesh.GetSubMesh.html`, `ScriptReference/Rendering.SubMeshDescriptor.html` | One submesh per material; `SubMeshDescriptor` fields (indexStart/indexCount/topology/baseVertex/bounds) |
| `MeshTopology` enum | `ScriptReference/MeshTopology.html` | Triangles/Quads/Lines/LineStrip/Points values, with a two-submesh mixed-topology code example |
| `isReadable` / CPU-side mesh data | `ScriptReference/Mesh-isReadable.html` | When a generated mesh must stay CPU-readable (combining at runtime, NavMesh baking, Mesh Collider with negative-scale/skew) vs. when to free the CPU copy |
| `MarkDynamic` / `UploadMeshData` / `MarkModified` | `ScriptReference/Mesh.MarkDynamic.html`, `ScriptReference/Mesh.UploadMeshData.html`, `ScriptReference/Mesh.MarkModified.html` | GPU buffer strategy hint for frequently-updated meshes; forcing an immediate upload; notifying renderers of an out-of-band change |
| Index format | `ScriptReference/Mesh-indexFormat.html`, `ScriptReference/Rendering.IndexFormat.html` | 16-bit (65535 vertex cap) vs. 32-bit index buffers, and the platform-support caveat for 32-bit on some GPUs |
| `Mesh.AllocateWritableMeshData` | `ScriptReference/Mesh.AllocateWritableMeshData.html` | Entry point to the advanced MeshData API: allocate one or more writable `MeshData` structs for job/Burst-friendly mesh construction, with a full tetrahedron-mesh worked example |
| `Mesh.ApplyAndDisposeWritableMeshData` | `ScriptReference/Mesh.ApplyAndDisposeWritableMeshData.html` | Commit populated `MeshData` structs to real `Mesh` objects and dispose the array; validation caveats around submesh index ranges |
| `Mesh.AcquireReadOnlyMeshData` / `MeshUtility` variant | `ScriptReference/Mesh.AcquireReadOnlyMeshData.html`, `ScriptReference/MeshUtility.AcquireReadOnlyMeshData.html` | Job-safe read-only snapshot of existing mesh data; `MeshUtility` variant skips the `isReadable` check for Editor-only tooling |
| `Mesh.MeshDataArray` / `Mesh.MeshData` structs | `ScriptReference/Mesh.MeshDataArray.html`, `ScriptReference/Mesh.MeshData.html` | Container and per-mesh struct types underlying the advanced API; disposal rules |
| `MeshData` buffer setup | `ScriptReference/Mesh.MeshData.SetVertexBufferParams.html`, `ScriptReference/Mesh.MeshData.GetVertexData.html`, `ScriptReference/Mesh.MeshData.SetIndexBufferParams.html`, `ScriptReference/Mesh.MeshData.GetIndexData.html`, `ScriptReference/Mesh.MeshData.SetSubMesh.html` | The `MeshData`-struct equivalents of the `Mesh`-level advanced methods, callable from worker threads/Jobs |
| `Mesh` advanced buffer API | `ScriptReference/Mesh.SetVertexBufferParams.html`, `ScriptReference/Mesh.SetVertexBufferData.html`, `ScriptReference/Mesh.SetIndexBufferParams.html`, `ScriptReference/Mesh.SetIndexBufferData.html`, `ScriptReference/Mesh.SetSubMesh.html` | Direct raw-buffer writes on the main-thread `Mesh` object (no Jobs required); minimal validation, maximum performance |
| `VertexAttributeDescriptor` / attribute enums | `ScriptReference/Rendering.VertexAttributeDescriptor.html`, `ScriptReference/Rendering.VertexAttribute.html`, `ScriptReference/Rendering.VertexAttributeFormat.html` | Declaring a custom vertex layout (which attributes, what data type/dimension, which stream) for the advanced API |
| `MeshUpdateFlags` | `ScriptReference/Rendering.MeshUpdateFlags.html` | Per-call flags to skip index validation, bounds recalculation, bone-bounds reset, or renderer notification when using the advanced API |
| Query a mesh's vertex attribute layout | `ScriptReference/Mesh.GetVertexAttribute.html`, `ScriptReference/Mesh.GetVertexAttributes.html`, `ScriptReference/Mesh.HasVertexAttribute.html` | Inspecting what attributes/format/stream an existing (possibly imported) mesh actually uses before writing compatible code against it |
| Combining meshes (scripting) | `ScriptReference/Mesh.CombineMeshes.html`, `ScriptReference/CombineInstance.html` | `mergeSubMeshes`/`useMatrices`/`hasLightmapData` parameters and a full worked "combine all children" example |
| Blend shapes (scripting) | `ScriptReference/Mesh.AddBlendShapeFrame.html`, `ScriptReference/Mesh.ClearBlendShapes.html`, `ScriptReference/Mesh.GetBlendShapeFrameCount.html`, `ScriptReference/Mesh.GetBlendShapeFrameWeight.html`, `ScriptReference/Mesh.GetBlendShapeIndex.html`, `ScriptReference/Mesh-blendShapeCount.html` | Adding/querying morph-target frames from code; delta-vertex/normal/tangent semantics |
| Skinning data (scripting) | `ScriptReference/Mesh-bindposes.html`, `ScriptReference/Mesh-boneWeights.html`, `ScriptReference/Mesh.SetBoneWeights.html`, `ScriptReference/Mesh.GetAllBoneWeights.html`, `ScriptReference/Mesh.GetBonesPerVertex.html` | Bind pose matrices, legacy 4-bones-per-vertex `BoneWeight` vs. modern up-to-255-bones `BoneWeight1`/`SetBoneWeights` |
| `MeshFilter` scripting reference | `ScriptReference/MeshFilter.html`, `ScriptReference/MeshFilter-sharedMesh.html` | `.mesh` (auto-instances a per-object copy) vs. `.sharedMesh` (edits the shared asset directly) — the split most procedural-mesh bugs trip over |

## Key Guidelines

### Basic Mesh Construction (vertices/triangles/UVs)

A `Mesh` stores vertex data as several parallel arrays of equal length — position, normal, tangent, color, and up to 8 UV channels — where index *i* in every array describes the same logical vertex (`Manual/mesh-vertex-data.html`). Positions are required; everything else is optional and can be left unassigned if a shader doesn't need it. Triangles are a flat `int[]` of indices into the vertex arrays, taken three at a time; Unity culls back-facing triangles by default using a clockwise winding rule, so the indices for each face must run clockwise as seen from the visible side or the triangle renders invisible from the front and visible from the back (`Manual/mesh-index-data.html`). The `Mesh` class exposes two parallel scripting surfaces: a "simple" API (`SetVertices`, `SetTriangles`, `SetNormals`, `SetUVs`, `SetColors`, `SetIndices`, `SetBoneWeights`, plus the older bare array properties like `.vertices`/`.triangles`/`.uv`) that validates the data you hand it and is the right default for nearly all procedural-mesh code, and an "advanced" buffer API (covered below) for cases where that validation overhead actually shows up in a profile (`ScriptReference/Mesh.html`). When building a mesh from scratch, always assign vertices before triangles — Unity validates triangle indices against the current vertex count at assignment time, so assigning triangles first (or against a stale, smaller vertex array) throws an out-of-bounds exception. When *rebuilding* a mesh that already has data (e.g. regenerating a procedural terrain chunk in place), call `Mesh.Clear()` first so the old, larger index buffer can't reference vertices that no longer exist once the new (possibly smaller) vertex array is assigned.

```csharp
using UnityEngine;

[RequireComponent(typeof(MeshFilter), typeof(MeshRenderer))]
public class ProceduralGrid : MonoBehaviour
{
    [SerializeField] private int width = 10;
    [SerializeField] private int depth = 10;
    [SerializeField] private float cellSize = 1f;

    void Start()
    {
        Mesh mesh = new Mesh { name = "ProceduralGrid" };

        int vertsPerRow = width + 1;
        int vertsPerCol = depth + 1;
        var vertices = new Vector3[vertsPerRow * vertsPerCol];
        var uvs = new Vector2[vertices.Length];

        for (int z = 0; z < vertsPerCol; z++)
        {
            for (int x = 0; x < vertsPerRow; x++)
            {
                int i = z * vertsPerRow + x;
                vertices[i] = new Vector3(x * cellSize, 0f, z * cellSize);
                uvs[i] = new Vector2((float)x / width, (float)z / depth);
            }
        }

        var triangles = new int[width * depth * 6];
        int t = 0;
        for (int z = 0; z < depth; z++)
        {
            for (int x = 0; x < width; x++)
            {
                int a = z * vertsPerRow + x;
                int b = a + vertsPerRow;
                // Clockwise winding as seen from above (+Y) so the top face is front-facing.
                triangles[t++] = a;
                triangles[t++] = b + 1;
                triangles[t++] = b;
                triangles[t++] = a;
                triangles[t++] = a + 1;
                triangles[t++] = b + 1;
            }
        }

        // Order matters: vertices/UVs first, triangles last, so index validation always succeeds.
        mesh.vertices = vertices;
        mesh.uv = uvs;
        mesh.triangles = triangles;
        mesh.RecalculateNormals();
        mesh.RecalculateBounds();

        GetComponent<MeshFilter>().mesh = mesh; // .mesh instances a per-object copy, safe to own and later mutate
    }
}
```

### Normals, Tangents, and Bounds Recalculation

Normals, tangents, and bounds are all *derived* data that Unity does not keep in sync automatically once you hand-edit vertex positions — every one of them requires an explicit recalculation call, and each has a different dependency chain. `Mesh.RecalculateNormals()` derives per-vertex normals from the triangle/vertex data; because normals are averaged across every triangle sharing a vertex, vertices that were deliberately duplicated at a UV seam or hard edge (so each triangle can have visually distinct normals there) get correctly sharp results, while unintentionally duplicated vertices produce faceted shading where smooth was expected (`ScriptReference/Mesh.RecalculateNormals.html`). `Mesh.RecalculateTangents()` must run *after* normals and UV0 are both present, since it derives the 4-component tangent (xyz direction + w sign for the binormal cross product) from vertex positions, normals, and texture coordinates together — skipping this on a mesh whose shader samples a normal map produces broken or flat-looking normal mapping even though normals themselves look correct (`ScriptReference/Mesh.RecalculateTangents.html`, `Manual/mesh-vertex-data.html`). `Mesh.RecalculateBounds()` recomputes the bounding volume used for frustum culling; assigning through the simple `.triangles` property triggers this automatically, but the advanced buffer API and `Mesh.MeshData.SetSubMesh` do not always recalculate mesh-level bounds, so an explicit call is required after buffer-API writes or the mesh may vanish prematurely under culling despite rendering correctly up close. Both `RecalculateNormals` and `RecalculateTangents` silently convert the mesh's vertex position (and normal/UV0, respectively) data to `VertexAttributeFormat.Float32` if a different format was in use, which is a hidden cost worth knowing about if a mesh was deliberately built with a compressed (FP16) position or normal format via the advanced API.

```csharp
void RebuildWithNormals(Mesh mesh, Vector3[] vertices, int[] triangles, Vector2[] uvs)
{
    mesh.Clear();
    mesh.vertices = vertices;
    mesh.uv = uvs;
    mesh.triangles = triangles;

    mesh.RecalculateNormals();   // from triangles + vertices
    mesh.RecalculateTangents();  // needs normals + UV0 already set
    mesh.RecalculateBounds();    // redundant here since .triangles already did it, but required after buffer-API writes
}
```

### Submeshes & Multi-Material Meshes

A submesh is a slice of the shared index buffer rendered with one material; a `Renderer` with N materials needs exactly N submeshes on its mesh, matched by array order to the `Renderer.materials` array. `Mesh.subMeshCount` sets how many slices exist, and each slice's index range/topology is described by a `SubMeshDescriptor` (`indexStart`, `indexCount`, `topology`, `baseVertex`, plus an auto-computed `bounds`/`firstVertex`/`vertexCount` unless `MeshUpdateFlags.DontRecalculateBounds` is passed) set via `Mesh.SetSubMesh` (`ScriptReference/Rendering.SubMeshDescriptor.html`, `ScriptReference/Mesh.SetSubMesh.html`). The simple API's `SetTriangles(triangles, submeshIndex)` overload is the easiest path for a small, fixed number of materials — call it once per submesh index after the shared vertex array is assigned. Submesh index ranges must not overlap one another; the advanced API does not check this for you and an overlap produces undefined rendering rather than an exception.

```csharp
void BuildTwoMaterialMesh(Mesh mesh, Vector3[] vertices, int[] groundTriangles, int[] roadTriangles)
{
    mesh.Clear();
    mesh.vertices = vertices;
    mesh.subMeshCount = 2;
    mesh.SetTriangles(groundTriangles, 0); // rendered with materials[0]
    mesh.SetTriangles(roadTriangles, 1);   // rendered with materials[1]
    mesh.RecalculateNormals();
    mesh.RecalculateBounds();
}
```

### Advanced MeshData API for Performance

`Mesh.MeshData`/`Mesh.MeshDataArray` (in `UnityEngine`, backed by `Unity.Collections.NativeArray`) is a lower-level API purpose-built for writing mesh data from worker threads inside C# Jobs, with Burst compilation, instead of the managed-array simple API which only the main thread can touch. `Mesh.AllocateWritableMeshData(meshCount)` returns a `MeshDataArray` of writable `MeshData` structs; populate each one by calling `SetVertexBufferParams` with a `VertexAttributeDescriptor[]` layout (which attributes, their `VertexAttributeFormat`/dimension, and which of up to 4 interleaved streams each lives in), writing into the `NativeArray<T>` returned by `GetVertexData<T>()`, then `SetIndexBufferParams` + writing into `GetIndexData<T>()`, then setting `subMeshCount` and calling `SetSubMesh` per submesh (`ScriptReference/Mesh.AllocateWritableMeshData.html`). Finally, `Mesh.ApplyAndDisposeWritableMeshData(dataArray, mesh)` commits the data to real `Mesh` object(s) and disposes the array in one call — after this the `MeshDataArray` is invalid and must not be touched again. The mirror-image read path, `Mesh.AcquireReadOnlyMeshData(mesh)`, snapshots an *existing* mesh's data for job-safe reading (zero-copy as long as the source mesh isn't modified while the snapshot is alive, and it must be disposed, ideally via a `using` block); it throws if `Mesh.isReadable` is false, whereas the Editor-only `MeshUtility.AcquireReadOnlyMeshData` skips that check. A parallel, main-thread-only "advanced" surface exists directly on `Mesh` (`SetVertexBufferParams`/`SetVertexBufferData`/`SetIndexBufferParams`/`SetIndexBufferData`/`SetSubMesh`) for cases that want raw-buffer performance without going through Jobs at all — same validation tradeoffs, no `MeshData`/allocation ceremony (`ScriptReference/Mesh.SetVertexBufferParams.html`). Every advanced-API call performs minimal validation by default; `Rendering.MeshUpdateFlags` (`Default`, `DontValidateIndices`, `DontResetBoneBounds`, `DontNotifyMeshUsers`, `DontRecalculateBounds`, `DontValidateLodRanges`) lets you selectively re-enable or suppress specific checks per call.

```csharp
using UnityEngine;
using UnityEngine.Rendering;

[RequireComponent(typeof(MeshFilter))]
public class TetrahedronMeshData : MonoBehaviour
{
    void Start()
    {
        // Allocate mesh data for one mesh.
        var dataArray = Mesh.AllocateWritableMeshData(1);
        var data = dataArray[0];

        // 4 faces, 3 unique vertices per face (12 total) since normals differ per face.
        data.SetVertexBufferParams(12,
            new VertexAttributeDescriptor(VertexAttribute.Position),
            new VertexAttributeDescriptor(VertexAttribute.Normal, stream: 1));

        var sqrt075 = Mathf.Sqrt(0.75f);
        var p0 = new Vector3(0, 0, 0);
        var p1 = new Vector3(1, 0, 0);
        var p2 = new Vector3(0.5f, 0, sqrt075);
        var p3 = new Vector3(0.5f, sqrt075, sqrt075 / 3);

        var pos = data.GetVertexData<Vector3>();
        pos[0] = p0; pos[1] = p1; pos[2] = p2;
        pos[3] = p0; pos[4] = p2; pos[5] = p3;
        pos[6] = p2; pos[7] = p1; pos[8] = p3;
        pos[9] = p0; pos[10] = p3; pos[11] = p1;
        // Normals are left unset here and filled in later by RecalculateNormals.

        data.SetIndexBufferParams(12, IndexFormat.UInt16);
        var ib = data.GetIndexData<ushort>();
        for (ushort i = 0; i < ib.Length; ++i) ib[i] = i;

        data.subMeshCount = 1;
        data.SetSubMesh(0, new SubMeshDescriptor(0, ib.Length));

        var mesh = new Mesh { name = "Tetrahedron" };
        Mesh.ApplyAndDisposeWritableMeshData(dataArray, mesh); // dataArray is invalid after this call
        mesh.RecalculateNormals();
        mesh.RecalculateBounds();

        GetComponent<MeshFilter>().mesh = mesh;
    }
}
```
*(Worked example per `ScriptReference/Mesh.AllocateWritableMeshData.html`.)* The same struct family also underlies reading: wrap `Mesh.AcquireReadOnlyMeshData` in a `using` block, index into the returned `MeshDataArray`, and call `data.GetVertices(destinationNativeArray)` or the equivalent `Get*` accessor to pull data out for a Job without allocating a managed array copy.

### Combining Meshes (CombineInstance)

`Mesh.CombineMeshes` merges several source meshes into one destination `Mesh`, which is a legitimate draw-call optimization for meshes that sit close together and never move relative to each other (a static cabinet made of many drawer meshes, for example) — each `CombineInstance` in the input array pairs a source `mesh` with a `transform` matrix, and one `CombineInstance` is required per *submesh* if a source mesh has more than one (`Manual/combining-meshes.html`, `ScriptReference/CombineInstance.html`). `mergeSubMeshes = true` flattens every instance into a single submesh (only valid when every instance shares one material and topology); `mergeSubMeshes = false` keeps each instance as its own submesh in the result, for combining meshes that still need distinct materials. `useMatrices = true` applies each instance's transform to the vertices during the combine so everything lands in a common space; source meshes must be CPU-readable (`Mesh.isReadable == true`) or Unity logs a warning and skips that instance entirely, silently producing an incomplete combined mesh. Note the explicit tradeoff versus the technique this replaces: Unity cannot cull parts of a combined mesh individually — if any part of the combined result is onscreen, all of it draws — so for scenes that need per-object culling, static batching is the better default and manual combining is reserved for cases where the combine itself removes enough draw calls to be worth that loss.

```csharp
[RequireComponent(typeof(MeshFilter), typeof(MeshRenderer))]
public class CombineChildren : MonoBehaviour
{
    void Start()
    {
        MeshFilter[] meshFilters = GetComponentsInChildren<MeshFilter>();
        var instances = new CombineInstance[meshFilters.Length];

        for (int i = 0; i < meshFilters.Length; i++)
        {
            instances[i] = new CombineInstance
            {
                mesh = meshFilters[i].sharedMesh,
                transform = meshFilters[i].transform.localToWorldMatrix,
            };
            meshFilters[i].gameObject.SetActive(false);
        }

        var combinedMesh = new Mesh();
        combinedMesh.CombineMeshes(instances); // mergeSubMeshes defaults true, useMatrices defaults true
        GetComponent<MeshFilter>().sharedMesh = combinedMesh;
    }
}
```

### Skinned & Blend-Shape Meshes

Deformable meshes carry two additional, optional categories of data beyond the base vertex/index buffers: bind poses (for skinned meshes) and blend shapes (for morph-target animation) (`Manual/mesh-data-deformable-meshes.html`). A skinned mesh's `bindposes` array stores, per bone, the inverse of that bone's transform at rest — one entry per bone, indexed to match `SkinnedMeshRenderer.bones` (`ScriptReference/Mesh-bindposes.html`). Per-vertex bone influence has two parallel APIs: the legacy `Mesh.boneWeights` property stores a fixed-size `BoneWeight` struct (exactly 4 bone/weight pairs) per vertex, while the modern `BoneWeight1`-based `Mesh.SetBoneWeights`/`Mesh.GetAllBoneWeights`/`Mesh.GetBonesPerVertex` trio supports a variable, larger count (up to 255) of weighted bones per vertex and is the preferred path for anything new — weights passed to `SetBoneWeights` must be sorted most-significant-first, and Unity stores them at reduced (currently ≥16-bit normalized) precision, so don't expect bit-exact round trips (`ScriptReference/Mesh.SetBoneWeights.html`). Blend shapes are added with `Mesh.AddBlendShapeFrame(name, weight, deltaVertices, deltaNormals, deltaTangents)`: the delta arrays must be `Mesh.vertexCount` long (deltas, not absolute positions — subtract the base mesh's vertices/normals/tangents to compute them), frames for a multi-frame blend shape must be added in increasing weight order, and frames can only be appended to a brand-new shape or to the last shape already on the mesh, never inserted mid-sequence (`ScriptReference/Mesh.AddBlendShapeFrame.html`). Procedurally generating a fully rigged skinned mesh from scratch is rare; the far more common pattern is generating or modifying the base geometry (vertices/normals/tangents) procedurally while leaving bind poses and bone weights authored by an importer/DCC tool untouched, or driving existing blend shape weights at runtime via `SkinnedMeshRenderer.SetBlendShapeWeight` rather than authoring new frames in code.

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Forgetting `RecalculateNormals`/`RecalculateBounds` after assigning vertices via the buffer API | Unlike the simple `.triangles` property (which auto-recalculates bounds), advanced buffer writes don't recompute normals or mesh-level bounds automatically — call both explicitly, and `RecalculateTangents` too if the shader samples a normal map |
| Backface culling from wrong winding order | Unity culls back-facing triangles using clockwise winding as seen from the visible side; a counter-clockwise index order makes an otherwise-correct triangle invisible from the front and visible from behind — verify winding, not just index correctness |
| Exceeding 65535 vertices with the default 16-bit index format | `Mesh.indexFormat` defaults to `IndexFormat.UInt16` (max index 65535); a mesh needing more vertices must set `indexFormat = IndexFormat.UInt32` before assigning vertices/indices, or split into multiple submeshes using `baseVertex` to stay within per-submesh 65535-vertex windows |
| Assigning triangles before vertices (or before enough vertices) | Unity validates triangle/index values against the current vertex array size at assignment time; assigning triangles first, or against a stale smaller array, throws an out-of-bounds exception — always set vertices (and other per-vertex arrays) first |
| Mutating a shared Mesh asset via `MeshFilter.sharedMesh` instead of an owned instance | `sharedMesh` edits the actual project asset (or an already-shared runtime mesh) in place, silently affecting every other object referencing it; use `MeshFilter.mesh` (which auto-instances a per-object copy) or explicitly `Instantiate`/`new Mesh()` a private copy before mutating |
| Rebuilding a mesh's data without calling `Mesh.Clear()` first | Assigning a new, smaller vertex array while the old, larger triangle/index array is still attached can produce out-of-bounds indices Unity rejects, or leftover submesh ranges that no longer make sense — clear before reassigning when vertex count can shrink |
| Reading from a mesh with `Mesh.isReadable == false` | Meshes imported without "Read/Write Enabled," or explicitly marked non-readable via `Mesh.UploadMeshData(true)`, throw on any script access to vertex/index arrays at runtime (Editor-only access outside Play mode still works); check/set `isReadable` before any runtime `Get*` call, or before passing to `Mesh.CombineMeshes`/`Mesh.AcquireReadOnlyMeshData` |
| Calling `MarkDynamic()` on a mesh that rarely changes | `MarkDynamic` hints the graphics API to allocate GPU buffers optimized for frequent CPU updates, which can slightly cost rendering-read performance on meshes that don't actually change often — reserve it for meshes updated every frame or near it |
| Writing raw buffer data with the advanced API and expecting the same validation as the simple API | `SetVertexBufferParams`/`SetIndexBufferData`/`SetSubMesh` and friends perform minimal validation by design (for performance); supplying out-of-range indices or overlapping submesh ranges produces undefined behavior instead of a clear exception — get the logic right with the simple API first if unsure, then port to the advanced API |
| Forgetting `ApplyAndDisposeWritableMeshData` invalidates the `MeshDataArray` | After applying, the array (and all `MeshData` structs taken from it) can no longer be read or written; holding a reference and touching it afterward is a use-after-dispose bug |
| Setting only UV0 but expecting `RecalculateTangents` to work with no UVs at all | `RecalculateTangents` needs vertex positions, normals, *and* UV0 together; if UVs are absent entirely, Unity fills tangents with a fallback `(1,0,0,1)` rather than throwing, which can silently produce wrong-looking normal mapping that's easy to miss in review |
| Combining meshes that aren't CPU-readable | `Mesh.CombineMeshes` silently skips (with a console warning) any `CombineInstance` whose source mesh has `isReadable == false`, producing a combined mesh that's missing geometry rather than failing loudly |
| Assuming submesh index ranges can overlap safely | Both the simple submesh API and `SetSubMesh` require non-overlapping `indexStart`/`indexCount` ranges per submesh; nothing enforces this at the API boundary, and overlaps produce visually wrong rendering that's hard to trace back to the cause |
| Using `Mesh.boneWeights` (max 4 bones/vertex) when more influence is needed | The legacy `BoneWeight`-based property hard-caps at 4 bones per vertex; if a rig needs more, use the `BoneWeight1`-based `SetBoneWeights`/`GetAllBoneWeights`/`GetBonesPerVertex` API instead, which supports up to 255 |
| Expecting `Mesh Collider` cooking to be free for a procedurally regenerated mesh | Re-cooking collision data on every regeneration of a procedural mesh (e.g. a deforming terrain patch) is a real per-update CPU cost, separate from the render-mesh update itself — see `unity-physics` for collider/cooking-option tuning once geometry generation itself is confirmed correct |

## Quick Reference

| Item | Purpose |
|------|---------|
| `new Mesh()` | Creates an empty mesh with submesh count and LOD count both starting at 1 |
| `Mesh.Clear()` | Wipes all vertex data and triangle/index data; call before reassigning a smaller vertex array |
| `Mesh.vertices` / `SetVertices` / `GetVertices` | Vertex position array (object space); required attribute for every mesh |
| `Mesh.normals` / `SetNormals` / `GetNormals` / `RecalculateNormals` | Per-vertex surface direction; optional but needed for correct lighting |
| `Mesh.tangents` / `SetTangents` / `GetTangents` / `RecalculateTangents` | 4-component tangent (xyz + w binormal sign); needed for normal-mapped shaders |
| `Mesh.uv`..`Mesh.uv8` / `SetUVs` / `GetUVs` | Up to 8 texture coordinate channels (`TEXCOORD0`-`7`) |
| `Mesh.colors` / `Mesh.colors32` / `SetColors` | Per-vertex float or byte-packed color |
| `Mesh.triangles` / `SetTriangles` / `SetIndices` | Index buffer; grouped per `MeshTopology` (3 per triangle, etc.) |
| `MeshTopology` | Triangles / Quads / Lines / LineStrip / Points — set per submesh |
| `Mesh.subMeshCount` / `SetSubMesh` / `SetSubMeshes` / `GetSubMesh` | Number of material-slot slices and their index ranges/topology |
| `SubMeshDescriptor` | `indexStart`, `indexCount`, `topology`, `baseVertex`, `bounds`, `firstVertex`, `vertexCount` for one submesh |
| `Mesh.bounds` / `RecalculateBounds` | Culling bounding volume; auto-updated by `.triangles`, must be called manually after buffer-API writes |
| `Mesh.indexFormat` (`IndexFormat.UInt16`/`UInt32`) | 16-bit (65535 vertex cap, default) vs. 32-bit index buffer |
| `Mesh.isReadable` | Whether CPU-side mesh data is retained after GPU upload; required for runtime `Get*` calls, combining, NavMesh baking |
| `Mesh.MarkDynamic()` | Hints the graphics API to use update-optimized GPU buffers for a frequently-modified mesh |
| `Mesh.UploadMeshData(markNoLongerReadable)` | Forces an immediate GPU upload of pending changes; can also free the CPU-side copy |
| `Mesh.MarkModified()` | Notifies Renderer components of an out-of-band mesh change |
| `Mesh.SetVertexBufferParams` / `SetVertexBufferData` | Advanced API: define and write raw vertex buffer layout/data on the main thread |
| `Mesh.SetIndexBufferParams` / `SetIndexBufferData` | Advanced API: define and write raw index buffer size/format/data |
| `VertexAttributeDescriptor` | Declares one vertex attribute's type/format/dimension/stream for the advanced API |
| `VertexAttribute` / `VertexAttributeFormat` | Enum of attribute kinds (Position, Normal, Tangent, Color, TexCoord0-7, BlendWeight, BlendIndices) and their possible data types |
| `Rendering.MeshUpdateFlags` | Per-call flags to skip index validation, bounds recalculation, bone-bounds reset, or renderer notification |
| `Mesh.AllocateWritableMeshData(count)` | Advanced API: allocate one or more writable `MeshData` structs for Job/Burst-friendly mesh construction |
| `Mesh.ApplyAndDisposeWritableMeshData(data, mesh)` | Commits populated `MeshData` to real `Mesh` object(s) and disposes the `MeshDataArray` |
| `Mesh.AcquireReadOnlyMeshData(mesh)` / `MeshUtility.AcquireReadOnlyMeshData` | Job-safe read-only snapshot of existing mesh data; Editor-only variant skips the `isReadable` check |
| `Mesh.MeshDataArray` / `Mesh.MeshData` | Container/per-mesh struct types for the advanced Jobs-friendly API |
| `Mesh.CombineMeshes(instances, mergeSubMeshes, useMatrices, hasLightmapData)` | Merges several source meshes/submeshes into this mesh as a draw-call optimization |
| `CombineInstance` | Pairs one source `mesh` (+ optional submesh index) with a `transform` matrix for `CombineMeshes` |
| `Mesh.AddBlendShapeFrame` / `ClearBlendShapes` / `GetBlendShapeFrameCount`/`Weight`/`Index` | Author and query morph-target blend shape data |
| `Mesh.bindposes` | Per-bone inverse rest-pose matrices for a skinned mesh, indexed to match `SkinnedMeshRenderer.bones` |
| `Mesh.boneWeights` (legacy) vs. `SetBoneWeights`/`GetAllBoneWeights`/`GetBonesPerVertex` (modern) | 4-bones-per-vertex `BoneWeight` struct vs. up-to-255-bones `BoneWeight1`-based API |
| `MeshFilter.mesh` vs. `MeshFilter.sharedMesh` | `.mesh` auto-instances a private per-object copy (safe to mutate); `.sharedMesh` edits the actual shared asset in place |
| `Mesh.HasVertexAttribute` / `GetVertexAttribute(s)` | Inspect what attributes/format/stream an existing mesh actually stores |

## Advanced Notes

**MeshData vs. the classic managed-array API.** The classic simple API (`SetVertices`, `.triangles`, etc.) operates on ordinary managed C# arrays/lists, which means it can only be called from the main thread and always pays a managed-allocation and per-call validation cost. `Mesh.MeshData`/`MeshDataArray` wraps `Unity.Collections.NativeArray<T>` buffers instead, which are safe to read and write from worker threads inside `IJob`/`IJobParallelFor` implementations and are Burst-compilable — this is the intended path for generating large amounts of procedural geometry (terrain chunks, voxel meshes, particle-driven geometry) every frame without spiking the main thread or triggering GC. The tradeoff is ceremony and reduced safety: you must get vertex layout, index format, and submesh ranges correct yourself, since the advanced API performs minimal validation by default (`ScriptReference/Mesh.html`, "Simple vs Advanced Mesh API"). A reasonable rule of thumb: build correctness with the simple API first, and only move hot, per-frame, high-volume mesh generation to `MeshData` + Jobs after profiling shows the managed-array path is the actual bottleneck.

**`MarkDynamic` for frequently-updated meshes.** Calling `Mesh.MarkDynamic()` once, ideally before the first vertex/index upload, tells the underlying graphics API (Direct3D/Vulkan/Metal) to allocate the mesh's GPU buffers using a strategy suited for frequent CPU-side writes rather than the default strategy optimized for static, rarely-changing geometry. This meaningfully speeds up repeated `SetVertices`/`SetVertexBufferData`/`UploadMeshData` calls on the same mesh (e.g. a wave-animated water plane, cloth-like deformation, or a mesh driven every `Update`), at a small potential cost to raw rendering-read throughput — so it should be reserved for meshes that are actually updated every frame or close to it, not applied blanket to every procedurally-generated mesh regardless of how often it changes (`ScriptReference/Mesh.MarkDynamic.html`).

**Mesh Collider cooking cost for procedural geometry.** Attaching a `MeshCollider` to a procedurally generated or frequently regenerated mesh means Unity must "cook" that geometry into a physics-engine-friendly collision representation every time the source mesh changes meaningfully — this cooking step is a distinct, often non-trivial CPU cost on top of the mesh-generation work itself, and its cost is tunable via the Mesh Collider's Cooking Options (`Manual/physics-optimization-cpu-mesh-cooking-options.html`, `Manual/prepare-mesh-for-mesh-collider.html`). If a procedural mesh needs to double as a collider and is regenerated often (deforming terrain, runtime-carved geometry), budget for re-cooking cost separately from render-mesh generation cost, and consider whether a simpler proxy collider (primitive shapes, a lower-resolution convex hull) can stand in instead of cooking the full-detail render mesh every update. Full Rigidbody/Collider/cooking-option tuning guidance lives in `unity-physics` — this skill only flags the cost as something to account for once the mesh-generation code itself is working.

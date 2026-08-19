---
name: unity-addressables-assets
description: Use when managing Unity asset loading/memory — the Addressables system, asset references, or legacy AssetBundles. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Addressables & Asset Management

## Retrieval Sources

Local doc coverage for this topic is **uneven and re-verified on disk** as of this pass. The Addressables package ships its own separate documentation set (hosted at `docs.unity3d.com/Packages/com.unity.addressables@.../`) that is **not** mirrored in this local copy — only two thin package-overview pages exist in `Manual/` (830 and 702 words respectively, both landing-page-style descriptions with no API tables, no code samples, and no cross-links to `AsyncOperationHandle`, `AssetReference`, `ResourceLocator`, or any other Addressables type). There is **no local `ScriptReference/` coverage at all** for the `UnityEngine.AddressableAssets` or `UnityEngine.ResourceManagement` namespaces — confirmed by directory listing, zero `Addressables.*.html` or `AsyncOperationHandle.*.html` files exist anywhere under `ScriptReference/`.

By contrast, the legacy **AssetBundle** system (the substrate Addressables is built on) has deep, current, real coverage: 19 Manual pages plus well over 100 ScriptReference pages, all present under `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` and confirmed via `wc -w` to contain substantive prose (700–2,200 words each), not stubs. The **Resources system** (the third leg of Unity asset loading) also has real Manual + ScriptReference coverage. Treat local retrieval as authoritative for AssetBundles, Resources, and Caching; treat it as **absent** for Addressables API specifics — those answers below are drawn from accurate pretrained knowledge and are labeled as such wherever used.

| Source | Path | Use for |
|---|---|---|
| Addressables package overview (thin stub) | `Manual/com.unity.addressables.html` | High-level description only, ~830 words, no API detail — do not rely on it for anything beyond a one-paragraph intro |
| Addressables for Android (thin stub) | `Manual/com.unity.addressables.android.html` | ~702 words; Android-specific packaging notes only |
| AssetBundle/Addressables determinism | `Manual/build-deterministic-assetbundles-addressables.html` | Reproducible builds across both systems; explains SBP content-hash vs. legacy input-hash |
| Use AssetBundles to load assets at runtime (section landing) | `Manual/assetbundles-section.html` | Top-level index of every AssetBundle Manual sub-page |
| Introduction to AssetBundles | `Manual/AssetBundlesIntro.html` | Core concepts, when to use AssetBundles vs. alternatives |
| Organizing assets into AssetBundles | `Manual/AssetBundles-Preparing.html` | Limitations (no scripts, no mixed scenes+assets, no StreamingAssets files, one bundle per asset), naming rules, organization strategy |
| Assign assets to an AssetBundle | `Manual/assetbundles-assign-assets.html` | Editor Inspector workflow and `AssetImporter.assetBundleName`/`assetBundleVariant` scripted workflow |
| Build assets into AssetBundles | `Manual/AssetBundles-Building.html` | `BuildPipeline.BuildAssetBundles` workflow; incremental build hashing (`IncrementalBuildHash`, `TypeTreeHash`) |
| Handling dependencies between AssetBundles | `Manual/AssetBundles-Dependencies.html` | Cross-bundle dependency resolution, implicit asset duplication when a shared object isn't its own bundle |
| Loading assets from AssetBundles | `Manual/AssetBundles-Native.html` | Full survey of load APIs: `LoadFromFile`, `LoadFromMemory`, `LoadFromStream`, sync vs. async |
| AssetBundle caching | `Manual/assetbundles-caching.html` | Disk-based cache via `UnityWebRequestAssetBundle`, LZ4 re-encoding on cache write, `Caching` class |
| Verifying downloaded AssetBundles | `Manual/AssetBundles-Integrity.html` | Hash/CRC verification, patching model, why Unity has no differential patching |
| Optimizing AssetBundle memory usage | `Manual/assetbundles-optimizing.html` | Loading cache, TypeTrees, `AssetBundle.memoryBudgetKB` |
| Analyzing AssetBundles | `Manual/assetbundles-analyze.html` | Build Report / Analyze window workflow for inspecting bundle contents and duplication |
| AssetBundle compression formats | `Manual/assetbundles-compression-format.html` | LZMA vs. LZ4 (`ChunkBasedCompression`) vs. uncompressed (`UncompressedAssetBundle`), tradeoffs |
| AssetBundle file format reference | `Manual/assetbundles-file-format.html` | Binary layout of `.bundle` archives, serialized file structure |
| AssetBundle platform considerations | `Manual/assetbundles-platforms.html` | StreamingAssets loading path, CDN/CCD delivery, Android asset packs, console DLC patterns |
| AssetBundles in Web | `Manual/webgl-assetbundles.html` | WebGL-specific constraints on bundle loading and compression |
| Dedicated Server AssetBundles | `Manual/dedicated-server-assetbundles.html` | Headless/server build considerations for bundle loading |
| Asset Bundle module page | `Manual/com.unity.modules.assetbundle.html` | Built-in module description for `UnityEngine.AssetBundleModule` |
| Unity Web Request Asset Bundle module page | `Manual/com.unity.modules.unitywebrequestassetbundle.html` | Built-in module description for `UnityWebRequestAssetBundle` |
| Resources system (landing) | `Manual/assets-resources-system.html` | Index page linking to the two sub-pages below |
| Load and unload assets with the Resources system | `Manual/assets-resources-system-load.html` | Concrete `Resources.Load<T>` code sample, multi-folder note, unload guidance |
| `AssetBundle` class | `ScriptReference/AssetBundle.html` | Full member list: load/unload API surface |
| `AssetBundle.LoadFromFile` / `LoadFromFileAsync` | `ScriptReference/AssetBundle.LoadFromFile.html`, `ScriptReference/AssetBundle.LoadFromFileAsync.html` | Local synchronous/async bundle load |
| `AssetBundle.LoadAssetAsync` | `ScriptReference/AssetBundle.LoadAssetAsync.html` | Async single-asset load from an already-open bundle |
| `AssetBundle.Unload` / `UnloadAsync` | `ScriptReference/AssetBundle.Unload.html`, `ScriptReference/AssetBundle.UnloadAsync.html` | Memory release, `unloadAllLoadedObjects` flag, async variant to avoid a frame hitch |
| `AssetBundleManifest` | `ScriptReference/AssetBundleManifest.html` | `GetAllDependencies`, `GetDirectDependencies`, `GetAssetBundleHash` — dependency graph queries |
| `BuildPipeline.BuildAssetBundles` | `ScriptReference/BuildPipeline.BuildAssetBundles.html` | Build entry point; overloads for manifest-driven and parameter-object builds |
| `BuildAssetBundleOptions` | `ScriptReference/BuildAssetBundleOptions.html` | All build flags: `ChunkBasedCompression`, `UncompressedAssetBundle`, `ForceRebuildAssetBundle`, `IgnoreTypeTreeChanges`, etc. |
| `Caching` class | `ScriptReference/Caching.html` | `ClearCache`, `IsVersionCached`, `MarkAsUsed`, multi-cache management |
| `UnityWebRequestAssetBundle` | `ScriptReference/Networking.UnityWebRequestAssetBundle.html` | `GetAssetBundle` overloads (URL, URL+hash, URL+version+CRC) for remote/cached loading |
| `DownloadHandlerAssetBundle` | `ScriptReference/Networking.DownloadHandlerAssetBundle.html` | `GetContent`, `assetBundle` accessor after a web request completes |
| `Resources.Load` / `LoadAsync` / `UnloadAsset` / `UnloadUnusedAssets` | `ScriptReference/Resources.Load.html`, `.LoadAsync.html`, `.UnloadAsset.html`, `.UnloadUnusedAssets.html` | Full Resources-folder API surface, sync/async load and manual unload |

## Key Guidelines

### Addressables Concepts (pretrained knowledge — local docs are a thin stub, flagged explicitly)

Everything in this subsection comes from accurate pretrained knowledge of the `com.unity.addressables` package, **not** from local retrieval — the local docs do not cover it beyond a one-paragraph overview, so verify against `docs.unity3d.com/Packages/com.unity.addressables@latest` or in-Editor API docs before shipping. Addressables is a content-management layer built on top of AssetBundles (and, for local-only content, direct asset references) that lets you reference an asset by a string **key** or **label** instead of a hard scene/inspector reference, resolve it asynchronously at runtime, and have Unity manage ref-counting, dependency loading, and (optionally) remote hosting for you. Content is organized into **Addressable Groups**, each of which maps to build settings — local vs. remote, which bundle(s) it packs into, compression — so you can, for example, ship core content in the player build while hosting a DLC group on a CDN that updates independently of app store releases. Every asynchronous Addressables call (`LoadAssetAsync`, `InstantiateAsync`, `LoadSceneAsync`, `DownloadDependenciesAsync`) returns an `AsyncOperationHandle` (or the generic `AsyncOperationHandle<T>`), which is a ref-counted handle: each successful load increments an internal reference count for that asset and its dependencies, and the count must be decremented with a matching `Addressables.Release(handle)` (or `Addressables.ReleaseInstance` for `InstantiateAsync` results) or the asset — and everything it depends on — stays resident. Because the count is reference-based rather than GC-based, a leaked handle is invisible to the Unity Profiler's normal GC pressure view; it shows up only as elevated resident memory in the Addressables/Memory profiler modules.

```csharp
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;

public class AddressableLoader : MonoBehaviour
{
    AsyncOperationHandle<GameObject> _handle;

    async void Start()
    {
        _handle = Addressables.LoadAssetAsync<GameObject>("Enemies/Goblin");
        await _handle.Task;

        if (_handle.Status == AsyncOperationStatus.Succeeded)
        {
            Instantiate(_handle.Result, transform.position, Quaternion.identity);
        }
        else
        {
            Debug.LogError($"Failed to load addressable: {_handle.OperationException}");
        }
    }

    void OnDestroy()
    {
        // Symmetric release — required even on a Succeeded handle, or the
        // asset (and its dependency bundles) never unload.
        if (_handle.IsValid())
            Addressables.Release(_handle);
    }
}
```

### Legacy AssetBundles — Building & Organizing (from real local docs)

`BuildPipeline.BuildAssetBundles` is the entry point for building the legacy system; it consumes the AssetBundle names/variants assigned to assets (via the Inspector's AssetBundle dropdown or `AssetImporter.SetAssetBundleNameAndVariant`) and a `BuildAssetBundleOptions` flag set, and writes `.bundle` files plus a top-level manifest bundle to the output folder. Per `Manual/AssetBundles-Preparing.html`, there are hard organizational constraints: you cannot combine scenes and non-scene assets in one bundle, you cannot include script assets or files already under `StreamingAssets` in a bundle, and an asset can only belong to **one** AssetBundle — if two bundles both need it and it isn't given its own bundle, Unity duplicates it into both, inflating total download size. Builds are incremental by default: Unity computes an `IncrementalBuildHash` per bundle from build inputs (assets, dependencies, target platform, platform-specific settings) and only rebuilds a bundle when that hash — or a secondary `TypeTreeHash` covering serialization-format changes — differs from the previous build's `.manifest` file. `BuildAssetBundleOptions.ForceRebuildAssetBundle` bypasses this cache entirely; `IgnoreTypeTreeChanges` skips only the secondary check. Because the incremental hash doesn't cover every possible build influence, treat "clean forced rebuild" as the reliable fallback whenever a bundle seems stale despite no apparent input changes.

```csharp
using UnityEditor;
using UnityEngine;
using System.IO;

public static class BundleBuilder
{
    [MenuItem("Tools/Build AssetBundles")]
    static void BuildAllBundles()
    {
        string outputPath = "Assets/StreamingAssets/AssetBundles";
        if (!Directory.Exists(outputPath))
            Directory.CreateDirectory(outputPath);

        BuildPipeline.BuildAssetBundles(
            outputPath,
            BuildAssetBundleOptions.ChunkBasedCompression, // LZ4, good default for StreamingAssets
            EditorUserBuildSettings.activeBuildTarget);
    }
}
```

### Legacy AssetBundles — Loading & Dependency Management (from real local docs)

`Manual/AssetBundles-Native.html` documents the full load surface: `AssetBundle.LoadFromFile`/`LoadFromFileAsync` for local disk paths (fastest — memory-maps rather than copies), `LoadFromMemory`/`LoadFromMemoryAsync` for an in-memory byte array (e.g., after custom decryption), `LoadFromStream`/`LoadFromStreamAsync` for a `Stream`, and `UnityWebRequestAssetBundle.GetAssetBundle` for remote or cache-backed loading over HTTP. Once a bundle handle is open, individual assets are pulled with `AssetBundle.LoadAsset`/`LoadAssetAsync` (single) or `LoadAllAssets`/`LoadAllAssetsAsync` (everything in the bundle). Per `Manual/AssetBundles-Dependencies.html`, bundles become dependent on one another whenever an object in bundle A references an object living in bundle B; if that referenced object isn't itself assigned to a bundle, Unity silently embeds a copy into every dependent bundle instead of erroring, which is a common unexpected size blow-up. `AssetBundleManifest` (obtained by loading the top-level manifest bundle that `BuildAssetBundles` always emits alongside the content bundles) exposes `GetAllDependencies(bundleName)` and `GetDirectDependencies(bundleName)` so you can load a bundle's full dependency chain **before** loading the bundle itself — loading out of order produces missing-reference assets rather than an explicit error.

```csharp
using UnityEngine;
using System.Collections;
using System.IO;

public class DependencyAwareLoader : MonoBehaviour
{
    IEnumerator LoadWithDependencies(string bundleName, string bundleDir)
    {
        // Manifest bundle is named after the output folder itself.
        var manifestBundle = AssetBundle.LoadFromFile(
            Path.Combine(bundleDir, Path.GetFileName(bundleDir)));
        var manifest = manifestBundle.LoadAsset<AssetBundleManifest>("AssetBundleManifest");

        foreach (string dep in manifest.GetAllDependencies(bundleName))
        {
            if (AssetBundle.GetAllLoadedAssetBundles() == null) { }
            AssetBundle.LoadFromFile(Path.Combine(bundleDir, dep));
        }

        var request = AssetBundle.LoadFromFileAsync(Path.Combine(bundleDir, bundleName));
        yield return request;

        AssetBundle bundle = request.assetBundle;
        if (bundle == null)
        {
            Debug.LogError($"Failed to load bundle {bundleName}");
            yield break;
        }

        var assetRequest = bundle.LoadAssetAsync<GameObject>("Player");
        yield return assetRequest;
        Instantiate(assetRequest.asset as GameObject);
    }
}
```

### Legacy AssetBundles — Caching, Compression & Integrity (from real local docs)

Unity's built-in disk cache (`Manual/assetbundles-caching.html`) intercepts bundles downloaded via `UnityWebRequestAssetBundle` and stores them re-encoded as LZ4 for fast subsequent loads, regardless of the original build compression — unless `Caching.compressionEnabled` is `false`, in which case bundles are cached uncompressed. The `Caching` class lets you inspect (`IsVersionCached`), pin (`MarkAsUsed`), and evict (`ClearCache`, `ClearCachedVersion`, `ClearAllCachedVersions`) cached versions, and supports multiple cache locations via `Caching.AddCache`/`GetAllCachePaths`. Per `Manual/assetbundles-compression-format.html`, there are three build-time compression choices: **LZMA** (default, smallest download but must decompress the whole archive before any object can be read — slow first access, and per `Manual/assetbundles-platforms.html` explicitly discouraged for `StreamingAssets` bundles because decompressing to a temp file is wasteful there), **LZ4** (`ChunkBasedCompression`, chunk-based so individual objects can be decompressed on demand — the cache's own re-encoding target, and the generally recommended default), and **uncompressed** (`UncompressedAssetBundle`, largest on disk, fastest to open, 16-byte aligned). `Manual/AssetBundles-Integrity.html` covers verification: `UnityWebRequestAssetBundle.GetAssetBundle` overloads accept a hash or CRC to validate downloaded content and trigger a re-download when a newer version is requested, but Unity has **no built-in differential patching** — every update re-downloads the full bundle; a patching system must track local vs. server version lists itself and diff them to decide what to re-fetch.

```csharp
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;

public class RemoteBundleLoader : MonoBehaviour
{
    IEnumerator LoadRemote(string url, Hash128 hash)
    {
        // Cache-aware: skips the download entirely if this hash is already cached.
        using UnityWebRequest req = UnityWebRequestAssetBundle.GetAssetBundle(url, hash, 0);
        yield return req.SendWebRequest();

        if (req.result != UnityWebRequest.Result.Success)
        {
            Debug.LogError($"Bundle download failed: {req.error}");
            yield break;
        }

        AssetBundle bundle = DownloadHandlerAssetBundle.GetContent(req);
        Debug.Log($"Loaded {bundle.name}, cached: {Caching.IsVersionCached(url, hash)}");
    }
}
```

### Legacy AssetBundles — Memory & Unloading (from real local docs)

`Manual/assetbundles-optimizing.html` describes what a loaded AssetBundle actually occupies: a **loading cache** of recently accessed file pages (sized by `AssetBundle.memoryBudgetKB`), and **TypeTrees** (serialization metadata, kept resident for the life of the bundle unless stripped at build time with `BuildAssetBundleOptions.DisableWriteTypeTree`). Unloading has two very different modes: `AssetBundle.Unload(false)` frees only the compressed archive and loading-cache memory — any objects already instantiated or loaded from the bundle remain fully resident and usable, which is correct when you plan to keep using loaded assets but don't need the bundle handle anymore. `AssetBundle.Unload(true)` additionally destroys every asset object that came from the bundle, including ones still referenced by live GameObjects, which is the source of the single most common AssetBundle bug (see Common Mistakes). `AssetBundle.UnloadAsync` performs the same work spread across frames to avoid a hitch on bundles with many objects.

```csharp
using UnityEngine;

public class BundleUnloadExample : MonoBehaviour
{
    AssetBundle _bundle;
    GameObject _spawnedPrefabInstance;

    void OnDisable()
    {
        if (_bundle == null) return;

        // Safe pattern: destroy consumers of the bundle's assets FIRST,
        // then Unload(true) once nothing references bundle-owned objects.
        if (_spawnedPrefabInstance != null)
            Destroy(_spawnedPrefabInstance);

        _bundle.Unload(true);
        _bundle = null;
    }
}
```

### Resources Folder Tradeoffs (from real local docs)

`Manual/assets-resources-system-load.html` documents the mechanism plainly: any asset placed under a folder literally named `Resources` (you can have several, in different subfolders, and packages can ship their own) is loaded by calling `Resources.Load<T>("path/relative/to/Resources/without/extension")`. The tradeoff is structural, not just stylistic: everything under any `Resources/` folder is force-included in the build and indexed at build time regardless of whether it's ever referenced by a scene, so it inflates both build size and the startup asset-index cost, and unlike AssetBundles/Addressables it offers no fine-grained partial unload — `Resources.UnloadAsset` frees one asset object, but the folder's build-time inclusion cost is already paid. `Resources.UnloadUnusedAssets` is a broader, expensive, asynchronous sweep that should not be called every frame.

```csharp
using UnityEngine;

public class LoadFromResources : MonoBehaviour
{
    public Renderer targetRenderer;
    Texture2D _loadedTexture;

    void Start()
    {
        _loadedTexture = Resources.Load<Texture2D>("Textures/MyTexture");
        if (_loadedTexture != null)
            targetRenderer.material.mainTexture = _loadedTexture;
    }

    void OnDestroy()
    {
        if (_loadedTexture != null)
        {
            Resources.UnloadAsset(_loadedTexture);
            _loadedTexture = null;
        }
    }
}
```

### Remote Content Delivery

For AssetBundles, `Manual/assetbundles-platforms.html` describes hosting bundles on a plain web server and pulling them with `UnityWebRequestAssetBundle`, or using Unity's **Cloud Content Delivery (CCD)** service, which the docs note integrates well with Addressables specifically for large, dynamic content sets. Console platforms typically don't fetch bundles directly over the web from in-game code at all — they're delivered through the platform's own DLC/store mechanism instead. On Android, bundles can be packaged into Google Play's native asset-pack format for store-managed delivery. For Addressables (pretrained knowledge, not locally documented), the equivalent mechanism is a **remote catalog**: you build a "Remote" content profile, host the generated catalog JSON plus its content bundles on a CDN or CCD bucket, and point the player's `RuntimeSettings`/catalog URL at that host — Addressables checks the remote catalog hash against the cached one (`Addressables.CheckForCatalogUpdates` / `UpdateCatalogs`) and only re-downloads bundles whose content hash changed, giving you post-release content or DLC updates without an app-store resubmission, layered on top of the exact same `UnityWebRequestAssetBundle` + `Caching` machinery described above.

```csharp
using UnityEngine;
using UnityEngine.AddressableAssets;
using System.Collections.Generic;

public class RemoteCatalogUpdater : MonoBehaviour
{
    async void Start()
    {
        var checkHandle = Addressables.CheckForCatalogUpdates(false);
        List<string> catalogsToUpdate = await checkHandle.Task;
        Addressables.Release(checkHandle);

        if (catalogsToUpdate.Count > 0)
        {
            var updateHandle = Addressables.UpdateCatalogs(catalogsToUpdate);
            await updateHandle.Task;
            Addressables.Release(updateHandle);
            Debug.Log("Remote catalogs updated — new/changed remote content is now resolvable.");
        }
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Leaking memory by never calling `Addressables.Release` | Handles are ref-counted, not garbage-collected on scene unload; always release in `OnDestroy`/cleanup paths symmetric to the load call |
| Bloating build size and cold-start time via `Resources/` | Everything in any `Resources/` folder is force-included and indexed at build time; move runtime-optional content to Addressables or AssetBundles |
| Treating `AssetBundle.Unload(false)` as a full unload | Leaves loaded object instances resident; use `Unload(true)` once no live references remain, or explicitly destroy consumers first |
| Calling `AssetBundle.Unload(true)` while objects from it are still in use | Destroys live, referenced objects out from under the scene, producing "missing" objects/`MissingReferenceException`; destroy consumers before unloading with `true` |
| Shipping AssetBundles without a versioning/hash scheme | Stale bundles get served from cache indefinitely; pass a `Hash128`/version to `UnityWebRequestAssetBundle.GetAssetBundle` on every content change |
| Mixing Addressables and manual AssetBundle loading of the same asset | Causes duplicate loads or dependency-resolution conflicts; pick one system per asset and be consistent within a project |
| Awaiting an `AsyncOperationHandle` without checking `Status` | Silent failures propagate as null assets; check `handle.Status == AsyncOperationStatus.Succeeded` before use |
| Loading a bundle without first loading its dependencies via `AssetBundleManifest.GetAllDependencies` | Produces assets with missing material/texture/mesh references rather than an explicit load error; always resolve and load the dependency chain first |
| Assuming the incremental build hash catches every change | `IncrementalBuildHash`/`TypeTreeHash` don't cover every build influence; when a bundle seems stale for no visible reason, force a clean rebuild with `BuildAssetBundleOptions.ForceRebuildAssetBundle` |
| Using LZMA compression for bundles shipped in `StreamingAssets` | LZMA requires decompressing the whole archive to a temp file before any object loads, which is wasteful for local reads; use `ChunkBasedCompression` (LZ4) or uncompressed instead |
| Assigning the same asset to two different AssetBundles | Not allowed for a bundle explicitly containing it, but an *unassigned* shared dependency gets silently duplicated into every bundle that references it, inflating total download size — give shared assets their own bundle |
| Mixing scenes and non-scene assets in one AssetBundle | Not supported; scenes must be built into a separate bundle from regular assets |
| Assuming Unity supports differential/delta patching of AssetBundles out of the box | It doesn't — every update re-downloads the full bundle via `UnityWebRequestAssetBundle`; a custom patcher must diff local vs. server version lists itself |
| Ignoring `AssetBundle.memoryBudgetKB` on memory-constrained platforms | The loading cache can grow unexpectedly on repeated random access across a large bundle; set an explicit budget rather than relying on defaults |
| Forgetting that the Editor can load AssetBundles built for *any* platform | Useful for quick iteration, but it masks platform-specific load failures (e.g. wrong compression, missing platform variant) that only surface in a real player build — always smoke-test on target platform before shipping |

## Quick Reference

| API / Concept | Purpose |
|---|---|
| `Addressables.LoadAssetAsync<T>(key)` | Load by key/label, returns `AsyncOperationHandle<T>` |
| `Addressables.InstantiateAsync(key)` | Load and instantiate a prefab, ref-counted like a load |
| `Addressables.Release(handle)` | Decrement ref count / free the loaded asset |
| `Addressables.ReleaseInstance(gameObject)` | Release + destroy an addressable-instantiated GameObject |
| `Addressables.LoadSceneAsync(key)` | Load a scene registered as an addressable |
| `Addressables.CheckForCatalogUpdates` / `UpdateCatalogs` | Detect and pull remote catalog/content changes at runtime |
| `Addressables.DownloadDependenciesAsync(key)` | Pre-warm/pre-download a key's dependency bundles (e.g. show a progress bar before a level) |
| `AsyncOperationHandle` / `AsyncOperationHandle<T>` | Handle+status wrapper returned by every Addressables async op |
| `AsyncOperationStatus` | `None` / `Succeeded` / `Failed` — always check before using `.Result` |
| Addressable Groups | Grouping that maps to output bundles and remote/local packing |
| Addressable Labels | Multi-key tagging for loading sets of assets together (e.g. `LoadAssetsAsync` by label) |
| Remote catalog | Enables post-release content updates without a resubmission |
| `AssetBundle.LoadFromFile(path)` / `LoadFromFileAsync` | Synchronous/async local bundle load (legacy system) |
| `AssetBundle.LoadFromMemory` / `LoadFromStream` (+ Async) | Load a bundle from an in-memory byte array or `Stream` |
| `AssetBundle.LoadAsset<T>` / `LoadAssetAsync<T>` | Load one asset from an already-open bundle |
| `AssetBundle.LoadAllAssets` / `LoadAllAssetsAsync` | Load every asset in a bundle at once |
| `AssetBundle.Unload(bool unloadAllLoadedObjects)` | Frees the bundle; `true` also destroys instantiated/loaded object data |
| `AssetBundle.UnloadAsync` | Frame-spread unload to avoid a hitch |
| `AssetBundle.memoryBudgetKB` | Caps the bundle's internal loading-cache memory |
| `AssetBundleManifest.GetAllDependencies` / `GetDirectDependencies` | Query a bundle's dependency graph before loading |
| `AssetBundleManifest.GetAssetBundleHash` | Per-bundle content hash for cache/version comparisons |
| `BuildPipeline.BuildAssetBundles` | Entry point for building legacy AssetBundles |
| `BuildAssetBundleOptions` | Build flags: `ChunkBasedCompression` (LZ4), `UncompressedAssetBundle`, `ForceRebuildAssetBundle`, `IgnoreTypeTreeChanges`, `DisableWriteTypeTree`, `StrictMode` |
| `UnityWebRequestAssetBundle.GetAssetBundle(url[, hash][, crc])` | Remote/cache-aware bundle download |
| `DownloadHandlerAssetBundle.GetContent(request)` | Extract the loaded `AssetBundle` from a completed web request |
| `Caching` class | `ClearCache`, `IsVersionCached`, `MarkAsUsed`, `AddCache` — disk cache management |
| `Caching.compressionEnabled` | Whether cached bundles are stored LZ4-compressed (default) or raw |
| Compression: LZMA / LZ4 (`ChunkBasedCompression`) / Uncompressed | Smallest+slowest-first-read / chunked-on-demand / largest+fastest-open |
| `Resources.Load<T>(path)` / `LoadAsync<T>` | Sync/async load from any `Resources/` folder; no fine-grained partial unload |
| `Resources.UnloadAsset(asset)` | Frees one Resources-loaded asset object |
| `Resources.UnloadUnusedAssets()` | Expensive async sweep of all unreferenced assets — don't call per-frame |
| `AssetImporter.SetAssetBundleNameAndVariant` | Scripted assignment of an asset to a bundle/variant |
| Unity Cloud Content Delivery (CCD) | Managed hosting service for remote bundle/catalog delivery, integrates with Addressables |

## Advanced Notes

**Memory management strategy: Addressables vs. Resources vs. AssetBundles.** All three systems ultimately manage the same tradeoff — build size vs. load latency vs. runtime memory vs. update flexibility — but they sit at different layers and are not interchangeable defaults.

- **`Resources/`** is the least flexible: everything under it is force-included in every build and indexed at build time whether or not it's ever loaded, and there is no partial/fine-grained unload — only whole-asset (`UnloadAsset`) or whole-project sweep (`UnloadUnusedAssets`). Its only real advantage is zero setup and guaranteed availability (no async download step, no missing-bundle failure mode). Reserve it for a small, fixed set of assets that must always be present regardless of build configuration — e.g. a fallback error material, a bootstrap config asset, or content referenced from code paths that run before any content-management system has initialized. Anything whose presence is conditional (per-platform, per-DLC, per-locale, "optional cosmetic content") does not belong here, because Resources content ships in **every** build variant unconditionally.

- **Legacy AssetBundles** are the correct choice when you need low-level control that Addressables' abstraction would get in the way of: custom patching/delta-update logic, non-standard hosting infrastructure, tight console platform DLC integration, or a codebase old enough (pre-Addressables, or migrating off it) that the investment in the newer package isn't justified. They demand you hand-manage the entire lifecycle yourself — bundle naming, dependency load order via `AssetBundleManifest`, cache versioning, and unload discipline (`Unload(true)` vs `Unload(false)`) — which is real ongoing engineering cost but buys full transparency: you can see and control exactly what's resident in memory at any moment, which matters on memory-constrained platforms (mobile, some consoles) where the automatic ref-counting of Addressables can obscure exactly why something is still resident.

- **Addressables** is correct for the common case: any project shipping optional, remote-updatable, or large/varied content (levels, character packs, seasonal cosmetics, localized voice-over) benefits from its automatic dependency resolution, ref-counted lifecycle, and unified local/remote key-based API. The cost is an added abstraction layer — when something leaks or won't unload, you're debugging the ref-count semantics of `AsyncOperationHandle` rather than a bundle you loaded yourself, and the local documentation gap noted above means you'll lean on pretrained knowledge or the public package docs more than for AssetBundles.

**Decision framework**: ask (1) *is this asset always needed regardless of build/platform/DLC configuration, with negligible size?* → Resources. (2) *Do I need custom patching, non-standard hosting, or console-store-managed DLC that Addressables' remote-catalog model doesn't fit?* → legacy AssetBundles, built directly with `BuildPipeline.BuildAssetBundles`. (3) *Everything else — optional content, remote-updatable content, anything organized around "load this by name/label when needed and free it later"* → Addressables. In practice, most modern Unity 6 projects should default to Addressables for all optional/streamed content, use `Resources/` only for the small always-resident bootstrap set, and reach for hand-rolled AssetBundles only when a specific platform or pipeline constraint rules Addressables out — since Addressables itself is *built on* AssetBundles under the hood, understanding the AssetBundle-layer guidance above (dependency load order, `Unload(true)` semantics, compression choice, caching) directly explains what Addressables is doing for you automatically, and is essential for diagnosing Addressables memory issues even though the two are used through very different APIs.

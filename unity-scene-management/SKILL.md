---
name: unity-scene-management
description: Use when loading, unloading, or organizing Unity scenes — SceneManager, additive/async scene loading, or multi-scene workflows. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Scene Management

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| SceneManager class overview | `ScriptReference/SceneManagement.SceneManager.html` | Full static API surface: load/unload, active-scene control, scene enumeration, events |
| SceneManager — sync load | `ScriptReference/SceneManagement.SceneManager.LoadScene.html` | Blocking `LoadScene(string)`, `LoadScene(int)`, `LoadScene(string, LoadSceneParameters)` overloads |
| SceneManager — async load | `ScriptReference/SceneManagement.SceneManager.LoadSceneAsync.html` | Non-blocking load overloads returning `AsyncOperation`; name/index/`LoadSceneParameters` variants |
| SceneManager — sync/async unload | `ScriptReference/SceneManagement.SceneManager.UnloadScene.html`, `ScriptReference/SceneManagement.SceneManager.UnloadSceneAsync.html` | Removing a loaded scene's GameObjects; `UnloadSceneOptions` |
| LoadSceneMode enum | `ScriptReference/SceneManagement.LoadSceneMode.html` | `Single` (replace all) vs `Additive` (layer on top) |
| LoadSceneParameters struct | `ScriptReference/SceneManagement.LoadSceneParameters.html` | Bundles `loadSceneMode` + `localPhysicsMode` for overloads that take one struct instead of separate args |
| Scene struct | `ScriptReference/SceneManagement.Scene.html` | The value-type handle returned by every load/get call — `name`, `path`, `buildIndex`, `isLoaded`, `isDirty`, `rootCount` |
| Scene — isLoaded / buildIndex | `ScriptReference/SceneManagement.Scene-isLoaded.html`, `ScriptReference/SceneManagement.Scene-buildIndex.html` | Checking load state and build-index without a full scene fetch |
| Scene — GetRootGameObjects / IsValid | `ScriptReference/SceneManagement.Scene.GetRootGameObjects.html`, `ScriptReference/SceneManagement.Scene.IsValid.html` | Enumerating a scene's top-level objects; guarding against a stale/default `Scene` handle |
| AsyncOperation | `ScriptReference/AsyncOperation.html` | Base type returned by `LoadSceneAsync`/`UnloadSceneAsync`; `isDone`, `progress`, `priority`, `completed` |
| AsyncOperation — allowSceneActivation | `ScriptReference/AsyncOperation-allowSceneActivation.html` | Gating the moment a background-loaded scene actually activates (loading-screen pattern) |
| AsyncOperation — progress / isDone / completed | `ScriptReference/AsyncOperation-progress.html`, `ScriptReference/AsyncOperation-isDone.html`, `ScriptReference/AsyncOperation-completed.html` | Polling vs event-driven completion; the 0.9-cap behavior while activation is held |
| Awaitable.FromAsyncOperation | `ScriptReference/Awaitable.FromAsyncOperation.html` | Wrapping an `AsyncOperation` for `await`-style consumption in an `async Awaitable` method (Unity 6 `Awaitable` API) |
| Scene events | `ScriptReference/SceneManagement.SceneManager-sceneLoaded.html`, `ScriptReference/SceneManagement.SceneManager-sceneUnloaded.html`, `ScriptReference/SceneManagement.SceneManager-activeSceneChanged.html` | Subscribing to load/unload/active-scene-change notifications instead of polling |
| SceneManager scene enumeration | `ScriptReference/SceneManagement.SceneManager.GetAllScenes.html`, `ScriptReference/SceneManagement.SceneManager.GetSceneAt.html`, `ScriptReference/SceneManagement.SceneManager-sceneCount.html`, `ScriptReference/SceneManagement.SceneManager-loadedSceneCount.html`, `ScriptReference/SceneManagement.SceneManager-sceneCountInBuildSettings.html` | Iterating every currently loaded scene; distinguishing "loaded" vs "loading" counts vs total scenes registered in the build |
| SceneManager scene lookup | `ScriptReference/SceneManagement.SceneManager.GetSceneByName.html`, `ScriptReference/SceneManagement.SceneManager.GetSceneByPath.html`, `ScriptReference/SceneManagement.SceneManager.GetSceneByBuildIndex.html` | Fetching a `Scene` handle for a scene that's already loaded, by name/path/index |
| SceneManager active-scene control | `ScriptReference/SceneManagement.SceneManager.GetActiveScene.html`, `ScriptReference/SceneManagement.SceneManager.SetActiveScene.html` | Reading/setting which loaded scene new instantiated objects, lighting settings, and `Physics.gravity`-style globals attach to |
| SceneManager scene creation/merging | `ScriptReference/SceneManagement.SceneManager.CreateScene.html`, `ScriptReference/SceneManagement.SceneManager.MergeScenes.html`, `ScriptReference/SceneManagement.SceneManager.MoveGameObjectToScene.html` | Creating an empty runtime scene, merging one loaded scene's contents into another, reparenting a GameObject into a different loaded scene |
| SceneUtility | `ScriptReference/SceneManagement.SceneUtility.html`, `ScriptReference/SceneManagement.SceneUtility.GetScenePathByBuildIndex.html`, `ScriptReference/SceneManagement.SceneUtility.GetBuildIndexByScenePath.html` | Converting between a scene's build index and its asset path without loading it |
| Resources.UnloadUnusedAssets | `ScriptReference/Resources.UnloadUnusedAssets.html` | Freeing assets left orphaned after `UnloadSceneAsync` removes a scene's GameObjects |
| Object.DontDestroyOnLoad | `ScriptReference/Object.DontDestroyOnLoad.html` | Marking a GameObject (and its whole hierarchy) to survive a `Single`-mode load |
| Multi-scene editing (Manual) | `Manual/MultiSceneEditing.html`, `Manual/setupmultiplescenes.html`, `Manual/scriptmultiplescenes.html` | Conceptual overview, editor workflow, and scripting patterns for having several scenes open/loaded at once |
| Working with scenes (Manual) | `Manual/working-with-scenes.html`, `Manual/CreatingScenes.html`, `Manual/scene-reloading.html` | Creating scenes, general scene concepts, and reload-current-scene patterns |
| Physics in multi-scene setups | `Manual/physics-multi-scene.html` | `LocalPhysicsMode` — isolating a scene's physics simulation from other loaded scenes |
| Lightmapping across multiple scenes | `Manual/bakemultiplescenes.html`, `Manual/light-probes-and-scene-loading.html` | Baking lighting that spans several open scenes; light-probe behavior when scenes stream in/out at runtime |
| Occlusion culling and scene streaming | `Manual/occlusion-culling-scene-loading.html` | How occlusion data behaves when scenes are additively loaded/unloaded around the camera |
| Build Settings scene list | `Manual/BuildSettings.html`, `Manual/build-profile-scene-list.html` | Registering scenes (and their order/build index) so `LoadScene`/`LoadSceneAsync` can resolve them by name or index in a built player; Unity 6's per-platform Build Profile scene list |
| Texture streaming (large-world memory) | `Manual/TextureStreaming.html`, `Manual/TextureStreaming-introduction.html` | Mipmap streaming that complements scene streaming for open-world memory budgets |

28 rows, all verified against `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/{Manual,ScriptReference}` before writing.

## Key Guidelines

### Sync vs Async Loading

`SceneManager.LoadScene` (`ScriptReference/SceneManagement.SceneManager.LoadScene.html`) loads a scene on the main thread and blocks until it's fully loaded — the calling frame stalls for the entire duration, which shows up as a hitch or freeze for anything beyond a tiny scene. `SceneManager.LoadSceneAsync` (`ScriptReference/SceneManagement.SceneManager.LoadSceneAsync.html`) starts the same work but returns immediately with an `AsyncOperation` handle, spreading the load across multiple frames so the game keeps rendering and responding. Both come in three overload shapes: by scene name (`string`), by build index (`int`), and by either of those plus a `LoadSceneMode` or a full `LoadSceneParameters` struct. Prefer `LoadSceneAsync` for anything triggered during gameplay (level transitions, streaming in a new zone); reserve `LoadScene` for cases where a hard stall is acceptable or even desirable, such as an initial splash-to-menu transition where nothing else is happening on screen anyway. In Unity 6, `Awaitable.FromAsyncOperation` (`ScriptReference/Awaitable.FromAsyncOperation.html`) lets an `async Awaitable` method `await` a scene load directly instead of polling `isDone` in `Update`:

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneLoader : MonoBehaviour
{
    public async Awaitable LoadLevelAsync(string sceneName)
    {
        AsyncOperation op = SceneManager.LoadSceneAsync(sceneName, LoadSceneMode.Single);
        await op; // Awaitable.FromAsyncOperation under the hood
        Debug.Log($"{sceneName} finished loading and is now active.");
    }
}
```

### Single vs Additive Load Mode

`LoadSceneMode` (`ScriptReference/SceneManagement.LoadSceneMode.html`) has two values. `Single` (the default when no mode is specified) unloads every currently loaded scene before loading the new one — this is the classic "go to the next level" behavior and is what happens if you call `LoadScene(name)` with no mode argument. `Additive` loads the new scene on top of whatever is already loaded, without touching existing scenes. Use `Additive` for: streaming an open world in chunks around the player, layering a persistent UI/HUD or audio-manager scene over swappable gameplay scenes, loading a separate lighting/environment scene alongside a separate gameplay-logic scene, or cooperative split-responsibility scenes edited by different team members (see `Manual/MultiSceneEditing.html`). `LoadSceneParameters` (`ScriptReference/SceneManagement.LoadSceneParameters.html`) bundles `loadSceneMode` together with `localPhysicsMode` (see `Manual/physics-multi-scene.html`) for the overloads that accept a single struct instead of positional arguments — set `localPhysicsMode` to `Physics2D`/`Physics3D` when a scene needs its own isolated physics simulation independent of other additively-loaded scenes.

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

// Boot flow: load a persistent UI/manager scene, then the first level additively on top.
public class BootLoader : MonoBehaviour
{
    void Start()
    {
        SceneManager.LoadScene("PersistentUI", LoadSceneMode.Single); // clears everything else
        SceneManager.LoadSceneAsync("Level01", LoadSceneMode.Additive); // layers gameplay on top
    }
}
```

### Multi-Scene Workflows

Multiple scenes can be open (in-Editor) or loaded (at runtime) simultaneously — see `Manual/MultiSceneEditing.html` for the concept, `Manual/setupmultiplescenes.html` for the Editor workflow, and `Manual/scriptmultiplescenes.html` for the scripting equivalent. `SceneManager.GetAllScenes()` and `SceneManager.GetSceneAt(int)` enumerate every currently loaded scene; `sceneCount` (`ScriptReference/SceneManagement.SceneManager-sceneCount.html`) reports how many are loaded right now, `loadedSceneCount` reports how many have finished loading (relevant while an additive load is still in flight), and `sceneCountInBuildSettings` reports the total registered in Build Settings regardless of load state. Exactly one loaded scene is the "active" scene at any time — `SceneManager.GetActiveScene()`/`SetActiveScene(Scene)` control it, and it determines where newly instantiated GameObjects without an explicit target scene land, which scene's Lighting settings apply, and which scene's static batching/navmesh data is treated as primary. `SceneManager.MoveGameObjectToScene` reparents an object into a different loaded scene without destroying/recreating it, and `SceneManager.CreateScene`/`MergeScenes` create an empty runtime-only scene and fold one scene's contents into another, respectively — useful for organizing dynamically spawned objects (e.g. pooled bullets) into their own scene so they can be bulk-unloaded later.

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class ZoneManager : MonoBehaviour
{
    // After loading a new zone additively, make it the active scene so
    // newly spawned enemies/pickups and its Lighting settings take effect.
    public void OnZoneLoaded(Scene loadedZone)
    {
        SceneManager.SetActiveScene(loadedZone);

        foreach (Scene scene in GetAllLoadedZones())
        {
            Debug.Log($"{scene.name}: loaded={scene.isLoaded}, buildIndex={scene.buildIndex}");
        }
    }

    System.Collections.Generic.IEnumerable<Scene> GetAllLoadedZones()
    {
        for (int i = 0; i < SceneManager.sceneCount; i++)
            yield return SceneManager.GetSceneAt(i);
    }
}
```

### Persistence Across Scenes (DontDestroyOnLoad)

`Object.DontDestroyOnLoad` (`ScriptReference/Object.DontDestroyOnLoad.html`) marks a GameObject's entire hierarchy so it survives a `Single`-mode scene load, which would otherwise destroy every object in the outgoing scene. This is required for anything that must persist across level transitions: an audio manager, a game-state/save-data singleton, a network connection manager, or a persistent input-handling object. It has no effect on `Additive` loads or unloads — those never destroy objects outside the scene being unloaded, so `DontDestroyOnLoad` is specifically insurance against a future `Single` load. Objects marked this way are moved into a special hidden scene (visible in the Hierarchy as `DontDestroyOnLoad` in the Editor) that never gets unloaded; guard against calling `DontDestroyOnLoad` on the same singleton twice (e.g. if its home scene is re-loaded additively) by checking for an existing instance first.

```csharp
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }

    void Awake()
    {
        if (Instance != null && Instance != this)
        {
            Destroy(gameObject); // a duplicate was created by re-loading this scene
            return;
        }
        Instance = this;
        DontDestroyOnLoad(gameObject);
    }
}
```

### Scene Events

`SceneManager.sceneLoaded` and `SceneManager.sceneUnloaded` (`ScriptReference/SceneManagement.SceneManager-sceneLoaded.html`, `ScriptReference/SceneManagement.SceneManager-sceneUnloaded.html`) fire after a load/unload completes, with the affected `Scene` (and, for `sceneLoaded`, the `LoadSceneMode` used) passed to the handler — this is the correct place to run logic that depends on the new scene's objects existing (spawning the player, wiring up references, starting AI), rather than assuming ordering relative to `Start()`/`Awake()` on scripts in the loading scene. `SceneManager.activeSceneChanged` (`ScriptReference/SceneManagement.SceneManager-activeSceneChanged.html`) fires when `SetActiveScene` changes which scene is active, passing the previous and new `Scene`. Always unsubscribe in `OnDestroy`/`OnDisable` to avoid a dangling delegate reference keeping a destroyed listener's closure alive or throwing on a stale scene reference.

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneEventLogger : MonoBehaviour
{
    void OnEnable()
    {
        SceneManager.sceneLoaded += HandleSceneLoaded;
        SceneManager.sceneUnloaded += HandleSceneUnloaded;
        SceneManager.activeSceneChanged += HandleActiveSceneChanged;
    }

    void OnDisable()
    {
        SceneManager.sceneLoaded -= HandleSceneLoaded;
        SceneManager.sceneUnloaded -= HandleSceneUnloaded;
        SceneManager.activeSceneChanged -= HandleActiveSceneChanged;
    }

    void HandleSceneLoaded(Scene scene, LoadSceneMode mode) =>
        Debug.Log($"Loaded '{scene.name}' with mode {mode}");

    void HandleSceneUnloaded(Scene scene) =>
        Debug.Log($"Unloaded '{scene.name}'");

    void HandleActiveSceneChanged(Scene previous, Scene next) =>
        Debug.Log($"Active scene: {previous.name} -> {next.name}");
}
```

### Memory Reclamation on Unload

`SceneManager.UnloadSceneAsync` (`ScriptReference/SceneManagement.SceneManager.UnloadSceneAsync.html`) destroys a scene's GameObjects and removes it from the loaded-scenes list, but it does **not** free the underlying assets (textures, meshes, materials, audio clips) those objects referenced — Unity keeps them cached in memory in case another loaded scene (or a fresh load of the same scene) needs them again. Call `Resources.UnloadUnusedAssets()` (`ScriptReference/Resources.UnloadUnusedAssets.html`) afterward to actually scan for and free assets with no remaining references; it also returns an `AsyncOperation` since the scan can be expensive on large projects. `Resources.UnloadUnusedAssets` only unloads assets that were loaded as part of a scene or via `Resources.Load`/`AssetBundle` — it does not garbage-collect managed C# objects, so pair it with `System.GC.Collect()` when profiling shows managed memory is also the culprit (use sparingly; forcing a full GC has its own frame-time cost). Do not call `UnloadUnusedAssets` every frame or on a timer — it's relatively expensive and should be triggered by actual scene-transition events.

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public IEnumerator UnloadZoneAndReclaim(string sceneName)
{
    AsyncOperation unload = SceneManager.UnloadSceneAsync(sceneName);
    yield return unload; // waits for GameObjects to be destroyed

    AsyncOperation cleanup = Resources.UnloadUnusedAssets();
    yield return cleanup; // waits for orphaned assets to be freed

    Debug.Log($"{sceneName} fully unloaded and its assets reclaimed.");
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Scene missing from Build Settings | Works in-Editor via direct scene reference but throws `LoadScene`/`LoadSceneAsync` errors in a build; add it in Build Settings (`Manual/BuildSettings.html`) or the platform's Build Profile scene list (`Manual/build-profile-scene-list.html`), or load it via Addressables/AssetBundle instead |
| Persistent manager destroyed on scene load | Forgot `DontDestroyOnLoad`; a `Single`-mode load wipes it along with everything else in the outgoing scene |
| Duplicate persistent singleton after re-entering its scene | `DontDestroyOnLoad` object's home scene gets loaded additively again, creating a second instance; guard `Awake()` with an existing-instance check and `Destroy(gameObject)` the duplicate |
| Using loaded-scene objects before load finishes | Code runs immediately after calling `LoadSceneAsync` instead of awaiting `isDone`/`sceneLoaded`/`await op`; the scene's objects don't exist yet on that frame |
| Memory not freed after unloading | `UnloadSceneAsync` only removes GameObjects, not assets; call `Resources.UnloadUnusedAssets()` afterward, and expect a managed-memory delta that only `GC.Collect()` addresses |
| Additive scene never becomes active | Multiple scenes loaded additively but `SetActiveScene` never called; new instantiated objects and Lighting settings land in whichever scene happened to be active before, not the new one |
| Loading screen stuck at 90% forever | `allowSceneActivation` left `false` with no code path ever setting it `true`; the operation intentionally holds `progress` at 0.9 and `isDone` at `false` until activation is allowed |
| Assuming `progress` reaches 1.0 before activation | `AsyncOperation.progress` caps at 0.9 while `allowSceneActivation == false`; code that waits for `progress >= 1f` without ever flipping activation spins forever — wait for `isDone` after setting `allowSceneActivation = true`, or check `progress >= 0.9f` as the "ready to activate" signal |
| Calling `LoadScene` (sync) mid-gameplay | Blocks the main thread for the full load duration, producing a visible freeze; use `LoadSceneAsync` for any load triggered during active play |
| Physics interactions bleeding between additive scenes | Two additively loaded scenes share the same global physics simulation by default; set `LoadSceneParameters.localPhysicsMode` (see `Manual/physics-multi-scene.html`) when scenes need isolated physics worlds |
| Referencing objects across scenes by scene index after reordering | Build index changes whenever Build Settings' scene list is reordered; prefer `GetSceneByName`/loading by name over hardcoded `buildIndex` values that drift silently |
| Not unsubscribing from `sceneLoaded`/`sceneUnloaded`/`activeSceneChanged` | Listener persists on a destroyed object or fires with a stale closure; unsubscribe in `OnDisable`/`OnDestroy` |
| Assuming `UnloadSceneAsync` also clears `DontDestroyOnLoad` objects | Objects moved to the `DontDestroyOnLoad` pseudo-scene are unaffected by unloading any regular scene — they must be destroyed explicitly if they should not persist |
| Baking lightmaps in isolation per scene, then loading additively | Cross-scene lightmap and light-probe data doesn't merge automatically at runtime the way it does when baked together; see `Manual/bakemultiplescenes.html` and `Manual/light-probes-and-scene-loading.html` for the multi-scene bake workflow before shipping a chunked open world |
| Treating occlusion culling data as scene-agnostic during streaming | Occlusion data is baked per-scene setup; streaming scenes in/out at runtime can produce culling artifacts if the baked configuration didn't account for it — see `Manual/occlusion-culling-scene-loading.html` |

## Quick Reference

| Item | Purpose |
|------|---------|
| `SceneManager.LoadScene(name/index)` | Synchronous, blocking scene load |
| `SceneManager.LoadSceneAsync(name/index, mode)` | Non-blocking load, returns `AsyncOperation` |
| `SceneManager.UnloadScene(name/index)` | Synchronous unload of a loaded scene |
| `SceneManager.UnloadSceneAsync(name/index)` | Non-blocking unload, returns `AsyncOperation` |
| `LoadSceneMode.Single` / `Additive` | Replace all loaded scenes vs. layer on top of them |
| `LoadSceneParameters` | Struct bundling `loadSceneMode` + `localPhysicsMode` for one-arg load overloads |
| `AsyncOperation.allowSceneActivation` | Gate the moment a loaded scene actually activates |
| `AsyncOperation.isDone` / `.progress` / `.priority` / `.completed` | Poll completion, track load progress (caps at 0.9 pre-activation), set scheduling priority, or subscribe a completion callback |
| `Awaitable.FromAsyncOperation` | Wrap an `AsyncOperation` so an `async Awaitable` method can `await` it |
| `SceneManager.sceneLoaded` / `sceneUnloaded` | Events fired on load/unload completion |
| `SceneManager.activeSceneChanged` | Event fired when the active scene changes |
| `SceneManager.GetActiveScene()` / `SetActiveScene(scene)` | Read/set which loaded scene new objects and Lighting settings attach to |
| `SceneManager.GetAllScenes()` / `GetSceneAt(i)` | Enumerate every currently loaded scene |
| `SceneManager.sceneCount` / `loadedSceneCount` / `sceneCountInBuildSettings` | Loaded-now count vs. finished-loading count vs. total registered in the build |
| `SceneManager.GetSceneByName/Path/BuildIndex` | Look up a `Scene` handle for an already-loaded scene |
| `SceneManager.CreateScene(name)` | Create an empty runtime-only scene |
| `SceneManager.MergeScenes(src, dst)` | Fold one loaded scene's contents into another, then unload the source |
| `SceneManager.MoveGameObjectToScene(go, scene)` | Reparent a GameObject into a different loaded scene |
| `SceneUtility.GetScenePathByBuildIndex` / `GetBuildIndexByScenePath` | Convert between a scene's build index and asset path without loading it |
| `Scene.isLoaded` / `.buildIndex` / `.name` / `.path` | Query state on a `Scene` value-type handle |
| `Scene.GetRootGameObjects()` | Enumerate a scene's top-level GameObjects |
| `Scene.IsValid()` | Guard against a default/stale `Scene` struct |
| `Resources.UnloadUnusedAssets()` | Reclaim asset memory orphaned after unloading |
| `Object.DontDestroyOnLoad(go)` | Keep an object's hierarchy alive across `Single`-mode loads |
| Build Settings / Build Profile scene list | Required registration for runtime `LoadScene` by name/index in a build |

## Advanced Notes

**Scene streaming for large open worlds.** A common architecture for open-world or seamless-level games splits the world into a grid or graph of small additive scenes (streaming cells) plus one persistent scene holding the player, camera, and manager singletons (loaded `Single` once at boot, never touched again). At runtime, a streaming controller tracks the player's position, computes which cells should be loaded (typically the current cell plus a ring of neighbors), and diff's that against what's currently loaded: cells that just entered range are `LoadSceneAsync`'d additively, cells that just left range are `UnloadSceneAsync`'d and followed by a debounced `Resources.UnloadUnusedAssets()` call (batched, not called per-cell, since it's relatively expensive). Keep multiple loads in flight concurrently rather than sequentially awaiting each one — `AsyncOperation.priority` can hint the scheduler which of several concurrent loads should be favored when there's contention. Because occlusion-culling and lightmap data are baked per-scene-setup (`Manual/occlusion-culling-scene-loading.html`, `Manual/bakemultiplescenes.html`), open-world projects generally need to bake with the actual multi-scene streaming layout active rather than each cell scene in isolation, or bake lighting into a lightweight lower-fidelity fallback and rely on realtime GI/light probes for boundary seams. Pair scene streaming with texture mip-streaming (`Manual/TextureStreaming.html`) so large per-cell texture sets don't spike memory the instant a cell's scene finishes loading — texture streaming trims mip levels independently of scene load state, based on camera distance and screen coverage.

**Loading-screen patterns.** The standard approach: `LoadSceneAsync` the target scene with `allowSceneActivation = false` immediately after starting the operation, then drive a progress bar off `AsyncOperation.progress` (remember it caps at 0.9 while activation is withheld — treat `progress >= 0.9f` as "ready", not `progress >= 1f`). Once the loading-screen fade-out/transition animation is ready to play, set `allowSceneActivation = true` and wait for `isDone` (or the `completed` callback) before tearing down the loading UI. For a two-scene loading-screen setup (a dedicated `LoadingScreen` scene loaded `Single` first, which then kicks off the real target scene additively and unloads itself once the target is active), sequence it as: unload old gameplay scene(s) → load `LoadingScreen` (`Single`) → `LoadSceneAsync(target, Additive)` with activation withheld → drive progress bar → activate → `SetActiveScene(target)` → `UnloadSceneAsync("LoadingScreen")`. For loads composed of multiple parallel operations (e.g. streaming in several cells at once, or a scene plus an Addressables bundle), aggregate progress by averaging each operation's `progress` rather than showing only the slowest one, and gate the "ready" state on every operation's `isDone` being true.

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class LoadingScreenController : MonoBehaviour
{
    public UnityEngine.UI.Slider progressBar;

    public IEnumerator TransitionTo(string targetScene)
    {
        AsyncOperation op = SceneManager.LoadSceneAsync(targetScene, LoadSceneMode.Additive);
        op.allowSceneActivation = false;

        while (op.progress < 0.9f)
        {
            progressBar.value = op.progress / 0.9f; // normalize 0..0.9 -> 0..1
            yield return null;
        }
        progressBar.value = 1f;

        // Play a fade-out, wait for input, etc., then release activation:
        yield return new WaitForSeconds(0.5f);
        op.allowSceneActivation = true;

        yield return op; // now completes for real

        Scene loaded = SceneManager.GetSceneByName(targetScene);
        SceneManager.SetActiveScene(loaded);
        yield return SceneManager.UnloadSceneAsync("LoadingScreen");
    }
}
```

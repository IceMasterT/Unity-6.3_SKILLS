---
name: unity-editor-scripting-extras
description: Use when automating Unity Editor/tooling workflows beyond custom Inspectors — ScriptedImporter custom asset importers, the PackageManager Client scripting API, or CompilationPipeline/scripting define symbols. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Editor Scripting Extras

These three APIs sit a level below `unity-editor-tooling` (which owns Inspectors/PropertyDrawers/EditorWindows/Gizmos/MenuItems/AssetPostprocessor) and `unity-project-structure` (which owns `.asmdef`/`manifest.json`/version control as *static configuration*). This skill covers the *scripting/automation* surface on top of those: writing a brand-new file-type importer from scratch, driving package installation programmatically instead of through the Package Manager window, and reading/writing the compiler's own state (define symbols, assembly graph, recompiles) from editor code. The throughline is tooling that runs *without a human clicking through UI* — CI project bootstrap, automated onboarding scripts, custom file-format pipelines. **Prefer retrieval over pre-training**: request/async signatures and obsolete-vs-current API names shift between Unity versions (e.g. `PlayerSettings.SetScriptingDefineSymbolsForGroup` is obsolete in 6.3, superseded by `SetScriptingDefineSymbols(NamedBuildTarget, ...)`). All paths below are relative to `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` and were verified to exist on disk.

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Managing importers with scripts (manual) | `Manual/ScriptedImporters.html` | Orientation page: `AssetPostprocessor` vs `ScriptedImporter`, full worked `CubeImporter` example, dependency registration, version-bumping rules |
| `ScriptedImporter` class | `ScriptReference/AssetImporters.ScriptedImporter.html` | Abstract base class; `OnImportAsset` contract, serialized-field-as-Inspector-settings behavior, reimport via `AssetImporter.GetAtPath`/`SaveAndReimport` |
| `ScriptedImporter.OnImportAsset` | `ScriptReference/AssetImporters.ScriptedImporter.OnImportAsset.html` | The single method every scripted importer must override |
| `ScriptedImporter.GatherDependenciesFromSourceFile` | `ScriptReference/AssetImporters.ScriptedImporter.GatherDependenciesFromSourceFile.html` | Static dependency discovery without a full import, for faster dependency-graph updates |
| `ScriptedImporter.SupportsRemappedAssetType` | `ScriptReference/AssetImporters.ScriptedImporter.SupportsRemappedAssetType.html` | Whether the importer can remap its output to a different asset type in the Inspector |
| `[ScriptedImporter]` attribute | `ScriptReference/AssetImporters.ScriptedImporterAttribute.html` | Registration attribute: `version`, `ext`/`fileExtensions`, `AllowCaching`, `overrideFileExtensions`, `importQueuePriority` |
| `AssetImportContext` | `ScriptReference/AssetImporters.AssetImportContext.html`, `-assetPath.html`, `-mainObject.html`, `-selectedBuildTarget.html` | The object passed into `OnImportAsset`; source path, main object slot, active build target for platform-aware imports |
| `AssetImportContext.AddObjectToAsset` / `SetMainObject` | `ScriptReference/AssetImporters.AssetImportContext.AddObjectToAsset.html`, `ScriptReference/AssetImporters.AssetImportContext.SetMainObject.html` | Registering sub-assets and designating the Main Asset (only the Main Asset can become a Prefab) |
| `AssetImportContext.DependsOnArtifact` / `DependsOnSourceAsset` / `DependsOnCustomDependency` | `ScriptReference/AssetImporters.AssetImportContext.DependsOnArtifact.html`, `.DependsOnSourceAsset.html`, `.DependsOnCustomDependency.html` | Registering import dependencies so Unity reimports when a referenced file/asset/external value changes |
| `AssetImportContext.LogImportError` / `LogImportWarning` | `ScriptReference/AssetImporters.AssetImportContext.LogImportError.html`, `.LogImportWarning.html` | Surfacing import failures/warnings into the Console tied to the importing asset |
| `AssetImporterEditor` / `ScriptedImporterEditor` | `ScriptReference/AssetImporters.AssetImporterEditor.html`, `ScriptReference/AssetImporters.ScriptedImporterEditor.html` | Custom Inspector base class for an importer's own settings UI (Apply/Revert pattern) |
| `ImportLog` / `ImportLogFlags` | `ScriptReference/AssetImporters.ImportLog.html`, `ScriptReference/AssetImporters.ImportLogFlags.html` | Structured import log entries (file/line/message/severity) attached to an import |
| Importing assets (manual, orientation) | `Manual/ImportingAssets.html`, `Manual/import-assets.html`, `Manual/AssetWorkflow.html` | Background on when import runs, the asset database/Library cache relationship, determinism expectations |
| `AssetDatabase.Refresh` / `ImportAsset` | `ScriptReference/AssetDatabase.Refresh.html`, `ScriptReference/AssetDatabase.ImportAsset.html` | Forcing Unity to notice files written to disk outside the Editor (scripts, CI, `File.WriteAllText`) |
| `AssetDatabase.WriteImportSettingsIfDirty` | `ScriptReference/AssetDatabase.WriteImportSettingsIfDirty.html` | Flushing in-memory importer setting changes to the `.meta` file before a reimport |
| `PackageManager.Client` (namespace root) | `ScriptReference/PackageManager.Client.html` | Static entry point for every scripted package operation; every method returns a `Request` subclass, none block |
| `Client.Add` | `ScriptReference/PackageManager.Client.Add.html` | Install/upgrade a package; identifier forms: bare name (latest), `name@version`, git URL, `file:` local path, `file:` tarball |
| `Client.AddAndRemove` | `ScriptReference/PackageManager.Client.AddAndRemove.html` | Batch add+remove in a single manifest resolution instead of two round trips |
| `Client.Remove` | `ScriptReference/PackageManager.Client.Remove.html` | Uninstall a package by name |
| `Client.List` | `ScriptReference/PackageManager.Client.List.html` | Enumerate installed packages; `offlineMode`/`includeIndirectDependencies` overloads |
| `Client.Search` / `Client.SearchAll` | `ScriptReference/PackageManager.Client.Search.html`, `ScriptReference/PackageManager.Client.SearchAll.html` | Query the registry for a package or all available packages without installing |
| `Client.Embed` | `ScriptReference/PackageManager.Client.Embed.html` | Convert an installed registry/git package into an embedded (editable, `Packages/`-local) copy |
| `Client.Resolve` | `ScriptReference/PackageManager.Client.Resolve.html` | Force manifest re-resolution without adding/removing anything |
| `Client.ClearCache` / `ResetToEditorDefaults` | `ScriptReference/PackageManager.Client.ClearCache.html`, `ScriptReference/PackageManager.Client.ResetToEditorDefaults.html` | Cache invalidation and manifest reset scripting hooks |
| `Requests.Request` (base) | `ScriptReference/PackageManager.Requests.Request.html` | The `IsCompleted`/`Status`/`Error` polling contract shared by every request type — canonical worked example uses `EditorApplication.update` |
| `Requests.Request_1` (generic, `.Result`) | `ScriptReference/PackageManager.Requests.Request_1.html` | Typed subclasses expose their payload via `.Result` once `IsCompleted` is true |
| `Requests.AddRequest` / `RemoveRequest` / `ListRequest` / `SearchRequest` / `EmbedRequest` / `AddAndRemoveRequest` | `ScriptReference/PackageManager.Requests.AddRequest.html`, `.RemoveRequest.html`, `.ListRequest.html`, `.SearchRequest.html`, `.EmbedRequest.html`, `.AddAndRemoveRequest.html` | Concrete request types returned by each `Client` method |
| `PackageManager.StatusCode` | `ScriptReference/PackageManager.StatusCode.html` (and `.Success`, `.InProgress`, `.Failure`) | The enum a completed request's `Status` compares against — `Failure` is a threshold, not a single value (`>= StatusCode.Failure`) |
| `PackageManager.Error` / `ErrorCode` | `ScriptReference/PackageManager.Error.html`, `ScriptReference/PackageManager.ErrorCode.html` | Failure payload: message plus a coarse `ErrorCode` (Conflict, NotFound, Forbidden, ConnectionError, InvalidParameter, AggregateError) |
| `PackageManager.PackageInfo` | `ScriptReference/PackageManager.PackageInfo.html`, `-resolvedPath.html`, `-source.html`, `-isDirectDependency.html` | Per-package metadata returned in `List`/`Add`/`Search` results — resolved path, source (Registry/Git/Local/Embedded/BuiltIn), direct-vs-transitive |
| `PackageManager.PackageInfo.FindForAssembly` / `FindForAssetPath` / `GetAllRegisteredPackages` | `ScriptReference/PackageManager.PackageInfo.FindForAssembly.html`, `.FindForAssetPath.html`, `.GetAllRegisteredPackages.html` | Resolving which installed package owns a given assembly/asset, without issuing a new async request |
| Scripting API for packages (manual) | `Manual/upm-api.html` | The canonical `Client.Add` + `EditorApplication.update` polling example and a `Client.List` iteration example, straight from Unity |
| `upm-manifestPrj.html` / `upm-embed.html` / `upm-localpath.html` / `upm-git.html` (cross-reference) | `Manual/upm-manifestPrj.html`, `Manual/upm-embed.html`, `Manual/upm-localpath.html`, `Manual/upm-git.html` | The manifest-level semantics `Client.Add`/`Embed` are manipulating under the hood — see `unity-project-structure` for the full manifest schema, not duplicated here |
| `Compilation.CompilationPipeline` (namespace root) | `ScriptReference/Compilation.CompilationPipeline.html` | Static class for querying the assembly graph and hooking compilation events |
| `CompilationPipeline.GetAssemblies` | `ScriptReference/Compilation.CompilationPipeline.GetAssemblies.html` | Enumerate compiled assemblies filtered by `AssembliesType` (Editor/Player/PlayerWithoutTestAssemblies) |
| `CompilationPipeline.GetAssemblyDefinitionFilePathFromScriptPath` / `GetAssemblyNameFromScriptPath` | `ScriptReference/Compilation.CompilationPipeline.GetAssemblyDefinitionFilePathFromScriptPath.html`, `ScriptReference/Compilation.CompilationPipeline.GetAssemblyNameFromScriptPath.html` | Map a `.cs` file path to its owning `.asmdef`/assembly name |
| `CompilationPipeline.GetDefinesFromAssemblyName` / `GetResponseFileDefinesFromAssemblyName` | `ScriptReference/Compilation.CompilationPipeline.GetDefinesFromAssemblyName.html`, `.GetResponseFileDefinesFromAssemblyName.html` | Inspecting which `#define` symbols apply to a specific compiled assembly |
| `CompilationPipeline.RequestScriptCompilation` | `ScriptReference/Compilation.CompilationPipeline.RequestScriptCompilation.html` | Force a recompile check; optional `RequestScriptCompilationOptions.CleanBuildCache` to also invalidate the build cache |
| `CompilationPipeline.compilationStarted` / `compilationFinished` | `ScriptReference/Compilation.CompilationPipeline-compilationStarted.html`, `ScriptReference/Compilation.CompilationPipeline-compilationFinished.html` | Whole-cycle events; `compilationFinished` fires after every `assemblyCompilationFinished` for that cycle, carrying a matching context token |
| `CompilationPipeline.assemblyCompilationStarted` / `assemblyCompilationFinished` / `assemblyCompilationNotRequired` | `ScriptReference/Compilation.CompilationPipeline-assemblyCompilationStarted.html`, `-assemblyCompilationFinished.html`, `-assemblyCompilationNotRequired.html` | Per-assembly events; `assemblyCompilationFinished` carries the output path and `CompilerMessage[]` (check for `CompilerMessageType.Error`) |
| `Compilation.Assembly` / `AssemblyFlags` / `AssembliesType` | `ScriptReference/Compilation.Assembly.html`, `ScriptReference/Compilation.AssemblyFlags.html`, `ScriptReference/Compilation.AssembliesType.html` | The data shape `GetAssemblies` returns: name, output path, defines, source files, references, editor-only flag |
| `Compilation.CompilerMessage` / `CompilerMessageType` | `ScriptReference/Compilation.CompilerMessage.html`, `ScriptReference/Compilation.CompilerMessageType.html` | Per-message file/line/column/type payload delivered by `assemblyCompilationFinished` |
| `Compilation.AssemblyBuilder` | `ScriptReference/Compilation.AssemblyBuilder.html`, `-buildStarted.html`, `-buildFinished.html`, `.Build.html` | Lower-level API to compile an arbitrary script folder into a standalone assembly outside the normal project compile — distinct from recompiling the project itself |
| `PlayerSettings.SetScriptingDefineSymbols` / `GetScriptingDefineSymbols` | `ScriptReference/PlayerSettings.SetScriptingDefineSymbols.html`, `ScriptReference/PlayerSettings.GetScriptingDefineSymbols.html` | Current (non-obsolete) API to read/write per-`NamedBuildTarget` define symbols |
| `PlayerSettings.SetScriptingDefineSymbolsForGroup` / `GetScriptingDefineSymbolsForGroup` (obsolete) | `ScriptReference/PlayerSettings.SetScriptingDefineSymbolsForGroup.html`, `ScriptReference/PlayerSettings.GetScriptingDefineSymbolsForGroup.html` | Older `BuildTargetGroup`-keyed overloads — flagged obsolete in-docs in favor of the `NamedBuildTarget` overloads; recognize this when reading legacy project code |
| `Build.NamedBuildTarget` | `ScriptReference/Build.NamedBuildTarget.html`, `.Standalone.html`, `.Android.html`, `.iOS.html`, `.Server.html`, `.FromBuildTargetGroup.html`, `.ToBuildTargetGroup.html` | The struct that replaced `BuildTargetGroup` as the define-symbols key; static properties per platform plus conversion helpers |
| Platform-dependent compilation (manual) | `Manual/platform-dependent-compilation.html` | Conceptual background: built-in platform symbols (`UNITY_EDITOR`, `UNITY_ANDROID`, …) vs. custom scripting define symbols, and where they're consumed (`#if`) |
| `EditorApplication.LockReloadAssemblies` / `UnlockReloadAssemblies` | `ScriptReference/EditorApplication.LockReloadAssemblies.html`, `ScriptReference/EditorApplication.UnlockReloadAssemblies.html` | Suppressing an assembly reload mid-operation (e.g. during a multi-step scripted sequence that must not be interrupted by a domain reload) |
| `EditorApplication.update` | `ScriptReference/EditorApplication.html` (member list) | The standard editor-tick event used to poll `Request.IsCompleted` without blocking the main thread |

## Key Guidelines

### Writing a ScriptedImporter

A `ScriptedImporter` (`Manual/ScriptedImporters.html`, `ScriptReference/AssetImporters.ScriptedImporter.html`) is how you teach Unity to treat an arbitrary file extension as a first-class asset type, the same way `.fbx` or `.png` are natively handled. The class lives in the `UnityEditor.AssetImporters` namespace (so it must be editor-only — put it under an `Editor/` folder or an editor-only `.asmdef`, per `unity-editor-tooling`/`unity-project-structure` conventions), derives from the abstract `ScriptedImporter`, and carries a `[ScriptedImporter(version, ext)]` attribute that registers which file extension(s) route through it. Unity calls your single required override, `OnImportAsset(AssetImportContext ctx)`, once per import; everything the importer produces — the primary asset, any sub-assets, warnings/errors — is written through the `ctx` parameter rather than returned. Public serialized fields on the importer class behave exactly like `MonoBehaviour` fields: they show up in the importer's Inspector and are what a paired `ScriptedImporterEditor` reads/writes.

Two rules matter for correctness beyond getting an example compiling. First, you must call `ctx.SetMainObject(...)` on exactly one of the objects you add via `ctx.AddObjectToAsset(identifier, obj)` — only the designated Main Asset is eligible to become a Prefab or be referenced as "the" asset in other Inspectors; the `identifier` string passed to `AddObjectToAsset` must stay stable across reimports of the same file, since it's how Unity tracks sub-asset identity between import runs (changing it orphans references that pointed at the old identifier). Second, any temporary Unity object created during import that is *not* passed to `AddObjectToAsset` must be explicitly destroyed with `DestroyImmediate` before `OnImportAsset` returns — the import pipeline does not garbage-collect unregistered native objects for you.

```csharp
using System.IO;
using UnityEngine;
using UnityEditor.AssetImporters;

// Registers a custom importer for files with the ".widget" extension.
// version: bump this integer whenever OnImportAsset's logic changes, so
// Unity invalidates previously cached import results for existing .widget assets.
[ScriptedImporter(version: 1, ext: "widget")]
public class WidgetImporter : ScriptedImporter
{
    [SerializeField] float scale = 1f;
    [SerializeField] Color tint = Color.white;

    public override void OnImportAsset(AssetImportContext ctx)
    {
        // Register a dependency: if this config file changes, Unity reimports
        // every .widget asset that declared the dependency, even though the
        // .widget file itself didn't change on disk.
        const string configPath = "Assets/Config/WidgetDefaults.asset";
        if (File.Exists(configPath))
            ctx.DependsOnArtifact(configPath);

        string json;
        try
        {
            json = File.ReadAllText(ctx.assetPath);
        }
        catch (IOException e)
        {
            ctx.LogImportError($"Could not read widget file: {e.Message}");
            return;
        }

        var data = JsonUtility.FromJson<WidgetData>(json);

        var go = new GameObject("Widget");
        var mesh = BuildWidgetMesh(data, scale);
        var meshFilter = go.AddComponent<MeshFilter>();
        meshFilter.sharedMesh = mesh;

        var material = new Material(Shader.Find("Standard")) { color = tint };
        var renderer = go.AddComponent<MeshRenderer>();
        renderer.sharedMaterial = material;

        // Identifiers must stay stable across reimports; they're the sub-asset's identity.
        ctx.AddObjectToAsset("mesh", mesh);
        ctx.AddObjectToAsset("material", material);
        ctx.AddObjectToAsset("main", go);
        ctx.SetMainObject(go);
    }

    [System.Serializable]
    class WidgetData { public float[] points; }

    static Mesh BuildWidgetMesh(WidgetData data, float scale) => new Mesh();
}
```

Pair it with a `ScriptedImporterEditor` (`ScriptReference/AssetImporters.ScriptedImporterEditor.html`) when the importer's own settings (here `scale`/`tint`) need a custom Inspector layout beyond the default field list; it follows the same Apply/Revert lifecycle every `AssetImporterEditor` uses (`ApplyRevertGUI`, `Apply`, `CanApply`, `HasModified`), which is distinct from the ordinary `CustomEditor`/`Editor` pattern covered in `unity-editor-tooling` because importer settings are staged and only committed (triggering a real reimport) on Apply.

### PackageManager.Client Scripting (async Request pattern)

`PackageManager.Client` (`ScriptReference/PackageManager.Client.html`, conceptual walkthrough in `Manual/upm-api.html`) is the scripted equivalent of the Package Manager window: `Client.Add`, `.Remove`, `.List`, `.Search`/`.SearchAll`, `.Embed`, `.Resolve`, `.AddAndRemove`. Every one of these methods is **asynchronous and non-blocking** — each returns immediately with a `Request` subclass (`AddRequest`, `RemoveRequest`, `ListRequest`, …) whose work happens on a background thread/process while the Editor keeps ticking. There is no synchronous "wait for it" call in this API; the only supported way to observe completion is polling `Request.IsCompleted` from an editor-tick callback, most commonly `EditorApplication.update` (`ScriptReference/PackageManager.Requests.Request.html`, and the canonical add-a-package example in `Manual/upm-api.html` does exactly this). Once `IsCompleted` is true, check `Request.Status` against `PackageManager.StatusCode` before touching `.Result` — `StatusCode.Success` means the typed `.Result` (from the generic `Request<T>`/`Request_1` base) is populated, while any code `>= StatusCode.Failure` means `.Result` is not meaningful and the failure detail lives in `Request.Error` (`PackageManager.Error`, with a coarse `PackageManager.ErrorCode` like `Conflict`, `NotFound`, `Forbidden`, `ConnectionError`).

```csharp
using UnityEditor;
using UnityEditor.PackageManager;
using UnityEditor.PackageManager.Requests;
using UnityEngine;

// Editor-only automation: install a package by name, poll to completion,
// and unsubscribe once done so the update callback doesn't leak.
public static class PackageBootstrap
{
    static AddRequest addRequest;

    [MenuItem("Tools/CI/Install Addressables Package")]
    public static void InstallAddressables()
    {
        // Bare name = latest compatible released version.
        // Use "com.unity.addressables@1.21.19" to pin an exact version.
        addRequest = Client.Add("com.unity.addressables");
        EditorApplication.update += Poll;
    }

    static void Poll()
    {
        if (addRequest == null || !addRequest.IsCompleted)
            return;

        if (addRequest.Status == StatusCode.Success)
            Debug.Log($"Installed {addRequest.Result.packageId}");
        else if (addRequest.Status >= StatusCode.Failure)
            Debug.LogError($"Package install failed: {addRequest.Error.message} ({addRequest.Error.errorCode})");

        EditorApplication.update -= Poll;
        addRequest = null;
    }
}
```

The `identifier` string accepted by `Client.Add` overloads its meaning by format: a bare package name resolves to the latest compatible released version; `"name@version"` pins an exact version (the only way to install a pre-release); a git URL (`"https://github.com/org/repo.git"`, optionally `#revision`) installs a git dependency; and `"file:/path/to/folder"` or `"file:/path/to/package.tgz"` install a local package or tarball — the same source forms documented for `manifest.json` entries in `unity-project-structure`, just issued through code instead of hand-edited JSON. `Client.List(offlineMode: true)` skips a registry round-trip when you only need locally-known package state (faster, works without network), while the default `offlineMode: false` refreshes against the registry first. `Client.AddAndRemove` batches an add and a remove into one manifest resolution pass — prefer it over sequential `Add` then `Remove` calls when swapping one package for another, since two separate calls trigger two separate dependency-graph resolutions.

### Scripting Define Symbols

Scripting define symbols are per-platform C# preprocessor symbols (`#if MY_SYMBOL`) layered on top of Unity's built-in platform symbols (`UNITY_EDITOR`, `UNITY_ANDROID`, `UNITY_IOS`, etc. — background in `Manual/platform-dependent-compilation.html`). The current scripting API keys them by `Build.NamedBuildTarget` (`ScriptReference/Build.NamedBuildTarget.html`), a struct with static members like `NamedBuildTarget.Standalone`, `.Android`, `.iOS`, `.Server`, plus `FromBuildTargetGroup`/`ToBuildTargetGroup` converters: `PlayerSettings.SetScriptingDefineSymbols(NamedBuildTarget, string[])` and `PlayerSettings.GetScriptingDefineSymbols(NamedBuildTarget)` (`ScriptReference/PlayerSettings.SetScriptingDefineSymbols.html`, `.GetScriptingDefineSymbols.html`). The older `BuildTargetGroup`-keyed overloads (`SetScriptingDefineSymbolsForGroup`/`GetScriptingDefineSymbolsForGroup`) are marked obsolete in-docs in favor of these — expect to see the old form in existing project code and migrate it opportunistically, but write new tooling against the `NamedBuildTarget` overloads.

```csharp
using UnityEditor;
using UnityEditor.Build;
using UnityEngine;

public static class DefineSymbolTool
{
    [MenuItem("Tools/CI/Add DEBUG_TELEMETRY Symbol")]
    public static void AddTelemetrySymbol()
    {
        var target = NamedBuildTarget.Standalone;
        var existing = PlayerSettings.GetScriptingDefineSymbols(target)
            .Split(';');

        if (System.Array.IndexOf(existing, "DEBUG_TELEMETRY") >= 0)
            return; // already present

        var updated = new System.Collections.Generic.List<string>(existing) { "DEBUG_TELEMETRY" };
        PlayerSettings.SetScriptingDefineSymbols(target, updated.ToArray());
        // Symbol is now staged; it only takes effect once the next
        // domain reload/recompile actually runs (see Compilation Callbacks below).
    }
}
```

Because a define-symbol change only changes *what the next compile sees*, not the code Unity is currently running, setting a symbol and immediately checking `#if MY_SYMBOL`-gated behavior in the same editor tick will observe the *old* state — the change is inert until a domain reload happens. If your automation needs code to observe the new symbol, either explicitly trigger a recompile (`CompilationPipeline.RequestScriptCompilation()`) and wait for `compilationFinished`, or accept that Unity's own auto-recompile-on-focus-change/on-file-save behavior will pick it up on the next natural reload.

### Compilation Callbacks & Forcing Recompiles

`Compilation.CompilationPipeline` (`ScriptReference/Compilation.CompilationPipeline.html`) is the scripting window into the compiler itself: querying the current assembly graph (`GetAssemblies(AssembliesType)`, returning `Compilation.Assembly[]` with name/output path/defines/source files/references), mapping a script file to its owning assembly (`GetAssemblyDefinitionFilePathFromScriptPath`, `GetAssemblyNameFromScriptPath`), and subscribing to compilation lifecycle events. Two event pairs exist at different granularity: `compilationStarted`/`compilationFinished` fire once for the whole compile cycle (a `compilationFinished` callback receives a context object correlating it to the matching `compilationStarted` call and is guaranteed to fire *after* every `assemblyCompilationFinished` in that cycle), while `assemblyCompilationStarted`/`assemblyCompilationFinished`/`assemblyCompilationNotRequired` fire once per individual assembly — `assemblyCompilationFinished` hands you the output path and a `CompilerMessage[]` you should scan for `CompilerMessageType.Error` to detect a failed assembly build (a failed compile does not throw or otherwise interrupt your subscriber; you must check the messages yourself).

`CompilationPipeline.RequestScriptCompilation()` asks Unity to check for a recompile — it is a *request*, not a guarantee: if Unity determines nothing actually needs recompiling (no changed files, no changed defines/references it hasn't already picked up), no compile happens and none of the events above fire. Pass `RequestScriptCompilationOptions.CleanBuildCache` when a define-symbol or other invalidation you made isn't itself sufficient to force Unity to notice (rare — normally changing symbols or writing a script file is enough to make the next request meaningful).

```csharp
using UnityEditor;
using UnityEditor.Compilation;
using UnityEngine;

// Editor-only automation that forces a recompile and reports success/failure
// once the whole cycle completes.
[InitializeOnLoad]
public static class RecompileWatcher
{
    static RecompileWatcher()
    {
        CompilationPipeline.assemblyCompilationFinished += OnAssemblyDone;
        CompilationPipeline.compilationFinished += OnCycleDone;
    }

    static bool anyErrors;

    static void OnAssemblyDone(string assemblyPath, CompilerMessage[] messages)
    {
        foreach (var m in messages)
        {
            if (m.type == CompilerMessageType.Error)
            {
                anyErrors = true;
                Debug.LogError($"{assemblyPath}: {m.message} ({m.file}:{m.line})");
            }
        }
    }

    static void OnCycleDone(object context)
    {
        Debug.Log(anyErrors ? "Recompile finished WITH ERRORS" : "Recompile finished cleanly");
        anyErrors = false;
    }

    [MenuItem("Tools/CI/Force Recompile")]
    static void ForceRecompile() => CompilationPipeline.RequestScriptCompilation();
}
```

`[InitializeOnLoad]` (covered fully in `unity-editor-tooling`) is what makes the static constructor re-subscribe on every domain reload — subscribing once in a `[MenuItem]`-invoked method without also re-subscribing after reloads means the handler silently stops firing the moment a recompile actually happens, since the domain (and every non-`[InitializeOnLoad]` static subscription on it) is torn down and rebuilt.

### Combining These for CI/Automated Project Setup

The three systems compose into a single unattended "bootstrap a project" script because they sit in a strict dependency order: packages must be installed before code that references their types can compile; define symbols that gate that code must be set before or alongside the package install; and nothing downstream (a build, a test run, further scripted asset generation) should proceed until the resulting recompile has actually finished cleanly. See Advanced Notes for the full composed script.

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Blocking/spin-waiting on a `PackageManager.Request` | There is no synchronous wait; every `Client` method is async. Poll `IsCompleted` from `EditorApplication.update` (or another editor-tick source) and unsubscribe once done — never loop calling `Thread.Sleep` on the main thread |
| Reading `.Result` before checking `.Status` | `.Result` on a failed request is not populated/meaningful; always branch on `Status == StatusCode.Success` vs `Status >= StatusCode.Failure` first |
| Treating `StatusCode.Failure` as the only failure value | `Status` should be compared with `>=`, not `==`, against `StatusCode.Failure` — other failure-range codes exist beyond the literal `Failure` member |
| `ScriptedImporter` version not bumped after logic changes | Unity caches import results keyed partly by the `[ScriptedImporter(version, ...)]` number; changing `OnImportAsset` behavior without incrementing `version` leaves already-imported assets stuck on stale cached output until something else forces reimport |
| Sub-asset identifier strings changed between versions | `ctx.AddObjectToAsset(identifier, obj)`'s identifier is the sub-asset's persistent identity across reimports; renaming it silently orphans any existing reference (material slot, prefab link) that pointed at the old identifier — treat identifiers as a stable contract, not a display label |
| Forgetting `ctx.SetMainObject(...)` | Without it, no object is registered as the Main Asset, so nothing importable-as-a-Prefab exists and other assets can't cleanly reference "the" imported object |
| Leaking temporary native objects in `OnImportAsset` | Any `Mesh`/`Material`/etc. created during import but never passed to `AddObjectToAsset` must be `DestroyImmediate`d before returning — the import pipeline won't clean these up for you |
| Forgetting `AssetDatabase.Refresh()` after writing files outside the Editor | A script/CI step that writes new files directly to `Assets/` via `File.WriteAllText` or similar doesn't trigger Unity's file watcher reliably in all contexts; call `AssetDatabase.Refresh()` (or `ImportAsset` for a specific known path) to guarantee the import pipeline notices |
| Assuming a define-symbol change is visible immediately | `SetScriptingDefineSymbols` only affects the *next* compile; code executing in the current domain still sees the old `#if` outcome until a recompile/domain reload actually happens |
| Using the obsolete `BuildTargetGroup`-keyed define-symbol overloads in new code | `SetScriptingDefineSymbolsForGroup`/`GetScriptingDefineSymbolsForGroup` are documented obsolete in favor of the `NamedBuildTarget`-keyed overloads; still functional, but new tooling should target the current API |
| Assuming `RequestScriptCompilation()` always triggers a compile | It's a request, not a command — if Unity thinks nothing changed, no compile runs and none of the `CompilationPipeline` events fire; don't gate automation purely on "I called RequestScriptCompilation, therefore compilationFinished will fire soon" without also handling the no-op case |
| Not checking `CompilerMessage[]` for errors in `assemblyCompilationFinished` | A failed assembly compile does not throw into your event handler; you must scan the delivered `CompilerMessage[]` for `CompilerMessageType.Error` yourself to detect failure |
| Subscribing to `CompilationPipeline` events without `[InitializeOnLoad]` re-subscription | A subscription made only inside a one-off menu command's method body is lost on the next domain reload (which a recompile itself causes); use `[InitializeOnLoad]`'s static constructor so the handler is re-attached every time |
| Racing a package install against a define-symbol-gated script | Code behind `#if MY_SYMBOL` that also `using`s a namespace from a package being installed in the same automation pass will fail to compile if the package `Add` request hasn't completed (and the resulting domain reload hasn't happened) before the symbol takes effect — sequence package install → wait for completion → set symbols → wait for compile, never all at once |
| Blocking script execution while `LockReloadAssemblies` is held | Every `EditorApplication.LockReloadAssemblies()` must be matched by exactly one `UnlockReloadAssemblies()`; an exception thrown between the two (or an early `return`) leaves reloads permanently locked for the rest of the Editor session until manually unlocked or the Editor restarts |
| ScriptedImporter mutating serialized fields without SaveAndReimport | Programmatically changing an importer's own serialized settings (via `AssetImporter.GetAtPath` + field set) has no effect on the already-imported asset until you call `EditorUtility.SetDirty` and `AssetImporter.SaveAndReimport` on it |

## Quick Reference

| Class / Method | Purpose |
|---|---|
| `[ScriptedImporter(version, ext)]` | Registers a `ScriptedImporter` subclass for one or more file extensions |
| `ScriptedImporter.OnImportAsset(AssetImportContext)` | Required override; the entire import happens here |
| `ScriptedImporter.GatherDependenciesFromSourceFile` | Optional lightweight dependency discovery, cheaper than a full import |
| `AssetImportContext.assetPath` | Path to the source file being imported |
| `AssetImportContext.AddObjectToAsset(id, obj)` | Registers a sub-asset with a stable identifier |
| `AssetImportContext.SetMainObject(obj)` | Designates the Main Asset (must be one of the added objects) |
| `AssetImportContext.DependsOnArtifact/SourceAsset/CustomDependency` | Registers extra reimport triggers beyond the source file itself |
| `AssetImportContext.selectedBuildTarget` | Active build target for platform-aware import logic; reading it registers a dependency on the target |
| `AssetImporterEditor` / `ScriptedImporterEditor` | Custom Inspector base for an importer's own settings, with Apply/Revert semantics |
| `AssetDatabase.Refresh()` / `ImportAsset(path)` | Forces Unity to notice externally-written files |
| `PackageManager.Client.Add(identifier)` | Install/upgrade a package; returns `AddRequest` |
| `Client.Remove(name)` | Uninstall a package; returns `RemoveRequest` |
| `Client.AddAndRemove(toAdd, toRemove)` | Batched add+remove in one resolution pass |
| `Client.List(offlineMode, includeIndirect)` | Enumerate installed packages; returns `ListRequest` |
| `Client.Search` / `SearchAll` | Query the registry without installing; returns `SearchRequest` |
| `Client.Embed(name)` | Convert an installed package to an embedded local copy |
| `Request.IsCompleted` / `.Status` / `.Error` | Poll-completion contract shared by every `PackageManager` request |
| `Request<T>.Result` | Typed payload, valid only once `IsCompleted && Status == StatusCode.Success` |
| `StatusCode.Success` / `>= StatusCode.Failure` | Comparison pattern for a finished request's outcome |
| `PlayerSettings.SetScriptingDefineSymbols(NamedBuildTarget, string[])` | Current API to set per-platform define symbols |
| `PlayerSettings.GetScriptingDefineSymbols(NamedBuildTarget)` | Current API to read per-platform define symbols |
| `NamedBuildTarget.Standalone/.Android/.iOS/.Server/...` | Static keys identifying each platform for the define-symbol APIs |
| `CompilationPipeline.GetAssemblies(AssembliesType)` | Enumerate the current compiled assembly graph |
| `CompilationPipeline.GetAssemblyDefinitionFilePathFromScriptPath` | Map a script file to its owning `.asmdef` |
| `CompilationPipeline.RequestScriptCompilation([options])` | Ask Unity to check for and perform a recompile |
| `CompilationPipeline.compilationStarted` / `compilationFinished` | Whole-cycle compile events |
| `CompilationPipeline.assemblyCompilationStarted` / `assemblyCompilationFinished` / `assemblyCompilationNotRequired` | Per-assembly compile events; `Finished` carries `CompilerMessage[]` |
| `CompilerMessage.type` / `.message` / `.file` / `.line` | Per-diagnostic payload; check `type == CompilerMessageType.Error` |
| `EditorApplication.LockReloadAssemblies()` / `UnlockReloadAssemblies()` | Suppress/resume domain reload during a multi-step scripted sequence |
| `[InitializeOnLoad]` | Ensures event subscriptions (Package Manager progress, Compilation events) survive/re-attach across domain reloads |

## Advanced Notes

**Composing a one-command project bootstrap.** The three systems chain in a fixed causal order, and the chain is exactly why none of them can be driven synchronously end-to-end in one function call: install packages (async `Client.Add`/`AddAndRemove`, which itself may trigger a domain reload once new package assemblies are pulled in) → set scripting define symbols that gate code depending on those packages (`PlayerSettings.SetScriptingDefineSymbols`, inert until the next compile) → explicitly force and wait for that compile (`CompilationPipeline.RequestScriptCompilation` + `compilationFinished`) → only then is it safe to run anything (tests, further scripted asset generation, a build) that depends on the newly-available types and symbols actually being live in the running domain. A minimal composed driver:

```csharp
using UnityEditor;
using UnityEditor.Build;
using UnityEditor.Compilation;
using UnityEditor.PackageManager;
using UnityEditor.PackageManager.Requests;
using UnityEngine;

[InitializeOnLoad]
public static class ProjectBootstrap
{
    const string DefineSymbol = "USES_ADDRESSABLES";
    static AddRequest addRequest;

    static ProjectBootstrap()
    {
        CompilationPipeline.compilationFinished += OnCompileFinished;
    }

    [MenuItem("Tools/CI/Bootstrap Project")]
    public static void Run()
    {
        addRequest = Client.Add("com.unity.addressables");
        EditorApplication.update += PollAdd;
    }

    static void PollAdd()
    {
        if (!addRequest.IsCompleted) return;
        EditorApplication.update -= PollAdd;

        if (addRequest.Status != StatusCode.Success)
        {
            Debug.LogError($"Bootstrap failed at package install: {addRequest.Error.message}");
            return;
        }

        var target = NamedBuildTarget.Standalone;
        var defines = new System.Collections.Generic.List<string>(
            PlayerSettings.GetScriptingDefineSymbols(target).Split(';'));
        if (!defines.Contains(DefineSymbol))
        {
            defines.Add(DefineSymbol);
            PlayerSettings.SetScriptingDefineSymbols(target, defines.ToArray());
        }

        // Package add + define change likely already queued a reload;
        // request explicitly so we get a deterministic completion signal either way.
        CompilationPipeline.RequestScriptCompilation();
    }

    static void OnCompileFinished(object context)
    {
        Debug.Log("Bootstrap complete: package installed, symbol set, project recompiled.");
        // Safe from here to run EditMode tests, generate assets via a ScriptedImporter-backed
        // pipeline, or kick off a build — all data this method depends on is now live.
    }
}
```

**Why `[InitializeOnLoad]` matters here specifically.** A package install or a define-symbol change routinely *causes* a domain reload on its own — Unity reloads the domain whenever the compiled assembly set changes. If the `compilationFinished` subscription were made inside `Run()` (a `[MenuItem]`-invoked instance method call) rather than in a static constructor decorated with `[InitializeOnLoad]`, the very reload the bootstrap triggers would tear down that subscription before it could fire, and the "bootstrap complete" signal would silently never arrive. This is the general shape of the trap: any automation that both *causes* reloads and *needs to observe* their completion must register its observers through `[InitializeOnLoad]`, not through the same call stack that kicks the process off.

**CI-specific considerations.** In a headless/batch-mode Editor invocation (`-batchmode -executeMethod`), there is no interactive window pumping `EditorApplication.update` on a human's schedule, but the update loop still runs — polling-based `Request` completion works the same way, just without visual feedback. What changes for CI is failure handling: a batch-mode process that never calls `EditorApplication.Exit` will hang the CI job indefinitely if a `Request` never completes (network failure mid-`Client.Add` with no timeout logic) or if `RequestScriptCompilation` never fires `compilationFinished` because nothing needed recompiling — always pair CI-facing automation like the driver above with an explicit timeout/watchdog and an unconditional `EditorApplication.Exit(code)` call once bootstrap either succeeds or definitively fails, rather than assuming the callback chain is guaranteed to reach a terminal state on its own.

**ScriptedImporter version bumps interacting with CI package installs.** If a bootstrap script installs a package that itself ships a `ScriptedImporter` for a custom asset type your project already has instances of, the reimport of those existing assets is driven by that package's own `[ScriptedImporter(version, ...)]` number, not by anything your bootstrap script controls — a package upgrade that bumps its importer version will trigger reimports of every matching asset in the project as a side effect of the `Client.Add` completing and the domain reloading, which is worth accounting for in CI time budgets on asset-heavy projects.

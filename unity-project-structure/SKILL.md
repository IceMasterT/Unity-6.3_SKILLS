---
name: unity-project-structure
description: Use when setting up a new Unity project's folder layout, configuring Assembly Definitions, managing packages via manifest.json, or setting up version control (.gitignore, meta files, Git LFS) for a Unity repo. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Project Structure

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Reserved folder name reference | `Manual/SpecialFolders.html` | Full table of reserved `Assets/` subfolders (`Editor`, `Editor Default Resources`, `Gizmos`, `Resources`, `Plugins`, `StreamingAssets`), per-folder max-count and valid-location rules, hidden/ignored asset patterns (dot-prefix, `~`-suffix, `cvs`, `.tmp`) |
| Predefined assemblies reference | `Manual/script-compile-order-folders.html` | The 4-phase default compilation order (`Assembly-CSharp-firstpass`, `Assembly-CSharp-Editor-firstpass`, `Assembly-CSharp`, `Assembly-CSharp-Editor`) and which folders map to which phase |
| Introduction to assemblies | `Manual/assembly-definitions-intro.html` | Why to split code into assemblies: compile-time isolation, reduced iteration time, explicit dependency graph |
| Creating assembly assets | `Manual/assembly-definitions-creating.html` | Step-by-step: creating `.asmdef`/`.asmref` via `Assets > Create > Scripting`, platform-specific assemblies, Editor-only assemblies |
| Referencing assemblies | `Manual/assembly-definitions-referencing.html` | GUID-vs-name reference rules, `overrideReferences`/`precompiledReferences`, referencing precompiled DLLs |
| Assembly Definition file format reference | `Manual/assembly-definition-file-format.html` | Full JSON schema for `.asmdef` (every key, type, required/optional) with two complete worked examples |
| Assembly Definition includes | `Manual/assembly-definition-includes.html` | Which files an `.asmdef`/`.asmref` folder scope actually pulls in, nesting rules |
| Assembly Definition metadata | `Manual/assembly-definition-metadata.html` | The `.meta` sidecar fields Unity generates for `.asmdef`/`.asmref` assets |
| Project manifest file | `Manual/upm-manifestPrj.html` | `Packages/manifest.json` schema: `dependencies`, `enableLockFile`, `resolutionStrategy`, `scopedRegistries`, `testables`, `pinnedPackages`, worked example |
| Package manifest file | `Manual/upm-manifestPkg.html` | `package.json` schema for an individual package (the file that lives inside every package, vs. the project manifest) |
| Managing packages via the manifest | `Manual/managing-packages-manifest.html` | Editing `manifest.json` directly (add/remove/change version) as an alternative to the Package Manager UI |
| Managing packages via the window | `Manual/managing-packages-window.html` | Package Manager UI workflows: install, remove, update, view dependencies |
| Embedded dependencies | `Manual/upm-embed.html` | What makes a package "embedded" (lives under `Packages/`), how embedded packages override manifest-declared versions, why to commit embedded package contents to source control |
| Local folder or tarball paths | `Manual/upm-localpath.html` | `"file:"`-prefixed manifest entries for local/offline package dependencies, path syntax on Windows vs. Unix |
| Introduction to Git dependencies | `Manual/upm-git.html` | Git URL package dependencies in `manifest.json`, protocol support (HTTP/HTTPS/SSH/FILE/GIT), revision pinning, subfolder syntax, Git client version requirement (>=2.14.0), Git LFS interaction |
| Git dependencies detail | `Manual/upm-git-dependencies.html` | Extended Git dependency syntax reference (revision + path combinations) |
| Scoped registries overview | `Manual/upm-scoped.html` | What scoped registries are, why organizations use them to distribute internal packages |
| Using a scoped registry in a project | `Manual/upm-scoped-use.html` | Adding a `scopedRegistries` entry to `manifest.json` and consuming packages from it |
| Hosting a scoped registry | `Manual/upm-scoped-host.html` | Standing up an npm-compatible registry server for your org's packages |
| Package Manager global cache | `Manual/upm-cache.html` | Global UPM cache location per OS (Windows/macOS/Linux paths), cache structure, offline reuse |
| Package Manager caches overview | `Manual/package-manager-caches.html` | Distinction between the UPM cache and the separate `.unitypackage` (Asset Store) cache |
| Version control (index) | `Manual/VersionControl.html` | Entry point linking Perforce integration, Smart Merge, and diff tool support pages |
| Version control integrations | `Manual/Versioncontrolintegration.html` | Step-by-step Editor setup for Perforce/UVCS, full Version Control project-settings property table |
| Version Control project settings reference | `Manual/class-VersionControlSettings.html` | Every `Edit > Project Settings > Version Control` field: Mode (Hidden/Visible meta files/Perforce), Perforce auth fields, Log Level, Automatic Add, Work Offline, Async Update |
| Editor project settings reference | `Manual/class-EditorManager.html` | `Edit > Project Settings > Editor` page: Asset Serialization Mode (Mixed/Force Binary/Force Text, default = Force Text), "Reduce version control noise" YAML option, Scene Handling |
| Perforce integration | `Manual/perForceIntegration.html` | Perforce-specific workflow details (workspace mapping, offline work, reconciliation) |
| Smart merge | `Manual/SmartMerge.html` | `UnityYAMLMerge` tool: exact binary paths on Windows/macOS, `mergespecfile.txt`, Off/Premerge/Ask modes, using it as git's `merge` driver |
| Diff tool support | `Manual/diff-tool-support.html` | Supported external diff/merge tools and how to configure a custom one in Editor preferences |
| Assembly Definition Importer | `Manual/class-AssemblyDefinitionImporter.html` | Inspector reference for the `.asmdef` importer/asset type |
| Assembly Definition Reference Importer | `Manual/class-AssemblyDefinitionReferenceImporter.html` | Inspector reference for the `.asmref` importer/asset type |
| Package Manifest Importer | `Manual/class-PackageManifestImporter.html` | Inspector reference for `package.json` when viewed inside Unity |
| `CompilationPipeline` class | `ScriptReference/Compilation.CompilationPipeline.html` (namespace root) | Editor scripting API to query assembly names/paths/defines programmatically (e.g. `GetAssemblyDefinitionFilePathFromScriptPath`, `GetAssemblyNameFromScriptPath`) — useful for build/CI tooling that needs to reason about the assembly graph |
| `PackageManager.Client` class | `ScriptReference/PackageManager.Client.html` (and `.Add`, `.Remove`, `.Embed`, `.List`, `.Resolve`, `.SearchAll`) | Editor scripting API for scripted package install/remove/embed — used to automate `manifest.json` changes from custom tooling instead of hand-editing |
| `AssetDatabase.AssetPathToGUID` / `GUIDToAssetPath` | `ScriptReference/AssetDatabase.AssetPathToGUID.html`, `ScriptReference/AssetDatabase.GUIDToAssetPath.html` | Resolving the GUIDs used in `.asmdef` `references` arrays and in `.meta` files |

## Key Guidelines

### Folder Layout & Special Folders

Unity reserves a fixed set of subfolder names directly under `Assets/` (or nested anywhere below it, per-folder rules vary) that change how their contents are treated at import and build time — this is not convention, it is enforced behavior:

- **`Editor`** — any folder named exactly `Editor`, anywhere under `Assets/`, unlimited count. Scripts inside compile into an editor-only assembly, are stripped from Player builds, and `MonoBehaviour`s inside cannot be attached to GameObjects as components. The exact location of an `Editor` folder affects which of the four default compile phases its contents land in (see Assembly Definitions below) — prefer an `.asmdef` with no platform restrictions plus `"includePlatforms": ["Editor"]` over relying on folder name/location if you need editor code that isn't at the top level.
- **`Editor Default Resources`** — root of `Assets/` only, **max 1 per project**. Assets loaded on demand via `EditorGUIUtility.Load`; always include the subfolder path in the load call if assets are nested.
- **`Resources`** — unlimited count, any location under `Assets/`. Everything inside is loadable via `Resources.Load` and is **always included in the build**, whether referenced by a scene or not. Overuse directly bloats build size and startup/load time — prefer Addressables or direct scene references for anything not needed for truly dynamic runtime lookup.
- **`Plugins`** — reserved for native/managed third-party plugin binaries; exact subfolder naming (e.g. `Plugins/Android`, `Plugins/x86_64`) determines platform targeting — see the platform plugin import docs, not covered here.
- **`Gizmos`** — root of `Assets/` only, max 1 per project. Icon/texture files consumed by `Gizmos.DrawIcon` in the Scene view; not shipped in Player builds.
- **`StreamingAssets`** — root of `Assets/` only, max 1 per project. Files copied byte-for-byte into the build output and read at runtime through `Application.streamingAssetsPath` rather than the AssetDatabase — the one place dot-prefixed files are *not* ignored during import.

**Hidden/ignored during import** (any of these anywhere under `Assets/` are skipped by the importer, `StreamingAssets` dot-files excepted): hidden OS folders, files/folders starting with `.`, files/folders ending with `~`, anything named `cvs`, files with a `.tmp` extension. Note the Editor auto-converts a leading `.` typed in its Create menu into a leading `_` to avoid crashes — to genuinely get a dot-prefixed folder (e.g. `.git`, `.github`) you must create it outside the Editor, directly on disk.

Beyond the reserved names, organize `Assets/` **by feature/domain, not by asset type** for anything beyond a small prototype (avoid a top-level `Scripts/`, `Prefabs/`, `Materials/` split once the project grows) — this keeps each feature's `.asmdef`-scoped code physically co-located with the art/data it owns, which is what makes assembly boundaries and ownership legible at a glance. A typical mid-size layout:

```
Assets/
  _Project/
    Characters/
      Player/
        Scripts/ (has its own Player.asmdef)
        Prefabs/
        Art/
      Enemies/
    Systems/
      Combat/
      Inventory/
    Editor/                 (project-wide editor tooling)
    Resources/              (kept small and audited)
    Tests/
      EditMode/
      PlayMode/
  Plugins/
  StreamingAssets/
Packages/
  manifest.json
  packages-lock.json
ProjectSettings/
```

A leading underscore folder (e.g. `_Project`) is a common convention (not a reserved name) to sort first-party content above Unity's own `Packages`/imported asset folders in the Project window's alphabetical sort.

### Assembly Definitions

Unity's default compilation model, absent any `.asmdef` files, splits **all** scripts into exactly four predefined assemblies based purely on folder location and name (`Manual/script-compile-order-folders.html`):

| Phase | Assembly | Contains |
|---|---|---|
| 1 | `Assembly-CSharp-firstpass` | Runtime scripts inside any top-level folder named `Plugins` |
| 2 | `Assembly-CSharp-Editor-firstpass` | Editor scripts inside an `Editor` folder nested anywhere inside a top-level `Plugins` folder |
| 3 | `Assembly-CSharp` | All other scripts not inside an `Editor` folder |
| 4 | `Assembly-CSharp-Editor` | All remaining scripts inside an `Editor` folder |

A script can only reference types compiled in its own phase or an earlier one — never a later phase. This is the root cause of "why can't my Editor script see my runtime class" style errors when folder placement crosses a phase boundary unexpectedly.

Creating your own `.asmdef` files (`Assets > Create > Scripting > Assembly Definition`) opts a folder subtree out of this default scheme entirely: the new assembly contains every script in that folder and its subfolders **except** subfolders that have their own `.asmdef`/`.asmref`. Two benefits compound as a project grows: (1) editing a script only triggers a recompile of assemblies whose dependency graph includes it, not the whole project; (2) the `references` array makes cross-feature dependencies explicit and enforced by the compiler, rather than implicit through global visibility. Use `.asmref` (`Assets > Create > Scripting > Assembly Definition Reference`) to fold an additional folder into an *existing* assembly without creating a second `.asmdef` — useful for splitting a feature's implementation and its `Editor/` subfolder into different physical locations that still compile as one logical assembly, or the reverse.

The full `.asmdef` JSON schema (`Manual/assembly-definition-file-format.html`):

| Key | Type | Notes |
|---|---|---|
| `name` | string | Required. Assembly name; also usable as a `references` target elsewhere |
| `references` | string[] | GUIDs (`"GUID:..."`) or assembly names of other `.asmdef`s. Must be consistently one form or the other within the array |
| `includePlatforms` / `excludePlatforms` | string[] | Mutually exclusive — one of the two must be empty |
| `allowUnsafeCode` | bool | Default `false` |
| `overrideReferences` | bool | Must be `true` for `precompiledReferences` to take effect |
| `precompiledReferences` | string[] | DLL filenames (with extension, no path) — only used when `overrideReferences: true` |
| `autoReferenced` | bool | Default `true` |
| `defineConstraints` | string[] | Symbols that gate whether this assembly compiles at all |
| `versionDefines` | object[] | `{name, expression, define}` — defines a symbol when a referenced package/module version matches a semver range |
| `noEngineReferences` | bool | Default `false`; `true` excludes implicit `UnityEngine`/`UnityEditor` references |

Real worked example from the docs (assembly-name form, platform-included, with precompiled DLL overrides):

```json
{
  "name": "BeeAssembly",
  "references": [
    "Unity.CollabProxy.Editor",
    "AssemblyB",
    "UnityEngine.UI",
    "UnityEngine.TestRunner",
    "UnityEditor.TestRunner"
  ],
  "includePlatforms": ["Android", "LinuxStandalone64", "WebGL"],
  "excludePlatforms": [],
  "overrideReferences": true,
  "precompiledReferences": ["Newtonsoft.Json.dll", "nunit.framework.dll"],
  "autoReferenced": false,
  "defineConstraints": ["UNITY_2019", "UNITY_INCLUDE_TESTS"],
  "versionDefines": [
    { "name": "com.unity.ide.vscode", "expression": "[1.7,2.4.1]", "define": "MY_SYMBOL" },
    { "name": "com.unity.test-framework", "expression": "[2.7.2-preview.8]", "define": "TESTS" }
  ],
  "noEngineReferences": false
}
```

Prefer GUID references over name references for `.asmdef`-to-`.asmdef` links in shared/team projects — a name reference silently breaks if you ever rename the referenced assembly, while a GUID (resolvable/settable via `AssetDatabase.AssetPathToGUID`/`GUIDToAssetPath`) survives renames. Whichever form you pick, use it consistently across the whole `references` array (mixing forms within one file is not supported).

### Package Manager & manifest.json

`Packages/manifest.json` is the **project manifest** — it declares only *direct* dependencies; the transitive graph is resolved automatically and (with lock files enabled) pinned in `Packages/packages-lock.json`. This is distinct from a `package.json` inside any individual package (the *package* manifest, `Manual/upm-manifestPkg.html`) — don't confuse the two when editing by hand.

Project manifest top-level keys (`Manual/upm-manifestPrj.html`):

| Key | Type | Purpose |
|---|---|---|
| `dependencies` | object | `{ "package.name": "version" }` — registry version, or `file:`/git-URL string instead of a version |
| `enableLockFile` | bool | Default `true` — enables `packages-lock.json` for deterministic resolution |
| `resolutionStrategy` | string | Semver upgrade policy for indirect dependencies; default `lowest` |
| `scopedRegistries` | array | Additional registries beyond the default, scoped by package-name prefix |
| `testables` | string[] | Packages whose tests should load in the Unity Test Framework |
| `pinnedPackages` | string[] | Package names locked to exactly the version in `dependencies`, ignoring Editor-compatibility resolution |

Example combining a scoped registry with direct, registry-resolved dependencies:

```json
{
  "scopedRegistries": [
    {
      "name": "My internal registry",
      "url": "https://my.internal.registry.com",
      "scopes": ["com.company"]
    }
  ],
  "dependencies": {
    "com.unity.package-1": "1.0.0",
    "com.unity.package-2": "2.0.0",
    "com.company.my-tools": "1.4.0"
  }
}
```

A dependency's *source* is encoded directly in its value string, not a separate field:

- **Registry**: a plain version string, e.g. `"1.3.1"` — resolved from the default registry or a matching scoped registry.
- **Local (`Manual/upm-localpath.html`)**: `"file:../SharedPackages/my-package"` or a tarball path — forward slashes always work; on Windows, escaped backslashes work but aren't recommended. Good for local offline iteration on a package before publishing it.
- **Git (`Manual/upm-git.html`)**: `"https://github.com/org/repo.git"`, optionally with `#<revision>` and `?path=/sub/folder` for a subfolder package. Requires a local Git client >= 2.14.0 on every machine and CI runner that resolves the project (and the Git LFS client too, if the repo uses LFS). Git dependencies can only be declared in the *project* manifest — packages cannot depend on each other via Git URLs.
- **Embedded (`Manual/upm-embed.html`)**: no manifest entry needed at all — any folder physically present under `Packages/` is auto-detected and always wins over a same-named registry/version entry in `dependencies`, even if that entry's version differs. If the project is under source control, the embedded package's full contents must be committed too, since nothing about it is reproducible from the manifest alone.

Always commit `Packages/packages-lock.json` alongside `manifest.json` — it is the resolved, deterministic dependency graph (direct + transitive, with exact versions), and without it different machines/CI runs can resolve slightly different transitive versions even from an identical manifest.

The global UPM package cache (`Manual/upm-cache.html`) lives outside the project entirely and must never be added to a repo:

| OS | Default path |
|---|---|
| Windows (user account) | `%LOCALAPPDATA%\Unity\cache\upm` |
| Windows (system account) | `%ALLUSERSPROFILE%\Unity\cache\upm` |
| macOS | `$HOME/Library/Caches/Unity/upm` |
| Linux | `$HOME/.cache/Unity/upm` |

### Version Control Setup

Two Editor settings under `Edit > Project Settings` control how cleanly a Unity project behaves under source control, and both should be set **before** the first commit:

1. **`Version Control` category → Mode** (`Manual/class-VersionControlSettings.html`): `Visible Meta Files` (default, correct choice for Git/any VCS Unity doesn't natively integrate with), `Hidden Meta Files` (hides `.meta` from the OS file browser — avoid, it makes `.meta` files easy to forget to commit), or `Perforce` (enables native Perforce integration fields: Username/Password/Workspace/Server/Host/Log Level/Automatic Add/Work Offline/Async Update).
2. **`Editor` category → Asset Serialization → Mode** (`Manual/class-EditorManager.html`): `Force Text` is the **documented default** and is what you want for Git — it stores scenes/prefabs/assets as diffable, mergeable YAML. `Mixed` leaves existing binary assets binary while defaulting new ones to Binary; `Force Binary` converts everything to a non-diffable binary format. The adjacent **"Reduce version control noise"** checkbox forces the Editor to keep YAML references on one line (splitting only past 80 characters), which measurably shrinks diff noise on scene/prefab changes — enable it for any team repo.

Every asset's `.meta` file must be committed alongside the asset it describes — it carries the asset's GUID, which is what every other asset's reference (material slot, prefab link, scene object reference, `.asmdef` GUID reference) actually points at internally. Deleting, regenerating, or failing to commit a `.meta` silently breaks every inbound reference, and Unity will not warn you at commit time — only when something shows up "Missing" in the Editor.

`.gitignore` for a Unity project (only ever ignore regenerable, machine-local output; everything else — including `ProjectSettings/`, source `Assets/`, and `Packages/`, is source of truth):

```gitignore
# Unity-generated, regenerable
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Uu]serSettings/
[Mm]emoryCaptures/
.vs/
.vscode/
.idea/
*.csproj
*.sln
*.user
*.userprefs
*.unityproj
*.tmp
*.pidb
*.svd
sysinfo.txt
crashlytics-build.properties
```

`ProjectSettings/` **must** be committed — it holds build target settings, tag/layer definitions, physics/quality/graphics/input settings, and more, serialized as text; treat it exactly like `Assets/`.

For merge conflicts in scenes and prefabs (which are YAML but still semantically structured, so a naive text merge frequently produces a corrupt file), configure `UnityYAMLMerge` as git's merge driver (`Manual/SmartMerge.html`). The tool ships inside the Editor install:

- Windows: `C:\Program Files\Unity\Editor\Data\Tools\UnityYAMLMerge.exe`
- macOS: `/Applications/Unity/Unity.app/Contents/Helpers/UnityYAMLMerge` (right-click the `.app` → Show Package Contents to browse there)

Its fallback behavior for unresolved conflicts/unknown file types is controlled by `mergespecfile.txt` next to the binary, and in the Version Control project settings its Smart Merge mode (visible once Mode is set to Perforce/UVCS) can be `Off`, `Premerge` (auto-resolve clean merges, produce base/theirs/mine files for the rest), or `Ask` (default — prompt on conflict).

If the project is managed through Unity's own native integrations rather than a generic VCS, use Perforce (`Manual/perForceIntegration.html`) or Unity Version Control (UVCS) — both are configured from the same `Version Control` settings page, switching Mode away from the Git-friendly "Visible Meta Files" default.

### Git LFS

Git handles large or frequently-changing binary files (textures, audio, video, FBX/model files, `.psd`, baked lighting data) poorly — every version is stored in full in history, bloating clone size and slowing every operation. Git LFS replaces the binary blob in the repo with a small pointer file and stores the actual content in LFS-backed storage, fetched on demand. Configure via `.gitattributes` at the project root:

```gitattributes
# Images
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.tga filter=lfs diff=lfs merge=lfs -text
*.tif filter=lfs diff=lfs merge=lfs -text
*.exr filter=lfs diff=lfs merge=lfs -text
*.hdr filter=lfs diff=lfs merge=lfs -text

# Audio / video
*.wav filter=lfs diff=lfs merge=lfs -text
*.mp3 filter=lfs diff=lfs merge=lfs -text
*.ogg filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text

# 3D / animation
*.fbx filter=lfs diff=lfs merge=lfs -text
*.blend filter=lfs diff=lfs merge=lfs -text

# Build artifacts occasionally checked in deliberately (installers, etc.)
*.apk filter=lfs diff=lfs merge=lfs -text
*.unitypackage filter=lfs diff=lfs merge=lfs -text
```

Do **not** LFS-track `.meta` files or any text-serialized asset (scenes/prefabs/`.asset` files in Force Text mode) — those need to stay diffable/mergeable as plain text; LFS pointer-ifying them defeats Smart Merge entirely. If the project also consumes a package as a Git dependency (`Manual/upm-git.html`) from a repo that itself uses Git LFS, every machine resolving that dependency — including CI — needs the Git LFS client installed, not just Git itself, or resolution silently fails to fetch the actual LFS-backed content.

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Committing `Library/`/`Temp/` | No Unity-specific `.gitignore` set up before the first commit; add one immediately, and if already committed, `git rm -r --cached` them before adding the ignore rule |
| `.meta` files missing or out of sync | Asset added outside the Editor (shell script, CI step, manual copy) without letting Unity generate its meta; always let the Editor import new assets, or generate matching `.meta` files yourself, and commit asset + `.meta` in the same commit |
| Broken GUID references after a merge | Two branches independently create assets that get colliding or regenerated GUIDs; resolve by keeping one side's `.meta` file intact (its GUID), never by deleting all conflicting `.meta`s and letting Unity regenerate — that reassigns new GUIDs and breaks every existing reference to those assets |
| Slow recompiles on every script edit | No `.asmdef` files present, so the whole project sits in one of the four default assemblies (usually `Assembly-CSharp`) and any change recompiles everything; split into feature-scoped assemblies with explicit `references` |
| Binary merge conflicts on scenes/prefabs | Asset Serialization left on `Mixed`/`Force Binary`; switch to `Force Text` in `Edit > Project Settings > Editor`, enable "Reduce version control noise", and configure `UnityYAMLMerge` as the merge driver for `*.unity`/`*.prefab` |
| Package versions drift between machines/CI | `Packages/packages-lock.json` not committed, or `enableLockFile` set to `false`; commit the lock file so transitive resolution is deterministic everywhere |
| Editor script referencing runtime code fails to compile | Script sits in a folder that lands in a later default compile phase (e.g. inside `Editor/`) than the runtime class it references, which is in an earlier phase; either move code or, better, create explicit `.asmdef`s so the dependency is a real, checked reference instead of an implicit folder-order one |
| `.asmdef` `references` array silently breaks after a rename | Using assembly-*name* references instead of GUID references; a rename of the referenced `.asmdef`'s `name` field breaks every reference by name. Use GUID form (`"GUID:..."`) for team/shared-package assemblies, and never mix name and GUID forms in the same array |
| Embedded package "won't update" no matter what `manifest.json` says | An embedded package under `Packages/<name>/` always overrides a same-named `dependencies` entry regardless of the version string there; to actually take the registry version, delete/move the embedded folder rather than edit the manifest |
| `Resources/` folder quietly grows the build and slows startup | Everything under any `Resources/` folder ships in every build and is scanned at startup regardless of whether it's referenced; audit its contents periodically or migrate to Addressables/scene references |
| More than one `Gizmos`/`StreamingAssets`/`Editor Default Resources` folder created | These three names are capped at exactly one occurrence, at the root of `Assets/` only; a second one is silently not treated specially. Keep a single canonical folder and put subfolders inside it |
| CI resolves a Git-URL package dependency but gets an empty/corrupt checkout | Git LFS client not installed in the CI image while the source repo of that Git dependency uses LFS; install both the Git client (>=2.14.0) and Git LFS on every resolving machine |
| Dot-prefixed folder created through the Editor becomes an underscore folder | The Editor's Create menu auto-converts a leading `.` to `_` to prevent crashes; if you specifically need a literal dot-prefixed folder (e.g. `.github/`), create it directly on the filesystem, not through Unity's UI |
| `precompiledReferences` entries in an `.asmdef` are ignored | `overrideReferences` was left `false`; it must be explicitly set to `true` for `precompiledReferences` to take effect at all |
| Team members see different `manifest.json` resolution results despite identical file | `resolutionStrategy` defaults to `lowest`, which can differ in practice from what a developer expects if they're used to "highest wins"; state the intended strategy explicitly if the team relies on non-default behavior |
| Perforce fields greyed out / not visible in Version Control settings | Those fields (Username, Password, Workspace, Server, Host, Automatic Add, Work Offline, Async Update, Verbose log level) are conditionally shown only when Mode is set to `Perforce` — set Mode first before expecting to configure them |

## Quick Reference

| Folder/File/Setting | Purpose |
|---|---|
| `Assets/Editor/` | Editor-only scripts; excluded from Player builds; unlimited count, any location |
| `Assets/Editor Default Resources/` | On-demand `EditorGUIUtility.Load` assets; root of `Assets/` only, max 1 |
| `Assets/Resources/` | Runtime-loadable via `Resources.Load`; always shipped in build regardless of reference; unlimited count |
| `Assets/StreamingAssets/` | Files copied verbatim to build output, read via `Application.streamingAssetsPath`; root of `Assets/` only, max 1 |
| `Assets/Plugins/` | Native/managed plugin binaries; also Phase 1/2 of default compile order |
| `Assets/Gizmos/` | Icons/textures for `Gizmos.DrawIcon`; root of `Assets/` only, max 1 |
| `*.asmdef` | Defines a new compiled assembly from its folder + non-`.asmdef` subfolders |
| `*.asmref` | Folds a folder into an existing `.asmdef`'s assembly without a new assembly |
| `Packages/manifest.json` | Project manifest: direct package dependencies, `scopedRegistries`, `resolutionStrategy`, `pinnedPackages`, `testables` |
| `Packages/packages-lock.json` | Resolved (direct + transitive) dependency graph; commit for reproducible resolution |
| `Packages/<name>/package.json` | Individual package manifest (name, version, dependencies) — different file from the project manifest |
| `Packages/<embedded-package>/` | Any folder physically present here is an embedded package; always wins over a same-named `dependencies` entry |
| `ProjectSettings/` | Serialized project-wide settings (build targets, tags/layers, physics, graphics, etc.); commit it, text-serialized |
| `UserSettings/` | Per-user Editor preferences (layout, etc.); machine-local, gitignore it |
| `.meta` files | GUID + import-settings sidecar for every asset/folder; always commit alongside its asset |
| `Edit > Project Settings > Version Control > Mode` | `Visible Meta Files` (default, Git-friendly) / `Hidden Meta Files` / `Perforce` |
| `Edit > Project Settings > Editor > Asset Serialization > Mode` | `Force Text` (default, diff/merge-friendly) / `Mixed` / `Force Binary` |
| `Edit > Project Settings > Editor > Reduce version control noise` | Keeps YAML references on one line to shrink scene/prefab diffs |
| `UnityYAMLMerge` | Ships with the Editor; semantic merge tool for `.unity`/`.prefab` YAML, configurable as git's merge driver |
| `%LOCALAPPDATA%\Unity\cache\upm` (Win) / `~/Library/Caches/Unity/upm` (macOS) / `~/.cache/Unity/upm` (Linux) | Global UPM package cache — machine-local, never commit |
| `Library/`, `Temp/`, `Obj/`, `Logs/`, `.vs/`, `Build/`/`Builds/`, `UserSettings/` | Regenerable/machine-specific — always gitignore |
| `.gitattributes` (`filter=lfs diff=lfs merge=lfs -text`) | Routes tracked binary extensions (textures, audio, video, models) through Git LFS |

## Advanced Notes

**Large-project scaling.** Once a project passes roughly a few dozen `.asmdef`s, the assembly *graph* itself becomes a maintenance surface: a tangled or overly-deep reference chain reintroduces the "everything recompiles" problem one level up, because changing a low-level shared assembly still invalidates everything above it. Keep a strict layering discipline (e.g. `Core` → `Systems` → `Features` → `Game`, references only pointing downward) and forbid cyclic references between feature assemblies — Unity's compiler rejects true cycles, but near-cycles through a shared "utility" assembly that itself references feature code produce the same recompile blowup indirectly. Use `Compilation.CompilationPipeline` (`ScriptReference/Compilation.CompilationPipeline.html`) from an Editor-only tooling script to programmatically dump the current assembly graph and its dependency edges for CI-side auditing (e.g. failing a PR that introduces a new cross-layer reference).

**Monorepo / multi-project package sharing.** For multiple Unity projects sharing internal code (a company with several titles, or a game plus a companion tool), there are three realistic strategies, in increasing order of process overhead but decreasing coupling risk:
1. **Git-URL package dependency** (`Manual/upm-git.html`) pointing every consuming project's `manifest.json` at a shared package repo, pinned to a revision — simplest to adopt, but every consumer must bump the pinned revision manually to pick up changes, and Git dependencies can't nest (a package can't itself declare a Git dependency).
2. **Scoped registry** (`Manual/upm-scoped.html`, `upm-scoped-use.html`, `upm-scoped-host.html`) hosting real semver-versioned releases of shared packages — the correct choice once more than a couple of projects consume the same code, since it gives proper version negotiation, `resolutionStrategy`, and `pinnedPackages` semantics that a raw Git URL can't.
3. **Embedded package via a git submodule or symlink** into each consuming project's `Packages/` folder — fastest local iteration loop (edits are immediately visible with no publish step) but requires every contributor to correctly manage the submodule/symlink, and since embedded packages must have their full contents committed if the project is under source control, a naive submodule setup can end up duplicating the shared code's history into every consumer repo.

For a true single-repo monorepo containing multiple Unity projects, keep shared code as one or more packages under a top-level `Packages/` (or a dedicated `shared-packages/` directory referenced by `file:` path) rather than duplicating an `Assets/` subtree across projects — this is the only structure where a single `.asmdef` edit is instantly visible to every consuming project's compiler without any explicit publish/update step, at the cost of every consumer needing an identical relative path layout for `file:`-based manifest entries to resolve.

**Package Manager caches at scale.** On CI fleets or shared build farms, redirect the global UPM cache (`Manual/upm-cache.html`, `Manual/package-manager-caches.html`) to a persistent, shared volume across ephemeral runners rather than accepting the per-user default — this turns every cold-cache full package re-download into a cache hit and is one of the largest easy wins for CI build-time variance on package-heavy projects.

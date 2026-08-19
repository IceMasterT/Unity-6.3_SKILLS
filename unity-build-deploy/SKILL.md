---
name: unity-build-deploy
description: Use when configuring or troubleshooting a Unity build — Build Settings, per-platform Player Settings, IL2CPP vs Mono, or CI batch-mode builds. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Build & Deploy

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Build from the Editor (overview) | `Manual/BuildSettings.html` | Entry point; explicitly notes "The Build Profiles window was previously named Build Settings" |
| Build Profiles introduction | `Manual/build-profiles.html` | Platform profiles vs named Build Profiles, per-profile scene lists, asset-file storage for VCS |
| Create/manage a build profile | `Manual/create-build-profile.html`, `Manual/build-profiles-override-settings.html`, `Manual/build-profiles-reference.html` | Adding a platform, duplicating profiles, per-profile setting overrides, window field reference |
| Scene list management | `Manual/build-profile-scene-list.html` | Add/remove/reorder/exclude scenes per build profile |
| Build pipeline customization | `Manual/build-customize-build-pipeline.html`, `Manual/BuildPlayerPipeline.html` | Where custom build logic hooks into the pipeline |
| Custom build script | `Manual/build-script-build.html` | Writing a static method that calls `BuildPipeline.BuildPlayer` |
| Build callbacks | `Manual/build-callbacks.html` | `IPreprocessBuildWithContext`, `IPostprocessBuildWithContext`, `IProcessSceneWithReport`, `BuildPlayerProcessor` |
| Command-line builds | `Manual/build-command-line.html` | `-build`, `-activeBuildProfile`, `-buildTarget`, `-executeMethod`, required/recommended args, worked Windows/macOS examples |
| Editor command-line arguments | `Manual/EditorCommandLineArguments.html` | Full Editor CLI arg reference (`-batchmode`, `-quit`, `-projectPath`, `-logFile`, `-nographics`, etc.) |
| Player command-line arguments | `Manual/PlayerCommandLineArguments.html` | Args accepted by the *built player* at runtime (distinct from Editor args) |
| General command-line reference | `Manual/CommandLineArguments.html` | Index page linking Editor vs Player CLI docs |
| Build path requirements | `Manual/build-path-requirements.html` | Valid output path constraints per platform |
| Clean build | `Manual/build-clean-build.html` | Forcing a full rebuild vs incremental |
| Build content/output layout | `Manual/build-content-output.html` | What files land in the output folder and why |
| Android build process | `Manual/android-BuildProcess.html`, `Manual/how-unity-builds-android-applications.html` | APK vs AAB publishing format, Build Profiles workflow, Export Project path |
| Android build settings reference | `Manual/android-build-settings.html` | Every field in the Android Build Profile/Player Settings panes |
| Android export to Android Studio | `Manual/android-export-process.html` | Exporting a Gradle project instead of building directly |
| Android keystores overview | `Manual/android-keystore.html` | Keystore Manager window, why re-signing with a new key breaks store updates |
| Android keystore creation | `Manual/android-keystore-create.html`, `Manual/android-keystore-manager.html`, `Manual/android-keystore-add-keys.html`, `Manual/android-keystore-load.html` | Creating/loading a keystore, adding keys, Keystore Manager UI reference |
| Android Player Settings (manual) | `Manual/class-PlayerSettingsAndroid.html` | Android-specific Player Settings fields (Identification, Configuration, Publishing) |
| Android Google Play distribution | `Manual/android-distribution-google-play.html`, `Manual/android-distribution.html` | AAB requirement, target/min API level constraints for store submission |
| Android app size / asset packs | `Manual/android-application-size-restrictions.html`, `Manual/android-optimize-distribution-size.html`, `Manual/android-asset-packs-in-unity.html` | Play Store size limits, install-time/on-demand asset packs |
| iOS build overview | `Manual/ios-building-and-delivering.html`, `Manual/how-unity-builds-ios-applications.html` | Section index; Unity emits an Xcode project, does not sign itself |
| iOS build settings reference | `Manual/BuildSettingsiOS.html`, `Manual/class-PlayerSettingsiOS.html` | Xcode export options, per-platform Player Settings (bundle ID, signing team, target SDK) |
| iOS app thinning / app slicing | `Manual/ios-app-slicing.html` | On-demand resources, bitcode-era slicing behavior |
| iOS encryption export compliance | `Manual/ios-encryption-export-regulations.html` | `ITSAppUsesNonExemptEncryption` / App Store Connect compliance question |
| iOS environment setup | `Manual/ios-environment-setup.html`, `Manual/ios-requirements-and-compatibility.html` | Xcode version requirements, macOS host requirement |
| WebGL build/publish overview | `Manual/webgl-building.html`, `Manual/webgl-building-distribution.html`, `Manual/webgl-intro.html` | Build folder structure (`.loader.js`, `.framework.js`, `.wasm`, `.data`), compression extensions |
| WebGL build settings reference | `Manual/web-build-settings.html`, `Manual/class-PlayerSettingsWebGL.html` | Compression method, decompression fallback, memory size, Player Settings fields |
| WebGL deployment / hosting | `Manual/webgl-deploying.html`, `Manual/webgl-server-configuration-code-samples.html` | Server MIME-type/Content-Encoding config for compressed builds (Apache/nginx examples) |
| WebGL templates | `Manual/webgl-templates.html`, `Manual/web-templates-build-configuration.html` | Custom HTML shell templates for the build |
| WebGL code stripping / size | `Manual/webgl-distributionsize-codestripping.html` | WebGL-specific stripping levers beyond the general managed-stripping page |
| IL2CPP introduction | `Manual/il2cpp-introduction.html` | AOT vs JIT, native-toolchain requirement, same-OS build requirement (Linux is the cross-compile exception) |
| IL2CPP additional args | `Manual/handling-IL2CPP-additional-args.html` | Passing extra flags to the IL2CPP/native C++ compiler and linker |
| IL2CPP runtime checks | `Manual/il2cpp-runtime-checks.html` | Null/array/divide-by-zero check levels and their perf cost |
| Scripting backends overview | `Manual/scripting-backends.html`, `Manual/scripting-backends-intro.html` | Mono vs IL2CPP decision matrix by platform |
| Scripting backend: IL2CPP / Mono detail | `Manual/scripting-backends-il2cpp.html`, `Manual/scripting-backends-mono.html` | Backend-specific build/runtime behavior |
| Managed code stripping overview | `Manual/managed-code-stripping.html` | UnityLinker static-analysis model, what stripping can and can't see (reflection-only code) |
| Configure stripping level | `Manual/managed-code-stripping-configure.html` | Minimal / Low / Medium / High stripping levels and where to set them |
| Stripping's effect on content | `Manual/managed-code-stripping-content.html` | How stripping interacts with Addressables/AssetBundle content |
| Preserving code from stripping | `Manual/managed-code-stripping-preserving.html`, `Manual/managed-code-stripping-marking-rules.html` | `[Preserve]` attribute, linker marking rules per stripping level |
| link.xml format reference | `Manual/managed-code-stripping-xml-formatting.html` | Exact `link.xml` schema/elements to whitelist assemblies/types/members |
| `BuildPipeline` class (ScriptReference) | `ScriptReference/BuildPipeline.html`, `ScriptReference/BuildPipeline.BuildPlayer.html` | `BuildPipeline.BuildPlayer(BuildPlayerOptions)` signature, returns `BuildReport`, invalidates scene object references after the call |
| `BuildPlayerOptions` (ScriptReference) | `ScriptReference/BuildPlayerOptions.html` (+ `-scenes`, `-locationPathName`, `-target`, `-targetGroup`, `-subtarget`, `-options`, `-extraScriptingDefines` members) | Fields to populate for a scripted build |
| `EditorUserBuildSettings` (ScriptReference) | `ScriptReference/EditorUserBuildSettings.html` (+ `-activeBuildTarget`, `-development`, `-il2CppCodeGeneration`, `-buildAppBundle`, `-androidBuildSubtarget` etc.) | Reading/writing the Editor's persisted build-config state from script |
| `BuildOptions` / `BuildTarget` enums (ScriptReference) | `ScriptReference/BuildOptions.html`, `ScriptReference/BuildTarget.html` | Flags like `Development`, `AllowDebugging`, `CompressWithLz4`; valid target enum values for `-buildTarget` |
| `PlayerSettings` (ScriptReference) | `ScriptReference/PlayerSettings.html`, `ScriptReference/PlayerSettings.Android.html`, `ScriptReference/PlayerSettings.iOS.html`, `ScriptReference/PlayerSettings.WebGL.html` | Scripting API to read/set bundle ID, scripting backend, stripping level, per-platform fields from a build script |
| Build callbacks interfaces (ScriptReference) | `ScriptReference/Build.IPreprocessBuildWithContext.html`, `ScriptReference/Build.IPostprocessBuildWithContext.html` (confirm path before citing further) | Programmatic pre/post-build hooks used by `BuildPipeline.BuildPlayer` |
| Dedicated server build | `Manual/dedicated-server-build.html` | Headless/server subtarget build configuration |
| Linux build | `Manual/build-for-linux.html`, `Manual/Buildsettings-linux.html`, `Manual/PlayerSettings-linux.html`, `Manual/linux-il2cpp-crosscompiler.html` | Linux target build settings and the one supported IL2CPP cross-compile path |
| macOS build & notarization | `Manual/macos-building.html`, `Manual/macos-building-notarization.html`, `Manual/macosbuildsettings.html`, `Manual/PlayerSettings-macOS.html` | Gatekeeper/notarization requirement for distributing outside the Mac App Store |
| Deterministic builds | `Manual/build-deterministic-builds-introduction.html`, `Manual/build-deterministic-builds.html`, `Manual/build-deterministic-assets.html` | Reproducible-build guarantees, what breaks determinism |

## Key Guidelines

### Build Profiles & Scene List
In Unity 6, the window at **File > Build Profiles** replaced the older "Build Settings" window — the docs explicitly flag this rename (`Manual/BuildSettings.html`: "The Build Profiles window was previously named Build Settings"). There are two profile types: **Platform** profiles (one per installed platform, sharing settings like Development Build and the scene list across all platform profiles) and named **Build Profiles** (independent, savable configurations with their own scene list, stored as asset files so they go through version control). Switching the active platform/profile gates which Player Settings tab is editable — always confirm the active profile before touching Player Settings. A scene not added to the profile's scene list is silently excluded from the build even though it loads fine in the Editor via `File > Open Scene`.

```
File > Build Profiles > select Android (or duplicate as a new named profile)
Scene List panel > drag scenes from Project window, verify checkboxes are ticked
Switch Profile > confirms this profile is now EditorUserBuildSettings.activeBuildTarget
```

### Player Settings
Player Settings hold identification (Company Name, Product Name — these drive `Application.persistentDataPath` and the bundle identifier `com.CompanyName.ProductName`), the icon set, splash screen, scripting backend, API compatibility level, and per-platform overrides (Android, iOS, WebGL each get their own settings block, documented separately: `class-PlayerSettingsAndroid.html`, `class-PlayerSettingsiOS.html`, `class-PlayerSettingsWebGL.html`). Set Company/Product Name before relying on save paths or before generating store listings, because changing the bundle ID after a store submission is treated as a new, unrelated app by both Google Play and the App Store.

```csharp
using UnityEditor;

PlayerSettings.companyName = "AcmeStudio";
PlayerSettings.productName = "SpaceRunner";
PlayerSettings.SetApplicationIdentifier(NamedBuildTarget.Android, "com.acmestudio.spacerunner");
```

### IL2CPP vs Mono
IL2CPP (Intermediate Language To C++) is Unity's ahead-of-time backend: it converts IL to C++ and compiles native machine code, required on platforms that forbid JIT (iOS, most consoles, WebGL) and default/required for Android and console store submissions. It yields smaller, faster runtime output at the cost of much longer build times, and — critically — it requires a native toolchain for the *target* platform and generally cannot cross-compile: building a macOS IL2CPP player requires a macOS Editor host, an iOS IL2CPP player requires macOS + Xcode, etc. The one documented exception is Linux, which supports cross-compiling from other desktop hosts (`Manual/linux-il2cpp-crosscompiler.html`). Mono JIT-compiles, iterates far faster locally, but is unavailable wherever the OS forbids JIT-compiled code. Use Mono for local dev-loop iteration and IL2CPP for release/store builds.

```csharp
PlayerSettings.SetScriptingBackend(NamedBuildTarget.Android, ScriptingImplementation.IL2CPP);
PlayerSettings.SetIl2CppCodeGeneration(NamedBuildTarget.Android, Il2CppCodeGeneration.OptimizeSpeed);
```

### Android Build & Signing
Unity can build Android output as an **APK** (default) or **Android App Bundle / AAB** — Google Play requires AAB, not APK, for new submissions. Toggle this via `File > Build Profiles > Android > Build App Bundle (Google Play)`, visible only when Export Project is disabled; if exporting to Android Studio instead, enable `Export Project` then `Export for App Bundle`. Release builds must be signed with a keystore (`Manual/android-keystore.html`, managed via the Keystore Manager window or Player Settings > Publishing Settings). The docs state explicitly: if you sign an app with a particular key and upload it to a store, **all future versions must be signed with the same key** — losing or rotating the keystore breaks the store update path permanently (Google Play App Signing can mitigate this if enrolled at first upload, but the local upload key still must stay consistent). Back the keystore file up outside the project directory; it is not source-control-safe to commit and is not recoverable if lost.

```
"C:\Program Files\Unity\Hub\Editor\6000.3.XXf1\Editor\Unity.exe" ^
  -batchmode -quit -nographics ^
  -projectPath "C:\path\to\Project" ^
  -buildTarget Android ^
  -executeMethod BuildScripts.BuildAndroidRelease ^
  -logFile C:\Logs\android-build.log
```
```csharp
static void BuildAndroidRelease() {
    PlayerSettings.Android.keystoreName = "Assets/release.keystore";
    PlayerSettings.Android.keystorePass = System.Environment.GetEnvironmentVariable("KEYSTORE_PASS");
    PlayerSettings.Android.keyaliasName = "release";
    PlayerSettings.Android.keyaliasPass = System.Environment.GetEnvironmentVariable("KEY_ALIAS_PASS");
    EditorUserBuildSettings.buildAppBundle = true;
    var report = BuildPipeline.BuildPlayer(new BuildPlayerOptions {
        scenes = new[] { "Assets/Scenes/Main.unity" },
        locationPathName = "Builds/Android/spacerunner.aab",
        target = BuildTarget.Android,
        options = BuildOptions.None
    });
    if (report.summary.result != UnityEditor.Build.Reporting.BuildResult.Succeeded)
        EditorApplication.Exit(1);
}
```
Never hard-code keystore passwords in a committed script — pull them from CI secrets/environment variables, as above.

### iOS Build & Provisioning
Unity does not produce a signed, submittable iOS binary directly — it exports an **Xcode project**, and signing/provisioning happens afterward in Xcode (or via `xcodebuild`/`altool`/Transporter in CI) using a matching provisioning profile and signing identity from an Apple Developer account. Building requires a macOS host with a compatible Xcode version installed (`Manual/ios-requirements-and-compatibility.html`). After export, App Store submissions must also answer the encryption-export-compliance question (`ITSAppUsesNonExemptEncryption` in Info.plist, `Manual/ios-encryption-export-regulations.html`) — most games without custom crypto declare non-exempt use as false to skip the export-compliance document requirement per submission.

```
/Applications/Unity/Hub/Editor/6000.3.XXf1/Unity.app/Contents/MacOS/Unity \
  -batchmode -quit -nographics \
  -projectPath "/path/to/Project" \
  -buildTarget iOS \
  -executeMethod BuildScripts.BuildIOS \
  -logFile ~/Logs/ios-build.log

# then, outside Unity, in CI:
xcodebuild -project Builds/iOS/Unity-iPhone.xcodeproj \
  -scheme Unity-iPhone -configuration Release \
  -archivePath build/App.xcarchive archive
xcodebuild -exportArchive -archivePath build/App.xcarchive \
  -exportPath build/export -exportOptionsPlist ExportOptions.plist
```

### WebGL Build
WebGL always uses IL2CPP (no Mono option — JIT is forbidden in the browser sandbox) and produces a Build folder containing `<name>.loader.js`, `<name>.framework.js`, `<name>.wasm`, and `<name>.data` (plus a `.symbols.json` for Release builds with Debug Symbols enabled, and a `.mem` file only for multithreaded builds). Compression method (Gzip/Brotli/Disabled) and Decompression Fallback settings change the served file extensions (`.gz`, `.br`, or `.unityweb`) and directly require matching web server MIME-type/Content-Encoding configuration (`Manual/webgl-server-configuration-code-samples.html`) — a WebGL build that "works locally" but fails on a real host is almost always a server config mismatch against the chosen compression method, not a Unity bug.

```
Unity -batchmode -quit -nographics \
  -projectPath "/path/to/Project" \
  -buildTarget WebGL \
  -executeMethod BuildScripts.BuildWebGL \
  -logFile /var/log/webgl-build.log
```
```csharp
static void BuildWebGL() {
    PlayerSettings.WebGL.compressionFormat = WebGLCompressionFormat.Brotli;
    PlayerSettings.WebGL.decompressionFallback = true;
    BuildPipeline.BuildPlayer(new BuildPlayerOptions {
        scenes = EditorBuildSettingsScene.GetActiveSceneList(EditorBuildSettings.scenes),
        locationPathName = "Builds/WebGL",
        target = BuildTarget.WebGL
    });
}
```

### CI / Batch-Mode Builds
Unity 6 documents four command-line build shapes (`Manual/build-command-line.html`): trigger with `-build` alone against `-buildTarget` or `-activeBuildProfile` (no custom script, equivalent to clicking Build in the Build Profiles window), or combine `-executeMethod` with either `-buildTarget` or `-activeBuildProfile` to run a custom scripted build. Required args are `-projectPath <path>` and `-quit`; strongly recommended args are `-batchmode`, `-logFile <path>`, and explicitly specifying `-buildTarget`/`-activeBuildProfile` — omitting the target makes Unity reuse whatever configuration was last active in the Editor, which is non-deterministic in CI. Add `-nographics` on headless build agents with no GPU/display. A hard batch-mode limitation: **you cannot build multiple targets in one invocation** — `BuildProfile.SetActiveBuildProfile`, `EditorUserBuildSettings.SwitchActiveBuildTargetAsync`, and `BuildPlayerOptions.target` all fail to take effect mid-process because a platform switch triggers an assembly reload that cannot happen while a script is running. Run one Unity process per target platform instead.

```
"C:\Program Files\Unity\Hub\Editor\6000.3.XXf1\Editor\Unity.exe" ^
  -executeMethod BuildScripts.BuildWindows64 ^
  -buildTarget StandaloneWindows64 ^
  -batchmode -quit ^
  -projectPath "C:\path\to\Project" ^
  -logFile C:\Logs\build.log
```
The `-executeMethod` target should return a non-zero **process exit code** on failure (e.g. `EditorApplication.Exit(1)` after inspecting `BuildReport.summary.result`) so CI can gate on exit status — don't rely on scraping `build.log` alone, since Unity can exit 0 even when the scripted logic threw before calling `BuildPipeline.BuildPlayer`.

### Managed Code Stripping
Managed code stripping runs the **Unity linker (UnityLinker)**, which performs static analysis over your project's managed assemblies (project scripts, packages/plugins, .NET Framework assemblies) at build time to remove unreachable classes, methods, and fields. Because it's static analysis, it cannot see code paths that only exist at runtime — anything invoked purely via reflection, `Activator.CreateInstance`, JSON-driven type lookup, or dependency injection looks "unreachable" and gets stripped unless you tell the linker to preserve it. Configure the stripping level (Minimal / Low / Medium / High — set per platform in Player Settings, `Manual/managed-code-stripping-configure.html`) and preserve specific code either with the `[Preserve]` attribute on classes/members or with a `link.xml` file at the project root (`Manual/managed-code-stripping-xml-formatting.html`) that whitelists assemblies, types, or members by name.

```xml
<!-- link.xml — preserve a type only referenced via reflection -->
<linker>
  <assembly fullname="Assembly-CSharp">
    <type fullname="MyGame.SaveData.PlayerProfile" preserve="all"/>
  </assembly>
</linker>
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Scene missing at runtime | Not added, or unchecked, in the active Build Profile's scene list; it still opens fine via the Editor's File > Open Scene, masking the problem — add it and verify load order |
| IL2CPP build fails on reflection-only types | UnityLinker's static analysis can't see reflection-only call sites and strips them; add a `link.xml` entry or `[Preserve]` attribute |
| Play Store rejects update / "app not signed with the same key" | Release re-signed with a new or regenerated keystore; always reuse and back up the original keystore file outside the repo |
| CI build "succeeds" with no output | Only `build.log` was scraped, not the process exit code; call `EditorApplication.Exit(1)` on a failed `BuildReport.summary.result` and check the exit status in CI |
| Slow local iteration | IL2CPP used for every dev cycle instead of Mono; use Mono locally, switch to IL2CPP only for release/store builds |
| Editing the wrong platform's settings | The Player Settings tab follows the active Build Profile; switch the platform/profile in `File > Build Profiles` first, then edit Player Settings |
| Batch-mode build silently uses the wrong platform | `-buildTarget`/`-activeBuildProfile` omitted from the CLI invocation; Unity falls back to whatever was last active in the Editor, which is not deterministic across CI runs |
| Attempting to build two platforms in one Unity process | `BuildProfile.SetActiveBuildProfile`/`SwitchActiveBuildTargetAsync`/`BuildPlayerOptions.target` don't take effect mid-script in batch mode because the platform switch needs an assembly reload; run one process per target instead |
| WebGL build "works locally, breaks when hosted" | Chosen Compression Method (Gzip/Brotli) or Decompression Fallback wasn't matched by the web server's MIME-type/Content-Encoding config; update server config per `Manual/webgl-server-configuration-code-samples.html` |
| IL2CPP build attempted cross-platform | IL2CPP generally requires building on a host of the same OS as the target (macOS builds need macOS, iOS needs macOS+Xcode); Linux is the only documented cross-compile exception |
| iOS build "fails to submit" after Unity build finishes | Unity only exports an Xcode project — it does not sign or archive; signing/provisioning/archiving must happen afterward in Xcode or `xcodebuild` with a matching provisioning profile |
| Company/Product Name changed mid-project | Changes the bundle identifier and `persistentDataPath`; stores treat a changed bundle ID as a new app, and existing local save files become unreachable |
| Keystore password hard-coded in a committed build script | Leaks the signing credential into version control; read it from a CI secret/environment variable instead |
| Stripping level raised late in a project without testing | High stripping can remove code only reachable via reflection/serialization that wasn't previously exercised by a QA pass; test a High-stripped build before shipping, not just Debug/Editor runs |
| Assuming "Build Settings" window still exists by that name | Unity 6 renamed it to Build Profiles (`File > Build Profiles`); old tutorials/menu paths referencing "Build Settings" are stale |

## Quick Reference

| Setting/Command | Purpose |
|---|---|
| `File > Build Profiles` | Unity 6's build configuration window (formerly "Build Settings") |
| Build Profiles > Scene List | Ordered, per-profile scene list for the build |
| Build Profiles > Switch Profile | Sets the active build target; gates which Player Settings are editable |
| Platform profile vs named Build Profile | Platform profiles share Development Build/scene data across all platforms; named profiles are independent, saved as asset files |
| Scripting Backend | Mono (fast iterate, JIT, unavailable on some platforms) vs IL2CPP (AOT, required for iOS/consoles/WebGL) |
| Managed Stripping Level | Minimal / Low / Medium / High — set per platform in Player Settings |
| `link.xml` | Whitelists assemblies/types/members the UnityLinker must not strip |
| `[Preserve]` attribute | Per-class/member alternative to `link.xml` |
| `-build` | CLI: trigger a build with no custom script (needs `-buildTarget` or `-activeBuildProfile`) |
| `-activeBuildProfile <path>` | CLI: build using a saved Build Profile asset |
| `-buildTarget <name>` | CLI: specify target platform (e.g. `StandaloneWindows64`, `Android`, `iOS`, `WebGL`) |
| `-executeMethod Class.Method` | CLI: invoke a static scripted-build method |
| `-batchmode -quit` | Headless CI build; `-quit` exits the Editor after execution |
| `-nographics` | Skip GPU/display init on headless build agents |
| `-logFile <path>` | Redirect the Editor log for CI capture |
| `-projectPath <path>` | Required: path to the Unity project being built |
| `BuildPipeline.BuildPlayer(BuildPlayerOptions)` | Scripting API entry point; returns a `BuildReport` |
| `BuildPlayerOptions.scenes/target/locationPathName/options` | Fields to populate for a scripted build |
| `BuildOptions.Development` / `.AllowDebugging` | Enables profiler/script debugging in the built player |
| `EditorUserBuildSettings.buildAppBundle` | Android: build AAB instead of APK |
| `PlayerSettings.Android.keystoreName/keystorePass/keyaliasName/keyaliasPass` | Android release signing config (read from env vars, never hard-code) |
| Android AAB requirement | Google Play requires AAB, not APK, for new submissions |
| Android keystore reuse | Must sign every release with the same key as the first upload, or the store update path breaks |
| iOS export | Unity exports an Xcode project; signing/archiving happens afterward in Xcode/`xcodebuild` |
| `ITSAppUsesNonExemptEncryption` | iOS Info.plist key answering App Store encryption-export compliance |
| WebGL output files | `.loader.js`, `.framework.js`, `.wasm`, `.data`, optional `.symbols.json`/`.mem` |
| WebGL compression | Gzip/Brotli/Disabled; must match server MIME-type/Content-Encoding config |
| `BuildReport.summary.result` | Scripted check for build success/failure; drive `EditorApplication.Exit(code)` from it |
| One process per target | Batch-mode limitation — cannot switch build target mid-process due to assembly reload |

## Advanced Notes

**Build automation pipelines.** A robust headless pipeline separates three concerns: (1) a static build script implementing `IPreprocessBuildWithContext`/`IPostprocessBuildWithContext` (`Manual/build-callbacks.html`) for environment-specific setup (injecting version numbers, swapping config files, validating required scenes are present) so this logic lives in source control rather than in ad hoc CI YAML; (2) a CLI invocation per target platform that always pins `-buildTarget`/`-activeBuildProfile` explicitly and never relies on "whatever the Editor last had active," since CI runners are frequently reused across branches/targets; (3) a post-build verification step that inspects the returned `BuildReport` (`summary.result`, `summary.totalErrors`, `summary.totalWarnings`) and fails the CI job on a non-`Succeeded` result via process exit code — treat `build.log` as a debugging artifact, not the pass/fail signal. For matrix builds (e.g. Windows + Android + WebGL from one commit), fan out to separate Unity processes/agents per target rather than trying to loop targets within a single Editor invocation, since target switching in batch mode does not reliably complete before the next script line runs (the assembly-reload constraint documented in `Manual/build-command-line.html`). Cache the Library folder between CI runs (Unity's asset database/import cache) to avoid full reimports on every build; a completely clean build should still be exercised periodically via `Manual/build-clean-build.html`'s clean-build option to catch stale-cache bugs.

**Per-platform submission requirements.** Android: Google Play requires an AAB (not APK) with a `targetSdkVersion` meeting Play's current minimum (check `Manual/android-distribution-google-play.html` and `Manual/android-setup-target-api.html` for the current floor, since Google raises this annually), signed consistently with the original upload key — enrolling in Google Play App Signing at first upload lets Google manage the *app signing* key while you retain an *upload* key, but the upload key itself must still remain stable. iOS: App Store Connect requires an archived, signed `.ipa` built from the Xcode project Unity exports, a valid distribution provisioning profile and signing certificate from an Apple Developer Program membership, and an answer to the `ITSAppUsesNonExemptEncryption` compliance question (`Manual/ios-encryption-export-regulations.html`); App Store builds also go through app-thinning/app-slicing (`Manual/ios-app-slicing.html`) which affects on-demand resource strategy for large asset sets. WebGL: there is no "app store" gate, but hosting requirements are strict — the serving origin must send correct `Content-Encoding`/MIME types matching the chosen compression method (`Manual/webgl-server-configuration-code-samples.html`), and browser memory limits mean large WebGL builds need `Decompression Fallback` or streaming considerations validated before shipping. Across all platforms, run a store-representative build (correct stripping level, correct scripting backend, Development Build disabled) through at least a smoke-test pass before submission — the common failure mode is a build that only ever ran as an Editor Play session or an unstripped Development Build, which behaves differently than the stripped IL2CPP Release artifact a store actually receives.

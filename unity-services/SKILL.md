---
name: unity-services
description: Use when integrating Unity Gaming Services — Cloud Save, Cloud Code, Economy, Remote Config, Analytics, Authentication, In-App Purchasing, Ads, Vivox, or Cloud Build/DevOps. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Gaming Services

Local doc coverage for Unity Gaming Services (UGS) is thin and lopsided: these services are administered through Unity's separate services/Dashboard docs site (docs.unity.com), not through the docs.unity3d.com Manual+ScriptReference download that this skill is grounded in. The large majority of what exists locally under `Manual/com.unity.services.*` are package-landing stubs — roughly 190-260 words of real body text each (Description, Version information, Compatible-with tables), useful for confirming a package name/version/compatibility but containing no API walkthroughs, no setup steps, and no code samples.

There are two real exceptions to that pattern, both re-verified in this pass:

1. **In-App Purchasing** has genuine step-by-step Manual content, but it is scoped specifically to the Windows/Xbox (Microsoft GDK) platform integration path — `windows-iap-implementation.html`, `windows-iap-verification.html`, `windows-iap-partner-center-unity-setup.html`, `xbox-games-on-windows-iap.html` — plus two ScriptReference pages for the Editor-only `UnityEditor.Purchasing.PurchasingSettings` class. There is **no** local documentation for the cross-platform runtime API (`IStoreListener`, `ConfigurationBuilder`, `Product`, `IExtensionProvider`, etc.) — that lives only on docs.unity.com.
2. **Unity Building Blocks** — a previously-missed category in this skill — are real procedural integration guides (700-1400+ words each) for Asset-Store-delivered sample scenes that demonstrate specific UGS features end to end: Player Accounts, Leaderboards, Achievements, Multiplayer Sessions, Matchmaker, and Vivox. These pages name the exact UGS packages each Building Block depends on and walk through Dashboard linking, environment setup, identity-provider configuration, and asset-store install steps. They are the closest thing to real procedural UGS documentation in this local doc set outside of IAP.

Treat **Key Guidelines** below as the primary source for API-level guidance; use retrieval only to confirm package identity/version or to walk a user through the Windows/Xbox IAP flow or a Building Block setup.

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Cloud Save stub | `Manual/com.unity.services.cloudsave.html` | Package identity/version (v3.x line for 6000.3). One-paragraph description: per-player key/value cloud persistence. No API detail. |
| Cloud Code stub | `Manual/com.unity.services.cloudcode.html` | Package identity/version only. No API detail. |
| Economy stub | `Manual/com.unity.services.economy.html` | Package identity/version only. No API detail. |
| Authentication stub | `Manual/com.unity.services.authentication.html` | Package identity/version only. No API detail. |
| Cloud Build stub | `Manual/com.unity.services.cloud-build.html` | Package identity/version only. No API detail. |
| Remote Config stub (fuller) | `Manual/com.unity.remote-config.html` (386 words — the fullest of the plain stubs) | Feature list with real substance: feature flagging, staged rollouts, kill switches, start/end dates, enable/disable rules, personalized targeting. Still no code samples. |
| Analytics stub | `Manual/com.unity.services.analytics.html` | Package identity/version only. No API detail. |
| Vivox package stub | `Manual/com.unity.services.vivox.html` | Package identity/version only — see Vivox Building Block pages below for the real setup walkthrough. |
| Ads Mediation / LevelPlay stub (semi-real) | `Manual/com.unity.services.levelplay.html` (424 words) | Not pure landing-page text — contains real, actionable **v9.0.0+ migration notes** (delete `Assets/LevelPlay`/`Assets/IronSource` folder, delete `Assets/Mobile Dependency Resolver` if present, re-enter Developer Settings values after upgrade). Cite this directly for anyone migrating an old Ads Mediation/IronSource integration. |
| Legacy Ads stub (deprecation notice) | `Manual/com.unity.ads.html` | States verbatim: **"as of January 31, 2026, the Unity Ads network is completing its transition to in-app bidding and direct integration of the Unity Ads SDK using this package for monetization purposes is no longer supported."** Points readers to the LevelPlay package-integration and migrate-from-Unity-Ads-to-LevelPlay docs on docs.unity.com. |
| Ads iOS support stub | `Manual/com.unity.ads.ios-support.html` | Package identity/version only, iOS-specific companion to the legacy Ads package. |
| In-App Purchasing package stub | `Manual/com.unity.purchasing.html` | Package identity/version plus a real feature-bullet list (single API across Apple App Store/Google Play, automatic Unity Analytics revenue integration). Still landing-page depth, not a walkthrough. |
| **IAP: implement (real Manual content)** | `Manual/windows-iap-implementation.html` (772 words) | "Implement in-app purchases with Unity IAP APIs" — real step-by-step Windows/Xbox purchase-flow implementation guide. |
| **IAP: verify (real Manual content)** | `Manual/windows-iap-verification.html` (445 words) | "Verify in-app purchases with Cloud Code" — ties IAP directly to Cloud Code server-side verification; the strongest local evidence for the "don't trust client-side receipt validation alone" guideline. |
| **IAP: Partner Center setup (real Manual content)** | `Manual/windows-iap-partner-center-unity-setup.html` (663 words) | "Set up Microsoft Partner Center and Unity Editor" — real account/App-linking setup steps for the Microsoft Store side of Windows/Xbox IAP. |
| **IAP: Xbox overview (real Manual content)** | `Manual/xbox-games-on-windows-iap.html` (345 words) | "In-app purchases for Xbox games on Windows" — parent/landing page for the three pages above; links them together as one flow. |
| Purchasing ScriptReference: PurchasingSettings | `ScriptReference/Purchasing.PurchasingSettings.html` | **`UnityEditor.Purchasing.PurchasingSettings`** — an Editor-only class ("Editor API for the Unity Services editor feature") that toggles whether IAP is enabled for the project. Not the runtime purchase API. |
| Purchasing ScriptReference: PurchasingSettings.enabled | `ScriptReference/Purchasing.PurchasingSettings-enabled.html` | The single documented member of the class above — a bool property that enables/disables the IAP service flag from an Editor script. |
| Leaderboards package stub | `Manual/com.unity.services.leaderboards.html` | Package identity/version (v2.3.4 line). Real one-line description: store/sort/rank player scores. See Building Block page below for the actual walkthrough. |
| Player Accounts package stub | `Manual/com.unity.services.playeraccounts.html` | Package identity/version (v1.0.0-pre.2, still pre-release as of this doc snapshot). See Building Block page below for the actual walkthrough. |
| Friends package stub | `Manual/com.unity.services.friends.html` | Package identity/version (v1.1.1). Real one-line description: friend requests, friends lists, block/unblock. No local walkthrough exists for this one. |
| CCD Management package stub | `Manual/com.unity.services.ccd.management.html` | Cloud Content Delivery — package identity/version (v3.0.3). Notes it's consumed by Addressables (1.19.6+) for CCD-backed remote content, not typically used standalone. |
| User Reporting package stub | `Manual/com.unity.services.user-reporting.html` | Package identity/version (v2.0.15). SDK for collecting in-app user/tester bug reports. |
| Deployment package stub | `Manual/com.unity.services.deployment.html` | Package identity/version. Describes itself as shared plumbing (the Deployment Window) that other UGS packages build on for pushing configuration — not used standalone. |
| Moderation package stub | `Manual/com.unity.services.moderation.html` | Package identity/version (v1.0.2). Toxicity-management/community-health tooling. |
| Cloud Diagnostics package stub | `Manual/com.unity.services.cloud-diagnostics.html` | Package identity/version (v1.0.12). Crash/exception collection SDK, sibling to User Reporting. |
| Services Tooling package stub | `Manual/com.unity.services.tooling.html` | Package identity/version (v1.4.1). Provides Config-as-Code editor integration for services without their own package (currently Access Control, Game Overrides). |
| Cloud Services APIs package stub | `Manual/com.unity.services.apis.html` | Package identity/version (v1.1.1). Low-level admin+client API access shared by other UGS packages; not something you call directly in normal usage. |
| Push Notifications package stub | `Manual/com.unity.services.push-notifications.html` | Package identity/version (v4.0.2). Rich push notifications (images) plus delivery analytics. |
| **Building Blocks: index (real Manual content)** | `Manual/building-blocks.html` (250 words) | Top-level index page linking Introduction, LiveOps, and Multiplayer Building Block families. |
| **Building Blocks: introduction (real Manual content)** | `Manual/building-blocks-introduction.html` (334 words) | Explains what a Building Block is (Asset-Store-delivered sample scene + scripts demonstrating a feature) and the general install/add-to-project flow. |
| **Building Blocks: LiveOps overview (real Manual content)** | `Manual/building-blocks-liveops.html` (264 words) | Index for the Achievements, Player Accounts, and Leaderboards Building Blocks. |
| **Building Blocks: LiveOps prerequisites (real Manual content)** | `Manual/building-blocks-liveops-prerequisites.html` (337 words) | Real setup steps: link project to Unity Dashboard, set a target environment, set up deployment — required before any LiveOps Building Block works. |
| **Building Blocks: Player Accounts (real Manual content)** | `Manual/building-blocks-liveops-player-accounts.html` (1442 words — deepest page in this doc set) | Full walkthrough: install the Player Accounts Building Block from the Asset Store, configure an identity provider, wire up the test scene. Explicitly names the three UGS services it depends on — **Authentication** (unique identity), **Cloud Save** (player/game data with access-class control), **Cloud Code** (server authority to write protected/private data). |
| **Building Blocks: Leaderboards (real Manual content)** | `Manual/building-blocks-liveops-leaderboard.html` (1209 words) | Full walkthrough for score-keeping + leaderboard display backed by the Leaderboards UGS service; scores stored as cloud player data. |
| **Building Blocks: Achievements (real Manual content)** | `Manual/building-blocks-liveops-achievements.html` (1405 words) | Full walkthrough for a cloud-synced achievement system usable across devices/platforms on the same project. |
| **Building Blocks: Multiplayer overview (real Manual content)** | `Manual/building-blocks-multiplayer.html` (235 words) | Index for the Multiplayer Sessions and Matchmaker Building Blocks. |
| **Building Blocks: Multiplayer prerequisites (real Manual content)** | `Manual/building-blocks-multiplayer-prerequisites.html` (708 words) | Real Dashboard-linking/organization-access setup steps required before either multiplayer Building Block works. |
| **Building Blocks: Multiplayer Sessions (real Manual content)** | `Manual/building-blocks-multiplayer-sessions.html` (1309 words) | Full walkthrough: pre-made UI for creating/joining sessions, debugging them, and connecting to Unity's Netcode packages. |
| **Building Blocks: Matchmaker (real Manual content)** | `Manual/building-blocks-multiplayer-matchmaking.html` (1292 words) | Full walkthrough: pre-made UI for Matchmaker queues and wiring matched results into Netcode. |
| **Building Blocks: Vivox overview (real Manual content)** | `Manual/building-blocks-vivox.html` (231 words) | Index/landing for the Vivox Building Block; links prerequisites and the comms walkthrough. |
| **Building Blocks: Vivox prerequisites (real Manual content)** | `Manual/building-blocks-vivox-prerequisites.html` (895 words) | Real setup steps: Unity project version, Dashboard/organization access, Editor sign-in permissions, before the Vivox Building Block can be used. |
| **Building Blocks: Vivox comms (real Manual content)** | `Manual/building-blocks-vivox-comms.html` (1297 words) | Full walkthrough: pre-made chat/roster UI, connecting players to Vivox channels, Editor interface tour. |

Total: 41 distinct on-disk paths cited above, spanning 28 stub-depth pages and 13 real-content pages (4 IAP Manual pages + 2 IAP ScriptReference pages + 7 Building Blocks real pages, counting each of the three LiveOps feature walkthroughs, both Multiplayer walkthroughs, and the Vivox comms/prerequisites pair separately from their index/prerequisite pages — see row-by-row "real" markers above for the exact 13). No `Unity.Services.*` runtime namespace (Authentication, CloudSave, CloudCode, Economy, RemoteConfig, Analytics, Vivox client SDK, Leaderboards client SDK, etc.) has any ScriptReference page in this local doc set — confirmed by an exhaustive `find` across `ScriptReference/` for `*Services*`, `*Purchasing*`, `IStoreListener`, `IAPButton`, which returned only unrelated matches (`LocationServiceStatus`, `MPE.ProcessService`) plus the two `Purchasing.PurchasingSettings` pages already listed.

Verified via `find`/`grep` against `Manual/` and `ScriptReference/` under `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/`, then stripping HTML tags and measuring stripped-text word counts to distinguish stub-depth pages (~190-260 words) from real procedural content (330-1450+ words), then spot-reading the real pages' opening paragraphs to confirm subject matter matches the filename.

## Key Guidelines

### Authentication & `UnityServices.InitializeAsync`

Every UGS feature sits behind one Unity Dashboard project, identified by a Project ID that's linked into the Unity Editor (Edit > Project Settings > Services, or the Services window). Before touching any other service API, a client must (1) call `UnityServices.InitializeAsync()` to bring up the UGS runtime and bind it to the linked project/environment, then (2) sign in through Authentication — anonymous sign-in for the simplest case, or a federated provider (Apple, Google, Steam, Facebook, etc.) when persistent cross-device identity matters. Skipping step 1 or 2 causes other service calls to throw or silently no-op. Initialization and sign-in are async and should run once per app session at startup, with the rest of the game gated behind their completion (a splash/loading screen is the common pattern). None of this works offline — even in the Editor, first-time initialization requires connectivity to reach Unity's servers.

```csharp
using System;
using System.Threading.Tasks;
using Unity.Services.Authentication;
using Unity.Services.Core;
using UnityEngine;

public class UgsBootstrap : MonoBehaviour
{
    async void Start()
    {
        try
        {
            await UnityServices.InitializeAsync();

            AuthenticationService.Instance.SignedIn += () =>
                Debug.Log($"Signed in. PlayerId: {AuthenticationService.Instance.PlayerId}");

            if (!AuthenticationService.Instance.IsSignedIn)
            {
                await AuthenticationService.Instance.SignInAnonymouslyAsync();
            }
        }
        catch (Exception e)
        {
            // AuthenticationException / RequestFailedException — no network,
            // Dashboard project not linked, or environment misconfigured.
            Debug.LogError($"UGS bootstrap failed: {e}");
        }
    }
}
```

This pattern (namespaces, method names, async signatures) reflects Unity's published UGS SDK conventions — it is **not** grounded in this local doc set, since no `Unity.Services.*` ScriptReference page exists locally. Verify exact signatures against the current package version on docs.unity.com or via the in-editor Services window before shipping.

### Cloud Save

Per-player key/value persistence — progress, preferences, unlocked content — synced across a signed-in player's devices. It is not a general-purpose database: there's no querying across players, no relational structure, and payload sizes are capped. Cloud Save distinguishes Player Data (owned/writable by the player's client) from Game Data (server-side, typically written only via Cloud Code) and supports "protected" access classes so a client can read but not write certain keys. The local `Manual/com.unity.services.cloudsave.html` page is a version/identity stub only; the real access-class model is documented in the Player Accounts Building Block page (`Manual/building-blocks-liveops-player-accounts.html`), which uses Cloud Save to store both player and protected game data as part of its test-scene walkthrough.

### Cloud Code

Server-authoritative logic, invoked from the client via an RPC-style call but executed in Unity's cloud — the mechanism for anything a client cannot be trusted to compute honestly: granting currency/items, validating a purchase receipt, resolving a match result, writing to protected Cloud Save keys or private Economy state. Cloud Code modules are written and deployed separately from the client build (via the Dashboard or CLI/CI), so shipping a Cloud Code change does not require a client update — useful for balance/anti-cheat fixes post-launch. The two real local pages that exercise Cloud Code are `Manual/windows-iap-verification.html` ("Verify in-app purchases with Cloud Code" — server-side receipt verification) and `Manual/building-blocks-liveops-player-accounts.html` (Cloud Code as the write path for protected/private Cloud Save data).

### Economy

Server-modeled virtual currencies and inventory (items, bundles, stores). The client reads balances/catalog through the Economy SDK, but any mutation that affects fairness — granting currency, crafting, purchase fulfillment — should route through Cloud Code rather than a direct client-side Economy write, for the same reason as Cloud Save: a modified client can call the same API. Economy integrates with IAP so a successful, server-verified purchase can trigger a currency/item grant in one authoritative path. No real local content exists for Economy beyond the `Manual/com.unity.services.economy.html` identity stub.

### Remote Config

Live-tunable values delivered without a client update: feature flags, staged rollouts, kill switches, event start/end dates, difficulty tuning, and audience-targeted rules/campaigns. Anything a live-ops team might want to change after ship — not just balance numbers but whether a feature is visible at all — belongs in Remote Config rather than hardcoded in the client. `Manual/com.unity.remote-config.html` is the fullest of the plain landing stubs (386 words) and is worth citing directly for its feature list, but it still stops short of API/code detail — no local page shows the client-side fetch/apply pattern.

### Analytics

Telemetry, funnels, and custom events. Analytics integrates automatically with Unity IAP for revenue tracking (per `Manual/com.unity.purchasing.html`'s own feature bullets) — a successful purchase flows into Analytics without extra wiring once both packages are installed and initialized. No real local walkthrough exists beyond the identity stub at `Manual/com.unity.services.analytics.html`.

### In-App Purchasing

This is the one UGS-adjacent area with genuine local depth, and it is worth walking through in full because the four real Manual pages form one coherent flow — but that flow is scoped specifically to **Windows and Xbox games built with the Microsoft GDK**, not the general Apple/Google IAP path.

The flow, in the order the docs present it:

1. **`Manual/xbox-games-on-windows-iap.html`** — the landing/overview page for "In-app purchases for Xbox games on Windows." Sets context: this is a Microsoft GDK Windows-games track, distinct from mobile IAP.
2. **`Manual/windows-iap-partner-center-unity-setup.html`** ("Set up Microsoft Partner Center and Unity Editor") — the account-side prerequisite. Create/configure the app listing and add-on/product entries in Microsoft Partner Center, then link that configuration into the Unity Editor project so the IAP package knows which store products exist.
3. **`Manual/windows-iap-implementation.html`** ("Implement in-app purchases with Unity IAP APIs") — the client-side implementation step: initializing the Unity IAP package against the Partner-Center-configured products and driving the actual purchase flow from game code.
4. **`Manual/windows-iap-verification.html`** ("Verify in-app purchases with Cloud Code") — the server-side step: taking the receipt/transaction produced by step 3 and validating it through a Cloud Code module before granting the purchased content, rather than trusting the client's own "purchase succeeded" callback.

That four-step shape — configure the store, implement the client purchase call, verify server-side — is the strongest evidence in this entire local doc set for the general guideline "pair client-side IAP with Cloud Code verification for anything valuable," and it should be cited as such even when a user's actual target platform is iOS/Android rather than Windows/Xbox, since the core store-config → client-purchase → server-verify shape carries over.

The package-level page, `Manual/com.unity.purchasing.html`, adds two real feature bullets worth quoting: a single API surface across the Apple App Store and Google Play Store (and, per the flow above, Microsoft Store), and automatic integration with Unity Analytics for revenue monitoring.

On the ScriptReference side, the **only** two IAP-related pages that exist locally are for `UnityEditor.Purchasing.PurchasingSettings` — an Editor-only class, not the runtime purchase API:

```csharp
// Editor-only script. UnityEditor.Purchasing.PurchasingSettings toggles whether
// the IAP service is enabled for this project, mirroring the checkbox in
// Project Settings > Services > In-App Purchasing.
// Grounded in ScriptReference/Purchasing.PurchasingSettings.html and
// ScriptReference/Purchasing.PurchasingSettings-enabled.html — this is the
// only Purchasing API documented in this local doc set.
using UnityEditor;
using UnityEditor.Purchasing;
using UnityEngine;

public static class TogglePurchasingService
{
    [MenuItem("Tools/UGS/Enable In-App Purchasing")]
    static void EnableIap()
    {
        PurchasingSettings.enabled = true;
        Debug.Log($"Unity IAP service enabled: {PurchasingSettings.enabled}");
    }
}
```

The runtime purchase API — `IStoreListener`, `ConfigurationBuilder`, `Product`, `IExtensionProvider`, `CodelessIAPStoreListener`, `IAPButton` — has **no** local ScriptReference page at all (confirmed by an exhaustive search of `ScriptReference/` for those symbols and for `*Purchasing*`/`*Services*` generally). Any code sample using those types is drawn from general Unity IAP knowledge, not this local doc set, and should be flagged as such; verify current signatures on docs.unity.com before shipping.

### Ads / LevelPlay Mediation

As of **January 31, 2026**, the legacy `com.unity.ads` package's direct-SDK-integration path is no longer supported for monetization — this is stated verbatim in `Manual/com.unity.ads.html`: "the Unity Ads network is completing its transition to in-app bidding and direct integration of the Unity Ads SDK using this package for monetization purposes is no longer supported." New and existing monetization integrations should use the **Ads Mediation package for LevelPlay** instead (`com.unity.services.levelplay`). The LevelPlay package's own local page (`Manual/com.unity.services.levelplay.html`) is unusually substantive for a "stub" — it carries real, actionable migration notes for anyone upgrading an existing Ads Mediation/IronSource integration to v9.0.0+: back up any Developer Settings values first, then delete the `Assets/LevelPlay` (or legacy `Assets/IronSource`) folder and the `Assets/Mobile Dependency Resolver` folder if present, before reinstalling and re-entering the Developer Settings values. Treat any request to add new Unity Ads monetization as a signal to steer toward LevelPlay, and treat any existing `com.unity.ads`-based integration as a migration candidate, not a bug to preserve.

### Vivox

Voice and text chat channels — a separate session/connection from gameplay netcode, requiring its own initialization and channel-join calls. The package identity stub (`Manual/com.unity.services.vivox.html`) carries no real setup detail, but the **Vivox Building Block** pages do: `Manual/building-blocks-vivox-prerequisites.html` (895 words — Dashboard/organization access, Editor sign-in permissions, minimum Unity version) and `Manual/building-blocks-vivox-comms.html` (1297 words — pre-made chat/roster UI components, connecting players to channels, an Editor-interface tour of the sample scene). These are Asset-Store-delivered sample-scene walkthroughs, not raw SDK API documentation, but they're the most concrete local material on Vivox integration shape (channel join/leave flow, UI wiring) available in this doc set.

### Cloud Build / Unity DevOps

Cloud-hosted CI triggered from source-control pushes — build configs targeting specific platforms, kicked off remotely rather than on a local machine. The only local coverage is the identity/version stub at `Manual/com.unity.services.cloud-build.html`; no setup or pipeline-configuration detail exists locally. Related plumbing packages worth knowing about when a Cloud Build/DevOps question comes up: `com.unity.services.deployment` (shared config-deployment tooling other UGS packages build on, per `Manual/com.unity.services.deployment.html`) and `com.unity.services.tooling` (Config-as-Code editor integration for services like Access Control and Game Overrides, per `Manual/com.unity.services.tooling.html`).

### Other UGS packages with only identity-stub coverage

Leaderboards (`Manual/com.unity.services.leaderboards.html`, v2.3.4 — but see the real Building Block walkthrough at `Manual/building-blocks-liveops-leaderboard.html`), Player Accounts (`Manual/com.unity.services.playeraccounts.html`, v1.0.0-pre.2, still pre-release — but see `Manual/building-blocks-liveops-player-accounts.html`), Friends (`Manual/com.unity.services.friends.html`, v1.1.1 — friend requests/lists/block-unblock, no walkthrough exists), CCD Management (Cloud Content Delivery, consumed by Addressables 1.19.6+ rather than used standalone), User Reporting and Cloud Diagnostics (bug-report and crash-collection SDKs), Moderation (toxicity/community-health tooling), Push Notifications (rich push + delivery analytics), and Cloud Services APIs (low-level shared API layer other packages sit on). All are real, shippable UGS packages — just undocumented locally beyond name/version/one-paragraph description.

## Common Mistakes

| Mistake | Why / fix |
|---------|-----------|
| Trusting client-side Cloud Save/Economy writes for fairness-affecting state | A modified client can write too — validate/mutate via Cloud Code instead. |
| Calling a service API before `InitializeAsync()`/sign-in completes | Throws or no-ops — initialize and authenticate every session before touching Cloud Save, Economy, Remote Config, etc. |
| Hardcoding values Remote Config exists to avoid hardcoding | Defeats over-the-air tuning; anything live-ops might change post-launch (flags, difficulty, event windows) belongs in Remote Config. |
| Assuming full UGS API docs live in the local Unity Manual | They mostly don't — this local set is landing stubs plus IAP Windows/Xbox pages and Building Blocks walkthroughs; lean on Key Guidelines and docs.unity.com for anything else. |
| Using legacy `com.unity.ads` for new monetization | Unsupported for monetization as of January 31, 2026 — use the Ads Mediation package for LevelPlay instead. |
| Upgrading Ads Mediation to v9.0.0+ without the folder cleanup | Leaves stale `Assets/LevelPlay`/`Assets/IronSource` and `Assets/Mobile Dependency Resolver` folders that break the upgrade — delete them first and back up Developer Settings values, per `Manual/com.unity.services.levelplay.html`. |
| Relying solely on IAP's client-side purchase callback as proof of purchase | First line of defense only — pair with Cloud Code server-side receipt verification, per the Windows/Xbox IAP flow (`windows-iap-implementation.html` → `windows-iap-verification.html`). |
| Assuming `UnityEditor.Purchasing.PurchasingSettings` is the runtime purchase API | It's an Editor-only toggle for enabling the IAP service — the runtime `IStoreListener`/`ConfigurationBuilder`/`Product` API has no local ScriptReference page at all. |
| Treating a Building Block as production-ready code to ship as-is | Building Blocks are Asset-Store sample scenes meant to demonstrate a feature and be adapted, not drop-in production systems — follow their prerequisites pages (Dashboard link, environment, deployment) before relying on them. |
| Skipping a Building Block's `*-prerequisites` page | Every Building Block family (LiveOps, Multiplayer, Vivox) has a dedicated prerequisites page (Dashboard linking, organization access, Editor sign-in permissions) that must be done first — the feature-specific walkthrough assumes it. |
| Assuming Economy, Leaderboards, Friends, or Vivox client SDKs have local API docs | None do — only package-identity stubs exist for these; verify exact method signatures on docs.unity.com or in-editor. |
| Confusing Cloud Diagnostics/User Reporting (crash/bug-report collection) with Analytics (gameplay telemetry) | Different packages, different purposes — don't assume one covers the other. |
| Assuming CCD (Cloud Content Delivery) or Deployment/Tooling packages are used standalone | CCD is normally driven through Addressables (1.19.6+); Deployment and Tooling are shared plumbing other UGS packages build on, not typically called directly. |
| Working entirely offline and expecting UGS to function | Every service, including first-time Editor initialization, requires connectivity to Unity's servers — nothing here works purely offline. |
| Assuming a package version referenced in an older tutorial matches what's installed | Package versions move independently of Unity Editor version — confirm the installed version via Package Manager or the local stub's "Version information" section (e.g., Player Accounts is still pre-release at v1.0.0-pre.2 as of this doc snapshot) rather than assuming GA status. |

## Quick Reference

| Service / Package | Local coverage | Purpose |
|---|---|---|
| Authentication | Identity stub only | Sign-in (anonymous/federated) gating every other UGS service. |
| Cloud Save | Identity stub only | Per-player key/value persistence, synced across devices; Player Data vs. protected Game Data. |
| Cloud Code | Identity stub + real IAP verification page + Building Block usage | Server-authoritative logic invoked from the client, run in the cloud. |
| Economy | Identity stub only | Server-modeled virtual currencies and inventory. |
| Remote Config | Fuller stub (386 words) | Live-tunable flags/values/campaigns without a client update. |
| Analytics | Identity stub only | Telemetry, funnels, custom events; auto-integrates with IAP for revenue. |
| Unity IAP (`com.unity.purchasing`) | **Real Manual walkthrough (Windows/Xbox) + 2 ScriptReference pages (Editor-only toggle)** | Unified store billing API (Apple, Google, Microsoft); runtime API undocumented locally. |
| Ads / LevelPlay Mediation | Semi-real stub (migration notes) | Mediated ad monetization; supersedes legacy `com.unity.ads` (unsupported for monetization since Jan 31, 2026). |
| Vivox | Identity stub + real Building Block walkthrough | Voice/text chat channels, separate session from gameplay netcode. |
| Cloud Build / Unity DevOps | Identity stub only | Cloud CI, build triggers from source control. |
| Leaderboards | Identity stub + real Building Block walkthrough | Store/sort/rank player scores. |
| Player Accounts | Identity stub (pre-release) + real Building Block walkthrough | Sign-in/identity building block combining Authentication + Cloud Save + Cloud Code. |
| Achievements (via Building Block) | No standalone package stub; real Building Block walkthrough only | Cloud-synced achievement system, cross-device/platform. |
| Friends | Identity stub only | Friend requests, friends lists, block/unblock. |
| Multiplayer Sessions | Real Building Block walkthrough | Pre-made UI for creating/joining sessions, connects to Netcode. |
| Matchmaker | Real Building Block walkthrough | Pre-made UI for matchmaking queues, connects to Netcode. |
| CCD (Cloud Content Delivery) | Identity stub only | Hosts/delivers live remote content; normally driven via Addressables. |
| User Reporting | Identity stub only | SDK for collecting user/tester bug reports in-app. |
| Cloud Diagnostics | Identity stub only | Cloud-based crash/exception collection. |
| Moderation | Identity stub only | Toxicity management / community-health tooling. |
| Push Notifications | Identity stub only | Rich push notifications with images, plus delivery analytics. |
| Deployment | Identity stub only | Shared config-deployment plumbing other UGS packages build on. |
| Services Tooling | Identity stub only | Config-as-Code editor integration (Access Control, Game Overrides). |
| Cloud Services APIs | Identity stub only | Low-level shared admin/client API layer underlying other packages. |
| Unity Building Blocks (meta-category) | Real Manual index + introduction pages | Asset-Store sample scenes demonstrating a UGS feature end to end; starting points, not production code. |

## Advanced Notes

Every service above is scoped by one **Unity Dashboard project**, identified by a **Project ID**, and further scoped by **environments** (e.g., production vs. development) within that project. This linkage is what `UnityServices.InitializeAsync()` resolves at runtime: the Editor's linked project/organization (Project Settings > Services, or the Services window) determines which Dashboard project's Cloud Save data, Economy catalog, Remote Config values, Cloud Code modules, and IAP product configuration a given build talks to. Getting this wrong — e.g., a build still linked to a development environment shipping to players, or a project not linked at all — is a common source of "it works in the Editor but not in the build" UGS bugs, and is usually the first thing to check when a service call fails silently or throws on startup.

Because this local doc set mirrors only the Unity Editor Manual/ScriptReference (docs.unity3d.com) and not the services documentation site, nearly everything below the package-identity level — client SDK method signatures for Cloud Save/Cloud Code/Economy/Remote Config/Analytics/Vivox/Leaderboards/Friends, Dashboard UI walkthroughs, Cloud Code module authoring/deployment, Economy catalog configuration, Remote Config rule/campaign authoring, and the full (non-Windows/Xbox) IAP runtime API — lives at **docs.unity.com** instead, organized by service (e.g., `docs.unity.com/en-us/cloud-code`, `docs.unity.com/en-us/economy`, `docs.unity.com/en-us/grow/levelplay/...`) rather than by Unity version. The in-editor **Services window** (Window > Services, or the Project Settings > Services tab) is the fastest way to confirm current linkage state, installed package versions, and jump directly to the relevant docs.unity.com page for whichever service is open. When a user's question goes past what's cited in Retrieval Sources above, say so explicitly and point to docs.unity.com rather than guessing at an API shape from pretrained knowledge.

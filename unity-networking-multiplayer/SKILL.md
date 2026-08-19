---
name: unity-networking-multiplayer
description: Use when implementing Unity multiplayer — Netcode for GameObjects, Relay/Lobby services, or NetworkBehaviour/client-server patterns. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Networking & Multiplayer

## Retrieval Sources

All paths are relative to `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/`. Re-verified on disk for this pass via `find`/`grep` over the full `Manual/` (3,476 HTML files) and `ScriptReference/` (35,594 HTML files) trees.

| # | Source | Path | Use for |
|---|--------|------|---------|
| 1 | NGO package landing page | `Manual/com.unity.netcode.gameobjects.html` | Confirms package identity/version: "Netcode for GameObjects is a high-level netcode SDK that provides networking capabilities to GameObject/MonoBehaviour workflows... sits on top of underlying transport layer." Version `2.13.1` for Editor `6000.3`. Stub page only — no API detail, no ScriptReference entries ship in this doc set. |
| 2 | Netcode for Entities package landing page | `Manual/com.unity.netcode.html` | **Do not confuse with NGO.** Separate DOTS/ECS-based netcode stack (`com.unity.netcode`, package name "Netcode for Entities", v`1.14.1`). Cited only to positively rule it out as the GameObject-workflow answer. |
| 3 | Unity Transport package landing page | `Manual/com.unity.transport.html` | Confirms the low-level transport NGO rides on: "the low-level interface for connecting and sending data through a network." Package `com.unity.transport`, v`2.7.4`. Stub page — no `NetworkDriver`/`NetworkConnection` API detail locally. |
| 4 | Multiplayer overview | `Manual/multiplayer.html` | Entry point; explicitly routes readers to the Multiplayer Center and to "the Unity Multiplayer documentation" (external, docs-multiplayer.unity3d.com) for NGO/Relay/Lobby specifics. |
| 5 | Multiplayer Center package page | `Manual/com.unity.multiplayer.center.html` | "Provides a starting point to create multiplayer games... recommend[s] specific packages and enable[s] you to easily access integrations, samples and documentation." Package recommender wizard, not an API reference. |
| 6 | Building Blocks overview | `Manual/building-blocks.html` | Index of all Building Blocks families: LiveOps (Achievements/Player Accounts/Leaderboards), Multiplayer Services, Vivox. |
| 7 | Building Blocks introduction | `Manual/building-blocks-introduction.html` | Explains Building Blocks are downloadable Asset Store packages (assets + scripts + a test scene), not part of the base Editor install. |
| 8 | Multiplayer Services Building Blocks index | `Manual/building-blocks-multiplayer.html` | Two sub-blocks: Multiplayer Sessions Building Block, Matchmaker Session Building Block. |
| 9 | Multiplayer Services Building Blocks prerequisites | `Manual/building-blocks-multiplayer-prerequisites.html` | Setup: Unity 6.0 LTS+ recommended, Unity Dashboard org access, Editor sign-in with org/project permissions, network access to Unity Services endpoints. Documents the `SessionSettings` ScriptableObject as the central config hub and its `SessionType` string as the primary session-management identifier. |
| 10 | Multiplayer Sessions Building Block | `Manual/building-blocks-multiplayer-sessions.html` | Session create/join via the Multiplayer Services SDK, Quick Join API, Session Browsing API, JoinCode sharing. Ships example scenes, UI Toolkit assets, a `SessionInfo`-style debug view, and explicitly documents connecting sessions to "Unity's Netcode packages." Closest local analog to Lobby concepts. |
| 11 | Matchmaker Session Building Block | `Manual/building-blocks-multiplayer-matchmaking.html` | Matchmaker Queue configuration (managed from the Unity project, deployed to cloud services for Play Mode), rule-based player pairing, optional NGO hookup for gameplay sync after a match forms. |
| 12 | Multiplayer Play Mode package page | `Manual/com.unity.multiplayer.playmode.html` | "Enables multiple editor instances to be opened simultaneously on the same development device." Package `com.unity.multiplayer.playmode`, v`2.0.2` for Editor `6000.3`. |
| 13 | Multiplayer Tools package page | `Manual/com.unity.multiplayer.tools.html` | "Adds a suite of tools that improve workflows for multiplayer development." Package `com.unity.multiplayer.tools`, v`2.2.10` for Editor `6000.3`. Keywords tag: Multiplayer, Netcode, GameObjects, Tools. |
| 14 | Web networking (WebGL/Web platform) | `Manual/webgl-networking.html` | Authoritative local source on browser networking constraints — see WebGL subsection below for full extraction. |
| 15 | Dedicated Server hub | `Manual/dedicated-server.html` | Index for the entire Dedicated Server platform documentation set (a genuinely deep, real local doc tree — see Host vs Dedicated Server section). |
| 16 | Dedicated Server introduction | `Manual/dedicated-server-introduction.html` | Defines Dedicated Server as a Desktop sub-target (Linux/macOS/Windows) optimized to cut CPU/memory/disk by stripping rendering- and audio-only assets and code from headless server builds. |
| 17 | Dedicated Server package page | `Manual/com.unity.dedicated-server.html` | Package `com.unity.dedicated-server`, v`2.0.2`. "Contains optimizations and workflow improvements for developing Dedicated Server platform." |
| 18 | Get started with Dedicated Server | `Manual/dedicated-server-get-started.html` | Index to Requirements / Player settings / Optimizations sub-pages. |
| 19 | Dedicated Server requirements | `Manual/dedicated-server-requirements.html` | Editor 2021.3 LTS+ required; install the platform-specific "Dedicated Server Build Support" module via Unity Hub (e.g. "Linux Dedicated Server Build Support"). |
| 20 | Dedicated Server Player settings | `Manual/dedicated-server-player-settings.html` | Only the "Other Settings" group applies (Configuration, Shader Settings, Shader Variant Loading, Script Compilation, Optimization, Stack Trace, Legacy Capture Logs); Icon/Splash/Presentation/Publishing settings don't apply to a headless target. |
| 21 | Dedicated Server optimizations | `Manual/dedicated-server-optimizations.html` | Automatic optimizations on this build target: deactivates the Audio Subsystem, removes lighting threads, strips some PlayerLoop callbacks, removes GPU-only assets. |
| 22 | Build for Dedicated Server | `Manual/dedicated-server-build.html` | Build Profiles workflow: File > Build Profiles > Add Build Profile > pick server platform (e.g. "Linux Server") > Switch Profile > Build. Also covers scripting and command-line build paths. |
| 23 | Dedicated Server AssetBundles | `Manual/dedicated-server-assetbundles.html` | Set `subtarget = StandaloneBuildSubtarget.Server` in `BuildAssetBundlesParameters` to apply Dedicated Server–specific AssetBundle optimizations (available since Editor 2023.1.0a21). |
| 24 | Desktop headless mode | `Manual/desktop-headless-mode.html` | Distinguishes plain `-batchmode -nographics` headless runs (no build-target optimizations, can't select from Build Settings) from the Dedicated Server build target (same headlessness plus real memory/CPU stripping). |
| 25 | What's New in Unity 6 — Netcode/Multiplayer section | `Manual/WhatsNewUnity6.html` | Real changelog content, see Advanced Notes/version-facts below: Multiplayer Services package (Lobby/Relay/Distributed Authority/Matchmaker/Multiplay Hosting setup, P2P/Dedicated/Distributed-Authority session types), Multiplayer Tools 2.2.1 (Distributed Authority support, Network Scene Visualization debugger), NGO codegen/`NetworkVariable`/`NetworkManager` event additions (`OnServerStarted`, `OnServerStopped`, `OnClientStarted`, `OnClientStopped`), new "Multiplayer roles" content-stripping system (Content/Automatic Selection, Safety Checks). |
| 26 | Upgrade Guide to Unity 6.3 | `Manual/UpgradeGuideUnity63.html` | Breaking-change notes: `NetworkTransform.Update` override removed in favor of `NetworkTransform.OnUpdate` / `INetworkUpdateSystem` + `NetworkUpdateLoop`; **Multiplay Hosting is deprecated** — no longer supported in Editor/runtime as of 6.3, service runs through **March 31, 2026** then shuts down; Multiplayer Play Mode defaults to 2.0.1 (drops remote-instance scenarios); Multiplayer Services defaults to 2.0.0 (drops Multiplay Hosting functionality). |
| 27 | Vivox package page | `Manual/com.unity.services.vivox.html` | Voice/text chat service for multiplayer, adjacent to but distinct from netcode — v`16.11.0`. Relevant when a "multiplayer" ask includes comms, not gameplay sync. |
| 28 | Vivox Building Block + prerequisites | `Manual/building-blocks-vivox.html`, `Manual/building-blocks-vivox-prerequisites.html` | UI-based voice/text chat quickstart; same Dashboard/org/Authentication prerequisites pattern as the Sessions/Matchmaker blocks. |
| 29 | Authentication service package page | `Manual/com.unity.services.authentication.html` | "Client SDK offering player identity management for Unity Gaming Services." Prerequisite for Sessions, Matchmaker, and Vivox Building Blocks alike — sign-in is what authorizes session/lobby calls. |
| 30 | UnityWebRequest module pages | `Manual/com.unity.modules.unitywebrequest.html` (+ `...assetbundle`/`...texture`/`...audio`/`...www` variants) | The only networking transport that works uniformly across all platforms including WebGL, since it doesn't touch raw sockets. |

**Re-verified negative finding — still true this pass:** no NGO/Relay/Lobby **API** reference is mirrored locally. Specifically, `find`/`grep` across both `Manual/` and `ScriptReference/` for `*NetworkBehaviour*`, `*NetworkObject*`, `*NetworkVariable*`, `*ServerRpc*`, `*ClientRpc*`, `*NetworkManager*`, `*NetworkTransform*`, `*relay*`, `*lobby*`, `Unity.NetCode.*` (Entities API), and `Unity.Networking.Transport.*` (Transport API) as **filenames** return zero matches (the `*network*` filename search returns 260 hits, all `UnityEngine.Networking.*` (`UnityWebRequest`/`PlayerConnection`), `Profiling.ProfilerArea.Network*`, or unrelated iOS/XR/Physics API pages — not NGO). A content-level grep (not just filenames) across every `Manual` and `ScriptReference` file for `NetworkBehaviour|NetworkObject|ServerRpc|ClientRpc` turns up exactly two substantive hits, both release notes (`WhatsNewUnity6.html`, `UpgradeGuideUnity63.html` — rows 25–26 above), not API documentation. Bottom line: this doc mirror carries the **package/platform/Building-Block layer** (what exists, how to set it up, prerequisites, build targets) in real depth, but the **NGO/Transport/Netcode-for-Entities scripting API** and the **Relay/Lobby service API** live only at docs-multiplayer.unity3d.com and Unity Services docs — never fabricate specific method signatures as if sourced locally; label pretrained-knowledge API claims as such (done throughout Key Guidelines below) and confirm exact signatures in-editor via Package Manager > Netcode for GameObjects > API docs, or online, before shipping code from them.

## Key Guidelines

### NGO Core Concepts (pretrained knowledge — not locally documented, verify signatures in-editor/online)

`NetworkObject` is the unit of network identity: attach it to any prefab or scene GameObject that needs to be spawned, despawned, owned, or observed across the network. `NetworkBehaviour` is the networked equivalent of `MonoBehaviour` — it's where `NetworkVariable<T>` fields and RPC methods live, and it requires a `NetworkObject` on the same GameObject (or a parent) to function. A single `NetworkObject` can carry multiple `NetworkBehaviour`s. Ownership (`IsOwner`, `IsServer`, `IsClient`, `IsHost`) determines who is allowed to write which networked state, and is the first thing to check in any callback whose behavior should differ between server and client.

```csharp
using Unity.Netcode;

public class PlayerHealth : NetworkBehaviour
{
    // Server writes, everyone reads — the standard permission pairing for synced state.
    public NetworkVariable<int> Health = new NetworkVariable<int>(
        100,
        NetworkVariableReadPermission.Everyone,
        NetworkVariableWritePermission.Server);

    public override void OnNetworkSpawn()
    {
        Health.OnValueChanged += (oldValue, newValue) => UpdateHealthBar(newValue);
    }
}
```

RPCs move discrete, one-off events across the wire rather than continuous state: `ServerRpc` for client→server requests (e.g. "I want to fire"), `ClientRpc` for server→client notifications (e.g. "you were hit"). A `ServerRpc` method conventionally ends in the suffix `ServerRpc` and is decorated with `[ServerRpc]`; add `RequireOwnership = false` only when a non-owner client legitimately needs to call it.

```csharp
[ServerRpc]
private void RequestFireServerRpc(Vector3 origin, Vector3 direction, ServerRpcParams rpcParams = default)
{
    // Server is authoritative: recompute the shot server-side, never trust origin/direction blindly
    // for damage — validate against last known server-side transform/cooldown before applying effects.
    if (!CanFire(rpcParams.Receive.SenderClientId)) return;
    ApplyShotServerSide(origin, direction);
    NotifyHitClientRpc(origin, direction);
}

[ClientRpc]
private void NotifyHitClientRpc(Vector3 origin, Vector3 direction)
{
    PlayMuzzleFlashAndTracer(origin, direction); // cosmetic only — safe to trust, it's server-sent
}
```

Spawn and despawn networked objects only through the network spawn flow — `NetworkObject.Spawn()` (server-only call, typically after `Instantiate()`) and `NetworkObject.Despawn()`. A bare `Instantiate()` with no follow-up `Spawn()` call produces a local-only GameObject with no network identity; clients never see it. Register every networked prefab in the `NetworkManager`'s prefab list (Network Prefabs Lists asset in modern NGO) on every peer before it can be spawned at runtime — a missing registration throws or silently fails to spawn on the peers that lack it.

### Server Authority & Security

Default to server-authoritative design for anything that affects fairness, persistence, or the game economy: movement validation, damage/hit registration, currency, inventory, matchmaking rank. Clients send *intent* (inputs, requested actions); the server *computes* the resulting state and is the only source of truth that gets replicated back out. This is what makes the design resistant to a modified client — an attacker can rewrite their own client freely, but every packet they send is just a request the server is free to reject or reinterpret. Pure client-authoritative movement (client owns and broadcasts its own transform) is simpler and lower-latency to build, and is a reasonable tradeoff for a couch-coop or non-competitive game, but is trivially cheatable (speed hacks, teleports) in anything competitive.

Never trust client-sent data for logic that must be fair or must persist. That means every `ServerRpc` body should treat its parameters as attacker-controlled input: reclamp values, revalidate cooldowns/state machines, and recompute outcomes server-side rather than accepting a client's claimed result.

```csharp
[ServerRpc]
private void SubmitScoreServerRpc(int reportedDelta)
{
    // WRONG pattern (do not do this): Score.Value += reportedDelta;
    // Right pattern: derive the actual delta from server-tracked game state.
    int verifiedDelta = ComputeScoreDeltaFromServerState();
    Score.Value += verifiedDelta;
}
```

Set `NetworkVariable` read/write permissions deliberately at declaration time (`NetworkVariableWritePermission.Server` for anything security-sensitive) rather than discovering at runtime that a client could write to state it shouldn't touch — NGO enforces the permission you declared, but a permissive default chosen out of laziness is a real vulnerability, not a hypothetical one. A listen-server host runs both the server and a local client in the same process; guard server-only branches with `IsServer` (not just "am I the host's player") so the host doesn't get an unfair, client-invisible advantage from code that silently skips validation for itself.

### Relay & NAT Traversal

Relay solves NAT traversal: most players sit behind a home router or CGNAT that makes their machine unreachable from the public internet, so direct peer connections routinely fail. Relay is a Unity-hosted cloud service that all connecting peers dial out to (an outbound connection, which almost every NAT/firewall permits), and it forwards traffic between them — no port-forwarding, no public IP, no UPnP configuration required from the player. In an NGO project, Relay is plugged in as the underlying connection for Unity Transport: the transport's relay server data (host/client join data, obtained from the Relay service after allocating/joining a relay session) is set on the `UnityTransport` component before `NetworkManager.StartHost()`/`StartClient()` is called.

```csharp
// Pretrained-knowledge sketch of the allocate → set relay data → start host flow.
// Verify exact Relay SDK method names/signatures online — not in this local doc mirror.
var allocation = await RelayService.Instance.CreateAllocationAsync(maxConnections: 8);
string joinCode = await RelayService.Instance.GetJoinCodeAsync(allocation.AllocationId);

var relayServerData = new RelayServerData(allocation, "dtls"); // "dtls" or "udp" connection type
NetworkManager.Singleton.GetComponent<UnityTransport>().SetRelayServerData(relayServerData);
NetworkManager.Singleton.StartHost();
```

A join code is the short human-shareable string players exchange to find each other's Relay allocation; joining clients call the Relay join-by-code API to fetch their own `RelayServerData`, then start as a client rather than a host. Relay adds one extra network hop of latency versus a direct connection or a co-located dedicated server, which is the tradeoff for guaranteed connectivity.

### Lobby / Sessions & Matchmaking

Lobby (or, in the local doc mirror's terms, the Multiplayer Sessions Building Block) solves *discovery*, not gameplay sync: it lets players create a session with metadata (name, game mode, player count, public/private), browse or query for open sessions, and join one — all before any Relay/NGO connection exists. Per `Manual/building-blocks-multiplayer-sessions.html`, the pattern locally documented covers create/join via the Multiplayer Services SDK, a Quick Join API (join any matching open session without browsing), a Session Browsing API (list-and-pick), and JoinCode-based direct sharing — and it explicitly integrates with NGO/Netcode-for-Entities/Unity Transport once a session is chosen, meaning the session layer's job ends where the connection layer's job begins. Per `Manual/WhatsNewUnity6.html`, the Multiplayer Services package's session system can back three different topologies: peer-to-peer (P2P), Dedicated Game Server, and Distributed Authority sessions — the app picks which at session-creation time via `SessionType`.

Matchmaker (Matchmaker Session Building Block) is a step earlier still: instead of a human browsing a session list, players submit into a queue and a rule-based matching service groups them automatically (skill, region, mode). Per `Manual/building-blocks-multiplayer-matchmaking.html`, queue configuration is authored as an asset in the Unity project and deployed to Unity's cloud services so it's live for Play Mode testing; once the Matchmaker groups a set of players by its rules, the app is responsible for turning that group into an actual connected session (again typically via NGO).

```csharp
// Sketch — Sessions SDK shape, not verified against a locally-mirrored API reference.
var sessionOptions = new SessionOptions
{
    MaxPlayers = 8,
    IsPrivate = false
}.WithRelayNetwork(); // ties the session to a Relay-backed NGO connection under the hood

ISession session = await MultiplayerService.Instance.CreateSessionAsync(sessionOptions);
string joinCode = session.Code; // share this out-of-band, or let others Quick Join/browse instead
```

### Host vs Dedicated Server

A **listen-server host** is a player's own client process that also runs the authoritative server logic in the same build — simplest to ship (one build, one process, no separate infra), but the host's machine and connection quality become a single point of failure/lag for every other player, and if the host quits, the session typically ends unless host migration is implemented. A **dedicated server** is a separate headless process, running nobody's local player camera/input, whose only job is to be authoritative; players connect to it purely as clients. This is what the local `dedicated-server-*` doc tree (rows 15–24) covers in real depth: the **Dedicated Server build target** is a sub-target of the Desktop platforms (Linux/macOS/Windows) purpose-built to cut CPU, memory, and disk versus a normal player build, by stripping rendering- and audio-only code paths that a headless process never needs.

Automatic optimizations the Dedicated Server target applies (per `Manual/dedicated-server-optimizations.html`): the Audio Subsystem is deactivated outright, lighting-related threads are removed, several `PlayerLoop` callbacks are disabled, and GPU-only assets are stripped from the build. Building one requires installing the platform-specific "Dedicated Server Build Support" module via Unity Hub (e.g. "Linux Dedicated Server Build Support"), then using File > Build Profiles > Add Build Profile to pick the server sub-target, Switch Profile, and Build (`Manual/dedicated-server-build.html`, `Manual/dedicated-server-requirements.html`). AssetBundles get the same treatment by setting `subtarget = StandaloneBuildSubtarget.Server` on `BuildAssetBundlesParameters` (`Manual/dedicated-server-assetbundles.html`). Don't confuse this with plain **desktop headless mode** (`-batchmode -nographics` command-line flags on an ordinary Desktop build) — headless mode skips graphics device init but applies none of the Dedicated Server target's memory/CPU stripping, and can't be selected from Editor Build Settings (`Manual/desktop-headless-mode.html`).

Per `Manual/WhatsNewUnity6.html`, Unity 6.0 also introduced **Multiplayer roles**, a content-stripping system letting one project build cleanly into either a Client or Server role: Content Selection (choose which GameObjects/Components exist per role), Automatic Selection (auto-remove component types per role), and Safety Checks (warn about null-reference risk from role-based stripping) — this is what lets a single project ship both a player build and a dedicated-server build without maintaining two codebases.

**Important currency note** (`Manual/UpgradeGuideUnity63.html`, verified locally): **Multiplay Hosting is deprecated as of Unity 6.3** — no longer supported in the Editor or at runtime; the hosted service itself keeps running only through **March 31, 2026**, after which it shuts down entirely, and the default Multiplayer Services package (2.0.0) has already dropped Multiplay-Hosting-specific functionality. Any new dedicated-server-hosting design should target a different hosting path (self-managed infrastructure, or whatever successor service Unity documents online) rather than Multiplay Hosting.

### WebGL Networking Constraints

Per `Manual/webgl-networking.html` (full local page content), Web/WebGL builds support networking in exactly two ways: `UnityWebRequest`, and the Unity Netcode networking package. `UnityWebRequest` on Web is implemented via the browser's JavaScript Fetch API, which means it inherits full browser security restrictions — same-origin policy and CORS. A request to any origin other than the one hosting the Unity content fails unless that origin's server sends the right `Access-Control-Allow-*` response headers; a missing/misconfigured CORS header surfaces as a browser-console error ("Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource..."), not a Unity-side exception. Never block synchronously on a `UnityWebRequest` (`while(!www.isDone) {}`) — browsers disallow blocking the main thread on a network transfer; use a coroutine with `yield return www.SendWebRequest();` instead.

The more fundamental constraint: **browsers do not allow direct access to raw TCP or UDP sockets**, for any code, for any reason — this is a browser sandbox rule, not a Unity limitation, and it's why Unity Transport's normal UDP-based NGO connection simply cannot run inside a WebGL build unmodified. The documented fix is to route through the Unity Netcode networking package's Web-compatible transport path (WebSocket-based under the hood, since WebSockets are one of the few socket-like primitives browsers do expose) rather than attempting a raw UDP relay/host connection from a WebGL client. The page further points to the WebSocket Protocol and WebRTC specs as background reading for anyone building a custom Web-compatible transport.

```csharp
#if UNITY_WEBGL && !UNITY_EDITOR
    // WebGL clients cannot open raw UDP sockets — ensure the transport config
    // actually resolves to a WebSocket-capable path before calling StartClient(),
    // and never assume the same UnityTransport settings that work on Desktop/Console
    // will connect from a WebGL build without verification.
#endif
```

## Common Mistakes

| Mistake | Why / fix |
|---------|-----------|
| Trusting client input for game-critical logic | RPC payloads are attacker-controlled; validate/recompute server-side, never apply a client-reported delta or result directly. |
| Spawning outside the network spawn flow | `Instantiate()` alone skips network identity; use `NetworkObject.Spawn()` server-side so the object actually replicates. |
| Prefab not registered with `NetworkManager` | Spawn fails/throws on any peer missing the prefab registration; keep the Network Prefabs Lists asset in sync across all builds. |
| Client writes to a server-only `NetworkVariable` | NGO enforces the read/write permission you declared; set `NetworkVariableWritePermission.Server` deliberately at declaration time instead of discovering the gap at runtime. |
| `NetworkVariable` ticked every frame for one-off events | Wastes bandwidth on continuous replication; use a `ServerRpc`/`ClientRpc` for discrete, occasional events instead. |
| Host code path treated identically to a pure client | A listen-server host is both server and client in one process; guard server-only logic with `IsServer`, or the host silently skips validation other clients are subject to. |
| Confusing Relay with Lobby/Sessions | Separate concerns — Sessions/Lobby find and describe a session (metadata, discovery, join code); Relay is the NAT-traversal transport that actually connects players once a session is chosen. |
| Assuming raw UDP sockets work in WebGL builds | Browsers block direct TCP/UDP access outright; WebGL networking must go through `UnityWebRequest` (CORS-gated HTTP) or a WebSocket-capable Netcode transport, not the same UnityTransport UDP config used on Desktop. |
| Ignoring CORS on WebGL `UnityWebRequest` calls | A cross-origin request without `Access-Control-Allow-Origin` on the target server fails in-browser with a Same Origin Policy error, not a catchable Unity exception — fix the server, not the client. |
| Blocking synchronously on a `UnityWebRequest` (`while(!www.isDone)`) | Browsers disallow blocking the main thread on a network transfer; use a coroutine with `yield return www.SendWebRequest();`. |
| Building a dedicated server from a normal Desktop build instead of the Dedicated Server build target | Misses the automatic CPU/memory optimizations (audio subsystem, lighting threads, PlayerLoop callbacks, GPU-only assets all stripped) that the dedicated Dedicated Server sub-target applies. |
| Treating `-batchmode -nographics` headless mode as equivalent to a Dedicated Server build | Headless mode skips only graphics-device init; it gets none of the Dedicated Server target's real memory/CPU stripping and can't be chosen from Editor Build Settings. |
| Planning new hosting around Multiplay Hosting | Deprecated as of Unity 6.3 (no longer supported in Editor/runtime); the hosted service itself shuts down March 31, 2026 — target a different hosting path for new work. |
| Citing NGO/Relay/Lobby method signatures as if sourced from this local doc mirror | The API reference for these isn't mirrored locally (verified above) — label such specifics as pretrained knowledge and confirm exact signatures in-editor or at docs-multiplayer.unity3d.com before shipping. |
| Overriding `NetworkTransform.Update` for authority-instance logic on NGO versions tied to Unity 6.3+ | That override point was removed; override `NetworkTransform.OnUpdate` instead, or implement `INetworkUpdateSystem` and register with `NetworkUpdateLoop` for authority-gain/loss logic (`Manual/UpgradeGuideUnity63.html`). |

## Quick Reference

| Concept | Purpose |
|---------|---------|
| `NetworkObject` | Network identity; unit of spawn/despawn/ownership for a GameObject. |
| `NetworkBehaviour` | Base class for networked scripts — home for `NetworkVariable`s and RPCs; needs a `NetworkObject` on the same/parent GameObject. |
| `NetworkVariable<T>` | Auto-replicated, permissioned networked state; set read/write permissions deliberately at declaration. |
| `ServerRpc` | Client→server one-off call; treat every parameter as attacker-controlled, revalidate server-side. |
| `ClientRpc` | Server→client one-off call; safe to trust since it's server-sent, typically used for cosmetic/notification events. |
| `NetworkManager` | Transport config, connection approval, prefab registry, `StartHost()`/`StartClient()`/`StartServer()` entry points; also raises `OnServerStarted`/`OnServerStopped`/`OnClientStarted`/`OnClientStopped` events (added Unity 6). |
| `NetworkTransform` | Built-in NGO component for replicating transform state; override `OnUpdate` (not `Update`) for custom authority-instance interpolation logic as of the Unity 6.3 upgrade. |
| Unity Transport (`com.unity.transport`) | Low-level transport layer NGO runs on top of; Relay plugs in here via `UnityTransport.SetRelayServerData()`. |
| Netcode for Entities (`com.unity.netcode`) | Separate DOTS/ECS-based netcode stack — not the same package as NGO; do not conflate. |
| Relay | Cloud NAT-traversal relay service; all peers dial outbound, no port-forwarding/public IP needed; adds one extra hop of latency. |
| Lobby / Sessions Building Block | Session creation, metadata, browsing, Quick Join, JoinCode; backs P2P, Dedicated Game Server, or Distributed Authority session types. |
| Matchmaker | Rule-based queue pairing that groups players before a session/connection forms. |
| Distributed Authority | A session/hosting mode (alongside P2P and Dedicated Game Server) where authority is distributed rather than centralized on one host/server; supported by Multiplayer Services + Multiplayer Tools 2.2.1+. |
| Multiplayer Center (`com.unity.multiplayer.center`) | In-Editor package/feature recommender and starting point for new multiplayer projects. |
| Multiplayer Play Mode (`com.unity.multiplayer.playmode`) | Runs multiple Editor instances simultaneously on one dev machine for local multi-client testing. |
| Multiplayer Tools (`com.unity.multiplayer.tools`) | Profiling/debugging suite for netcode, incl. Network Scene Visualization (added as of Multiplayer Tools 2.1.0 Preview). |
| Multiplayer roles | Unity 6 content-stripping system (Content Selection, Automatic Selection, Safety Checks) for building one project into distinct Client/Server role builds. |
| Dedicated Server build target | Optimized Desktop sub-target (Linux/macOS/Windows) for headless server builds; strips audio, lighting threads, some PlayerLoop callbacks, GPU-only assets. |
| Desktop headless mode | Plain `-batchmode -nographics` flags on a normal Desktop build; no build-target-level stripping, not selectable from Build Settings. |
| Host (listen server) | A player's client process also running authoritative server logic; simplest to ship, single point of failure, may need host migration. |
| Authentication (`com.unity.services.authentication`) | Player identity/sign-in SDK; a prerequisite for Sessions, Matchmaker, and Vivox Building Blocks. |
| Vivox (`com.unity.services.vivox`) | Managed voice/text chat service for multiplayer, separate concern from gameplay-state netcode. |
| Multiplay Hosting | **Deprecated as of Unity 6.3**; unsupported in Editor/runtime, service shuts down March 31, 2026 — do not build new hosting plans around it. |
| `UnityWebRequest` | The one networking API that works uniformly across all platforms incl. WebGL, since it never touches raw sockets; CORS-gated on Web. |
| WebGL/Web networking constraint | No raw TCP/UDP sockets in-browser at all; use `UnityWebRequest` (HTTP/CORS) or a WebSocket-capable Netcode transport instead of a Desktop-style UDP config. |

## Advanced Notes

**Client-server vs. peer-to-peer, chosen deliberately.** Client-server (whether via a listen-server host or a true dedicated server) centralizes authority in one place, which is what makes anti-cheat, persistence, and debugging tractable: there is exactly one process whose state is ever "true," every other participant is a client reporting intent and rendering a replica. The cost is infrastructure — someone has to run that authoritative process, whether that's a player's own machine (host, free but unreliable/exploitable) or a real server (dedicated, costs money/ops but reliable and secure). Peer-to-peer removes that infrastructure cost — every participant talks directly to every other participant, with no single owned "truth" — but pays for it twice over: NAT traversal is harder because *every pair* of peers needs a working path (not just each peer to one server), and security is harder because there's no natural place to put validation — any peer asserting authority over some piece of state is exactly as trustworthy (or untrustworthy) as a modified client would be in a client-server design. This is why most real NGO projects that use P2P at all restrict it to non-competitive or cosmetic-stakes games, or layer a lightweight arbitration scheme (e.g. Distributed Authority, where ownership of specific objects is handed to whichever peer is best positioned to own them, with the platform/service still mediating who's allowed to claim what) on top rather than running fully trustless P2P. Unity's own Multiplayer Services package (per `Manual/WhatsNewUnity6.html`, verified locally) treats all three — P2P, Dedicated Game Server, and Distributed Authority — as first-class session types precisely because the right choice is genuinely per-project, trading off cost, latency, cheat-resistance, and ops burden differently each time.

**Where the full NGO/Relay/Lobby docs actually live, since they aren't local:**
- Netcode for GameObjects (concepts, full API reference, tutorials, samples): **docs-multiplayer.unity3d.com** — "About Netcode for GameObjects (Unity Multiplayer Networking)" is the entry point this local doc set's own `webgl-networking.html` page links out to.
- Unity Relay, Unity Lobby, Multiplayer Services SDK (session/matchmaking APIs, Distributed Authority setup): **Unity Gaming Services documentation** at **docs.unity.com** (Multiplayer Services section) and the Unity Dashboard's own in-product docs/reference for each service.
- Unity Transport low-level API (`NetworkDriver`, `NetworkConnection`, pipeline stages): also docs-multiplayer.unity3d.com, under the Transport section — not mirrored in this Manual/ScriptReference download despite the package landing page existing locally.
- In-editor fallback when offline: Package Manager > select the installed Netcode for GameObjects / Unity Transport / Multiplayer Services package > "View documentation" opens the bundled offline API docs shipped inside the package itself, which *do* contain full scripting API detail even though this local `Documentation/en` mirror does not.
- Release-note-level ground truth for exact current-version behavior changes: this local mirror's own `Manual/WhatsNewUnity6.html` and `Manual/UpgradeGuideUnity63.html` are surprisingly reliable for recent NGO/Multiplayer-Services/Multiplayer-Tools changes (see Retrieval Sources rows 25–26) even though they aren't structured as API references — check them before asserting how a specific NGO version behaves.

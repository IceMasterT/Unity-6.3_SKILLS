---
name: unity-input-system
description: Use when handling player input in Unity via the Input System package — Action Maps, PlayerInput, control schemes, or device switching. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Input System

**Local docs gap (re-verified 2026-08-19)**: this doc mirror still lacks the Input System package's own manual — there is no `Manual/InputSystem*` chapter tree, no Action Maps / Input Actions asset walkthrough, no PlayerInput component reference, and no control-scheme documentation anywhere under `Manual/` or `ScriptReference/`. `Manual/com.unity.inputsystem.html` was re-checked directly (698 words total) and is confirmed to be nothing but the Package Manager listing stub: it states the package name (`com.unity.inputsystem`), version (1.20.0, released for Unity Editor 6000.3 / Unity 6.3 LTS), a one-line description ("A new input system which can be used as a more extensible and customizable alternative to Unity's classic input system in UnityEngine.Input"), and keywords (`input, events, keyboard, mouse, gamepad, touch, vr, xr`) — it has zero usage content, no code samples, and no links into a deeper Input System manual. Treat every Input System package workflow/API claim below (Action Maps, PlayerInput, control schemes, device pairing) as **pretrained knowledge, explicitly flagged as such** — verify precision against the in-Editor Package Manager docs (Window > Package Manager > Input System > View Documentation) or the package's own `Documentation~` folder/Samples before shipping code from this skill unmodified. Everything about the **legacy Input Manager** and the general input landscape (mobile, XR, Web, UI Toolkit) below is grounded in real, re-verified local files.

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Introduction to Input | `Manual/input-introduction.html` | Framing: Unity has two input paths (Input System package vs. legacy Input Manager); when each applies |
| Input landing page | `Manual/Input.html` | Top-level topic map linking Introduction to Input and Mobile Keyboard; states the package-vs-legacy split explicitly |
| Input System package stub | `Manual/com.unity.inputsystem.html` | Confirms installed package identity/version (`com.unity.inputsystem` 1.20.0 for Unity 6000.3) only — no workflow docs, re-verified empty of usage content |
| Legacy Input overview | `Manual/InputLegacy.html` | Index/rationale for the legacy Input Manager section; explicit deprecation notice ("will be removed in future versions of Unity") |
| Input Manager reference | `Manual/class-InputManager.html` | Full Project Settings > Input Manager reference: axis properties (Name, Gravity, Dead, Sensitivity, Snap, Type, Axis, JoyNum), Physical Keys option, key-naming conventions |
| Mobile device input (legacy) | `Manual/MobileInput.html` | Legacy `Input.touches`, `Input.Touch` struct, multi-touch limits per platform, mouse-simulation-from-touch behavior, worked `TouchInput` example |
| IME input | `Manual/IMEInput.html` | Input Method Editor integration for non-ASCII text entry; lists platforms where IME is NOT yet supported (iOS, Android, Web, Linux, embedded) |
| Android input | `Manual/android-input.html` | Confirms Input System package is Android's *default* for new projects; exact settings path for switching (`Android Player Settings > Other Settings > Configuration > Active Input Handling`) |
| iOS input overview | `Manual/ios-input-overview.html` | iOS touchscreen keyboard auto-activation on Input Field focus; points to Game Controller support and Input Manager |
| iOS input (scripting index) | `Manual/ios-input.html` | Topic index for iOS input scripting: input overview + Game Controller API |
| Input in Web (WebGL) | `Manual/webgl-input.html` | Gamepad/joystick support via HTML5 Gamepad API, W3-spec button-index mapping table, touch/keyboard/cursor-lock/fullscreen caveats specific to Web builds |
| XR input options | `Manual/xr-input-overview.html` | Enumerates the 5 ways to read XR input (XR Interaction Toolkit, OpenXR interaction profiles, "traditional" Input System/Input Manager, `XR.InputDevice`/`XR.Node` APIs, third-party libs) and how they combine |
| UI Toolkit input events | `Manual/UIE-Input-Events.html` | `InputEvent` on `TextInputBaseField`-derived controls: fires per keystroke, trickles/bubbles, differs from `ChangeEvent` |
| Active Input Handling setting | `Manual/playersettings-windows.html` | Project Settings > Player > Configuration: the "Input Manager (Old)" / "Input System Package (New)" / "Both" switch, definitive source for the Advanced Notes section below |
| Legacy Input class (script ref) | `ScriptReference/Input.html` | `UnityEngine.Input` class description; default axis names ("Horizontal"/"Vertical"/"Mouse X"/"Mouse Y"/"Fire1-3"); Physical Keys note (default-on since 2022.1) |
| `Input.GetAxis` (script ref) | `ScriptReference/Input.GetAxis.html` | Exact signature `public static float GetAxis(string axisName)`; return range semantics for joystick/keyboard (-1..1) vs. mouse delta (unbounded, sensitivity-scaled) |
| `Input.GetKey` / `GetButton` / `GetMouseButton` (script ref) | `ScriptReference/Input.GetKey.html`, `ScriptReference/Input.GetButton.html`, `ScriptReference/Input.GetMouseButton.html` | Held-down polling signatures (`bool GetKey(string)`, `bool GetButton(string buttonName)`, `bool GetMouseButton(int button)`); mouse button index convention (0=left,1=right,2=middle) |
| Touch/mouse position properties (script ref) | `ScriptReference/Input-touches.html`, `ScriptReference/Input-mousePosition.html` | Property-level reference for `Input.touches` array and `Input.mousePosition` (used alongside `MobileInput.html`'s worked example) |

## Key Guidelines

### Action Maps and the Input Actions Asset (new Input System — pretrained knowledge)

An **Input Actions asset** (`.inputactions`, a JSON file with a custom Inspector) is the root container for the new Input System's configuration. Inside it, **Action Maps** group related actions under a name — typically one per gameplay context (`Player`, `UI`, `Vehicle`, `Dialogue`). Each **Action** inside a map (e.g. `Move`, `Jump`, `Fire`) is a logical operation with an expected value type (`Button`, `Value`, `Pass-Through`) and one or more **bindings** that map physical controls (keyboard key, gamepad stick, touch) to it. Only one Action Map's actions fire at a time in the common single-context setup, though multiple maps can be enabled simultaneously (e.g. `Player` + `UI` both active for a pause menu overlay). Critically, a map's actions are inert until the map itself is enabled — this is the single most common "why doesn't my input work" bug (see Common Mistakes).

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerActions : MonoBehaviour
{
    [SerializeField] private InputActionAsset actions;
    private InputActionMap playerMap;
    private InputAction jumpAction;

    void Awake()
    {
        playerMap = actions.FindActionMap("Player", throwIfNotFound: true);
        jumpAction = playerMap.FindAction("Jump");
    }

    void OnEnable()
    {
        playerMap.Enable();               // map must be enabled or nothing fires
        jumpAction.performed += OnJump;
    }

    void OnDisable()
    {
        jumpAction.performed -= OnJump;
        playerMap.Disable();
    }

    private void OnJump(InputAction.CallbackContext ctx) => Debug.Log("Jumped");
}
```

### PlayerInput Component Behaviors (new Input System — pretrained knowledge)

The `PlayerInput` component is the standard bridge between an Input Actions asset and a GameObject, and it exposes a **Behavior** dropdown with four distinct dispatch strategies. **Send Messages** calls a method named exactly `On<ActionName>` on any component on the *same* GameObject via `SendMessage` — simple but reflection-based and silently no-ops on a name mismatch. **Broadcast Messages** does the same but walks children too, useful when input-reactive components live on child objects. **Invoke Unity Events** exposes one `UnityEvent<InputAction.CallbackContext>` per action in the Inspector, letting designers wire callbacks without code. **Invoke C# Events** is the code-first option: subscribe to `PlayerInput.onActionTriggered` (fires for every action) or read `PlayerInput.currentActionMap` and its actions' `.performed`/`.started`/`.canceled` events directly — this is generally the most robust and refactor-safe choice for programmer-driven projects.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

// PlayerInput Behavior set to "Invoke C# Events"
public class PlayerController : MonoBehaviour
{
    private PlayerInput playerInput;

    void Awake()
    {
        playerInput = GetComponent<PlayerInput>();
        playerInput.onActionTriggered += HandleAction;
    }

    private void HandleAction(InputAction.CallbackContext ctx)
    {
        if (ctx.action.name == "Jump" && ctx.performed)
            Jump();
    }

    // Alternative: Behavior = "Send Messages" — method name must match the
    // action name exactly and be public (or at least accessible) on this GameObject.
    public void OnMove(InputValue value)
    {
        Vector2 move = value.Get<Vector2>();
    }

    private void Jump() => Debug.Log("Jump!");
}
```

### Polling vs. Event-Driven Reading (new Input System — pretrained knowledge)

Two reading styles coexist and are chosen per-action based on its shape. **Event-driven** (`action.performed += callback`, or PlayerInput's Behavior dispatch) suits discrete, one-shot actions — jump, fire, pause — where you want to react exactly once per press, and `.started`/`.performed`/`.canceled` map naturally to press/hold-threshold/release semantics (for a simple button, `.performed` fires on press). **Polling** (`action.ReadValue<T>()`, or the convenience booleans `action.IsPressed()` / `action.WasPressedThisFrame()` / `action.WasReleasedThisFrame()`) suits continuous per-frame reads — movement vectors, look deltas, throttle — where `Update()` needs the current analog state every frame regardless of whether it changed. Mixing styles is normal: read `Move` by polling in `Update`, react to `Jump` by event.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class MovementPoller : MonoBehaviour
{
    [SerializeField] private InputActionReference moveActionRef;
    [SerializeField] private float speed = 5f;

    void Update()
    {
        // Continuous, per-frame poll — no event subscription needed.
        Vector2 move = moveActionRef.action.ReadValue<Vector2>();
        transform.Translate(new Vector3(move.x, 0, move.y) * speed * Time.deltaTime);

        if (moveActionRef.action.WasPressedThisFrame())
            Debug.Log("Move axis crossed the actuation threshold this frame");
    }
}
```

### Control Schemes and Device Switching (new Input System — pretrained knowledge)

A **Control Scheme** (defined inside the Input Actions asset, e.g. `Keyboard&Mouse`, `Gamepad`, `Touch`) declares which device types are required/optional for that scheme to be considered active, and bindings can be tagged so they only apply under specific schemes. `PlayerInput` auto-detects and switches the active scheme when it sees input from a qualifying device — but only devices actually listed in a scheme's requirements are eligible candidates, so a scheme missing "Gamepad" from its device list will never auto-activate on a controller press no matter how the bindings are set up. Auto-switching can be disabled per-PlayerInput via `neverAutoSwitchControlSchemes`, which is typically done in local multiplayer once a device is explicitly paired to a player so a second controller's input doesn't hijack player 1. Subscribe to `PlayerInput.onControlsChanged` to react to a scheme/device swap (e.g. redraw on-screen prompts from keyboard icons to gamepad icons).

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class SchemeAwarePrompts : MonoBehaviour
{
    private PlayerInput playerInput;

    void Awake()
    {
        playerInput = GetComponent<PlayerInput>();
        playerInput.onControlsChanged += OnControlsChanged;
    }

    private void OnControlsChanged(PlayerInput input)
    {
        Debug.Log($"Active scheme: {input.currentControlScheme}");
        // Swap UI glyphs, e.g. keyboard-key icons vs. gamepad-button icons.
    }
}
```

### Legacy Input Manager: Axes, Buttons, Keys (Project Settings — grounded, `Manual/class-InputManager.html`)

The Input Manager (Edit > Project Settings > Input Manager) defines a flat list of named **virtual axes**, each with properties: `Name` (the string scripts reference), `Negative Button`/`Positive Button` (+ `Alt` variants), `Gravity` (units/sec the axis falls back toward neutral when input stops), `Dead` (deadzone before analog movement registers), `Sensitivity` (units/sec toward target, digital devices only), `Snap` (reset to zero when the opposite direction is pressed), `Type` (Key or Mouse Button / Mouse Movement / Joystick Axis), `Axis`, and `JoyNum`. New projects ship with defaults including `Horizontal`/`Vertical` (WASD + arrows + joystick), `Mouse X`/`Mouse Y` (mouse delta), and `Fire1`/`Fire2`/`Fire3` (Ctrl/Alt/Cmd + mouse/joystick buttons). The **Physical Keys** option (default-on since Unity 2022.1, per `ScriptReference/Input.html`) maps `KeyCode`s to the *physical* key position (ANSI/ISO QWERTY layout) rather than the user's language layout, so "W" always means the same physical key regardless of AZERTY/QWERTY.

```csharp
using UnityEngine;
using UnityEngine.EventSystems;

// Configure axes first: Edit > Project Settings > Input Manager.
public class LegacyMover : MonoBehaviour
{
    [SerializeField] private float speed = 5f;

    void Update()
    {
        float h = Input.GetAxis("Horizontal"); // -1..1, smoothed by Gravity/Sensitivity
        float v = Input.GetAxis("Vertical");
        transform.Translate(new Vector3(h, 0, v) * speed * Time.deltaTime);

        if (Input.GetButtonDown("Fire1"))
            Debug.Log("Fired");

        if (Input.GetKeyDown(KeyCode.Space))
            Debug.Log("Jumped via raw KeyCode");
    }
}
```

### Legacy Discrete Polling: GetKey / GetButton / GetMouseButton (grounded, `ScriptReference/Input.GetKey.html` etc.)

Unlike axes, discrete legacy input is read via three families of static methods, all polling-only and evaluated once per `Update()` call: `Input.GetKey(string|KeyCode)` / `GetKeyDown` / `GetKeyUp` for raw keyboard keys; `Input.GetButton(string buttonName)` / `GetButtonDown` / `GetButtonUp` for named virtual buttons configured in the Input Manager (recommended over raw `GetKey` because it's rebindable without code changes); and `Input.GetMouseButton(int button)` / `GetMouseButtonDown` / `GetMouseButtonUp` where `0`=left, `1`=right, `2`=middle. `GetButton`/`GetKey` return `true` for every frame the control is held (useful for auto-fire); the `*Down`/`*Up` variants fire exactly once on the transition frame.

```csharp
using UnityEngine;

public class LegacyClicker : MonoBehaviour
{
    void Update()
    {
        if (Input.GetMouseButtonDown(0))
            Debug.Log("Left click this frame");

        if (Input.GetButton("Fire1")) // held-down, auto-fire style
            Debug.Log("Firing continuously");

        if (Input.GetKey(KeyCode.LeftShift))
            Debug.Log("Sprint modifier held");
    }
}
```

### Legacy Mobile & Touch Input (grounded, `Manual/MobileInput.html`, `ScriptReference/Input-touches.html`)

On mobile, the legacy `Input` class exposes `Input.touches` (an array of `Touch` structs updated once per frame) plus accelerometer and location data. iOS devices track up to 5 simultaneous touches; Android's simultaneous-touch limit varies by hardware (2–5+). Each `Touch` has a `.phase` (`Began`, `Moved`, `Stationary`, `Ended`, `Canceled`) and `.position` in screen space. `MobileInput.html`'s canonical example (reproduced faithfully below) raycasts from each newly-began touch.

```csharp
using UnityEngine;

public class TouchInput : MonoBehaviour
{
    public GameObject particle;

    void Update()
    {
        foreach (Touch touch in Input.touches)
        {
            if (touch.phase == TouchPhase.Began)
            {
                Ray ray = Camera.main.ScreenPointToRay(touch.position);
                if (Physics.Raycast(ray))
                    Instantiate(particle, transform.position, transform.rotation);
            }
        }
    }
}
```

Note IME text entry (`Manual/IMEInput.html`) is *not yet* supported on iOS, Android, Web, Linux, or Embedded platforms (Embedded Linux/QNX) even though it works on desktop Windows/macOS — relevant when building any non-ASCII in-game text field for those targets.

### Local Multiplayer with PlayerInputManager (new Input System — pretrained knowledge)

`PlayerInputManager` (a scene-level singleton-style component, distinct from the per-player `PlayerInput`) handles join-in-progress local multiplayer: it listens for "join" input (a button press on an unpaired device) and instantiates one `PlayerInput` prefab instance per joining player, pairing that instance to the triggering device exclusively so a second controller's input doesn't leak into player 1's actions. Typical setup: set `PlayerInputManager.playerPrefab` to a prefab carrying its own `PlayerInput` + Input Actions asset reference, set the join behavior (Join Players When Button Is Pressed / Join Players When Any Button Is Pressed / Join Players Manually), and subscribe to `onPlayerJoined`/`onPlayerLeft` to spawn/despawn associated game objects (camera viewport splits, score UI, etc.).

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class MultiplayerSpawner : MonoBehaviour
{
    void Awake()
    {
        var manager = GetComponent<PlayerInputManager>();
        manager.onPlayerJoined += OnPlayerJoined;
    }

    private void OnPlayerJoined(PlayerInput player)
    {
        Debug.Log($"Player {player.playerIndex} joined on {player.currentControlScheme}");
        // Assign a split-screen camera viewport, score slot, etc.
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Actions never fire | Action Map not enabled — call `.Enable()` on the map (or the whole `InputActionAsset`) at startup; `PlayerInput` enables its default map automatically but a manually-constructed map does not |
| Old/new Input calls throw `InvalidOperationException` | Active Input Handling mismatched — set it to "Both" during migration, "Input System Package (New)" once all `UnityEngine.Input` calls are removed (see Advanced Notes) |
| Scheme doesn't switch on new device | Device not listed in that scheme's requirements, or `PlayerInput.neverAutoSwitchControlSchemes` is `true` |
| `ReadValue<Vector2>` throws `InvalidOperationException` | The binding's actual control type doesn't match the action's declared value type (e.g. reading a `Button`-type action as `Vector2`) |
| Send Messages callback never fires | Method name doesn't exactly match `On<ActionName>`, or isn't accessible on a component on the same GameObject (Broadcast Messages is needed for children) |
| `Input.GetAxis` feels sluggish or snappy in the wrong way | `Gravity`/`Sensitivity` tuned for a different input type — Sensitivity applies to digital devices only; mouse-mapped axes ignore Gravity/Sensitivity entirely and use raw delta × axis sensitivity |
| `GetKey`/`GetButton` fires every frame when only one press was wanted | Used the held-down variant instead of `GetKeyDown`/`GetButtonDown`; held variants are for auto-fire/continuous states, not one-shot triggers |
| WASD acts wrong on AZERTY keyboards | "Use Physical Keys" is off, or code reads `KeyCode` values expecting physical position — enable Physical Keys (default since 2022.1) per `ScriptReference/Input.html` |
| IME text entry silently doesn't work on iOS/Android/Web/Linux | IME is not supported on those platforms per `Manual/IMEInput.html` — build a custom on-screen keyboard flow or restrict non-ASCII entry to supported platforms |
| Gamepad button indices don't match across platforms | Web/WebGL follows the W3 Gamepad API spec button ordering (`Manual/webgl-input.html`), which can diverge from native platform mappings — never hardcode raw button indices cross-platform; use named axes/actions instead |
| XR input reads nothing / conflicts with OpenXR | Legacy Input Manager is explicitly unsupported when the OpenXR plug-in is active (`Manual/xr-input-overview.html`) — must use Input System, XR Interaction Toolkit, or `XR.InputDevice`/`XR.Node` APIs instead |
| Touch count assumptions break on Android | Android has no unified multi-touch limit (varies 2–5+ fingers by device) unlike iOS's fixed 5-touch cap (`Manual/MobileInput.html`) — don't assume a fixed max touch count |
| `PlayerInputManager` spawns players on any key press unexpectedly | Join behavior left on "Join Players When Any Button Is Pressed" instead of a specific join button or "Join Players Manually" |
| Multiple `PlayerInput`s all react to one controller | Devices not explicitly paired/`neverAutoSwitchControlSchemes` not set — without pairing, any `PlayerInput` still listening for auto-switch can steal the device |
| UI Toolkit `InputEvent` assumed to fire on programmatic value sets | `InputEvent` (`Manual/UIE-Input-Events.html`) only fires for direct user input into `TextInputBaseField`-derived controls, not when a script sets the field's value — use `ChangeEvent` to catch all value changes including scripted ones |

## Quick Reference

| Class/Component | System | Purpose |
|---|---|---|
| `InputActionAsset` | New Input System | Container asset for all Action Maps, actions, bindings, and control schemes |
| `InputActionMap` | New Input System | Named group of actions (e.g. Player, UI); enable/disable as a unit via `.Enable()`/`.Disable()` |
| `InputAction` | New Input System | Single logical action; exposes `.performed`/`.started`/`.canceled` events, `ReadValue<T>()`, `IsPressed()`, `WasPressedThisFrame()` |
| `InputAction.CallbackContext` | New Input System | Struct passed to action callbacks; carries `.action`, `.control`, `.phase`, `.ReadValue<T>()` |
| `InputActionReference` | New Input System | Serializable reference to an action inside an asset, for Inspector-exposed fields |
| `PlayerInput` | New Input System | Component bridging an Input Actions asset to a GameObject/player; owns the Behavior dispatch mode and `currentControlScheme` |
| `PlayerInputManager` | New Input System | Spawns/manages multiple `PlayerInput` instances for local multiplayer join-in |
| `InputValue` | New Input System | Value wrapper passed to Send-Messages-style `On<Action>(InputValue value)` methods |
| `InputSystem` (static) | New Input System | Low-level device/event API (device add/remove, manual event queuing) — rarely needed directly |
| `InputDevice` / `Gamepad` / `Keyboard` / `Mouse` / `Touchscreen` | New Input System | Device classes with `.current` static accessors for direct device polling outside actions |
| `UnityEngine.Input` (static) | Legacy Input Manager | Main polling entry point: `GetAxis`, `GetAxisRaw`, `GetKey`/`GetKeyDown`/`GetKeyUp`, `GetButton`/`GetButtonDown`/`GetButtonUp`, `GetMouseButton` family |
| Input Manager (Project Settings asset) | Legacy Input Manager | Edit > Project Settings > Input Manager; defines named virtual axes (`Name`, `Gravity`, `Dead`, `Sensitivity`, `Snap`, `Type`, `Axis`, `JoyNum`) |
| `Touch` / `Input.touches` | Legacy Input Manager | Per-finger touch struct/array with `.phase` and `.position`, read each frame |
| `Input.mousePosition` / `Input.mouseScrollDelta` | Legacy Input Manager | Screen-space mouse position and scroll wheel delta, polled per frame |
| `Input.acceleration` / `Input.gyro` | Legacy Input Manager | Accelerometer vector and gyroscope data on supporting mobile devices |
| `Input.compositionString` | Legacy Input Manager | IME composition buffer for non-ASCII text entry — use instead of raw key reads for in-game text input |
| `KeyCode` (enum) | Legacy Input Manager | Physical/logical key identifiers passed to `GetKey`/`GetKeyDown`/`GetKeyUp` |
| `XR.InputDevice` / `XR.InputDevices` | XR (either system) | Low-level XR controller/HMD feature-usage queries (position, buttons, haptics) independent of Input System actions |
| `InputEvent` (UI Toolkit) | UI Toolkit | Fires per-keystroke on `TextInputBaseField`-derived controls; trickles and bubbles, not cancellable |

## Advanced Notes

**Active Input Handling project setting.** Unity ships a single project-wide switch that governs which input API surface is compiled/active, found at Project Settings > Player > Configuration (per-platform tab; the exact submenu path varies slightly by platform, e.g. Android's is `Player Settings > Other Settings > Configuration > Active Input Handling` per `Manual/android-input.html`, and the general reference lives at `Manual/playersettings-windows.html`). It has three values: **Input Manager (Old)** — only the legacy `UnityEngine.Input` API compiles, package APIs are unavailable; **Input System Package (New)** — only the new Input System compiles, and legacy `Input.*` calls throw/no-op; **Both** — both APIs are available simultaneously, which is the recommended setting *during* a migration so old and new code can coexist while you convert call sites incrementally, but should be turned off (set to "Input System Package (New)") once migration is complete since running both event pipelines has a small runtime cost and risks double-handling input. Changing this setting requires a project reload/domain reload to take effect.

**Migrating from legacy Input to the new Input System.** A pragmatic migration path: (1) install/confirm the `com.unity.inputsystem` package via Package Manager; (2) set Active Input Handling to "Both" so nothing breaks mid-migration; (3) create an Input Actions asset, and for each legacy call site translate the semantic, not the literal API — a `Input.GetAxis("Horizontal")` read in `Update()` becomes a `Move` action of type `Value`/`Vector2` polled via `ReadValue<Vector2>()`, not a line-for-line swap; (4) replace ad hoc `Input.GetKeyDown(KeyCode.Space)` checks with named actions (`Jump`) bound to whatever keys/buttons are semantically equivalent, gaining rebindability for free; (5) add a `PlayerInput` component per player and choose a Behavior (see Key Guidelines above) matching the codebase's existing style — C# Events for programmer-heavy codebases, Unity Events for designer-configured ones; (6) once every `UnityEngine.Input`/`Input.*` call site is converted and verified, flip Active Input Handling to "Input System Package (New)" to drop the legacy code path and its runtime cost. Legacy `Input.touches`/`Input.acceleration`/`Input.compositionString` (IME) and a few platform-specific APIs (some Game Controller / IME paths) don't yet have full 1:1 New-Input-System equivalents on every platform, so audit platform-specific manual pages (`MobileInput.html`, `IMEInput.html`, `android-input.html`, `ios-input.html`, `webgl-input.html`, `xr-input-overview.html`) for gaps before removing the legacy path entirely — this local doc set is the authoritative source for those platform caveats even though it lacks the Input System's own workflow manual.

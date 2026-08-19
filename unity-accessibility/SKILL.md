---
name: unity-accessibility
description: Use when implementing accessibility features in a Unity project — screen reader support, the Accessibility Hierarchy, or related APIs. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Accessibility

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Accessibility module overview | `Manual/com.unity.modules.accessibility.html`, `Manual/accessibility/_index.html`, `Manual/accessibility/module-intro.html` | What the Accessibility module covers, supported platforms, how to enable it via Package Manager's Built-in section |
| Module architecture & performance | `Manual/accessibility/module-architecture.html` | The six core components (AssistiveSupport, AccessibilityHierarchy, AccessibilityNode, IAccessibilityNotificationDispatcher, AccessibilitySettings, VisionUtility), module lifecycle stages (start/Update/LateUpdate/quit), and performance best practices |
| Accessibility concepts index | `Manual/accessibility/accessibility-concepts/_index.html` | Landing page linking screen-reader and fundamentals concept pages |
| Screen reader fundamentals | `Manual/accessibility/accessibility-concepts/fundamentals.html` | Baseline concepts: what accessibility means for Unity apps, why native UI systems aren't accessible by default |
| Screen reader intro | `Manual/accessibility/accessibility-concepts/screen-readers-intro.html` | Screen reader traversal model, roles/labels/values concepts, gesture-vs-announcement model |
| Screen reader get-started walkthrough | `Manual/accessibility/screen-readers-get-started.html` | Full worked example: UI Toolkit button → AccessibilityHierarchy → AccessibilityNode → frame tracking → invoked event wiring; also documents the built-in **Window > Accessibility > Hierarchy Viewer** debug tool |
| Accessibility module entry point (ScriptReference) | `ScriptReference/UnityEngine.AccessibilityModule.html` | Module-level index of every public type in `UnityEngine.Accessibility` |
| AccessibilityHierarchy class | `ScriptReference/Accessibility.AccessibilityHierarchy.html`, `Accessibility.AccessibilityHierarchy-ctor.html` | Class description: hierarchy operates independently of the UI hierarchy; empty-constructor signature `public AccessibilityHierarchy()` |
| AccessibilityHierarchy — adding nodes | `Accessibility.AccessibilityHierarchy.AddNode.html`, `Accessibility.AccessibilityHierarchy.InsertNode.html` | `AddNode(string label, AccessibilityNode parent)` appends; `InsertNode(int childIndex, string label, AccessibilityNode parent)` places at a specific position |
| AccessibilityHierarchy — removing/clearing | `Accessibility.AccessibilityHierarchy.RemoveNode.html`, `Accessibility.AccessibilityHierarchy.Clear.html` | Removing a single node (and its subtree) vs. resetting the whole hierarchy to empty (also clears screen reader focus) |
| AccessibilityHierarchy — moving nodes | `Accessibility.AccessibilityHierarchy.MoveNode.html` | `MoveNode(AccessibilityNode node, AccessibilityNode newParent, int newChildIndex)`; documents a **non-trivial per-platform cost** (high on iOS, moderate on macOS) when called on the active hierarchy |
| AccessibilityHierarchy — lookups | `Accessibility.AccessibilityHierarchy.TryGetNode.html`, `Accessibility.AccessibilityHierarchy.TryGetNodeAt.html`, `Accessibility.AccessibilityHierarchy.ContainsNode.html` | `TryGetNode(int id, out AccessibilityNode node)` by ID; `TryGetNodeAt` by hierarchy position; `ContainsNode` membership check |
| AccessibilityHierarchy — tree queries | `Accessibility.AccessibilityHierarchy.GetLowestCommonAncestor.html` | `GetLowestCommonAncestor(AccessibilityNode firstNode, AccessibilityNode secondNode)` — returns null if the two nodes share no ancestor |
| AccessibilityHierarchy — frame refresh | `Accessibility.AccessibilityHierarchy.RefreshNodeFrames.html` | Bulk-updates every node's `frame` from its `frameGetter` and calls `SendLayoutChanged(null)`; use on scroll/orientation-change, not per-frame |
| AccessibilityHierarchy — root nodes | `Accessibility.AccessibilityHierarchy-rootNodes.html` | `rootNodes` property — top-level nodes with no parent |
| AccessibilityNode class | `ScriptReference/Accessibility.AccessibilityNode.html` | Full class description: nodes are independent data structures, not tied to visual element lifecycle; lists supported `RuntimePlatform`s |
| AccessibilityNode — identity & tree fields | `Accessibility.AccessibilityNode-id.html`, `Accessibility.AccessibilityNode-parent.html`, `Accessibility.AccessibilityNode-children.html`, `Accessibility.AccessibilityNode-isActive.html` | Unique ID; parent/children tree links; `isActive` (default `true`) toggles screen-reader visibility without removing the node |
| AccessibilityNode — semantic content | `Accessibility.AccessibilityNode-label.html`, `Accessibility.AccessibilityNode-value.html`, `Accessibility.AccessibilityNode-hint.html`, `Accessibility.AccessibilityNode-role.html`, `Accessibility.AccessibilityNode-state.html` | The five fields a screen reader actually announces: what it is, its current value, extra guidance, its control type, its status |
| AccessibilityNode — screen geometry | `Accessibility.AccessibilityNode-frame.html`, `Accessibility.AccessibilityNode-frameGetter.html` | `frame` (Rect in screen coordinates) can be set directly or computed lazily via `frameGetter` (a `Func<Rect>`), which also backstops `RefreshNodeFrames` |
| AccessibilityNode — focus | `Accessibility.AccessibilityNode-isFocused.html`, `Accessibility.AccessibilityNode-focusChanged.html` | Whether the screen reader is currently on this node, and the main-thread event fired when that changes |
| AccessibilityNode — direct interaction | `Accessibility.AccessibilityNode-allowsDirectInteraction.html` | Whether raw touch passes through to the element instead of being intercepted by screen-reader gestures (e.g., for custom drawing surfaces) |
| AccessibilityNode — interaction events | `Accessibility.AccessibilityNode-invoked.html`, `Accessibility.AccessibilityNode-dismissed.html`, `Accessibility.AccessibilityNode-scrolled.html` | Fired on the screen reader's "activate", "dismiss", and "scroll" gestures respectively |
| AccessibilityNode — increment/decrement events | `Accessibility.AccessibilityNode-incremented.html`, `Accessibility.AccessibilityNode-decremented.html` | Fired on "increment"/"decrement" gestures (e.g., adjusting a slider without dragging) |
| AccessibilityNode — deprecated event | `Accessibility.AccessibilityNode-selected.html` | `selected` is **deprecated — use `invoked` instead** (confirmed in current docs) |
| AccessibilityNode — utility overrides | `Accessibility.AccessibilityNode.GetHashCode.html`, `Accessibility.AccessibilityNode.ToString.html` | Standard hash/debug-string overrides |
| AccessibilityRole enum | `Accessibility.AccessibilityRole.html` | Enum overview + a full worked C# example assigning roles per `VisualElement` type and building custom UI Toolkit controls for roles with no built-in element |
| AccessibilityRole values | `Accessibility.AccessibilityRole.Button.html`, `.Container.html`, `.Dropdown.html`, `.Header.html`, `.Image.html`, `.KeyboardKey.html`, `.None.html`, `.ScrollView.html`, `.SearchField.html`, `.Slider.html`, `.StaticText.html`, `.TabBar.html`, `.TabButton.html`, `.TextField.html`, `.Toggle.html` | Every concrete role value; `None` is the explicit fallback when no role fits — combine with a strong `label`/`hint` instead |
| AccessibilityState enum | `Accessibility.AccessibilityState.html` | Bit-flag-style enum for a node's current status (e.g., checked/disabled); `None` is the fallback, paired with `value`/`hint` for detail |
| AccessibilityState values | `Accessibility.AccessibilityState.None.html`, `.Disabled.html`, `.Selected.html`, `.Expanded.html` | Concrete state flags surfaced to screen readers |
| AccessibilityScrollDirection enum | `Accessibility.AccessibilityScrollDirection.html` | Direction reported to/via the `scrolled` event and `SendPageScrolledAnnouncement` |
| AccessibilityScrollDirection values | `.Unknown.html`, `.Up.html`, `.Down.html`, `.Left.html`, `.Right.html`, `.Forward.html`, `.Backward.html` | All seven direction values, including axis-relative Forward/Backward for non-2D scroll contexts |
| AssistiveSupport class | `ScriptReference/Accessibility.AssistiveSupport.html` | Entry point class description; documents supported `RuntimePlatform`s (Android 8+, iOS, macOS, Windows) and that platform behavior may differ slightly but stays consistent with native UI conventions |
| AssistiveSupport — screen reader status | `Accessibility.AssistiveSupport-isScreenReaderEnabled.html`, `Accessibility.AssistiveSupport-screenReaderStatusChanged.html` | `isScreenReaderEnabled` (static bool) for polling; `screenReaderStatusChanged` (main-thread event) for reacting live |
| AssistiveSupport — active hierarchy | `Accessibility.AssistiveSupport-activeHierarchy.html` | `activeHierarchy` — assigning it publishes the tree (and calls `SendScreenChanged(null)`); auto-reset to `null` when the screen reader turns off; **non-trivial cost** on iOS/macOS to build/tear down |
| AssistiveSupport — focus event | `Accessibility.AssistiveSupport-nodeFocusChanged.html` | Fires when the user moves screen-reader focus to a different node anywhere in the active hierarchy |
| AssistiveSupport — notification dispatcher | `Accessibility.AssistiveSupport-notificationDispatcher.html` | Exposes the `IAccessibilityNotificationDispatcher` singleton used to push change notifications to the screen reader |
| AssistiveSupport — status override | `Accessibility.AssistiveSupport-screenReaderStatusOverride.html`, `Accessibility.AssistiveSupport.ScreenReaderStatusOverride.html`, `.OSDriven.html`, `.ForceEnabled.html`, `.ForceDisabled.html` | Forces `isScreenReaderEnabled` to a fixed value for Editor/Player testing without a real assistive device; `OSDriven` is the default |
| IAccessibilityNotificationDispatcher interface | `ScriptReference/Accessibility.IAccessibilityNotificationDispatcher.html` | Implemented by `AssistiveSupport.notificationDispatcher`; warns that rapid repeated notifications of the same type may be coalesced/dropped by the screen reader |
| IAccessibilityNotificationDispatcher methods | `.SendAnnouncement.html`, `.SendLayoutChanged.html`, `.SendScreenChanged.html`, `.SendPageScrolledAnnouncement.html` | `SendAnnouncement(string)` for ambient one-off messages (no effect from an iOS button callback); `SendLayoutChanged`/`SendScreenChanged` for local vs. large-scale UI changes; `SendPageScrolledAnnouncement` for scroll completion |
| AccessibilitySettings class | `ScriptReference/Accessibility.AccessibilitySettings.html` | Read-only OS accessibility preference mirror; supported on Android and iOS only (not macOS/Windows) |
| AccessibilitySettings — font scale | `Accessibility.AccessibilitySettings-fontScale.html`, `Accessibility.AccessibilitySettings-fontScaleChanged.html` | User's OS text-size multiplier and its change event |
| AccessibilitySettings — bold text | `Accessibility.AccessibilitySettings-isBoldTextEnabled.html`, `Accessibility.AccessibilitySettings-boldTextStatusChanged.html` | OS bold-text preference and its change event |
| AccessibilitySettings — captions | `Accessibility.AccessibilitySettings-isClosedCaptioningEnabled.html`, `Accessibility.AccessibilitySettings-closedCaptioningStatusChanged.html` | OS closed-captioning preference and its change event |
| VisionUtility class | `ScriptReference/Accessibility.VisionUtility.html`, `Accessibility.VisionUtility.GetColorBlindSafePalette.html` | `GetColorBlindSafePalette` returns a palette distinguishable under normal vision, deuteranopia, protanopia, and tritanopia — general visual-accessibility helper independent of the screen-reader APIs |
| Player loop insertion points | `ScriptReference/PlayerLoop.PreLateUpdate.AccessibilityUpdate.html`, `ScriptReference/PlayerLoop.PostLateUpdate.AccessibilityLateUpdate.html` | Confirms the module-architecture doc's lifecycle claim: accessibility event batching runs in `PreLateUpdate`, automatic frame refresh runs in `PostLateUpdate` |

All 90 `Accessibility.*` ScriptReference pages plus the 7 Manual accessibility pages and the 2 `PlayerLoop` insertion-point pages above were re-verified on disk under `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` as part of this pass; nothing in this table is inferred from memory alone.

## Key Guidelines

### AccessibilityHierarchy & Node Trees

The `AccessibilityHierarchy` is a **separate data model that runs in parallel to your scene/UI hierarchy**, not a reflection of it — screen readers cannot see `GameObject`s, `VisualElement`s, or `RectTransform`s directly, only the tree of `AccessibilityNode`s you build and publish. A node's position in this tree (its `parent`/`children`, and its rank among `rootNodes`) determines screen-reader focus order and grouping; some platforms traverse depth-first through children before siblings, others use on-screen position (`frame`). Because nested structures are harder to navigate by voice, the module-architecture doc explicitly recommends flattening the hierarchy where possible. Build the hierarchy with `AddNode`/`InsertNode`, prune it with `RemoveNode`/`Clear`, and reparent/reorder with `MoveNode` — but treat `MoveNode` and reassigning `AssistiveSupport.activeHierarchy` as expensive operations on iOS/macOS when done against the *active* hierarchy, since Unity must rebuild native accessibility objects; batch structural changes rather than performing them one at a time on the live tree.

```csharp
using UnityEngine;
using UnityEngine.Accessibility;

public class MenuAccessibilityBuilder : MonoBehaviour
{
    AccessibilityHierarchy m_Hierarchy;

    void BuildMenuHierarchy()
    {
        m_Hierarchy = new AccessibilityHierarchy();

        // Root container groups the whole panel.
        AccessibilityNode panelNode = m_Hierarchy.AddNode("Main Menu", parent: null);
        panelNode.role = AccessibilityRole.Container;

        // Child nodes under the panel — order here drives read order.
        AccessibilityNode startNode = m_Hierarchy.AddNode("Start Game", panelNode);
        startNode.role = AccessibilityRole.Button;

        AccessibilityNode settingsNode = m_Hierarchy.AddNode("Settings", panelNode);
        settingsNode.role = AccessibilityRole.Button;

        // Insert a node at a specific position instead of appending.
        AccessibilityNode quitNode = m_Hierarchy.InsertNode(0, "Quit", panelNode);
        quitNode.role = AccessibilityRole.Button;

        // Find the lowest common ancestor of two nodes, e.g. for focus management.
        AccessibilityNode commonAncestor =
            m_Hierarchy.GetLowestCommonAncestor(startNode, settingsNode); // panelNode
    }
}
```

### AssistiveSupport & Screen Reader Detection

`AssistiveSupport` is the single entry point that connects your hierarchy to the OS: check `AssistiveSupport.isScreenReaderEnabled` to know whether assistive tech is active right now, and subscribe to `AssistiveSupport.screenReaderStatusChanged` to react the moment the user turns it on or off — build/refresh and assign your hierarchy in that handler rather than eagerly at startup, since Unity discards the active hierarchy whenever the screen reader is off (to save resources) and resets `activeHierarchy` to `null` automatically. Setting `AssistiveSupport.activeHierarchy` only takes effect while `isScreenReaderEnabled` is `true`; you must re-set it every time the screen reader is (re-)enabled, not just once. Use `AssistiveSupport.nodeFocusChanged` if you need to know which node the screen reader is currently on (e.g., to sync a visual focus highlight).

```csharp
using UnityEngine;
using UnityEngine.Accessibility;

public class ScreenReaderGate : MonoBehaviour
{
    AccessibilityHierarchy m_Hierarchy;

    void OnEnable()
    {
        AssistiveSupport.screenReaderStatusChanged += OnScreenReaderStatusChanged;
        // Handle the case where a screen reader is already on when this object enables.
        if (AssistiveSupport.isScreenReaderEnabled)
            OnScreenReaderStatusChanged(true);
    }

    void OnDisable()
    {
        AssistiveSupport.screenReaderStatusChanged -= OnScreenReaderStatusChanged;
    }

    void OnScreenReaderStatusChanged(bool enabled)
    {
        if (enabled)
        {
            m_Hierarchy ??= BuildHierarchy();
            AssistiveSupport.activeHierarchy = m_Hierarchy; // publish it
        }
        // No need to explicitly clear activeHierarchy on disable — Unity does it.
    }

    AccessibilityHierarchy BuildHierarchy() => new AccessibilityHierarchy();
}
```

### Labeling & Roles for Screen Readers

Every node needs a meaningful `label` (what the element *is*) and, where relevant, a `value` (its current state, such as a slider's number or a toggle's on/off), a `hint` (what happens if the user activates it), a `role` (`AccessibilityRole.Button`, `.Toggle`, `.Header`, `.Slider`, `.StaticText`, `.TextField`, `.SearchField`, `.Dropdown`, `.TabBar`/`.TabButton`, `.KeyboardKey`, `.ScrollView`, `.Container`, `.Image`, …), and a `state` (`AccessibilityState.Disabled`, `.Selected`, `.Expanded`, or `.None`). Generic or empty labels ("Button", "Image", "") are the single most common real-world screen-reader complaint — they leave the user with no idea what the control does. If nothing in `AccessibilityRole` fits, fall back to `AccessibilityRole.None` and put the missing semantic information into `label`/`hint` instead of leaving the role unset carelessly. Respond to the node's `invoked` (activate), `incremented`/`decremented` (steppers/sliders), `dismissed`, and `scrolled` events — do not use the deprecated `selected` event, which the docs explicitly say to replace with `invoked`.

```csharp
using UnityEngine.Accessibility;

void ConfigureVolumeSlider(AccessibilityHierarchy hierarchy, AccessibilityNode parent, int currentVolume)
{
    AccessibilityNode sliderNode = hierarchy.AddNode("Master Volume", parent);
    sliderNode.role = AccessibilityRole.Slider;
    sliderNode.value = $"{currentVolume}%";
    sliderNode.hint = "Adjusts the overall game volume";
    sliderNode.state = currentVolume == 0 ? AccessibilityState.None : AccessibilityState.None;

    sliderNode.incremented += () => SetVolume(currentVolume + 10);
    sliderNode.decremented += () => SetVolume(currentVolume - 10);
    sliderNode.invoked    += () => ToggleMute();
}
```

### Integrating with UI Toolkit/uGUI Widgets

The screen-reader APIs are UI-framework-agnostic — they work identically with UI Toolkit, uGUI, a custom framework, or non-UI 2D/3D world objects — because nodes are independent of any visual element's lifecycle. That independence cuts both ways: Unity does **not** auto-populate nodes for your widgets, and a node's `frame`/`label`/`state` will silently drift out of sync with the widget it represents unless you explicitly keep them updated. The standard pattern (per the official get-started walkthrough) is: create one `AccessibilityNode` per interactive/informative visual element, track the widget's `GeometryChangedEvent` (UI Toolkit) or its `RectTransform`/layout changes (uGUI) to recompute `frame` in screen coordinates (`worldBound.position * scaledPixelsPerPoint`, or equivalent), and wire the node's `invoked` event to the same code path the widget's own click/submit handler uses. For elements with a role that has no matching built-in `VisualElement` (custom controls used in UXML/UI Builder), the `AccessibilityRole` reference page has a worked example of authoring a custom control with the correct role assigned directly.

```csharp
using UnityEngine;
using UnityEngine.Accessibility;
using UnityEngine.UIElements;

public class AccessibleStartMenu : MonoBehaviour
{
    Button m_Button;
    AccessibilityHierarchy m_AccessibilityHierarchy;
    AccessibilityNode m_AccessibilityNode;

    void OnEnable()
    {
        VisualElement root = GetComponent<UIDocument>().rootVisualElement;
        m_Button = root.Q<Button>("startButton");
        m_Button.clicked += OnButtonClicked;
        m_Button.RegisterCallback<GeometryChangedEvent>(OnGeometryChanged);

        CreateAccessibilityHierarchy();
    }

    void OnDisable()
    {
        m_Button.clicked -= OnButtonClicked;
        m_Button.UnregisterCallback<GeometryChangedEvent>(OnGeometryChanged);
    }

    void CreateAccessibilityHierarchy()
    {
        m_AccessibilityHierarchy = new AccessibilityHierarchy();
        m_AccessibilityNode = m_AccessibilityHierarchy.AddNode(m_Button.text);
        m_AccessibilityNode.role = AccessibilityRole.Button;
        m_AccessibilityNode.state = m_Button.enabledSelf
            ? AccessibilityState.None
            : AccessibilityState.Disabled;
        // Route the screen reader's "activate" gesture to the same handler as a real tap.
        m_AccessibilityNode.invoked += () =>
            m_Button.SendEvent(NavigationSubmitEvent.GetPooled());
    }

    void OnGeometryChanged(GeometryChangedEvent evt)
    {
        Rect worldRect = m_Button.worldBound;
        float scale = m_Button.panel.scaledPixelsPerPoint;
        m_AccessibilityNode.frame = new Rect(worldRect.position * scale, worldRect.size * scale);
    }

    void OnButtonClicked() => Debug.Log("Start Game button clicked");
}
```

### Testing with Platform Screen Readers

This is a runtime-platform feature — it drives the *actual* OS screen reader (VoiceOver on iOS/macOS, TalkBack on Android, Narrator on Windows) inside a built Player — and is distinct from Unity Editor UX/accessibility. Use `AssistiveSupport.screenReaderStatusOverride` (values `OSDriven`, `ForceEnabled`, `ForceDisabled`) to force `isScreenReaderEnabled` to a known value in the Editor or a development Player when no real assistive device is attached, so your `screenReaderStatusChanged` handler and hierarchy-build path can be exercised without a physical accessibility session. That said, override testing is a smoke test, not a substitute for the real thing: gesture sets differ per platform (see Advanced Notes), and only an on-device VoiceOver/TalkBack/Narrator session exercises the actual native traversal, announcement timing, and gesture mapping. Unity also ships a built-in inspector for this: **Window > Accessibility > Hierarchy Viewer** lets you inspect live `AccessibilityNode` properties (label, role, state, frame) in Play Mode without leaving the Editor, which is the fastest way to catch structural mistakes before an on-device pass.

```csharp
using UnityEngine.Accessibility;

#if UNITY_EDITOR
public static class AccessibilityTestUtility
{
    public static void ForceScreenReaderForTesting(bool enabled)
    {
        AssistiveSupport.screenReaderStatusOverride = enabled
            ? AssistiveSupport.ScreenReaderStatusOverride.ForceEnabled
            : AssistiveSupport.ScreenReaderStatusOverride.ForceDisabled;
    }

    public static void RestoreOSDrivenStatus()
    {
        AssistiveSupport.screenReaderStatusOverride =
            AssistiveSupport.ScreenReaderStatusOverride.OSDriven;
    }
}
#endif
```

### Broader Accessibility Practices (beyond the API)

Screen-reader support is only one axis of accessibility. `AccessibilitySettings` mirrors OS-level preferences — `fontScale`, `isBoldTextEnabled`, `isClosedCaptioningEnabled` (Android and iOS only; unsupported on macOS/Windows Player) — so read these at startup and react to their `*Changed` events instead of hardcoding fixed font sizes or ignoring OS caption preferences. `VisionUtility.GetColorBlindSafePalette` returns a palette distinguishable under normal vision, deuteranopia, protanopia, and tritanopia — use it for any UI or gameplay signal (team colors, status indicators, minimap markers) that currently relies on hue alone, and pair it with contrast-conscious design and non-color redundant cues (icons, patterns, text). Beyond what any Unity API covers directly: ensure adequate color contrast ratios, make font/UI scale respect `fontScale` rather than fixed pixel sizes, and provide input remapping / alternative control schemes for players who cannot use the default scheme — these matter for players who are not screen-reader users at all, and a project can pass every screen-reader checklist item while still being inaccessible on these other axes.

```csharp
using UnityEngine;
using UnityEngine.Accessibility;

public class AccessibilityPreferencesSync : MonoBehaviour
{
    void OnEnable()
    {
        ApplyFontScale(AccessibilitySettings.fontScale);
        AccessibilitySettings.fontScaleChanged += ApplyFontScale;
        AccessibilitySettings.closedCaptioningStatusChanged += ApplyCaptionPreference;
    }

    void OnDisable()
    {
        AccessibilitySettings.fontScaleChanged -= ApplyFontScale;
        AccessibilitySettings.closedCaptioningStatusChanged -= ApplyCaptionPreference;
    }

    void ApplyFontScale(float scale) { /* scale UI Toolkit root's font-size or uGUI CanvasScaler */ }
    void ApplyCaptionPreference(bool enabled) { /* show/hide subtitle track */ }

    Color[] GetTeamColors(int count) =>
        VisionUtility.GetColorBlindSafePalette(count, minHueDistance: 0f, minSaturationDistance: 0f);
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Custom UI Toolkit/uGUI widgets never registered in the hierarchy | Unity doesn't auto-populate nodes for hand-rolled widgets; explicitly create and add an `AccessibilityNode` for each custom control |
| Nodes with empty or generic labels/roles | Copy-pasted node setup without per-element text; screen readers announce nothing useful — always set `label`/`role`/`value` per node |
| Never testing with a real platform screen reader | Editor-only testing hides real announcement/order bugs; use `screenReaderStatusOverride` in-editor, then verify on-device with VoiceOver/TalkBack/Narrator |
| Hierarchy built once at startup and never refreshed | Dynamic UI (menus opening/closing, lists scrolling) drifts from the tree; rebuild/refresh affected nodes when layout changes, and call `RefreshNodeFrames` after bulk layout shifts |
| Treating accessibility as screen-reader-only | Ignoring contrast, font scaling, and input remapping leaves non-screen-reader accessibility needs unaddressed even when the API checklist is "done" |
| Rebuilding/reassigning `activeHierarchy` every frame or on every minor change | `activeHierarchy` assignment and `MoveNode` on the active hierarchy have real native cost (high on iOS, moderate on macOS); batch structural changes instead of doing them continuously |
| Using the deprecated `selected` event | Docs mark `AccessibilityNode.selected` deprecated in favor of `invoked`; new code should never wire up `selected` |
| Assuming `activeHierarchy` stays set once assigned | Unity automatically resets `activeHierarchy` to `null` whenever the screen reader turns off, to save resources; you must re-assign it every time `screenReaderStatusChanged` fires `true`, not just once at app start |
| Never calling `SendLayoutChanged`/`SendScreenChanged` after structural edits | Modifying the active hierarchy's structure without notifying the dispatcher leaves the screen reader's internal state stale; use `SendLayoutChanged` for local changes and `SendScreenChanged` for large-scale ones |
| Spamming `SendAnnouncement` or other notifications every frame | The dispatcher docs warn that sending the same notification type in quick succession can cause the screen reader to skip some of them; send notifications only when something meaningfully changed |
| Calling `SendAnnouncement` from an iOS button callback and expecting it to work | Documented iOS-specific no-op case — announcements triggered from a button's own callback have no effect on iOS; trigger them from elsewhere in the flow |
| Forgetting `frame` is in screen coordinates, not local/world coordinates | Node frames must be computed as `worldBound.position * scaledPixelsPerPoint` (or the uGUI equivalent) — using raw local rect or world-space coordinates misplaces the on-screen hit target for position-based screen-reader navigation |
| Reading `AccessibilitySettings` properties on macOS/Windows and expecting real values | `AccessibilitySettings` (fontScale, bold text, captions) is documented as Android/iOS only; unlike `AssistiveSupport`, it has no macOS/Windows support |
| Deeply nested accessibility hierarchies mirroring deep GameObject hierarchies | The architecture doc explicitly recommends flattening — deep nesting makes screen-reader navigation slow and confusing even if the visual UI is shallow |
| Creating nodes for every visual element indiscriminately | Performance guidance recommends nodes only for elements that are interactive/informative and currently visible on screen; hierarchy size and update frequency both affect performance while a screen reader is active |

## Quick Reference

| Item | Purpose |
|------|---------|
| `AccessibilityHierarchy` | Tree of nodes exposed to the platform screen reader; independent of the visual UI hierarchy |
| `AccessibilityHierarchy()` (ctor) | Creates an empty hierarchy |
| `AccessibilityHierarchy.AddNode(label, parent)` | Creates and appends a node under `parent` (or at root if `null`) |
| `AccessibilityHierarchy.InsertNode(childIndex, label, parent)` | Creates and inserts a node at a specific position |
| `AccessibilityHierarchy.RemoveNode` | Removes a node (and its subtree) |
| `AccessibilityHierarchy.Clear` | Resets hierarchy to empty, clears screen-reader focus |
| `AccessibilityHierarchy.MoveNode(node, newParent, newChildIndex)` | Reparents/reorders a node; costly on iOS/macOS when active |
| `AccessibilityHierarchy.TryGetNode(id, out node)` | Look up a node by ID |
| `AccessibilityHierarchy.TryGetNodeAt` | Look up a node by hierarchy position |
| `AccessibilityHierarchy.ContainsNode` | Membership check |
| `AccessibilityHierarchy.GetLowestCommonAncestor(a, b)` | Finds shared ancestor of two nodes |
| `AccessibilityHierarchy.RefreshNodeFrames()` | Bulk-refreshes all node frames from `frameGetter`, notifies via `SendLayoutChanged` |
| `AccessibilityHierarchy.rootNodes` | Top-level nodes with no parent |
| `AccessibilityNode` | Single element: label, value, hint, role, state, frame, focus/invoke events |
| `AccessibilityNode.id` | Unique node identifier |
| `AccessibilityNode.parent` / `.children` | Tree links |
| `AccessibilityNode.isActive` | Whether the node is currently exposed to screen readers (default `true`) |
| `AccessibilityNode.label` | Short description announced by the screen reader |
| `AccessibilityNode.value` | Current value/state text (e.g. slider number) |
| `AccessibilityNode.hint` | Extra guidance on what interacting does |
| `AccessibilityNode.role` | Control-type semantic (`AccessibilityRole`) |
| `AccessibilityNode.state` | Status flags (`AccessibilityState`) |
| `AccessibilityNode.frame` | Bounding rect in screen coordinates |
| `AccessibilityNode.frameGetter` | `Func<Rect>` that computes `frame` lazily/automatically |
| `AccessibilityNode.isFocused` / `.focusChanged` | Current screen-reader focus state and its change event |
| `AccessibilityNode.allowsDirectInteraction` | Lets raw touch bypass screen-reader gesture interception |
| `AccessibilityNode.invoked` | Fired on screen-reader "activate" gesture |
| `AccessibilityNode.dismissed` | Fired on "dismiss" gesture |
| `AccessibilityNode.scrolled` | Fired on "scroll" gesture |
| `AccessibilityNode.incremented` / `.decremented` | Fired on "increment"/"decrement" gestures |
| `AccessibilityNode.selected` | Deprecated — use `invoked` |
| `AccessibilityRole` (enum) | Button, Toggle, Header, Slider, StaticText, TextField, SearchField, Dropdown, Container, Image, KeyboardKey, ScrollView, TabBar, TabButton, None |
| `AccessibilityState` (enum) | None, Disabled, Selected, Expanded |
| `AccessibilityScrollDirection` (enum) | Unknown, Up, Down, Left, Right, Forward, Backward |
| `AssistiveSupport` | Entry point — screen reader status, active hierarchy, status/focus-changed events, notification dispatcher |
| `AssistiveSupport.isScreenReaderEnabled` | Static bool — is a screen reader on right now |
| `AssistiveSupport.screenReaderStatusChanged` | Event fired when the OS screen reader is toggled |
| `AssistiveSupport.activeHierarchy` | The hierarchy currently published to the screen reader |
| `AssistiveSupport.nodeFocusChanged` | Event fired when screen-reader focus moves to a different node |
| `AssistiveSupport.notificationDispatcher` | The `IAccessibilityNotificationDispatcher` instance |
| `AssistiveSupport.screenReaderStatusOverride` | Forces `isScreenReaderEnabled` for testing |
| `AssistiveSupport.ScreenReaderStatusOverride` (enum) | `OSDriven` (default), `ForceEnabled`, `ForceDisabled` |
| `IAccessibilityNotificationDispatcher` | Interface for pushing notifications to the screen reader |
| `IAccessibilityNotificationDispatcher.SendAnnouncement(string)` | Ambient one-off message; no effect from an iOS button callback |
| `IAccessibilityNotificationDispatcher.SendLayoutChanged` | Notifies of a local layout change, optionally refocuses a node |
| `IAccessibilityNotificationDispatcher.SendScreenChanged` | Notifies of a large-scale screen change, optionally refocuses a node |
| `IAccessibilityNotificationDispatcher.SendPageScrolledAnnouncement` | Notifies that a scroll action completed |
| `AccessibilitySettings` | Read-only OS accessibility preferences (Android/iOS only) |
| `AccessibilitySettings.fontScale` / `.fontScaleChanged` | OS text-size multiplier and its change event |
| `AccessibilitySettings.isBoldTextEnabled` / `.boldTextStatusChanged` | OS bold-text preference and its change event |
| `AccessibilitySettings.isClosedCaptioningEnabled` / `.closedCaptioningStatusChanged` | OS captioning preference and its change event |
| `VisionUtility.GetColorBlindSafePalette` | Colorblind-safe palette helper for general visual accessibility |
| `PlayerLoop.PreLateUpdate.AccessibilityUpdate` | Player-loop point where batched OS accessibility events are processed |
| `PlayerLoop.PostLateUpdate.AccessibilityLateUpdate` | Player-loop point where node frames auto-refresh after window move/resize |
| **Window > Accessibility > Hierarchy Viewer** | Editor tool for inspecting live `AccessibilityNode` properties in Play Mode |

## Advanced Notes

**Platform differences (VoiceOver vs. TalkBack vs. Narrator).** The Accessibility module is a platform-abstraction layer: you write one hierarchy and one set of node properties, and Unity translates them to each OS's native accessibility tree — but the docs are explicit that "these APIs might result in slight behavior differences across platforms," always in the direction of matching each platform's own native conventions rather than being identical everywhere. Concretely:
- **VoiceOver (iOS/iPadOS, macOS)** uses swipe-based gesture navigation (swipe right/left to move focus forward/back through the hierarchy, double-tap to activate — mapped to `invoked` — swipe up/down for `incremented`/`decremented` on sliders/steppers) and generally favors a **depth-first traversal** of the node tree. iOS is also where `AccessibilityHierarchy.MoveNode` and reassigning `AssistiveSupport.activeHierarchy` are documented as having their **highest** native cost, scaling with subtree/hierarchy size — minimize structural churn on the active hierarchy specifically on this platform. `SendAnnouncement` is documented to silently no-op when called from inside an iOS button's own callback.
- **TalkBack (Android, API 26+/Android 8.0+)** uses a comparable but distinct swipe/explore-by-touch gesture set and reads `label`/`value`/`hint`/`role`/`state` through Android's `AccessibilityNodeInfo` equivalent. Android is the only other platform (besides iOS) where `AccessibilitySettings` (font scale, bold text, closed captioning) actually returns live OS values — macOS and Windows Players do not support `AccessibilitySettings` at all, only `AssistiveSupport`/hierarchy features.
- **Narrator (Windows)** and **VoiceOver (macOS)** are supported by `AssistiveSupport`/`AccessibilityHierarchy` per the class docs' platform list, but `AccessibilitySettings` has no effect there — plan font-scale/caption/bold-text handling as mobile-only behavior, with desktop platforms needing their own settings UI if you want equivalent user control.
- Regardless of platform, gestures vary but all map to the **same underlying node events** (`invoked`, `dismissed`, `scrolled`, `incremented`, `decremented`) — this is the entire point of the abstraction: you never branch your gameplay code on `Application.platform` to handle accessibility interaction, only to decide whether to read `AccessibilitySettings` (mobile) at all.

**How the Accessibility Hierarchy relates to the visual UI tree.** The two trees are deliberately decoupled: an `AccessibilityNode` "exists and functions independently of its corresponding visual elements," per the `AccessibilityNode` class docs — hiding, reordering, or restyling a `VisualElement`/`RectTransform` does **not** automatically change the visibility, position, or content of the node that represents it. This is why every one of the worked examples above wires explicit synchronization: `GeometryChangedEvent` → `frame` updates, `enabledSelf`/interactable state → `AccessibilityNode.state`, and widget click handlers ↔ `invoked`. Practical consequences:
- A node with `isActive = true` but a `frame` that was never updated to match its widget's current position will report a stale hit-target to the screen reader (relevant on platforms that use frame-based rather than pure tree-order navigation).
- Destroying/hiding a `VisualElement` without also calling `RemoveNode` (or setting `isActive = false`) leaves a "ghost" node in the hierarchy that a screen reader can still focus on even though nothing is visually there.
- The accessibility tree can be **shallower or differently shaped** than the visual tree by design — the architecture doc recommends flattening it for navigability, so a deeply nested UI Toolkit/uGUI layout should often collapse to a flatter accessibility structure (e.g., group a whole card into one `Container` node with a synthesized label rather than exposing every internal decorative child).
- Because the accessibility hierarchy is UI-framework-agnostic, the same mechanism covers pure 3D/2D world objects with no UI representation at all (e.g., an interactable object in the game world) — you build a node for it exactly as you would for a UI Toolkit button, there's just no `VisualElement` on the other end to keep in sync.
- Only the hierarchy attached to the application's **main window** is exposed to screen readers; content on secondary displays/windows has no accessibility representation regardless of how its visual tree is structured.

---
name: unity-ui
description: Use when building Unity UI — deciding between UI Toolkit (UXML/USS) and uGUI (Canvas/RectTransform), or implementing either. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity UI

## Retrieval Sources

All paths below were verified with `find`/`ls` against `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` on this pass. **Confirmed gap, re-checked and still true:** the local `ScriptReference/` has no API pages for the uGUI package's `UnityEngine.UI` namespace — `EventSystem`, `CanvasScaler`, `GraphicRaycaster`, `Selectable`, `Button`, `Image`, `Text`, `ScrollRect`, `Toggle`, etc. do not exist anywhere under `ScriptReference/` (checked both an `EventSystem*.html` glob and a `UI.*.html` glob at the ScriptReference root — zero hits for either). Only `UnityEngine.UIModule.html` (the built-in, non-package UI module) is present, and it covers exactly seven types: `Canvas`, `CanvasGroup`, `CanvasRenderer`, `RectTransform`/`RectTransformUtility`, `RenderMode`, `AdditionalCanvasShaderChannels`, `StandaloneRenderResize`. For uGUI component APIs (Button, Selectable, EventSystem, etc.), state plainly that local docs don't cover them and fall back to general knowledge, flagging it as unverified against local sources.

| Source | Path | Use for |
|--------|------|---------|
| UI systems comparison | `Manual/UI-system-compare.html` | Deciding UI Toolkit vs uGUI for a given use case |
| UI Toolkit overview | `Manual/UIToolkits.html` | UI Toolkit concepts, panels, when to use it |
| UI Toolkit intro (systems index) | `Manual/ui-systems/introduction-ui-toolkit.html` | Shorter conceptual on-ramp to UI Toolkit, links into the ui-systems/ subtree |
| Best-practice guide: intro to UI Toolkit | `Manual/best-practice-guides/introduction-to-ui-toolkit.html` | Curated workflow guidance beyond the reference manual |
| uGUI package manual | `Manual/com.unity.ugui.html` | uGUI package description: "GameObject-based UI system that uses Components and the Game View" |
| UI Toolkit module page | `Manual/com.unity.modules.uielements.html` | What ships in the built-in UIElements module |
| uGUI module page | `Manual/com.unity.modules.ui.html` | What ships in the built-in UI module (Canvas/RectTransform live here, not in a package) |
| UXML structure | `Manual/UIE-UXML.html` | Writing UXML markup, visual tree structure |
| UXML element reference: VisualElement | `Manual/UIE-uxml-element-VisualElement.html` | Base UXML element's attributes (one of 100+ per-control UXML reference pages under `Manual/UIE-uxml-element-*.html`) |
| USS styling | `Manual/UIE-USS.html` | USS selectors, properties, styling elements |
| USS selectors overview | `Manual/UIE-USS-Selectors.html` | Class/name/type/pseudo-class/descendant/child selector syntax |
| USS supported properties | `Manual/UIE-USS-SupportedProperties.html` | Full USS property list incl. `-unity-slice-*` (nine-slicing), flex layout properties |
| USS properties reference table | `Manual/UIE-USS-Properties-Reference.html` | Animatable-vs-not table for every USS property |
| UI Builder getting started | `Manual/UIB-getting-started.html` | Visual UXML/USS authoring tool workflow |
| UI Builder interface overview | `Manual/UIB-interface-overview.html` | UI Builder panes: Hierarchy, Viewport, StyleSheets, Inspector |
| Runtime UI event/input handling | `Manual/UIE-Runtime-Event-System.html` | How input reaches UI Toolkit panels at runtime |
| Runtime panel settings | `Manual/UIE-Runtime-Panel-Settings.html` | `PanelSettings` asset fields: scale mode, reference resolution, sort order |
| Create a UI Toolkit runtime panel | `Manual/UIE-create-panel.html` | Step-by-step UIDocument + PanelSettings runtime setup |
| How-to: create runtime UI | `Manual/UIE-HowTo-CreateRuntimeUI.html` | Worked runtime UI Toolkit example |
| Coordinate & position system | `Manual/UIE-coordinate-and-position-system.html` | UI Toolkit's coordinate space, position: relative vs absolute |
| UQuery | `Manual/UIE-UQuery.html` | Querying the visual tree from C# (`.Q<T>()`, `.Query()`) |
| Events overview | `Manual/UIE-Events.html` | Event system model: trickle-down + bubble-up phases |
| Events reference | `Manual/UIE-Events-Reference.html` | Full list of UI Toolkit event classes (PointerDownEvent, ClickEvent, etc.) |
| Click events | `Manual/UIE-Click-Events.html` | ClickEvent specifics |
| Pointer events | `Manual/UIE-Pointer-Events.html` | PointerDownEvent/PointerMoveEvent/etc. specifics |
| Manipulators | `Manual/UIE-manipulators.html` | Reusable drag/click behavior objects attached via `AddManipulator` |
| Custom controls | `Manual/UIE-custom-controls.html` | Authoring new C# VisualElement subclasses with UxmlFactory/UxmlTraits |
| Layout engine | `Manual/UIE-LayoutEngine.html` | Yoga-based flexbox layout engine UI Toolkit uses |
| Draw order | `Manual/UIE-draw-order.html` | How visual tree order determines paint order/z-index-like stacking |
| Masking | `Manual/UIE-masking.html` | Clipping child content (`overflow: hidden` equivalent) |
| Transform (UI Toolkit) | `Manual/UIE-Transform.html` | translate/rotate/scale on VisualElements |
| Visual tree overview | `Manual/UIE-VisualTree.html` | The VisualElement tree model itself |
| Visual tree landing page | `Manual/UIE-VisualTree-landing.html` | Index into the visual-tree topic cluster |
| Panels overview | `Manual/UIE-panels.html` | Editor panels vs runtime panels distinction |
| Style sheets overview | `Manual/UIE-style-sheet.html` | StyleSheet asset mechanics, cascade, specificity |
| About USS | `Manual/UIE-about-uss.html` | USS vs CSS differences (subset, no cascade from external files by default, etc.) |
| Transitioning from uGUI | `Manual/UIE-Transitioning-From-UGUI.html` | Direct uGUI → UI Toolkit concept mapping |
| Runtime performance considerations | `Manual/UIE-performance-consideration-runtime.html` | Perf guidance specific to runtime (non-Editor) UI Toolkit panels |
| Usage hints for draw calls | `Manual/UIE-use-usage-hints-to-reduce-draw-calls-and-geometry-regeneration.html` | `usageHints` flags to cut batching/geometry-rebuild cost |
| Relative/absolute positioning example | `Manual/UIE-relative-absolute-positioning-example.html` | Worked example of `position: absolute` inside a flex layout |
| World Space UI (UI Toolkit) | `Manual/ui-systems/world-space-ui.html` | Rendering a UI Toolkit panel onto a world-space mesh/texture |
| World Space UI: size/position examples | `Manual/ui-systems/world-space-ui-size-and-position-examples.html` | Worked sizing/positioning math for world-space panels |
| Create world-space UI | `Manual/ui-systems/create-world-space-ui.html` | Step-by-step world-space UI Toolkit setup |
| Canvas class reference | `ScriptReference/Canvas.html` | uGUI Canvas render modes, sorting, scale factor |
| CanvasGroup class reference | `ScriptReference/CanvasGroup.html` | Group-level alpha/interactable/blocksRaycasts fade & disable pattern |
| CanvasRenderer class reference | `ScriptReference/CanvasRenderer.html` | Low-level uGUI mesh renderer backing every Graphic |
| RenderMode enum reference | `ScriptReference/RenderMode.html` | The three Canvas render-mode enum values |
| RectTransform class reference | `ScriptReference/RectTransform.html` | uGUI anchoring, pivot, offsets for responsive layout |
| RectTransformUtility class reference | `ScriptReference/RectTransformUtility.html` | Screen↔local point conversion, world corners, bounds calculation |
| UIDocument class reference | `ScriptReference/UIElements.UIDocument.html` | Component hosting a UI Toolkit visual tree in a scene |
| PanelSettings class reference | `ScriptReference/UIElements.PanelSettings.html` | Full field list: scaleMode, referenceResolution, sortingOrder, targetTexture |
| VisualElement class reference | `ScriptReference/UIElements.VisualElement.html` | Base class API: Add/Remove/Query/style/class-list members |
| VisualTreeAsset class reference | `ScriptReference/UIElements.VisualTreeAsset.html` | Compiled UXML asset; `.Instantiate()` / `.CloneTree()` |
| StyleSheet class reference | `ScriptReference/UIElements.StyleSheet.html` | Compiled USS asset type |
| UIModule namespace index | `ScriptReference/UnityEngine.UIModule.html` | Confirms exactly which uGUI-adjacent types are built-in (not package) locally |
| UIElementsModule namespace index | `ScriptReference/UnityEngine.UIElementsModule.html` | Namespace index for the UI Toolkit runtime module |

That is 56 verified rows — well past the 12-20 target — spanning UI Toolkit fundamentals, UXML/USS authoring, UI Builder, the runtime event system, world-space UI, custom controls/manipulators, performance, and the uGUI Canvas/RectTransform surface that does exist locally.

## Key Guidelines

### UI Toolkit vs uGUI Decision

UI Toolkit is Unity's modern retained-mode UI system: a browser-like model with a visual tree (UXML), a CSS subset for styling (USS), and a Yoga-based flexbox layout engine (`Manual/UIE-LayoutEngine.html`). It decouples structure, style, and behavior into separate files/languages the way web front-ends do. uGUI (`Manual/com.unity.ugui.html`) is the older GameObject-based system: every element is a GameObject with a `RectTransform` plus Components (`Image`, `Button`, `Text`, etc.), styled and behaviorally scripted directly on those Components/MonoBehaviours, and arranged live in the Game View. Per `Manual/UI-system-compare.html`, prefer UI Toolkit for complex editor tooling, data-heavy runtime UI (lists, dynamic dashboards), and anywhere draw-call/layout-rebuild cost matters at scale; prefer uGUI where the asset-store/prefab ecosystem, designer-driven Inspector-only workflows, or existing project conventions dominate. They are not mutually exclusive within one project — a game can render its HUD in uGUI while using UI Toolkit for a settings/inventory screen — but avoid mixing them for the *same* screen since input routing, layout math, and asset pipelines don't share state.

```
Decision checklist (Manual/UI-system-compare.html):
- Editor tooling / custom inspectors / EditorWindow        -> UI Toolkit (only supported path)
- Runtime UI with large dynamic lists/data binding          -> UI Toolkit (ListView, data binding)
- Runtime UI needing 3rd-party asset-store components fast  -> uGUI
- World-space diegetic UI (health bars in 3D space)          -> uGUI is simpler; UI Toolkit needs
                                                                 a RenderTexture-backed world panel
                                                                 (Manual/ui-systems/world-space-ui.html)
- Team is designers who work entirely in the Inspector       -> uGUI
```

### UXML/USS Authoring

UXML is XML that declares the visual tree's structure (which elements exist and how they nest); USS is a CSS subset that declares appearance (`Manual/UIE-UXML.html`, `Manual/UIE-USS.html`). A `VisualTreeAsset` (the compiled UXML) is instantiated at runtime or loaded by a `UIDocument`; USS files are attached either in UXML via `<Style src="...">` or in code via `visualElement.styleSheets.Add(...)`. Selectors support type (`Button`), class (`.my-class`), name (`#my-name`), descendant (space), direct child (`>`), and pseudo-classes (`:hover`, `:checked`) — see `Manual/UIE-USS-Selectors.html`. Every built-in control has its own UXML reference page under `Manual/UIE-uxml-element-*.html` (e.g. `UIE-uxml-element-Button.html`, `UIE-uxml-element-ListView.html`) listing that element's specific attributes.

```xml
<!-- UXML: MainMenu.uxml -->
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <Style src="MainMenu.uss" />
    <ui:VisualElement name="root" class="menu-root">
        <ui:Label text="Settings" class="menu-title" />
        <ui:Button name="start-btn" text="Start Game" class="menu-button" />
        <ui:Button name="quit-btn" text="Quit" class="menu-button menu-button--danger" />
    </ui:VisualElement>
</ui:UXML>
```

```css
/* USS: MainMenu.uss */
.menu-root {
    flex-direction: column;
    align-items: center;
    padding: 24px;
}
.menu-button {
    width: 200px;
    height: 40px;
    margin-top: 8px;
}
.menu-button:hover {
    scale: 1.05 1.05;
}
.menu-button--danger {
    background-color: rgb(180, 40, 40);
}
```

```csharp
// C#: load the compiled UXML and wire up behavior
[SerializeField] VisualTreeAsset menuAsset; // assign MainMenu.uxml in Inspector
void OnEnable()
{
    var root = GetComponent<UIDocument>().rootVisualElement;
    root.Clear();
    menuAsset.CloneTree(root);
    root.Q<Button>("start-btn").clicked += () => Debug.Log("Start pressed");
}
```

### Canvas & RectTransform (uGUI)

Every uGUI element sits on a `RectTransform` (`ScriptReference/RectTransform.html`), a 2D specialization of `Transform` that stores `anchorMin`, `anchorMax`, `anchoredPosition`, `sizeDelta`, `pivot`, and `offsetMin`/`offsetMax` instead of raw world position/scale. A `Canvas` (`ScriptReference/Canvas.html`) is the root render surface; its `renderMode` (`ScriptReference/RenderMode.html`) picks the render path — Screen Space - Overlay draws directly to the screen with no camera; Screen Space - Camera renders through an assigned camera at a fixed plane distance (`Canvas.planeDistance`); World Space treats the Canvas as an ordinary 3D object with a `RectTransform` sized in world units, usable for diegetic/VR UI. `CanvasRenderer` (`ScriptReference/CanvasRenderer.html`) is the low-level mesh renderer every `Graphic` uses internally — rarely touched directly. `CanvasGroup` (`ScriptReference/CanvasGroup.html`) lets you fade (`alpha`), disable interaction (`interactable`), and block/allow raycasts (`blocksRaycasts`) for an entire subtree in one component, which is the standard way to implement fade-in/out panels or temporarily-disabled UI without touching every child.

```csharp
using UnityEngine;

// Fully stretch a RectTransform to fill its parent, then inset by 20px on all sides.
void FillParentWithMargin(RectTransform rt, float margin = 20f)
{
    rt.anchorMin = Vector2.zero;   // bottom-left of parent
    rt.anchorMax = Vector2.one;    // top-right of parent
    rt.pivot = new Vector2(0.5f, 0.5f);
    rt.offsetMin = new Vector2(margin, margin);   // left/bottom inset
    rt.offsetMax = new Vector2(-margin, -margin); // right/top inset
}

// Fade a panel out and stop it from blocking raycasts, via CanvasGroup.
IEnumerator FadeOut(CanvasGroup group, float duration)
{
    float t = 0f;
    group.interactable = false;
    while (t < duration)
    {
        t += Time.deltaTime;
        group.alpha = Mathf.Lerp(1f, 0f, t / duration);
        yield return null;
    }
    group.blocksRaycasts = false;
}
```

### Responsive Layout / Anchoring

In uGUI, anchors (not raw position) define responsive behavior: `anchorMin`/`anchorMax` are normalized (0-1) points on the *parent* rect that the element's corners lock to. When `anchorMin == anchorMax` (a "point anchor"), the element has a fixed size and `anchoredPosition` is the offset from that anchor point — it does not resize when the parent does. When `anchorMin != anchorMax` (a "stretch anchor", e.g. min `(0,0)` max `(1,1)`), the element's edges track the parent's edges and it resizes with the parent; in this mode `offsetMin`/`offsetMax` (exposed in the Inspector as Left/Bottom/Right/Top) become the inset from those edges rather than a size. `RectTransformUtility` (`ScriptReference/RectTransformUtility.html`) provides the screen-to-local conversions (`ScreenPointToLocalPointInRectangle`, `WorldToScreenPoint`) needed for custom input handling, drag-and-drop, or converting a world-space marker into 2D HUD position. In UI Toolkit, responsive layout instead comes from flexbox (`flex-grow`, `flex-shrink`, `align-items`, `justify-content` in USS) plus the `PanelSettings` scale mode (`Manual/UIE-Runtime-Panel-Settings.html`) — `Constant Pixel Size`, `Scale With Screen Size` (with a reference resolution and width/height match slider), or `Constant Physical Size`, directly analogous to uGUI's `CanvasScaler` modes.

```csharp
using UnityEngine;

// Convert a fixed-size RectTransform to a top-right-anchored HUD element
// that keeps a constant 16px margin regardless of parent resize.
void AnchorTopRight(RectTransform rt, Vector2 size, float margin = 16f)
{
    rt.anchorMin = new Vector2(1f, 1f);
    rt.anchorMax = new Vector2(1f, 1f);
    rt.pivot = new Vector2(1f, 1f);
    rt.sizeDelta = size;
    rt.anchoredPosition = new Vector2(-margin, -margin);
}
```

### Runtime Event System

uGUI needs exactly one `EventSystem` in the scene to route input (pointer/gamepad/keyboard) to `Selectable`s such as `Button`; a `GraphicRaycaster` on each interactive Canvas determines which `Graphic` a pointer hit. **Local docs do not cover these types' APIs** (see Retrieval Sources note) — this paragraph is drawn from general Unity knowledge, not the local ScriptReference. UI Toolkit's runtime input model is different and better documented locally: `Manual/UIE-Runtime-Event-System.html` describes how the active input backend (legacy Input Manager or the Input System package) is translated into UI Toolkit's own event classes (`PointerDownEvent`, `ClickEvent`, `NavigationMoveEvent`, etc., enumerated in `Manual/UIE-Events-Reference.html`) and dispatched through the visual tree in two phases — TrickleDown (root-to-target) then BubbleUp (target-to-root), per `Manual/UIE-Events.html`. Each `UIDocument`'s `PanelSettings` asset is the runtime-panel equivalent of a Canvas+EventSystem pairing — it owns sort order, scaling, and (indirectly, via the shared input backend) how input reaches that panel. Register handlers with `RegisterCallback<T>` on any `VisualElement`, or attach a reusable `Manipulator` (`Manual/UIE-manipulators.html`) such as `PointerManipulator`-derived drag logic instead of hand-rolling callback registration on every element.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class DragHandle : PointerManipulator
{
    public DragHandle(VisualElement target) { this.target = target; }

    protected override void RegisterCallbacksOnTarget()
    {
        target.RegisterCallback<PointerDownEvent>(OnPointerDown, TrickleDown.TrickleDown);
        target.RegisterCallback<PointerUpEvent>(OnPointerUp);
    }
    protected override void UnregisterCallbacksFromTarget()
    {
        target.UnregisterCallback<PointerDownEvent>(OnPointerDown, TrickleDown.TrickleDown);
        target.UnregisterCallback<PointerUpEvent>(OnPointerUp);
    }
    void OnPointerDown(PointerDownEvent evt) => target.CapturePointer(evt.pointerId);
    void OnPointerUp(PointerUpEvent evt) => target.ReleasePointer(evt.pointerId);
}

// usage: myElement.AddManipulator(new DragHandle(myElement));
```

### UQuery, Custom Controls, and Data Access

`Manual/UIE-UQuery.html` documents the LINQ-like query API (`.Q<Button>("start-btn")`, `.Query<Label>(className: "menu-title").ToList()`) used to find descendants of the visual tree by name, type, or USS class — the UI Toolkit equivalent of `GameObject.Find`/`GetComponentInChildren`. For a reusable widget, `Manual/UIE-custom-controls.html` covers subclassing `VisualElement` and registering a `UxmlFactory`/`UxmlTraits` (or the newer `[UxmlElement]`/`[UxmlAttribute]` source-generator attributes) so the control can be dropped into UXML/UI Builder like any built-in element.

```csharp
using UnityEngine.UIElements;

[UxmlElement]
public partial class HealthBar : VisualElement
{
    [UxmlAttribute] public float Value { get; set; } = 1f;

    public HealthBar()
    {
        style.height = 20;
        style.backgroundColor = new StyleColor(Color.gray);
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| UI breaks at other resolutions | Anchors left center-fixed instead of relative to parent edges/stretch; set anchor presets to match intended responsive behavior (see Responsive Layout / Anchoring above). |
| Poor batching / dropped frames (uGUI) | Too many Canvases, or one Canvas mixing static and frequently-updated elements; split dynamic UI into its own Canvas so a rebuild doesn't dirty the whole hierarchy. |
| Buttons don't respond to clicks (uGUI) | No `EventSystem` in the scene, or no `GraphicRaycaster` on the Canvas, or another full-screen `Graphic` with `raycastTarget` on is intercepting the hit above it. |
| CanvasScaler misconfigured | Fixed pixel size used instead of Scale With Screen Size, so UI is wrong size on other devices/aspect ratios; local docs don't cover `CanvasScaler`'s API directly (see Retrieval Sources note) — the UI Toolkit analogue, `PanelSettings.scaleMode`, is documented at `Manual/UIE-Runtime-Panel-Settings.html`. |
| UXML/USS not applying | Style sheet not attached to the `UIDocument`'s root/`VisualTreeAsset` via `<Style src="...">` or `styleSheets.Add`, or class names mismatched between UXML `class="..."` and USS `.selector` names. |
| VisualElement never appears | `UIDocument.visualTreeAsset` not assigned, or the element was added to a detached tree (never parented under `rootVisualElement`), or its USS resolves `display: none`. |
| Runtime UI Toolkit panel invisible in build | `PanelSettings` not assigned to the `UIDocument`, or the theme style sheet (`PanelSettings.themeStyleSheet`) is missing so default control styling (e.g. Button chrome) never resolves. |
| Layout looks right in Editor, wrong at runtime | Editor UI (UIElements in an `EditorWindow`) and runtime UI (`UIDocument` in a scene) use the same UXML/USS language but different default styling/theme — always test runtime panels in Play Mode or a build, not just the UI Builder preview. |
| Clicking through world-space UI to objects behind it | World Space Canvas (uGUI) or a world-space UI Toolkit panel (`Manual/ui-systems/world-space-ui.html`) needs its own collider/raycast setup — a `GraphicRaycaster` alone doesn't stop 3D physics raycasts from passing through. |
| Nine-slice image looks stretched/blurry at borders | `-unity-slice-*` USS properties (`Manual/UIE-USS-SupportedProperties.html#unity-slice`) not set to match the source sprite's actual border pixels; in uGUI the equivalent is the sprite's Border values in the Sprite Editor, not a USS property. |
| Deep nesting kills UI Toolkit layout perf | Every added/removed/resized VisualElement can trigger a Yoga layout pass (`Manual/UIE-LayoutEngine.html`); batch structural changes and avoid rebuilding large subtrees every frame — use `usageHints` (`Manual/UIE-use-usage-hints-to-reduce-draw-calls-and-geometry-regeneration.html`) for elements that animate every frame. |
| Confusing UQuery `Q()` scope | `.Q<T>()` searches descendants only, not the element it's called on, and stops at the first match in document order — an unnamed duplicate control elsewhere in the tree can silently steal a query intended for a different instance. |
| Manipulator not receiving events | Forgot to call `target.AddManipulator(...)`, or `RegisterCallbacksOnTarget`/`UnregisterCallbacksFromTarget` aren't paired correctly, leaking a handler or never attaching one. |
| Treating USS as full CSS | UI Toolkit's USS is a documented subset (`Manual/UIE-about-uss.html`) — no CSS Grid, no arbitrary cascading `@import` of runtime-computed values, and only the properties listed in `Manual/UIE-USS-SupportedProperties.html` are recognized; unsupported properties fail silently rather than erroring loudly. |
| Assuming `RectTransform.rect` is world space | `rect` is always in the element's own local space with the origin at its own pivot; use `RectTransformUtility.WorldToScreenPoint`/`GetWorldCorners` (`ScriptReference/RectTransformUtility.html`) to go to screen/world space. |

## Quick Reference

| Component/Concept | System | Purpose |
|--------------------|--------|---------|
| `Canvas` | uGUI | Root render surface; render mode (`ScriptReference/RenderMode.html`) + sort order + scale factor |
| `CanvasScaler` | uGUI | Scales Canvas contents for different resolutions/DPI (not in local ScriptReference — see gap note) |
| `CanvasRenderer` | uGUI | Low-level mesh renderer backing every `Graphic` (`ScriptReference/CanvasRenderer.html`) |
| `CanvasGroup` | uGUI | Group alpha/interactable/blocksRaycasts control for a whole subtree (`ScriptReference/CanvasGroup.html`) |
| `RectTransform` | uGUI | 2D layout transform: anchors, pivot, offsets, sizeDelta (`ScriptReference/RectTransform.html`) |
| `RectTransformUtility` | uGUI | Static helpers: screen↔local/world point conversion, bounds (`ScriptReference/RectTransformUtility.html`) |
| `EventSystem` | uGUI | Scene-wide manager routing input to Selectables (not in local ScriptReference — see gap note) |
| `GraphicRaycaster` | uGUI | Per-Canvas raycaster determining which Graphic a pointer hit (not in local ScriptReference) |
| `Selectable` / `Button` / `Toggle` / `Image` / `Text` | uGUI | Interactive/visual Components (not in local ScriptReference) |
| `UIDocument` | UI Toolkit | Hosts a UI Toolkit visual tree in a scene (`ScriptReference/UIElements.UIDocument.html`) |
| `PanelSettings` | UI Toolkit | Runtime panel scaling, rendering target, sort order, theme (`ScriptReference/UIElements.PanelSettings.html`) |
| `VisualElement` | UI Toolkit | Base class for every node in the visual tree (`ScriptReference/UIElements.VisualElement.html`) |
| `VisualTreeAsset` | UI Toolkit | Compiled UXML asset; `.Instantiate()`/`.CloneTree()` (`ScriptReference/UIElements.VisualTreeAsset.html`) |
| `StyleSheet` | UI Toolkit | Compiled USS asset (`ScriptReference/UIElements.StyleSheet.html`) |
| UXML | UI Toolkit | XML markup defining a visual tree's structure (`Manual/UIE-UXML.html`) |
| USS | UI Toolkit | CSS-subset style language for visual tree appearance (`Manual/UIE-USS.html`) |
| UI Builder | UI Toolkit | Visual UXML/USS authoring tool (`Manual/UIB-getting-started.html`) |
| UQuery | UI Toolkit | LINQ-like descendant query API: `.Q<T>()`, `.Query<T>()` (`Manual/UIE-UQuery.html`) |
| Manipulator | UI Toolkit | Reusable pointer/gesture behavior object attached via `AddManipulator` (`Manual/UIE-manipulators.html`) |
| Layout engine (Yoga) | UI Toolkit | Flexbox layout solver driving all USS `flex-*`/`align-*` properties (`Manual/UIE-LayoutEngine.html`) |
| Event trickle/bubble | UI Toolkit | Two-phase event dispatch: TrickleDown then BubbleUp (`Manual/UIE-Events.html`) |
| `usageHints` | UI Toolkit | Per-element rendering hints to reduce draw calls/geometry rebuilds for animated elements |
| `-unity-slice-*` | UI Toolkit (USS) | Nine-slice border properties for background images (`Manual/UIE-USS-SupportedProperties.html#unity-slice`) |
| World Space Canvas / world-space panel | uGUI / UI Toolkit | 3D-positioned UI for diegetic/VR use (uGUI: Canvas render mode; UI Toolkit: `Manual/ui-systems/world-space-ui.html`) |

## Advanced Notes

**Editor-only vs runtime UI Toolkit.** UI Toolkit is one API used in two very different contexts, and code/USS that works in one doesn't automatically work in the other. Editor UI (custom `EditorWindow`s, Inspectors) uses the Editor's own implicit panel and default Editor theme, and can freely use Editor-only APIs (`ObjectField`, `PropertyField`, `IMGUIContainer` interop per `Manual/UIE-IMGUI-migration.html`). Runtime UI requires an explicit `UIDocument` + `PanelSettings` pair (`Manual/UIE-Runtime-Panel-Settings.html`, `Manual/UIE-create-panel.html`), must reference a `themeStyleSheet` for built-in controls to render with any chrome at all (there is no implicit Editor theme at runtime), and is subject to the runtime performance guidance in `Manual/UIE-performance-consideration-runtime.html` that doesn't apply to Editor tooling. Always verify a runtime panel in Play Mode/a build — the UI Builder's canvas preview approximates but does not guarantee runtime appearance, especially for theme-dependent default control styling.

**Nine-slicing.** Both systems support nine-slice scaling (stretching a bordered image without distorting its corners), but through different mechanisms: uGUI configures border pixels on the `Sprite` asset itself (Sprite Editor border handles), consumed automatically by `Image` when its Image Type is set to Sliced; UI Toolkit instead sets the `-unity-slice-left/right/top/bottom` USS properties (`Manual/UIE-USS-SupportedProperties.html#unity-slice`) directly on the element consuming the `background-image`, independent of any metadata baked into the source texture. Mismatched slice values (USS or Sprite Editor) relative to the image's actual border pixels is the most common cause of blurry or stretched-looking button/panel corners in both systems.

**World-space UI differs structurally between systems.** uGUI's World Space Canvas is just a `Canvas` whose `RectTransform` is treated as an ordinary 3D object sized in world units — no extra render target is involved. UI Toolkit has no native "3D transform" for a panel; instead a runtime panel is rendered into a `RenderTexture` (via `PanelSettings.targetTexture`) which is then displayed on a `MeshRenderer`/quad in the scene, per `Manual/ui-systems/world-space-ui.html` and the worked math in `Manual/ui-systems/world-space-ui-size-and-position-examples.html`. This means UI Toolkit world-space UI incurs an extra render-texture pass uGUI's approach does not.

**Migrating from uGUI to UI Toolkit.** `Manual/UIE-Transitioning-From-UGUI.html` is a direct concept-to-concept map (Canvas → Panel/UIDocument, RectTransform anchors → USS flexbox/positioning, Image/Text Components → VisualElement/Label with USS, prefab variants → UXML templates) and is the fastest way to translate an existing uGUI screen rather than redesigning it from scratch in UI Toolkit idioms.

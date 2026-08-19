---
name: unity-ui-toolkit-runtime
description: Use when writing C# code against UI Toolkit's runtime API — building custom VisualElement controls, manipulators, data binding, or querying/mutating the UI Toolkit element tree from script. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity UI Toolkit Runtime API

This skill is the deep-dive into UI Toolkit's C# `UnityEngine.UIElements` runtime surface: custom controls, manipulators, event handling, data binding, and scripted style/query access. It deliberately does **not** cover UXML/USS authoring basics, UI Builder, or the UI Toolkit-vs-uGUI decision — see `unity-ui` for those. Assume the reader already has a `UIDocument` showing UXML content and wants to know how to drive, extend, and manipulate it from code.

## Retrieval Sources

All paths below were verified with `find`/`ls` against `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` on this pass. The local `ScriptReference/UIElements.*` namespace alone contains 3,535 pages (one per member, not per class), so this table is a curated set of the load-bearing types/members for runtime scripting, not an exhaustive index — use `find ScriptReference -iname "UIElements.<ClassName>*"` to pull every member page for a class this table names.

| Source | Path | Use for |
|--------|------|---------|
| VisualElement class reference | `ScriptReference/UIElements.VisualElement.html` | Core API surface: hierarchy (`Add`/`Remove`/`Insert`/`Children`), `Q`/query entry points, `style`/`resolvedStyle`, `schedule`, pseudo-states, `userData` |
| VisualElement.Hierarchy struct | `ScriptReference/UIElements.VisualElement.Hierarchy.html` | Low-level child manipulation bypassing `contentContainer` redirection (`Add`, `Insert`, `RemoveAt`, `Sort`) |
| VisualElement-contentContainer | `ScriptReference/UIElements.VisualElement-contentContainer.html` | Why `Add()` on a composite control may not add where you expect; override point for custom containers |
| VisualElement-style | `ScriptReference/UIElements.VisualElement-style.html` | Writable inline `IStyle` accessor for scripted style overrides |
| VisualElement-resolvedStyle | `ScriptReference/UIElements.VisualElement-resolvedStyle.html` | Read-only computed/cascaded `IResolvedStyle` after USS+inline+layout resolution |
| VisualElement-customStyle | `ScriptReference/UIElements.VisualElement-customStyle.html` | `ICustomStyle` accessor for reading custom USS properties (`--my-prop`) declared via `[UxmlAttribute]`-free USS extension |
| VisualElement-layout | `ScriptReference/UIElements.VisualElement-layout.html` | Post-layout `Rect` (position+size) in parent space |
| VisualElement-worldBound / localBound | `ScriptReference/UIElements.VisualElement-worldBound.html` | World-space vs local-space bounding rect, needed for hit-testing/placement math |
| VisualElement-panel | `ScriptReference/UIElements.VisualElement-panel.html` | The `IPanel` this element is attached to; null until attached, key null-check for lifecycle bugs |
| VisualElement-generateVisualContent | `ScriptReference/UIElements.VisualElement-generateVisualContent.html` | Callback for custom immediate-mode drawing (`MeshGenerationContext`) inside a control |
| VisualElement.MarkDirtyRepaint | `ScriptReference/UIElements.VisualElement.MarkDirtyRepaint.html` | Force a repaint after mutating state that `generateVisualContent` depends on |
| VisualElement-usageHints | `ScriptReference/UIElements.VisualElement-usageHints.html` | `UsageHints` flags (`DynamicTransform`, `GroupTransform`) to cut cost of frequently-animated elements |
| VisualElement-schedule | `ScriptReference/UIElements.VisualElement-schedule.html` | Entry point to the element's scheduler for polling/delayed/repeating work |
| IVisualElementScheduler / ScheduledItem | `ScriptReference/UIElements.IVisualElementScheduler.html`, `ScriptReference/UIElements.IVisualElementScheduledItem.html` | Fluent scheduling API: `.Execute(...)`, `.Every(ms)`, `.Until(...)`, `.StartingIn(ms)` |
| VisualElementExtensions | `ScriptReference/UIElements.VisualElementExtensions.html` | `AddManipulator`/`RemoveManipulator`, `StretchToParentSize`, coordinate conversion helpers |
| UQueryBuilder\<T\> | `ScriptReference/UIElements.UQueryBuilder_1.html` | Fluent chained query filters returned by `.Query<T>()`: `.Name()`, `.Class()`, `.OfType()`, `.Where()`, `.ForEach()`, `.ToList()`, `.Build()` |
| UQueryState\<T\> | `ScriptReference/UIElements.UQueryState_1.html` | Reusable, cached compiled query object returned by `.Build()` — re-run without re-parsing the query |
| CallbackEventHandler | `ScriptReference/UIElements.CallbackEventHandler.html` | Base class providing `RegisterCallback<T>`/`UnregisterCallback<T>`/`RegisterCallbackOnce<T>` — the actual home of event registration (`VisualElement` inherits it) |
| Manipulator (interface/base) | `ScriptReference/UIElements.Manipulator.html` | `target` property + `RegisterCallbacksOnTarget`/`UnregisterCallbacksFromTarget` contract every manipulator implements |
| MouseManipulator | `ScriptReference/UIElements.MouseManipulator.html` | Legacy mouse-event manipulator base with `activators` (`ManipulatorActivationFilter` list) for button/modifier gating |
| PointerManipulator | `ScriptReference/UIElements.PointerManipulator.html` | Pointer-event manipulator base (mouse+touch+pen unified); preferred over `MouseManipulator` for new code |
| Clickable | `ScriptReference/UIElements.Clickable.html` | Built-in manipulator backing `Button`'s click behavior; reusable for custom clickable controls |
| ContextualMenuManipulator | `ScriptReference/UIElements.ContextualMenuManipulator.html` | Right-click/context-menu manipulator wiring into `ContextualMenuPopulateEvent` |
| ClickEvent | `ScriptReference/UIElements.ClickEvent.html` | High-level click event (fires after a matched down+up within tap thresholds) |
| PointerDownEvent / PointerUpEvent / PointerMoveEvent | `ScriptReference/UIElements.PointerDownEvent.html`, `ScriptReference/UIElements.PointerUpEvent.html`, `ScriptReference/UIElements.PointerMoveEvent.html` | Low-level pointer lifecycle events manipulators build on |
| PointerCaptureHelper | `ScriptReference/UIElements.PointerCaptureHelper.html` | `CapturePointer`/`ReleasePointer`/`HasPointerCapture` extension methods so drags keep receiving events off-element |
| EventBase / EventBase\<T\> | `ScriptReference/UIElements.EventBase.html`, `ScriptReference/UIElements.EventBase_1.html` | Base event class: `target`, `currentTarget`, `propagationPhase`, `StopPropagation`, `PreventDefault`, pooled-event lifecycle |
| EventCallback\<TEventType\> | `ScriptReference/UIElements.EventCallback_1.html` | Delegate type every `RegisterCallback<T>` call expects |
| TrickleDown enum | `ScriptReference/UIElements.TrickleDown.html` | `TrickleDown.NoTrickleDown` (default, bubble phase) vs `TrickleDown.TrickleDown` (capture phase) passed to `RegisterCallback` |
| PropagationPhase enum | `ScriptReference/UIElements.PropagationPhase.html` | `TrickleDown`/`AtTarget`/`BubbleUp` values read from `evt.propagationPhase` inside a handler |
| EventInterestAttribute | `ScriptReference/UIElements.EventInterestAttribute.html` | Perf annotation declaring which event types a custom element actually needs, skipping dispatch overhead for the rest |
| IEventHandler | `ScriptReference/UIElements.IEventHandler.html` | Minimal interface the event system dispatches to; implemented by `CallbackEventHandler`/`VisualElement` |
| Focusable / FocusController | `ScriptReference/UIElements.Focusable.html`, `ScriptReference/UIElements.FocusController.html` | Keyboard-focus base class and the panel-level controller managing focus ring/tab order |
| KeyDownEvent / NavigationMoveEvent / ExecuteCommandEvent | `ScriptReference/UIElements.KeyDownEvent.html`, `ScriptReference/UIElements.NavigationMoveEvent.html`, `ScriptReference/UIElements.ExecuteCommandEvent.html` | Keyboard, D-pad/gamepad navigation, and platform command (copy/paste) event types |
| ChangeEvent\<T\> | `ScriptReference/UIElements.ChangeEvent_1.html` | Generic value-changed event fired by every `BaseField<T>`-derived control |
| INotifyValueChanged\<T\> | `ScriptReference/UIElements.INotifyValueChanged_1.html` | Interface a control implements to participate in `SetValueWithoutNotify`/`RegisterValueChangedCallback` value plumbing |
| BaseField\<T\> | `ScriptReference/UIElements.BaseField_1.html` | Base class for all built-in value-editing controls; the template for a custom value field |
| BaseBoolField | `ScriptReference/UIElements.BaseBoolField.html` | Concrete example of a `BaseField<bool>` subclass (backs `Toggle`) |
| BindableElement | `ScriptReference/UIElements.BindableElement.html` | `VisualElement` subclass adding `bindingPath` for the legacy `SerializedObject` binding path |
| BindableElement-bindingPath | `ScriptReference/UIElements.BindableElement-bindingPath.html` | The UXML `binding-path` attribute's backing property |
| IBindable / IBinding | `ScriptReference/UIElements.IBindable.html`, `ScriptReference/UIElements.IBinding.html` | Contracts for legacy Editor `SerializedObject`/`SerializedProperty` binding |
| BindingExtensions | `ScriptReference/UIElements.BindingExtensions.html` | `Bind`, `BindProperty`, `Unbind`, `TrackPropertyValue`, `TrackSerializedObjectValue` — the Editor-Inspector binding entry points |
| Binding (runtime) | `ScriptReference/UIElements.Binding.html` | Base class for the newer runtime data-binding system's binding objects; `MarkDirty`, `OnActivated`/`OnDeactivated`/`OnDataSourceChanged` lifecycle |
| DataBinding | `ScriptReference/UIElements.DataBinding.html` | Concrete general-purpose runtime `Binding` connecting a `dataSourcePath` to a target property via `bindingId` |
| BindingId | `ScriptReference/UIElements.BindingId.html` | Identifies a bindable property on a control (used with `[CreateProperty]`-generated bindable properties) |
| BindingMode | `ScriptReference/UIElements.BindingMode.html` | `ToTarget`, `ToSource`, `TwoWay`, `ToTargetOnce` — direction/frequency of a runtime binding |
| BindingResult / BindingStatus | `ScriptReference/UIElements.BindingResult.html`, `ScriptReference/UIElements.BindingStatus.html` | Per-update binding outcome (`Success`/`Pending`/`Failure`) surfaced for diagnostics |
| BindingLogLevel / Binding.SetGlobalLogLevel | `ScriptReference/UIElements.BindingLogLevel.html`, `ScriptReference/UIElements.Binding.SetGlobalLogLevel.html` | Controls console verbosity for failed/pending binding resolution — essential when a binding silently does nothing |
| INotifyBindablePropertyChanged | `ScriptReference/UIElements.INotifyBindablePropertyChanged.html` | Interface a plain C# data-source object implements to push change notifications into the binding system |
| IDataSourceViewHashProvider | `ScriptReference/UIElements.IDataSourceViewHashProvider.html` | Lets a custom data source report a cheap "did anything change" hash instead of the binding system diffing every read |
| ConverterGroup / ConverterGroups | `ScriptReference/UIElements.ConverterGroup.html`, `ScriptReference/UIElements.ConverterGroups.html` | Registering custom type converters (e.g. `int` &lt;-&gt; enum-as-string) for runtime data binding |
| DataSourceContext | `ScriptReference/UIElements.DataSourceContext.html` | Resolved `(dataSource, dataSourcePath)` pair returned by `VisualElement.GetDataSourceContext()` |
| VisualElement-dataSource / dataSourcePath | `ScriptReference/UIElements.VisualElement-dataSource.html`, `ScriptReference/UIElements.VisualElement-dataSourcePath.html` | Per-element scripted data-source assignment consumed by inherited/child bindings |
| VisualElement.SetBinding / GetBinding / ClearBinding(s) | `ScriptReference/UIElements.VisualElement.SetBinding.html`, `ScriptReference/UIElements.VisualElement.GetBinding.html`, `ScriptReference/UIElements.VisualElement.ClearBindings.html` | Scripted attach/detach of a `Binding` object to a `BindingId` on an element |
| VisualElement.GetBindingInfos / TryGetLastBindingToUIResult | `ScriptReference/UIElements.VisualElement.GetBindingInfos.html`, `ScriptReference/UIElements.VisualElement.TryGetLastBindingToUIResult.html` | Debugging API to inspect what's bound and the last update's result |
| UxmlElementAttribute | `ScriptReference/UIElements.UxmlElementAttribute.html` | `[UxmlElement]` source-generator attribute registering a `VisualElement` subclass as a UXML tag (Unity 6 replacement for `UxmlFactory`) |
| UxmlAttributeAttribute | `ScriptReference/UIElements.UxmlAttributeAttribute.html` | `[UxmlAttribute]` marking a C# property as a UXML-settable attribute |
| UxmlFactory\<T,U\> / UxmlTraits | `ScriptReference/UIElements.UxmlFactory_2.html`, `ScriptReference/UIElements.UxmlTraits.html` | Legacy (pre-source-generator) custom-control registration pattern; still needed for advanced attribute types |
| VisualElement.UxmlSerializedData | `ScriptReference/UIElements.VisualElement.UxmlSerializedData.html` | Backing serialized-data class the `[UxmlElement]` generator emits per control |
| IStyle | `ScriptReference/UIElements.IStyle.html` | Full writable style-property interface exposed by `VisualElement.style` |
| IResolvedStyle | `ScriptReference/UIElements.IResolvedStyle.html` | Read-only resolved/computed values exposed by `VisualElement.resolvedStyle` |
| StyleColor / StyleFloat / StyleLength / StyleEnum\<T\> | `ScriptReference/UIElements.StyleColor.html`, `ScriptReference/UIElements.StyleFloat.html`, `ScriptReference/UIElements.StyleLength.html`, `ScriptReference/UIElements.StyleEnum_1.html` | Wrapper structs every `IStyle` property returns, carrying a `StyleKeyword` (`Null`/`Auto`/`None`/`Undefined`) alongside the value |
| CustomStyleProperty\<T\> / ICustomStyle | `ScriptReference/UIElements.CustomStyleProperty_1.html`, `ScriptReference/UIElements.ICustomStyle.html` | Reading custom USS properties (`--my-color`) from C# via `customStyle.TryGetValue` |
| StyleSheet | `ScriptReference/UIElements.StyleSheet.html` | Compiled USS asset type; runtime `styleSheets.Add/Remove` target |
| VisualElement-styleSheets / VisualElementStyleSheetSet | `ScriptReference/UIElements.VisualElement-styleSheets.html`, `ScriptReference/UIElements.VisualElementStyleSheetSet.html` | Per-element attached-stylesheet collection, scriptable at runtime |
| VisualTreeAsset | `ScriptReference/UIElements.VisualTreeAsset.html` | Compiled UXML asset; `.Instantiate()`/`.CloneTree()` entry points for runtime instancing |
| UIDocument | `ScriptReference/UIElements.UIDocument.html` | Component hosting a visual tree in a scene: `rootVisualElement`, `visualTreeAsset`, `panelSettings`, `parentUI` (nested UIDocuments) |
| PanelSettings | `ScriptReference/UIElements.PanelSettings.html` | Runtime panel config: `scaleMode`, `referenceResolution`, `sortingOrder`, `targetTexture`, `themeStyleSheet` |
| PanelSettings-scaleMode | `ScriptReference/UIElements.PanelSettings-scaleMode.html` | The three runtime scale strategies (Constant Pixel Size / Scale With Screen Size / Constant Physical Size) |
| IPanel | `ScriptReference/UIElements.IPanel.html` | Root panel abstraction an attached `VisualElement.panel` returns; `visualTree`, `focusController`, `contextType` |
| IPanel-contextType | `ScriptReference/UIElements.IPanel-contextType.html` | Distinguishes `ContextType.Editor` vs `ContextType.Player` panels — relevant for code shared between EditorWindow and runtime UI |
| ListView / BaseListView / MultiColumnListView | `ScriptReference/UIElements.ListView.html`, `ScriptReference/UIElements.BaseListView.html`, `ScriptReference/UIElements.MultiColumnListView.html` | Virtualized list controls — the standard way to display large/dynamic runtime data without manually pooling elements |
| Manual: Data binding overview | `Manual/UIE-data-binding.html` | Conceptual overview distinguishing legacy `SerializedObject` binding from the newer general-purpose runtime binding system |
| Manual: Editor (SerializedObject) binding | `Manual/UIE-editor-binding.html` | The Inspector-oriented binding path (`BindableElement`/`bindingPath`/`Bind`) — Editor-only, not for gameplay data |
| Manual: Runtime binding overview | `Manual/UIE-runtime-binding.html` | The general-purpose C#-object binding system usable outside the Editor |
| Manual: Get started with runtime binding | `Manual/UIE-get-started-runtime-binding.html` | Minimal worked example wiring a plain C# class to UI via `[CreateProperty]` |
| Manual: Define a runtime binding data source | `Manual/UIE-runtime-binding-define-data-source.html` | How to mark a class/property bindable (`[CreateProperty]`, `INotifyBindablePropertyChanged`) |
| Manual: Runtime binding types | `Manual/UIE-runtime-binding-types.html` | Built-in `Binding` subclasses (`DataBinding` etc.) and when to write a custom one |
| Manual: Runtime binding custom types/converters | `Manual/UIE-runtime-binding-custom-types.html`, `Manual/UIE-create-runtime-binding-type-converter.html` | Registering a `ConverterGroup` for a type the binding system can't convert automatically |
| Manual: Runtime binding mode/update | `Manual/UIE-runtime-binding-mode-update.html` | `BindingMode` semantics and when re-evaluation happens (per-frame vs on-demand) |
| Manual: Runtime binding logging levels | `Manual/UIE-runtime-binding-logging-levels.html` | Diagnosing silently-failing bindings via `BindingLogLevel` |
| Manual: Bind to a list | `Manual/UIE-bind-to-list.html` | Binding a `ListView`/`ItemsSource` to a runtime collection |
| Manual: Bind custom control to data | `Manual/UIE-bind-custom-control-to-data.html`, `Manual/UIE-create-bindable-custom-control.html` | Making a hand-written custom control participate in data binding |
| Manual: Create custom controls | `Manual/UIE-create-custom-controls.html` | Canonical custom-control authoring walkthrough ([UxmlElement] + [UxmlAttribute]) |
| Manual: Create custom style for a custom control | `Manual/UIE-create-custom-style-custom-control.html` | Exposing custom USS properties (`--my-prop`) consumed via `ICustomStyle` |
| Manual: Events handling in a custom control | `Manual/UIE-events-handling-custom-control.html` | Registering/overriding event handling inside a custom `VisualElement` subclass |
| Manual: Events dispatching | `Manual/UIE-Events-Dispatching.html` | Mechanics of how `SendEvent`/the panel dispatcher walks the trickle/bubble chain |
| Manual: Events overview | `Manual/UIE-Events.html` | Two-phase (TrickleDown then BubbleUp) propagation model |
| Manual: Events reference | `Manual/UIE-Events-Reference.html` | Full enumerated list of event classes by category |
| Manual: Click events / Pointer events / Mouse events / Keyboard events / Focus events / Navigation events / Command events / Drag events / Layout events / Panel events / Tooltip events / Transition events | `Manual/UIE-Click-Events.html` (+ sibling `UIE-*-Events.html` pages) | Per-category event semantics and firing order |
| Manual: Capture events | `Manual/UIE-Capture-Events.html` | Pointer capture model (`CapturePointer`/`ReleasePointer`) for drag interactions that must keep tracking outside element bounds |
| Manual: Synthesizing events | `Manual/UIE-Events-Synthesizing.html` | Manually constructing/sending an event (`using (var evt = ClickEvent.GetPooled()) target.SendEvent(evt);`) for tests/simulated input |
| Manual: Manipulators | `Manual/UIE-manipulators.html` | Conceptual model: manipulators as reusable, composable input-behavior objects attached via `AddManipulator` |
| Manual: Runtime event system | `Manual/UIE-Runtime-Event-System.html` | How Input Manager / Input System input reaches a runtime panel and becomes UI Toolkit events |
| Manual: Runtime panel settings | `Manual/UIE-Runtime-Panel-Settings.html` | Full `PanelSettings` field reference for runtime panels |
| Manual: Create a UI Toolkit runtime panel | `Manual/UIE-create-panel.html` | Step-by-step `UIDocument`+`PanelSettings` runtime setup |
| Manual: Move elements at runtime | `Manual/UIE-move-elements-at-runtime.html` | Reparenting/repositioning visual-tree nodes from script after initial layout |
| Manual: Runtime performance considerations | `Manual/UIE-performance-consideration-runtime.html` | Layout/style-recalculation cost guidance specific to runtime (non-Editor) panels |
| Manual: Troubleshooting custom control library compilation | `Manual/UIE-troubleshooting-custom-control-library-compilation.html` | Common `[UxmlElement]` source-generator compile failures and fixes |
| Manual: Custom control attribute guides | `Manual/ui-systems/custom-control-attributes-built-in-types.html`, `Manual/ui-systems/custom-control-attributes-complex-data-types.html`, `Manual/ui-systems/custom-control-customize-uxml-attributes.html`, `Manual/ui-systems/custom-control-customize-uxml-tag-names.html` | `[UxmlAttribute]` usage for primitives vs complex types, renaming attributes/tags |
| Manual: Migrate a custom control | `Manual/ui-systems/migrate-custom-control.html` | Moving a `UxmlFactory`/`UxmlTraits` control to the `[UxmlElement]` source-generator pattern |
| Manual: Advanced data-binding best-practice guide | `Manual/best-practice-guides/ui-toolkit-for-advanced-unity-developers/data-binding.html` | Curated advanced patterns beyond the reference-manual binding pages |
| UnityEngine.UIElementsModule namespace index | `ScriptReference/UnityEngine.UIElementsModule.html` | Top-level namespace index confirming what ships in the built-in module vs the `com.unity.modules.uielements` package boundary |

That is 103 verified rows spanning the visual tree, event/manipulator system, both binding systems (legacy Editor `SerializedObject` binding and the newer runtime data-binding system), custom-control authoring, runtime style scripting, and `UIDocument`/`PanelSettings` runtime hosting.

## Key Guidelines

### VisualElement Tree Basics & Querying

Every node in a UI Toolkit tree — panels, containers, and built-in controls alike — is a `VisualElement` (`ScriptReference/UIElements.VisualElement.html`). Structural mutation goes through `Add`/`Insert`/`Remove`/`RemoveAt`/`Clear`, which by default target the element's `contentContainer` (`ScriptReference/UIElements.VisualElement-contentContainer.html`) rather than the element itself — a composite control (e.g. one with a header plus a body) commonly overrides `contentContainer` to redirect `Add()` into an inner child, and `hierarchy.Add` (via the `VisualElement.Hierarchy` struct, `ScriptReference/UIElements.VisualElement.Hierarchy.html`) is the escape hatch that bypasses that redirection when you need to add directly to the true root. Once attached, `panel` (`ScriptReference/UIElements.VisualElement-panel.html`) is non-null and `worldBound`/`localBound` become meaningful; treat `panel == null` as "not yet part of a live tree" and gate any panel-dependent logic on `AttachToPanelEvent`/`DetachFromPanelEvent`.

Finding elements uses two APIs layered on `UQueryState<T>` (`ScriptReference/UIElements.UQueryState_1.html`): the shorthand `element.Q<T>(name, className)` for "give me the first match," and the fluent `element.Query<T>().Class("foo").Where(...)` builder (`UQueryBuilder<T>`, `ScriptReference/UIElements.UQueryBuilder_1.html`) for multi-match filtering, ending in `.ToList()`/`.ForEach(...)`/`.Build()`. `.Build()` compiles the query into a reusable `UQueryState<T>` you can re-run cheaply — build it once (e.g. in a field) rather than re-parsing the query string every call.

```csharp
using UnityEngine.UIElements;

public class InventoryPanelController
{
    readonly VisualElement root;
    readonly UQueryState<VisualElement> slotQuery;

    public InventoryPanelController(VisualElement root)
    {
        this.root = root;
        // Compile once; re-run cheaply every time inventory state changes.
        slotQuery = root.Query<VisualElement>(className: "inventory-slot").Build();
    }

    public void RefreshHighlights(int selectedIndex)
    {
        int i = 0;
        slotQuery.ForEach(slot =>
        {
            slot.EnableInClassList("inventory-slot--selected", i == selectedIndex);
            i++;
        });
    }

    // Single-match lookup by name — the common case for wiring up one specific control.
    public Button FindConfirmButton() => root.Q<Button>("confirm-btn");
}
```

### Building Custom Controls ([UxmlElement]/[UxmlAttribute])

A custom control is a `VisualElement` subclass that composes built-in elements (or draws its own content) and exposes state as C# properties. `Manual/UIE-create-custom-controls.html` documents the modern pattern: mark the class `partial` and attach `[UxmlElement]` (`ScriptReference/UIElements.UxmlElementAttribute.html`), which triggers a source generator that emits the UXML-factory boilerplate (`VisualElement.UxmlSerializedData`, `ScriptReference/UIElements.VisualElement.UxmlSerializedData.html`) so the control becomes usable as a UXML tag and shows up in UI Builder's library. Any property meant to be settable from UXML gets `[UxmlAttribute]` (`ScriptReference/UIElements.UxmlAttributeAttribute.html`); primitive types (int/float/string/enum/Color) work directly, while complex types need the patterns in `Manual/ui-systems/custom-control-attributes-complex-data-types.html`. The older `UxmlFactory<T,U>`/`UxmlTraits` pattern (`ScriptReference/UIElements.UxmlFactory_2.html`, `ScriptReference/UIElements.UxmlTraits.html`) still works and is sometimes still necessary for attribute types the generator doesn't support — `Manual/ui-systems/migrate-custom-control.html` maps one pattern to the other.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

[UxmlElement]
public partial class HealthBar : VisualElement
{
    static readonly string ussFillClass = "health-bar__fill";

    readonly VisualElement fill;
    float value = 1f;

    [UxmlAttribute]
    public float Value
    {
        get => value;
        set
        {
            this.value = Mathf.Clamp01(value);
            fill.style.width = new Length(this.value * 100f, LengthUnit.Percent);
        }
    }

    [UxmlAttribute]
    public Color FillColor
    {
        get => fill.resolvedStyle.backgroundColor;
        set => fill.style.backgroundColor = value;
    }

    // Parameterless constructor is required — UI Builder/UXML instantiates via it.
    public HealthBar()
    {
        AddToClassList("health-bar");
        style.height = 18;
        style.backgroundColor = new StyleColor(new Color(0.15f, 0.15f, 0.15f));

        fill = new VisualElement();
        fill.AddToClassList(ussFillClass);
        fill.style.height = new Length(100, LengthUnit.Percent);
        fill.style.backgroundColor = new StyleColor(Color.red);
        hierarchy.Add(fill); // bypass any future contentContainer override

        Value = 1f;
    }
}

// Usage in UXML once compiled: <HealthBar value="0.75" fill-color="#22CC22" />
```

### Manipulators & Pointer Events

A `Manipulator` (`ScriptReference/UIElements.Manipulator.html`) packages reusable pointer/gesture behavior as an object attached to a target via `target.AddManipulator(...)` (`VisualElementExtensions.AddManipulator`, `ScriptReference/UIElements.VisualElementExtensions.AddManipulator.html`) instead of hand-registering callbacks inline. `Manual/UIE-manipulators.html` frames this as composition: a control can carry several independent manipulators (drag + context-menu + hover-highlight) without any of them knowing about each other. Subclass `PointerManipulator` (`ScriptReference/UIElements.PointerManipulator.html`, unifies mouse/touch/pen) or the older `MouseManipulator` (`ScriptReference/UIElements.MouseManipulator.html`, mouse-only, uses `ManipulatorActivationFilter` for button/modifier gating) and implement the paired `RegisterCallbacksOnTarget`/`UnregisterCallbacksFromTarget` (`ScriptReference/UIElements.Manipulator.RegisterCallbacksOnTarget.html`) — these are called automatically when the manipulator is added/removed, and every callback registered in one must be unregistered in the other or the target leaks a handler when the manipulator is removed.

`RegisterCallback<T>` (on `CallbackEventHandler`, `ScriptReference/UIElements.CallbackEventHandler.html`) takes an optional `TrickleDown` phase argument (`ScriptReference/UIElements.TrickleDown.html`): the default `TrickleDown.NoTrickleDown` registers for the bubble-up phase (event travels leaf→root), while `TrickleDown.TrickleDown` registers for the capture phase (root→leaf, fires first). A drag manipulator typically wants `PointerDownEvent` in TrickleDown so it sees the press before any child stops propagation, then calls `target.CapturePointer(evt.pointerId)` (`PointerCaptureHelper`, `ScriptReference/UIElements.PointerCaptureHelper.html`) so subsequent `PointerMoveEvent`s keep arriving even if the pointer leaves the element's bounds — essential for drag-to-resize/reorder UI, per `Manual/UIE-Capture-Events.html`.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

/// Drags `target` freely within its parent using absolute positioning.
public class DragManipulator : PointerManipulator
{
    Vector2 startPointerPos;
    Vector2 startElementPos;
    bool dragging;

    public DragManipulator(VisualElement target) => this.target = target;

    protected override void RegisterCallbacksOnTarget()
    {
        target.RegisterCallback<PointerDownEvent>(OnPointerDown);
        target.RegisterCallback<PointerMoveEvent>(OnPointerMove);
        target.RegisterCallback<PointerUpEvent>(OnPointerUp);
    }

    protected override void UnregisterCallbacksFromTarget()
    {
        target.UnregisterCallback<PointerDownEvent>(OnPointerDown);
        target.UnregisterCallback<PointerMoveEvent>(OnPointerMove);
        target.UnregisterCallback<PointerUpEvent>(OnPointerUp);
    }

    void OnPointerDown(PointerDownEvent evt)
    {
        if (evt.button != 0) return;
        dragging = true;
        startPointerPos = evt.position;
        startElementPos = new Vector2(target.resolvedStyle.left, target.resolvedStyle.top);
        target.CapturePointer(evt.pointerId);
        evt.StopPropagation();
    }

    void OnPointerMove(PointerMoveEvent evt)
    {
        if (!dragging || !target.HasPointerCapture(evt.pointerId)) return;
        Vector2 delta = (Vector2)evt.position - startPointerPos;
        target.style.left = startElementPos.x + delta.x;
        target.style.top = startElementPos.y + delta.y;
    }

    void OnPointerUp(PointerUpEvent evt)
    {
        if (!dragging) return;
        dragging = false;
        target.ReleasePointer(evt.pointerId);
    }
}

// usage: myPanel.AddManipulator(new DragManipulator(myPanel));
```

### Data Binding

UI Toolkit has two distinct binding systems and the docs are explicit that they don't interoperate (`Manual/UIE-data-binding.html`). The **legacy Editor binding** system connects `BindableElement`s (`ScriptReference/UIElements.BindableElement.html`) to a `SerializedObject`/`SerializedProperty` via `bindingPath` and `BindingExtensions.Bind`/`BindProperty` (`ScriptReference/UIElements.BindingExtensions.html`) — this only works inside the Editor (custom Inspectors, `EditorWindow`), documented at `Manual/UIE-editor-binding.html`. The **runtime binding** system (`Manual/UIE-runtime-binding.html`, `Manual/UIE-get-started-runtime-binding.html`) is general-purpose and works in builds: any plain C# object can be a data source, its bindable properties are exposed with `[CreateProperty]` (from `Unity.Properties`, consumed by the binding system), and a `VisualElement` is connected to a source path via `dataSource`/`dataSourcePath` (`ScriptReference/UIElements.VisualElement-dataSource.html`) plus either a UXML `binding` markup block or a scripted `SetBinding(bindingId, new DataBinding { ... })` call (`ScriptReference/UIElements.VisualElement.SetBinding.html`). `BindingMode` (`ScriptReference/UIElements.BindingMode.html`) controls direction: `ToTarget` (source→UI, one-way live), `ToTargetOnce` (source→UI, snapshot), `ToSource` (UI→source), `TwoWay`. A data source that wants to push live updates (not just be polled) implements `INotifyBindablePropertyChanged` (`ScriptReference/UIElements.INotifyBindablePropertyChanged.html`) and raises property-changed notifications; per `Manual/UIE-runtime-binding-define-data-source.html` this is what makes `ToTarget`/`TwoWay` bindings refresh without a manual poll loop.

```csharp
using Unity.Properties;
using UnityEngine.UIElements;

// Plain C# data source — no MonoBehaviour/ScriptableObject required.
public class PlayerStats : INotifyBindablePropertyChanged
{
    public event EventHandler<BindablePropertyChangedEventArgs> propertyChanged;

    int health = 100;
    [CreateProperty]
    public int Health
    {
        get => health;
        set
        {
            if (health == value) return;
            health = value;
            propertyChanged?.Invoke(this, new BindablePropertyChangedEventArgs(nameof(Health)));
        }
    }
}

public class StatsPanelController
{
    public StatsPanelController(VisualElement root, PlayerStats stats)
    {
        root.dataSource = stats;

        var healthLabel = root.Q<Label>("health-label");
        healthLabel.SetBinding("text", new DataBinding
        {
            dataSourcePath = new PropertyPath(nameof(PlayerStats.Health)),
            bindingMode = BindingMode.ToTarget,
        });
    }
}
```

For lists, bind a `ListView`'s `itemsSource` to a runtime collection (`Manual/UIE-bind-to-list.html`) rather than manually instantiating one element per item — `ListView`/`MultiColumnListView` (`ScriptReference/UIElements.ListView.html`, `ScriptReference/UIElements.MultiColumnListView.html`) virtualize rows, recycling a small pool of visual elements regardless of collection size. If a binding silently does nothing, raise `BindingLogLevel` (`ScriptReference/UIElements.BindingLogLevel.html`, `Manual/UIE-runtime-binding-logging-levels.html`) via `Binding.SetPanelLogLevel`/`Binding.SetGlobalLogLevel` before assuming the data source is wrong — most "binding doesn't update" bugs are a missing `[CreateProperty]`, a `PropertyPath` typo, or a `BindingMode` that doesn't include the direction you need, and the log level surfaces exactly which.

### Runtime Style Manipulation

`VisualElement.style` (`ScriptReference/UIElements.VisualElement-style.html`, typed as `IStyle`, `ScriptReference/UIElements.IStyle.html`) is the writable inline-style surface — every USS property has a same-named C# property here (`style.width`, `style.flexDirection`, `style.backgroundColor`, ...), and assigning to it is equivalent to an inline `style="..."` attribute: it wins over any USS selector regardless of specificity. Each property's value type wraps the raw value with a `StyleKeyword` (`StyleLength`, `StyleColor`, `StyleEnum<T>`, etc., e.g. `ScriptReference/UIElements.StyleLength.html`) so you can express `StyleKeyword.Null` (fall through to USS), `Auto`, `None`, or `Initial` in addition to a concrete value — assigning a raw value like `style.width = 100` implicitly wraps it as a concrete `StyleLength`, but clearing an inline override requires explicitly assigning `StyleKeyword.Null`, not just leaving it unset. `resolvedStyle` (`IResolvedStyle`, `ScriptReference/UIElements.IResolvedStyle.html`) is the read-only opposite: the final cascaded/computed value after USS + inline + layout, useful for reading "what size did this actually end up" after a layout pass, but never assignable.

Custom USS properties (`--my-highlight-color: red;`) aren't exposed on `IStyle`/`IResolvedStyle` at all — read them via `customStyle.TryGetValue` (`ICustomStyle`/`CustomStyleProperty<T>`, `ScriptReference/UIElements.ICustomStyle.html`) inside a `CustomStyleResolvedEvent` handler, which is how a custom control lets USS/UI Builder theme it without hardcoding colors in C#, per `Manual/UIE-create-custom-style-custom-control.html`.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class ThemedPanel : VisualElement
{
    static readonly CustomStyleProperty<Color> highlightColorProp = new("--highlight-color");
    Color highlightColor = Color.yellow; // fallback if USS doesn't define it

    public ThemedPanel()
    {
        RegisterCallback<CustomStyleResolvedEvent>(OnCustomStyleResolved);
    }

    void OnCustomStyleResolved(CustomStyleResolvedEvent evt)
    {
        if (customStyle.TryGetValue(highlightColorProp, out Color color))
            highlightColor = color;
    }

    public void Highlight(bool on)
    {
        // StyleKeyword.Null clears any inline override and falls back to USS/default.
        style.borderBottomColor = on
            ? new StyleColor(highlightColor)
            : new StyleColor(StyleKeyword.Null);
    }
}
```

### UIDocument & PanelSettings

`UIDocument` (`ScriptReference/UIElements.UIDocument.html`) is the MonoBehaviour that hosts a visual tree in a scene: it exposes `rootVisualElement` (the entry point for all `Q`/`Add` calls), takes a `visualTreeAsset` (`VisualTreeAsset.CloneTree()`, `ScriptReference/UIElements.VisualTreeAsset.html`, is what actually instantiates the UXML into live elements under the root), and references a `PanelSettings` asset (`ScriptReference/UIElements.PanelSettings.html`) that owns everything about how that panel renders: `scaleMode` (`ScriptReference/UIElements.PanelSettings-scaleMode.html` — Constant Pixel Size / Scale With Screen Size / Constant Physical Size, the runtime analogue of uGUI's `CanvasScaler`), `referenceResolution`, `sortingOrder` (multiple `UIDocument`s can share one `PanelSettings` and layer via this), and `targetTexture` (redirect the panel into a `RenderTexture` instead of the screen, the basis for world-space UI Toolkit panels per `unity-ui`'s coverage). Multiple `UIDocument`s can also nest via `parentUI`, letting one panel's tree host another as a subtree rather than each owning an independent root. `Manual/UIE-create-panel.html` and `Manual/UIE-Runtime-Panel-Settings.html` walk the full setup; critically, a runtime panel needs `PanelSettings.themeStyleSheet` assigned or built-in controls (`Button`, `Toggle`, etc.) render with zero default chrome — there is no implicit Editor theme at runtime.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

[RequireComponent(typeof(UIDocument))]
public class MainMenuController : MonoBehaviour
{
    [SerializeField] VisualTreeAsset menuAsset;

    void OnEnable()
    {
        var doc = GetComponent<UIDocument>();
        VisualElement root = doc.rootVisualElement;
        root.Clear();
        menuAsset.CloneTree(root);

        root.Q<Button>("start-btn").clicked += OnStartClicked;
        root.Q<Button>("quit-btn").clicked += () => Application.Quit();
    }

    void OnStartClicked() => Debug.Log("Starting game...");
}
```

### Event System (bubbling/trickling)

Every UI Toolkit event dispatch walks the tree in two phases (`Manual/UIE-Events.html`, `Manual/UIE-Events-Dispatching.html`): **TrickleDown** (capture), root-to-target, then **BubbleUp** (default), target-to-root. `PropagationPhase` (`ScriptReference/UIElements.PropagationPhase.html`) tells a handler which phase it's currently in (`AtTarget` when `currentTarget == target`). `evt.StopPropagation()` halts further phase traversal (a common footgun: it does not automatically prevent the event's *default* action — call `evt.PreventDefault()` separately if that's also needed, e.g. suppressing a click's default behavior while still consuming the event). `EventInterestAttribute` (`ScriptReference/UIElements.EventInterestAttribute.html`) lets a custom `VisualElement` declare up front which event categories it actually handles, letting the dispatcher skip it entirely for irrelevant events — worth adding to custom controls that sit deep in large trees since dispatch cost is paid per-ancestor per-event regardless of whether any callback is registered. Events can also be synthesized for tests or simulated input, per `Manual/UIE-Events-Synthesizing.html`, by pulling a pooled instance and sending it directly.

```csharp
using UnityEngine.UIElements;

// Programmatically simulate a click for a UI test, bypassing real input.
void SimulateClick(VisualElement target)
{
    using (var evt = ClickEvent.GetPooled())
    {
        evt.target = target;
        target.SendEvent(evt);
    }
}

// A handler that only wants the capture phase, and stops the event there.
void RegisterCaptureOnlyHandler(VisualElement el)
{
    el.RegisterCallback<PointerDownEvent>(evt =>
    {
        if (evt.propagationPhase == PropagationPhase.TrickleDown)
        {
            evt.StopPropagation(); // no descendant/bubble handler will see it
        }
    }, TrickleDown.TrickleDown);
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Re-querying the tree every frame/update | `Q<T>()`/`Query<T>()` walk the visual tree on every call; cache the returned `VisualElement`/`UQueryState<T>` (via `.Build()`) in a field instead of calling `Q` inside `Update`, a scheduled tick, or a per-frame `generateVisualContent` callback. |
| Forgetting to unregister callbacks | `RegisterCallback<T>` without a matching `UnregisterCallback<T>` (or a `Manipulator` whose `UnregisterCallbacksFromTarget` doesn't mirror every registration in `RegisterCallbacksOnTarget`) leaks a handler that keeps firing after the element is logically "done," and can hold a reference preventing GC. |
| Assuming `StopPropagation()` also prevents default behavior | It only halts further trickle/bubble traversal; call `evt.PreventDefault()` separately (`ScriptReference/UIElements.EventBase.html`) to suppress the control's own default handling of the event. |
| Mixing uGUI/IMGUI mental models into UI Toolkit code | There is no `EventSystem`/`GraphicRaycaster` equivalent to configure — input routing to a runtime panel is handled by `PanelSettings` + the active input backend (`Manual/UIE-Runtime-Event-System.html`); and UI Toolkit events are pooled/typed classes, not `GUI.Button()`-style immediate-mode calls. |
| Wrong `TrickleDown` phase for a manipulator | A drag/press manipulator registered in the default bubble phase can be intercepted by a child that calls `StopPropagation()` first; register `PointerDownEvent` with `TrickleDown.TrickleDown` when the manipulator must see the event before descendants can swallow it. |
| Forgetting `CapturePointer` on drag manipulators | Without `target.CapturePointer(evt.pointerId)`, `PointerMoveEvent`/`PointerUpEvent` stop reaching the target the instant the pointer leaves its bounds mid-drag, breaking fast drags. |
| Expecting inline `style` writes to be undoable by leaving them unset | Once `style.someProperty = x` is assigned, USS selectors for that property no longer apply — clearing it requires explicitly assigning `StyleKeyword.Null`, not just stopping future writes. |
| Assuming `resolvedStyle` is writable | `IResolvedStyle` is read-only; attempting to treat it like `style` is a compile error, but developers new to the API often reach for the wrong one when reading vs writing. |
| Mixing up the two binding systems | `BindableElement`/`bindingPath`/`SerializedObject` binding (Editor-only) and `dataSource`/`SetBinding`/`DataBinding` runtime binding are unrelated systems with similar-sounding names; a `bindingPath` set on a control outside the Editor does nothing. |
| Binding silently not updating | Missing `[CreateProperty]` on the source property, a `PropertyPath` string that doesn't match the property name, a `BindingMode` that excludes the needed direction (e.g. `ToTargetOnce` when live updates were expected), or the source not implementing `INotifyBindablePropertyChanged` for a `ToTarget`/`TwoWay` binding — raise `BindingLogLevel` to see which. |
| Custom control invisible/unthemed at runtime but fine in UI Builder | `PanelSettings.themeStyleSheet` not assigned on the runtime panel — UI Builder's preview uses the Editor's implicit theme, which doesn't exist in a build. |
| `[UxmlElement]` control doesn't show up as a UXML tag | Class isn't `partial`, doesn't derive from `VisualElement` (or a `VisualElement` subclass), or the project has a compile error in the same assembly silently blocking the source generator — check `Manual/UIE-troubleshooting-custom-control-library-compilation.html`. |
| Treating `contentContainer` and the element itself as the same thing | A composite custom control that overrides `contentContainer` redirects `Add()`/`Children()` to an inner element; code that needs the true outer root must use `hierarchy.Add`/`hierarchy.Children()` instead. |
| Deep per-frame structural changes | Adding/removing/resizing elements every frame (e.g. rebuilding a whole list instead of updating in place) re-triggers Yoga layout on the affected subtree each time; prefer mutating existing elements' values/style or a virtualized `ListView` over rebuild-from-scratch patterns. |

## Quick Reference

| Class / Member | Category | Purpose |
|---|---|---|
| `VisualElement` | Core | Base class for every visual-tree node |
| `VisualElement.Q<T>()` / `.Query<T>()` | Query | Single-match / fluent multi-match descendant search |
| `UQueryBuilder<T>` / `UQueryState<T>` | Query | Fluent filter chain; `.Build()` compiles a reusable cached query |
| `VisualElement.hierarchy` | Core | Raw child-manipulation struct bypassing `contentContainer` |
| `VisualElement.style` / `IStyle` | Style | Writable inline style properties (wins over USS) |
| `VisualElement.resolvedStyle` / `IResolvedStyle` | Style | Read-only final computed style after cascade+layout |
| `VisualElement.customStyle` / `ICustomStyle` | Style | Read custom USS properties (`--x`) via `TryGetValue` |
| `CustomStyleResolvedEvent` | Style/Event | Fires when custom USS properties are (re)resolved for an element |
| `StyleKeyword` | Style | `Null`/`Auto`/`None`/`Initial`/`Undefined` sentinel on style value wrappers |
| `VisualElement.MarkDirtyRepaint()` | Rendering | Force repaint after state used by `generateVisualContent` changes |
| `generateVisualContent` | Rendering | Callback for custom drawing via `MeshGenerationContext` |
| `usageHints` | Perf | Flags (`DynamicTransform`, `GroupTransform`) to reduce cost of frequently-animated elements |
| `schedule` / `IVisualElementScheduler` | Scheduling | Fluent `.Execute()`, `.Every(ms)`, `.Until()` polling/delay API |
| `RegisterCallback<T>` / `UnregisterCallback<T>` | Events | Attach/detach a typed event handler, optional `TrickleDown` phase |
| `TrickleDown` | Events | `NoTrickleDown` (bubble, default) vs `TrickleDown` (capture) |
| `PropagationPhase` | Events | `TrickleDown` / `AtTarget` / `BubbleUp` |
| `evt.StopPropagation()` / `evt.PreventDefault()` | Events | Halt further dispatch / suppress default action (independent!) |
| `EventInterestAttribute` | Perf/Events | Declares which event categories a custom element needs, skips the rest |
| `Manipulator` | Manipulators | Base contract: `target` + `RegisterCallbacksOnTarget`/`UnregisterCallbacksFromTarget` |
| `PointerManipulator` / `MouseManipulator` | Manipulators | Pointer-unified vs mouse-only manipulator base classes |
| `Clickable` | Manipulators | Reusable click-behavior manipulator (backs `Button`) |
| `ContextualMenuManipulator` | Manipulators | Right-click/context-menu wiring |
| `target.AddManipulator(...)` | Manipulators | Attach a manipulator (`VisualElementExtensions`) |
| `CapturePointer` / `ReleasePointer` / `HasPointerCapture` | Input | Keep receiving pointer events outside element bounds mid-drag |
| `[UxmlElement]` | Custom controls | Registers a `VisualElement` subclass as a UXML tag (source generator) |
| `[UxmlAttribute]` | Custom controls | Exposes a C# property as a UXML-settable attribute |
| `UxmlFactory<T,U>` / `UxmlTraits` | Custom controls | Legacy manual registration pattern |
| `BindableElement` / `bindingPath` | Binding (Editor) | Legacy `SerializedObject`/`SerializedProperty` binding, Editor-only |
| `BindingExtensions.Bind/BindProperty/Unbind` | Binding (Editor) | Attach/detach Editor `SerializedObject` bindings |
| `[CreateProperty]` | Binding (runtime) | Marks a C# property as bindable for the runtime binding system |
| `INotifyBindablePropertyChanged` | Binding (runtime) | Data source pushes change notifications for live `ToTarget`/`TwoWay` bindings |
| `dataSource` / `dataSourcePath` | Binding (runtime) | Per-element scripted binding source assignment |
| `SetBinding` / `GetBinding` / `ClearBinding(s)` | Binding (runtime) | Scripted attach/detach of a `Binding` to a `BindingId` |
| `DataBinding` | Binding (runtime) | General-purpose concrete `Binding` connecting a path to a target property |
| `BindingMode` | Binding (runtime) | `ToTarget` / `ToSource` / `TwoWay` / `ToTargetOnce` |
| `BindingLogLevel` / `Binding.SetGlobalLogLevel` | Binding (runtime) | Diagnostics for silently failing/pending bindings |
| `ConverterGroup` | Binding (runtime) | Custom type converters for otherwise-unbindable types |
| `ListView` / `MultiColumnListView` | Data display | Virtualized list/table controls bound via `itemsSource` |
| `UIDocument` | Hosting | Scene component owning `rootVisualElement`, `visualTreeAsset`, `panelSettings` |
| `PanelSettings` | Hosting | Runtime panel scale mode, sort order, render target, theme |
| `VisualTreeAsset.CloneTree()` | Hosting | Instantiate compiled UXML into a live subtree |
| `StyleSheet` / `styleSheets` | Hosting | Compiled USS asset and per-element attached-stylesheet set |
| `IPanel` / `panel` | Hosting | Root panel abstraction; null until the element is attached |

## Advanced Notes

**Layout/style recalculation cost.** UI Toolkit's layout engine is Yoga (flexbox); any change to a property that affects box geometry (size, padding, margin, flex properties, adding/removing/reordering children) invalidates layout for the affected subtree and triggers a re-solve on the next update, while style-only changes (color, opacity below the point of triggering a new stacking context) are cheaper repaint-only invalidations. `Manual/UIE-performance-consideration-runtime.html` frames the practical rule: batch structural changes (build a subtree off-tree, then `Add()` it once) rather than mutating the live tree element-by-element in a loop, and prefer updating existing elements' values over destroying/recreating them for per-frame or per-tick UI (health bars, score counters). `usageHints` (`ScriptReference/UIElements.VisualElement-usageHints.html`) exists specifically to tell the renderer an element will move/transform every frame so it can put that element on its own rendering path and avoid regenerating geometry for the whole batch each time — apply it to elements animated continuously (a dragged icon, a pulsing indicator), not globally, since it has its own memory/state overhead.

**Editor-window vs runtime usage differ more than the shared API suggests.** The same `VisualElement`/UXML/USS code compiles and mostly behaves identically whether hosted in an `EditorWindow`'s root or a runtime `UIDocument`, but three things diverge: (1) theming — the Editor supplies an implicit default theme for built-in controls, while a runtime panel renders those controls with no chrome at all unless `PanelSettings.themeStyleSheet` is explicitly assigned; (2) input routing — Editor UI receives input through the Editor's own event pump, while runtime UI depends on `PanelSettings` plus whichever input backend (legacy Input Manager or Input System package) is active, per `Manual/UIE-Runtime-Event-System.html`; (3) `IPanel.contextType` (`ScriptReference/UIElements.IPanel-contextType.html`) reports `ContextType.Editor` vs `ContextType.Player`, which is the correct branch point for any shared code that must behave differently in the two hosts (e.g. skipping Editor-only Inspector fields like `PropertyField`/`ObjectField` at runtime). Always verify a runtime panel in Play Mode or an actual build — the UI Builder canvas preview approximates runtime appearance but doesn't guarantee it, especially for anything theme-dependent.

**The two binding systems are not layered on each other.** The runtime binding system (`Binding`/`DataBinding`/`dataSource`) was added well after the legacy `SerializedObject`-oriented `BindableElement` system and is a parallel implementation, not a superset — code written against one doesn't automatically benefit from features of the other (e.g. `BindingLogLevel` diagnostics apply to the runtime system, not to `SerializedObject` binding failures, which surface differently). When a project needs both an Inspector/Editor tool and a runtime UI screen for conceptually "the same" data, expect to write two separate binding wire-ups rather than sharing one.

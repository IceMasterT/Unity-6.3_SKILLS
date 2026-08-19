---
name: unity-editor-tooling
description: Use when building custom Unity Editor extensions — custom Inspectors, PropertyDrawers, EditorWindow tools, Gizmos, menu items, or AssetPostprocessors. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Editor Tooling

Editor scripting APIs and Undo/serialization semantics shift between Unity versions. **Prefer retrieval over pre-training** for exact method signatures and attribute usage. All paths below are relative to `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` and were verified to exist on disk.

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Editor class | `ScriptReference/Editor.html`, `Editor.OnInspectorGUI.html`, `Editor.DrawDefaultInspector.html`, `Editor-serializedObject.html`, `Editor-target.html`, `Editor-targets.html` | Custom Inspector base class, drawing default UI, accessing the bound object(s) |
| EditorWindow | `ScriptReference/EditorWindow.html`, `EditorWindow.GetWindow.html`, `EditorWindow.CreateGUI.html`, `EditorWindow.OnGUI.html`, `Manual/editor-EditorWindows.html`, `Manual/UIE-HowTo-CreateEditorWindow.html` | Tool windows: opening/focusing, IMGUI vs UI Toolkit entry points |
| PropertyDrawer | `ScriptReference/PropertyDrawer.html`, `PropertyDrawer.OnGUI.html`, `PropertyDrawer.GetPropertyHeight.html`, `PropertyDrawer.CreatePropertyGUI.html`, `CustomPropertyDrawer.html`, `Manual/editor-PropertyDrawers.html` | Custom field rendering for serialized types/attributes, IMGUI and UI Toolkit variants |
| CustomEditor / CanEditMultipleObjects | `ScriptReference/CustomEditor.html`, `CanEditMultipleObjects.html`, `Manual/editor-CustomEditors.html`, `Manual/UsingCustomEditorTools.html`, `Manual/SL-CustomEditor.html` | Attaching inspectors to components, multi-object edit rules |
| SerializedObject / SerializedProperty | `ScriptReference/SerializedObject.html`, `SerializedObject.FindProperty.html`, `SerializedObject.ApplyModifiedProperties.html`, `SerializedProperty.html`, `SerializedProperty-hasMultipleDifferentValues.html` | Undo-safe, prefab-safe field editing; iterating and mutating serialized data |
| EditorGUILayout / EditorGUI / EditorGUIUtility | `ScriptReference/EditorGUILayout.html`, `EditorGUI.html`, `EditorGUIUtility.html`, `EditorGUI.PropertyField.html`, `EditorGUILayout.PropertyField.html` | Editor-only IMGUI widget sets for Inspectors and EditorWindows |
| MenuItem | `ScriptReference/MenuItem.html`, `MenuItem-ctor.html`, `MenuItem-validate.html`, `MenuItem-priority.html` | Registering menu commands and keyboard shortcuts, validate functions |
| Gizmos & Handles | `ScriptReference/Gizmos.html`, `Handles.html`, `Manual/gizmos-and-handles.html`, `Manual/gizmos-handles-programming.html`, `Manual/gizmos-introduction.html`, `Manual/GizmosMenu.html` | Scene view drawing (Gizmos = decorative), interactive handles (Handles = manipulable) |
| SceneView | `ScriptReference/SceneView.html`, `SceneView-duringSceneGui.html`, `SceneView-beforeSceneGui.html` | Custom scene-view GUI/tool hooks outside a component's own gizmo callbacks |
| AssetPostprocessor | `ScriptReference/AssetPostprocessor.html`, `AssetPostprocessor.OnPostprocessAllAssets.html`, `AssetPostprocessor-assetImporter.html`, `AssetPostprocessor-assetPath.html`, `AssetPostprocessor.GetPostprocessOrder.html` | Import-time hooks (OnPreprocess*/OnPostprocess*), forcing reimport-time settings |
| Asset import pipeline | `Manual/ImportingAssets.html`, `Manual/import-assets.html`, `Manual/AssetWorkflow.html`, `Manual/ScriptedImporters.html` | Background on when postprocessor callbacks fire relative to import |
| EditorUtility | `ScriptReference/EditorUtility.SetDirty.html`, `EditorUtility.DisplayDialog.html`, `EditorUtility.ClearDirty.html` | Marking non-`SerializedObject` mutations dirty for saving; blocking dialogs/progress bars |
| Undo | `ScriptReference/Undo.html`, `Undo.RecordObject.html` | Registering undoable operations outside `SerializedObject` (e.g. `GameObject.AddComponent` in editor code) |
| EditorApplication / InitializeOnLoad | `ScriptReference/EditorApplication.html`, `InitializeOnLoadAttribute.html`, `InitializeOnLoadMethodAttribute.html` | Editor lifecycle callbacks, running code on editor load/domain reload |
| EditorTool / Overlays | `ScriptReference/EditorTools.EditorTool.html`, `Overlays.Overlay.html` | Custom scene-view tools (toolbar tools) and dockable scene-view overlay panels |
| UI Toolkit editor UI | `ScriptReference/UIElements.VisualElement.html`, `UIElements.PropertyField.html`, `UIElements.IMGUIContainer.html`, `Manual/UIE-editor-binding.html`, `Manual/UIE-support-for-editor-ui.html` | UI Toolkit-based custom Inspectors/EditorWindows/PropertyDrawers, IMGUI↔UI Toolkit interop |
| Extending the Editor (orientation) | `Manual/ExtendingTheEditor.html` | High-level map of every editor extension mechanism before diving into specifics |

## Key Guidelines

### Custom Inspectors

Attach a custom Inspector with `[CustomEditor(typeof(MyComponent))]` on a class deriving from `Editor` (`ScriptReference/CustomEditor.html`, `Editor.html`). Override `OnInspectorGUI` to draw fields; call `DrawDefaultInspector()` if you only want to prepend/append UI to the stock layout (`Editor.DrawDefaultInspector.html`). Always route field reads/writes through `serializedObject`/`SerializedProperty`, never by casting `target` to your component type and setting fields directly — direct field mutation on `target` bypasses Undo registration, prefab-override tracking, and multi-object edit, and won't mark the scene dirty. Wrap the whole method body with `serializedObject.Update()` at the top and `serializedObject.ApplyModifiedProperties()` at the bottom; `ApplyModifiedProperties` is what registers the Undo step and calls `SetDirty` internally.

```csharp
using UnityEditor;
using UnityEngine;

[CustomEditor(typeof(WeaponConfig))]
[CanEditMultipleObjects]
public class WeaponConfigEditor : Editor
{
    SerializedProperty damageProp;
    SerializedProperty cooldownProp;

    void OnEnable()
    {
        damageProp = serializedObject.FindProperty("damage");
        cooldownProp = serializedObject.FindProperty("cooldown");
    }

    public override void OnInspectorGUI()
    {
        serializedObject.Update();

        EditorGUILayout.PropertyField(damageProp);
        EditorGUILayout.PropertyField(cooldownProp);

        if (damageProp.floatValue < 0f)
            EditorGUILayout.HelpBox("Damage cannot be negative.", MessageType.Warning);

        serializedObject.ApplyModifiedProperties();
    }
}
```

### CanEditMultipleObjects and multi-object edit

Add `[CanEditMultipleObjects]` (`ScriptReference/CanEditMultipleObjects.html`) only once the drawer correctly handles mixed values — check `SerializedProperty.hasMultipleDifferentValues` before rendering a field as if all selected objects agreed, and never read `target` when multiple objects are selected; use `targets` (`Editor-targets.html`) instead. `SerializedObject`/`SerializedProperty` already average/blank mixed values for you when you go through `EditorGUILayout.PropertyField`, so prefer that over manually branching on `hasMultipleDifferentValues` unless you're building custom (non-`PropertyField`) UI.

```csharp
public override void OnInspectorGUI()
{
    serializedObject.Update();
    EditorGUI.showMixedValue = damageProp.hasMultipleDifferentValues;
    EditorGUI.BeginChangeCheck();
    float newDamage = EditorGUILayout.FloatField("Damage", damageProp.floatValue);
    if (EditorGUI.EndChangeCheck())
        damageProp.floatValue = newDamage;
    EditorGUI.showMixedValue = false;
    serializedObject.ApplyModifiedProperties();
}
```

### PropertyDrawers

A `PropertyDrawer` targets either a `[Serializable]` type via `[CustomPropertyDrawer(typeof(MyType))]`, or a `PropertyAttribute` subclass (`ScriptReference/PropertyDrawer.html`, `CustomPropertyDrawer.html`, `Manual/editor-PropertyDrawers.html`). Prefer the attribute form (`[CustomPropertyDrawer(typeof(MyAttribute))]` bound to a `PropertyAttribute`) for reusable decorations applied to built-in types like `int`/`string`, since you can't add a `CustomPropertyDrawer` directly to types you don't own. IMGUI drawers override `OnGUI(Rect, SerializedProperty, GUIContent)` and must also override `GetPropertyHeight` whenever the drawer isn't exactly one standard line tall — Unity uses the returned height to lay out the Rect it passes to `OnGUI`, and a drawer that draws multiple controls without adjusting height will get clipped or overlapped. The newer UI Toolkit path overrides `CreatePropertyGUI(SerializedProperty)` and returns a `VisualElement` tree instead; prefer this for new drawers and don't mix `OnGUI` and `CreatePropertyGUI` in the same class — Unity picks one code path per drawer instance and mixing them is unsupported.

```csharp
// Attribute-based drawer for a range-limited int on any target type.
public class MinMaxIntAttribute : PropertyAttribute
{
    public readonly int min, max;
    public MinMaxIntAttribute(int min, int max) { this.min = min; this.max = max; }
}

[CustomPropertyDrawer(typeof(MinMaxIntAttribute))]
public class MinMaxIntDrawer : PropertyDrawer
{
    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        var attr = (MinMaxIntAttribute)attribute;
        if (property.propertyType != SerializedPropertyType.Integer)
        {
            EditorGUI.LabelField(position, label.text, "Use MinMaxInt with int.");
            return;
        }
        EditorGUI.BeginProperty(position, label, property);
        int value = EditorGUI.IntSlider(position, label, property.intValue, attr.min, attr.max);
        property.intValue = value;
        EditorGUI.EndProperty();
    }
}
```

```csharp
// UI Toolkit equivalent entry point (same attribute reused).
[CustomPropertyDrawer(typeof(MinMaxIntAttribute))]
public class MinMaxIntDrawerUITK : PropertyDrawer
{
    public override VisualElement CreatePropertyGUI(SerializedProperty property)
    {
        var attr = (MinMaxIntAttribute)attribute;
        var slider = new SliderInt(property.displayName, attr.min, attr.max);
        slider.BindProperty(property);
        return slider;
    }
}
```

### EditorWindows

Choose an `EditorWindow` (`ScriptReference/EditorWindow.html`) for tools that operate across the project or scene independent of a single selected object; choose a `CustomEditor` when the UI is specific to one component's serialized data. Open/focus a window with `EditorWindow.GetWindow<T>()`. For IMGUI windows, implement `OnGUI()`; for UI Toolkit windows, implement `CreateGUI()` (`EditorWindow.CreateGUI.html`) and build a `VisualElement` tree onto `rootVisualElement` instead — Unity calls `CreateGUI` once when the window is enabled/shown and does not call `OnGUI` afterward if `CreateGUI` is implemented, so pick one style per window, matching the same IMGUI-vs-UI-Toolkit split as PropertyDrawers.

```csharp
public class WeaponBalancerWindow : EditorWindow
{
    [MenuItem("Tools/Weapon Balancer")]
    public static void Open() => GetWindow<WeaponBalancerWindow>("Weapon Balancer");

    void CreateGUI()
    {
        var refreshButton = new Button(RefreshList) { text = "Refresh" };
        rootVisualElement.Add(refreshButton);
        rootVisualElement.Add(new ListView());
    }

    void RefreshList() { /* rebuild the list contents */ }
}
```

### Gizmos, Handles, and SceneView hooks

`Gizmos` (`ScriptReference/Gizmos.html`) draws non-interactive shapes in the Scene view via `MonoBehaviour.OnDrawGizmos`/`OnDrawGizmosSelected` — these are ordinary runtime-class callbacks Unity invokes only in the editor, so no `Editor`/`CustomEditor` class is required. `OnDrawGizmos` runs every repaint for every object in the scene, selected or not, so keep it cheap; move expensive or selection-only drawing into `OnDrawGizmosSelected`. `Handles` (`ScriptReference/Handles.html`) draws interactive, draggable scene-view controls (position/rotation handles, buttons, sliders) and is normally called from inside a `CustomEditor`'s `OnSceneGUI()` override, or from a static handler on `SceneView.duringSceneGui` (`ScriptReference/SceneView-duringSceneGui.html`) for tooling that isn't tied to a component selection at all — e.g. a global scene-view overlay. `SceneView.duringSceneGui` replaced the older `SceneView.onSceneGUIDelegate` and fires once per Scene view per repaint; subscribe in `OnEnable`/static constructor and unsubscribe in `OnDisable` to avoid duplicate handler stacking across domain reloads.

```csharp
[CustomEditor(typeof(PatrolPath))]
public class PatrolPathEditor : Editor
{
    void OnSceneGUI()
    {
        var path = (PatrolPath)target;
        for (int i = 0; i < path.points.Length; i++)
        {
            EditorGUI.BeginChangeCheck();
            Vector3 newPos = Handles.PositionHandle(path.points[i], Quaternion.identity);
            if (EditorGUI.EndChangeCheck())
            {
                Undo.RecordObject(path, "Move Patrol Point");
                path.points[i] = newPos;
            }
        }
    }
}

// Component-level gizmo, no Editor class needed:
public class PatrolPath : MonoBehaviour
{
    public Vector3[] points;
    void OnDrawGizmosSelected()
    {
        Gizmos.color = Color.yellow;
        for (int i = 0; i < points.Length - 1; i++)
            Gizmos.DrawLine(points[i], points[i + 1]);
    }
}
```

### MenuItems

Register menu commands with `[MenuItem("Tools/My Tool")]` on a static method (`ScriptReference/MenuItem.html`); non-static methods and methods on classes with compile errors silently fail to register, which is the most common cause of a missing menu entry. Add a keyboard shortcut with a trailing `%`/`#`/`&`/`_` modifier syntax in the path string (e.g. `"Tools/My Tool %g"` for Ctrl/Cmd+G). Add a validate function via a second `[MenuItem(path, true)]` overload with matching path and identical method signature returning `bool` — Unity polls the validate function to enable/disable/gray out the menu entry (`MenuItem-validate.html`, `MenuItem-priority.html` controls ordering/separators).

```csharp
public static class SelectionTools
{
    [MenuItem("Tools/Align Selected To Grid")]
    static void AlignToGrid()
    {
        foreach (var t in Selection.transforms)
        {
            Undo.RecordObject(t, "Align To Grid");
            t.position = new Vector3(Mathf.Round(t.position.x), Mathf.Round(t.position.y), Mathf.Round(t.position.z));
        }
    }

    [MenuItem("Tools/Align Selected To Grid", true)]
    static bool ValidateAlignToGrid() => Selection.transforms.Length > 0;
}
```

### AssetPostprocessors

An `AssetPostprocessor` subclass (`ScriptReference/AssetPostprocessor.html`) does not need to be attached to anything — Unity discovers and instantiates it automatically for every asset import as long as it lives in an editor-only context. Override static-like instance callbacks such as `OnPreprocessTexture`/`OnPostprocessTexture`, or the batched `OnPostprocessAllAssets(string[] importedAssets, string[] deletedAssets, string[] movedAssets, string[] movedFromAssetPaths)` for project-wide hooks that don't need per-importer settings access. Inside per-asset preprocess callbacks, mutate the importer exposed via `assetImporter` (cast to the specific importer type, e.g. `TextureImporter`) — this is the supported way to force import settings, and changes here happen before the asset is written to the library, unlike a postprocess-time `AssetDatabase.ImportAsset` call which triggers a second, more expensive reimport. Use `GetPostprocessOrder()` to control ordering when multiple postprocessors touch the same asset.

```csharp
public class TextureImportSettings : AssetPostprocessor
{
    void OnPreprocessTexture()
    {
        if (!assetPath.Contains("/UI/")) return;
        var importer = (TextureImporter)assetImporter;
        importer.textureType = TextureImporterType.Sprite;
        importer.mipmapEnabled = false;
    }

    static void OnPostprocessAllAssets(
        string[] importedAssets, string[] deletedAssets,
        string[] movedAssets, string[] movedFromAssetPaths)
    {
        foreach (var path in importedAssets)
            if (path.EndsWith(".weaponconfig.json"))
                Debug.Log($"Weapon config changed: {path}");
    }
}
```

### SerializedObject/SerializedProperty and Undo

`SerializedObject`/`SerializedProperty` (`ScriptReference/SerializedObject.html`, `SerializedProperty.html`) is Unity's reflection-safe, prefab-override-aware, multi-object-aware layer over a target's serialized fields — it is what makes Undo, prefab overrides, and multi-select editing work correctly for free. Any editor code that mutates fields *outside* this system — e.g. calling `gameObject.AddComponent<T>()` from a menu command, or setting a public field directly on a `ScriptableObject` you loaded via `AssetDatabase` — must manually call `Undo.RecordObject(obj, "description")` *before* the mutation (or `Undo.RegisterCreatedObjectUndo` for new objects, `ScriptReference/Undo.html`) and, if the object isn't a `SerializedObject`-driven component being saved through `ApplyModifiedProperties`, call `EditorUtility.SetDirty(obj)` (`EditorUtility.SetDirty.html`) afterward so the editor knows to persist the change. `SerializedObject.ApplyModifiedProperties()` performs both of these steps internally, which is why it's the preferred path whenever the data model allows it.

```csharp
[MenuItem("Tools/Create Config Asset")]
static void CreateConfigAsset()
{
    var config = ScriptableObject.CreateInstance<WeaponConfig>();
    AssetDatabase.CreateAsset(config, "Assets/NewWeaponConfig.asset");
    Undo.RegisterCreatedObjectUndo(config, "Create Weapon Config");
    EditorUtility.SetDirty(config);
    AssetDatabase.SaveAssets();
}
```

### Editor-only code separation

Keep editor-only code out of player builds: wrap it in `#if UNITY_EDITOR` / `#endif`, or put the script under a folder literally named `Editor/` anywhere in `Assets/` (Unity excludes all `Editor/` folder contents from player builds automatically), or, in an assembly-definition-based project, mark the `.asmdef` as an Editor-only assembly (`Manual/ExtendingTheEditor.html`). Referencing `UnityEditor` types from a script that ships in the player causes build failures, since `UnityEditor.dll` isn't included in player builds.

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Editing `target` fields directly | Skips Undo and prefab-override recording; use `SerializedProperty` + `ApplyModifiedProperties()` instead of casting `target` |
| Changes don't persist after restart/save | Forgot `EditorUtility.SetDirty(target)` after a non-`SerializedProperty` mutation (e.g. direct field set on a loaded `ScriptableObject`) |
| Heavy work inside `OnInspectorGUI`/`OnGUI` | Runs every repaint (many times/sec, and on every mouse move over the window); cache expensive results and invalidate only when underlying data actually changes |
| Blank/broken Inspector on multi-select | `[CanEditMultipleObjects]` omitted, or code reads `target` instead of `targets`/`serializedObject`, which throws or silently only reflects one of several selected objects |
| Drawer values stale after Undo | Reading cached local (non-`SerializedProperty`) state instead of re-reading `SerializedProperty` values fresh on every draw call — Undo/Redo doesn't invalidate manually cached fields |
| `[MenuItem]` not appearing | Method isn't `static`, the script has compile errors anywhere in the project, or the path string is malformed (missing leading segment) |
| `PropertyDrawer` UI clipped or overlapping | `GetPropertyHeight` wasn't overridden (or returns the wrong value) after adding more than one line of controls in `OnGUI`; Unity lays out the `Rect` using the declared height, not the actual drawn content |
| Mixing `OnGUI`/`CreatePropertyGUI` (or `OnGUI`/`CreateGUI`) in one drawer/window | Unsupported — Unity picks a single rendering path per drawer/window instance; mixing IMGUI and UI Toolkit entry points on the same class causes inconsistent or missing UI |
| `OnDrawGizmos` tanking Scene view framerate | Runs for every object every repaint regardless of selection; move expensive/selection-only drawing to `OnDrawGizmosSelected`, and avoid per-frame allocations inside either callback |
| Duplicate/stacking `SceneView.duringSceneGui` handlers | Subscribed in a static constructor or `OnEnable` without a matching unsubscribe in `OnDisable`, so each domain reload or re-enable adds another handler instance |
| AssetPostprocessor changes trigger a second reimport | Mutating the importer via `AssetDatabase.ImportAsset` from a postprocess callback instead of setting fields directly on `assetImporter` inside a preprocess callback (`OnPreprocessTexture` etc.), which applies before the asset is written once |
| Editor-only script breaks the player build | `UnityEditor` namespace referenced from a script not excluded from player builds — wrap in `#if UNITY_EDITOR`, move to an `Editor/` folder, or mark the containing `.asmdef` as editor-only |
| Custom Inspector doesn't reflect field renames | `SerializedObject.FindProperty("fieldName")` uses the literal serialized field name as a string; a C# rename without updating the string (or without `[FormerlySerializedAs]`) returns `null` and throws on `.floatValue` etc. |
| Undo doesn't group correctly across multiple ops | Each `Undo.RecordObject`/menu action gets a new undo group by default; use `Undo.IncrementCurrentGroup()` and `Undo.CollapseUndoOperations()` intentionally to merge a multi-step operation into a single Ctrl+Z step |
| EditorWindow state resets on script reload | Non-`[SerializeField]` fields on an `EditorWindow` are lost across domain reloads (assembly recompiles, entering Play mode); mark fields that must survive with `[SerializeField]` since `EditorWindow` is itself a `ScriptableObject` |

## Quick Reference

| Attribute / Class / Method | Purpose |
|---|---|
| `[CustomEditor(typeof(T))]` | Bind an `Editor` subclass to component/asset type `T` |
| `[CanEditMultipleObjects]` | Allow the custom Inspector to edit multi-selections |
| `[CustomPropertyDrawer(typeof(T))]` | Bind a `PropertyDrawer` to a serializable type or a `PropertyAttribute` |
| `[MenuItem("Path/Name")]` / `[MenuItem(path, true)]` | Register a static method as a menu command/shortcut, or its validate function |
| `[InitializeOnLoad]` / `[InitializeOnLoadMethod]` | Run static editor code on editor load and after every domain reload |
| `Editor.OnInspectorGUI()` | Draw the custom Inspector body (IMGUI) |
| `Editor.DrawDefaultInspector()` | Draw the stock auto-generated Inspector inside a custom one |
| `Editor.target` / `Editor.targets` | The single/multiple selected object(s) an Editor is inspecting |
| `EditorWindow.GetWindow<T>()` | Open or focus a tool window |
| `EditorWindow.CreateGUI()` | UI Toolkit entry point for window content (replaces `OnGUI` for that window) |
| `PropertyDrawer.OnGUI` / `GetPropertyHeight` | IMGUI drawer body and required height calculation |
| `PropertyDrawer.CreatePropertyGUI(SerializedProperty)` | UI Toolkit drawer entry point, returns a `VisualElement` |
| `SerializedObject.Update()` / `ApplyModifiedProperties()` | Sync from/to the native object; the latter registers Undo and dirties the object |
| `SerializedObject.FindProperty(string)` | Look up a serialized field by its literal C# field name |
| `SerializedProperty.hasMultipleDifferentValues` | True when a multi-object selection has differing values for this field |
| `EditorGUILayout` / `EditorGUI` | Editor-only auto-layout / manual-rect widget sets (not the runtime `GUILayout`/`GUI`) |
| `EditorGUI.BeginChangeCheck()` / `EndChangeCheck()` | Detect whether a control's value changed this frame, to gate expensive writes |
| `EditorGUI.showMixedValue` | Renders a control's "—" mixed-value state for multi-object edit |
| `EditorUtility.SetDirty(obj)` | Mark an object dirty for saving when not going through `SerializedObject` |
| `EditorUtility.DisplayDialog` / `DisplayProgressBar` | Modal dialogs and progress bars for editor tooling |
| `Undo.RecordObject(obj, name)` | Register pre-mutation state for Undo outside `SerializedObject` |
| `Undo.RegisterCreatedObjectUndo(obj, name)` | Make object creation (e.g. `AddComponent`, `CreateInstance`) undoable |
| `Gizmos` (`OnDrawGizmos`/`OnDrawGizmosSelected`) | Non-interactive Scene view shape drawing on ordinary `MonoBehaviour`s |
| `Handles` (`OnSceneGUI` / `SceneView.duringSceneGui`) | Interactive, draggable Scene view controls |
| `SceneView.duringSceneGui` | Static event for Scene view GUI not tied to a component's own `OnSceneGUI` |
| `AssetPostprocessor.OnPreprocess*` / `OnPostprocess*` | Per-importer or batched hooks into the asset import pipeline |
| `AssetPostprocessor.GetPostprocessOrder()` | Controls execution order among multiple postprocessors on the same asset |
| `EditorTools.EditorTool` | Custom scene-view toolbar tool (an alternative to per-component `OnSceneGUI`) |
| `Overlays.Overlay` | Dockable/floating Scene view panel (modern alternative to custom `OnSceneGUI` GUI blocks) |
| `UIElements.VisualElement` / `PropertyField` / `IMGUIContainer` | UI Toolkit building blocks for editor UI; `IMGUIContainer` embeds legacy IMGUI inside a UI Toolkit tree |

## Advanced Notes

**UI Toolkit as the modern alternative to IMGUI.** Unity's newer editor UI stack (`Manual/UIE-editor-binding.html`, `Manual/UIE-support-for-editor-ui.html`) builds Inspectors, EditorWindows, and PropertyDrawers out of `VisualElement` trees instead of immediate-mode `OnGUI` calls. Key differences that affect design decisions:

- **Retained mode vs immediate mode.** IMGUI rebuilds and re-lays-out the entire UI every repaint; UI Toolkit builds the tree once (in `CreateGUI`/`CreatePropertyGUI`) and only updates what changes, which is both faster for complex tool UIs and enables USS (Unity Style Sheets, CSS-like) styling that IMGUI has no equivalent for.
- **Data binding.** `UIElements.PropertyField` (`ScriptReference/UIElements.PropertyField.html`) can bind directly to a `SerializedProperty` via `BindProperty`, handling Undo/dirty/multi-object-edit semantics automatically without manual `Update()`/`ApplyModifiedProperties()` calls each frame — see `Manual/UIE-Binding.html` for the full binding model (not itself listed in Retrieval Sources above since it's several pages deep; start from `Manual/UIE-editor-binding.html`).
- **Interop.** `UIElements.IMGUIContainer` lets you embed a block of legacy `OnGUI`-style code inside an otherwise UI-Toolkit-built window or Inspector — useful for migrating incrementally, or for wrapping third-party IMGUI drawers you can't rewrite. The reverse (embedding a `VisualElement` inside pure IMGUI) is not supported.
- **When to still use IMGUI.** `Handles`/`Gizmos` Scene view drawing has no UI Toolkit equivalent — `OnSceneGUI` and `SceneView.duringSceneGui` remain IMGUI-based regardless of whether the owning Inspector/window uses UI Toolkit. Small, one-off editor scripts (a single debug button) are often still faster to write in IMGUI than to set up a `VisualElement` tree for.
- **Migration granularity.** The IMGUI/UI-Toolkit choice is made per drawer/window class, not per project — a project can mix `OnInspectorGUI`-based Editors for simple components with `CreateGUI`-based Editors for complex ones without conflict, as long as no single class mixes both entry points (see Common Mistakes).

**EditorTool and Overlays for Scene view tooling.** For tools that need a dedicated Scene view toolbar button and their own manipulation mode (like the built-in Move/Rotate/Scale tools), prefer `EditorTools.EditorTool` (`ScriptReference/EditorTools.EditorTool.html`) over ad hoc `SceneView.duringSceneGui` handlers — it integrates with Unity's tool-context system so your tool activates/deactivates cleanly alongside the built-in ones. For persistent on-screen panels in the Scene or other views, `Overlays.Overlay` (`ScriptReference/Overlays.Overlay.html`) is the modern, dockable/floating replacement for hand-rolled `Handles.BeginGUI`/`EndGUI` overlay blocks, and exposes a `rootVisualElement` so overlay content is built with UI Toolkit.

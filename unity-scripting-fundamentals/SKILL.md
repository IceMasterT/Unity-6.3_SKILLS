---
name: unity-scripting-fundamentals
description: Use when writing or reviewing Unity C# scripts — MonoBehaviour lifecycle methods, ScriptableObjects, coroutines, events/delegates, serialization, or custom attributes. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Scripting Fundamentals

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| MonoBehaviour class ref | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.html` | Full lifecycle/message method list, signatures, `useGUILayout`/`runInEditMode` fields |
| MonoBehaviour manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/class-MonoBehaviour.html` | Conceptual overview of MonoBehaviour vs plain C# classes |
| Execution order manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/execution-order.html` | Full Player-loop diagram; exact Awake/OnEnable/Start/Update/FixedUpdate ordering; `sceneLoaded` timing relative to `OnEnable`/`Start` |
| Script Execution Order manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-execution-order.html` | Project Settings override of the default undefined sibling order |
| Event-functions overview | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/event-functions.html` | What qualifies as a "magic method" Unity calls by reflection/name |
| Per-frame event optimization | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/events-per-frame-optimization.html` | Cost of empty `Update`/`FixedUpdate` stubs; when to strip them |
| `MonoBehaviour.Awake` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.Awake.html` | Awake signature/timing detail |
| `MonoBehaviour.OnEnable` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.OnEnable.html` | OnEnable signature/timing detail |
| `MonoBehaviour.Start` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.Start.html` | Start signature/timing detail |
| `MonoBehaviour.OnDisable` / `OnDestroy` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.OnDisable.html`, `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.OnDestroy.html` | Teardown ordering, what's still valid to call inside each |
| `MonoBehaviour.OnValidate` / `Reset` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.OnValidate.html`, `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.Reset.html` | Editor-only Inspector-change/component-added callbacks |
| `MonoBehaviour.destroyCancellationToken` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour-destroyCancellationToken.html` | Cancellation token tied to the object's destruction, for `Awaitable`/`async` cleanup |
| `MonoBehaviour.didAwake` / `didStart` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour-didAwake.html`, `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour-didStart.html` | Query whether Awake/Start has already run on an instance |
| `MonoBehaviour.Invoke` / `InvokeRepeating` / `CancelInvoke` / `IsInvoking` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.Invoke.html`, `.InvokeRepeating.html`, `.CancelInvoke.html`, `.IsInvoking.html` | Reflection-based delayed/repeating calls (legacy alternative to coroutines) |
| ScriptableObject class ref | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ScriptableObject.html` | `CreateInstance`, lifecycle members, full API |
| ScriptableObject manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/class-ScriptableObject.html` | When/why to use SOs, `CreateAssetMenu` workflow, data-container pattern |
| `ScriptableObject.OnEnable`/`OnDisable`/`OnDestroy`/`Awake`/`Reset`/`OnValidate` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ScriptableObject.OnEnable.html` (+ sibling files `.OnDisable.html`, `.OnDestroy.html`, `.Awake.html`, `.Reset.html`, `.OnValidate.html`) | Asset-lifecycle semantics, distinct from MonoBehaviour scene lifecycle |
| Coroutine class ref | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Coroutine.html` | The opaque handle type returned by `StartCoroutine` |
| Coroutines manual overview | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/Coroutines.html` | Conceptual intro, `IEnumerator`-based execution model |
| Coroutine yield instructions manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/coroutines-yield-instructions.html` | Full catalog: `WaitForSeconds`, `WaitForFixedUpdate`, `WaitForEndOfFrame`, `WaitUntil`, `WaitWhile`, nested coroutines |
| Coroutine analysis manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/coroutines-analyzing.html` | Profiling/debugging coroutine execution |
| `MonoBehaviour.StartCoroutine`/`StopCoroutine`/`StopAllCoroutines` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/MonoBehaviour.StartCoroutine.html`, `.StopCoroutine.html`, `.StopAllCoroutines.html` | Exact start/stop API surface and overloads |
| Serialization overview manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-serialization.html` | Top-level index of the serialization doc set |
| Serialization rules manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-serialization-rules.html` | What Unity can/can't serialize; `[Serializable]`/`[NonSerialized]`/`[HideInInspector]` interaction |
| How Unity uses serialization | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-serialization-how-unity-uses.html` | Prefabs, scene saves, undo/redo, hot-reload all go through the serializer |
| Serialization best practices | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-serialization-best-practices.html` | Versioning fields safely, avoiding data loss on refactor |
| Custom serialization manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/script-serialization-custom-serialization.html` | `ISerializationCallbackReceiver` pattern for types the serializer can't handle natively |
| `ISerializationCallbackReceiver` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ISerializationCallbackReceiver.html` (+ `.OnBeforeSerialize.html`, `.OnAfterDeserialize.html`) | Hook points to flatten/rebuild `Dictionary<>` and similar unserializable types |
| `[SerializeField]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/SerializeField.html` | Force-serialize a private field |
| `[SerializeReference]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/SerializeReference.html` | Serialize polymorphic/interface-typed references by reference, not value |
| `[HideInInspector]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/HideInInspector.html` | Keep a field serialized but hidden from the Inspector |
| Attributes manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/unity-attributes.html` | Built-in attribute catalog (Header, Range, Tooltip, Space, Multiline, TextArea, Delayed, ContextMenu...) |
| `[Range]` / `[Header]` / `[Tooltip]` / `[Space]` / `[Multiline]` / `[TextArea]` / `[Delayed]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/RangeAttribute.html`, `HeaderAttribute.html`, `TooltipAttribute.html`, `SpaceAttribute.html`, `MultilineAttribute.html`, `TextAreaAttribute.html`, `DelayedAttribute.html` | Exact constructor params for each Inspector-display attribute |
| `[ContextMenu]` / `ContextMenuItemAttribute` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ContextMenu.html`, `ContextMenuItemAttribute.html` | Add right-click Inspector actions bound to a method/field |
| `[RequireComponent]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/RequireComponent.html` | Auto-adds and guards against removal of dependency components |
| `[DisallowMultipleComponent]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/DisallowMultipleComponent.html` | Prevent duplicate components on one GameObject |
| `[ExecuteAlways]` / `[ExecuteInEditMode]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ExecuteAlways.html`, `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/ExecuteInEditMode.html` | Run lifecycle methods in Edit mode (tooling, gizmos, live preview) |
| `[CreateAssetMenu]` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/CreateAssetMenuAttribute.html` | Adds "Create > ..." menu entry for a `ScriptableObject` subclass; `fileName`/`menuName`/`order` |
| UnityEvents manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/unity-events.html` | Inspector-wireable events vs C# delegates/`Action` |
| `UnityEvent` / generic `UnityEvent<T0..T3>` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Events.UnityEvent.html`, `Events.UnityEvent_1.html`…`_4.html` | `AddListener`/`RemoveListener`/`Invoke` API, up to 4 generic type params |
| `UnityEventBase` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Events.UnityEventBase.html` | Persistent-listener introspection (editor tooling level) |
| `ObjectPool<T>` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Pool.ObjectPool_1.html` (+ `-ctor.html`, `.Get.html`, `.Release.html`, `.Clear.html`, `.Dispose.html`, `.CountActive.html`, `.CountInactive.html`, `.CountAll.html`) | Built-in generic pooling implementation, ctor delegates, capacity/collection-check params |
| `IObjectPool<T>` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Pool.IObjectPool_1.html` | Pool abstraction interface for swapping pool implementations |
| Domain reload manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/domain-reloading.html` | What domain reload resets on Play-mode entry; disabling it and its state-reset obligations |
| Async/await support manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/async-await-support.html` | `async`/`await` in Unity scripts, general constraints |
| Awaitable introduction manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/async-awaitable-introduction.html` | `Awaitable` vs `Task`, when to prefer each |
| Awaitable examples manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/async-awaitable-examples.html` | Worked examples: cancellation, main/background thread hops |
| `Awaitable` class ref | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Awaitable.html` (+ `.NextFrameAsync.html`, `.WaitForSecondsAsync.html`, `.FixedUpdateAsync.html`, `.MainThreadAsync.html`, `.BackgroundThreadAsync.html`, `.Cancel.html`, `.IsCompleted.html`) | Unity's pooled awaitable type: async alternative to coroutines |
| `AwaitableCompletionSource` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/AwaitableCompletionSource.html` | Manually complete/cancel an `Awaitable` from callback-based APIs |
| `Object.Instantiate` / `Object.Destroy` / `DestroyImmediate` / `DontDestroyOnLoad` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Object.Instantiate.html`, `.Destroy.html`, `.DestroyImmediate.html`, `.DontDestroyOnLoad.html` | Object creation/teardown semantics that interact with the lifecycle |
| `Object.FindFirstObjectByType`/`FindObjectsByType` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Object.FindFirstObjectByType.html`, `Object.FindObjectsByType.html` | Current (non-obsolete) replacement for `FindObjectOfType`/`FindObjectsOfType` |
| `Component.GetComponent`/`TryGetComponent` | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/ScriptReference/Component.GetComponent.html`, `.TryGetComponent.html` | Caching-vs-lookup cost; `TryGetComponent` avoids exceptions/null-checks |
| Generic Animations manual | `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/GenericAnimations.html` | Unity's own use of generics in the Animation system, as a worked precedent |

## Key Guidelines

### Lifecycle & Execution Order

Unity calls "event functions" (`Awake`, `OnEnable`, `Start`, `Update`, etc.) by name/reflection, not through an interface — they are not virtual overrides, so a typo in the signature (wrong case, wrong parameter list, accidentally `static`) fails silently with no compiler error. `Awake` runs exactly once per instantiation, immediately after the object is created/loaded, regardless of whether the component or GameObject is enabled — use it for self-contained initialization that doesn't depend on other objects. `OnEnable` runs every time the object transitions to active (including the first time, right after `Awake`, and again after any `SetActive(true)`/`enabled = true` toggle) — use it to (re)subscribe to events and reset per-activation state, paired with unsubscription in `OnDisable`. `Start` runs exactly once, only if the object is enabled, immediately before the first `Update` — this is the right place for cross-object references, because every other object's `Awake` in the scene is guaranteed to have already run (but not necessarily their `Start`). `SceneManager.sceneLoaded` fires after `OnEnable` but before `Start` for all objects in a freshly loaded scene, per the execution-order manual — don't assume `Start` is the earliest point a loaded scene is queryable. Execution order across *different* GameObjects at the same phase (e.g. two objects' `Awake`) is undefined unless explicitly pinned via Project Settings > Script Execution Order — never rely on scene-hierarchy or component-list ordering by default; if two systems have a hard ordering dependency, either use that setting or restructure so the dependency is resolved lazily (e.g. a property that self-initializes on first access) rather than assumed. `FixedUpdate` runs on a fixed timestep, potentially zero or multiple times per rendered frame, before the physics step — put physics-affecting code (`Rigidbody` forces/velocity) here, never in `Update`. `LateUpdate` runs once per frame after every `Update` call across all objects has completed — use it for camera-follow and other logic that must react to a fully-updated frame. `OnDisable` and `OnDestroy` fire on deactivation/destruction respectively; `OnDestroy` is also called on scene unload for objects not marked `DontDestroyOnLoad`, and on application quit (mobile/editor ordering can vary — don't do critical save-on-quit work only in `OnDestroy` without also handling `OnApplicationQuit`/`OnApplicationPause`). `OnValidate` and `Reset` are Editor-only: `OnValidate` runs whenever a serialized field changes in the Inspector (including via Undo), `Reset` runs once when the component is first added or when "Reset" is chosen from its context menu — never put runtime-only logic (e.g. GetComponent expecting a scene reference) in `OnValidate` without null-guarding, since it also runs outside Play mode.

```csharp
public class Turret : MonoBehaviour
{
    [SerializeField] private float fireRate = 1f;
    private Health targetHealth; // cross-object ref resolved in Start

    private void Awake()
    {
        // Safe: only touches this object's own components.
        GetComponent<Animator>();
    }

    private void OnEnable()
    {
        GameEvents.OnWaveStarted += HandleWaveStarted;
    }

    private void OnDisable()
    {
        GameEvents.OnWaveStarted -= HandleWaveStarted; // must mirror OnEnable exactly
    }

    private void Start()
    {
        // Safe here: every object's Awake in the scene has already run.
        targetHealth = FindFirstObjectByType<Health>();
    }

    private void HandleWaveStarted() { /* ... */ }
}
```

### Serialization

Unity's serializer is not the .NET `[Serializable]`/`BinaryFormatter` system, and only understands a specific subset of types: it serializes public fields and `[SerializeField]`-marked private/protected fields of primitives, strings, enums, Unity object references, and `[Serializable]`-marked plain classes/structs (nested up to a fixed depth) — it never serializes properties, static fields, `const`/`readonly` fields, or fields marked `[NonSerialized]`. `[Serializable]` is not inherited: every class in a hierarchy that needs serialization must be marked individually. Unity cannot natively serialize `Dictionary<TKey,TValue>`, interfaces, `abstract` base-typed fields, or multidimensional arrays (jagged arrays of serializable types are fine) — for these, either restructure the data (e.g. two parallel `List<>`s instead of a `Dictionary`) or implement `ISerializationCallbackReceiver` to flatten the data into a serializable shape in `OnBeforeSerialize` and rebuild it in `OnAfterDeserialize`. `[SerializeReference]` is the exception for polymorphism: it lets a field typed as an interface or abstract class serialize by reference to a concrete `[Serializable]` instance, preserving the runtime type across domain reloads and scene saves — but every concrete type used this way must itself be `[Serializable]`, and renaming/moving those types breaks existing serialized data unless `[FormerlySerializedAs]`-style migration is handled. Every serialized field, public or private, appears in the Inspector by default; `[HideInInspector]` keeps a field serialized (round-trips through saves/prefabs) while hiding it from the Inspector UI, whereas `[NonSerialized]` removes it from serialization entirely (and as a side effect it also disappears from the Inspector). Because serialization drives prefab instantiation, scene saving, Undo/Redo, and Editor hot-reload alike (per `script-serialization-how-unity-uses.html`), a field that fails to serialize correctly can silently reset on every domain reload during iteration — this is a common source of "my value keeps resetting in the Editor" bugs.

```csharp
[Serializable]
public struct DamageInfo
{
    public float amount;
    public DamageType type;
}

public class Weapon : MonoBehaviour
{
    [SerializeField] private DamageInfo baseDamage;      // struct, serializes fine
    [SerializeField, HideInInspector] private int hitCount; // serialized, hidden from UI

    // Interfaces need SerializeReference + a concrete [Serializable] implementation.
    [SerializeReference] private IWeaponModifier modifier;
}

[Serializable]
public class SpreadModifier : IWeaponModifier
{
    public float spreadDegrees;
}

// Dictionary isn't serializable directly — flatten it.
public class LootTable : MonoBehaviour, ISerializationCallbackReceiver
{
    private Dictionary<string, int> weights = new();
    [SerializeField] private List<string> keys = new();
    [SerializeField] private List<int> values = new();

    public void OnBeforeSerialize()
    {
        keys.Clear(); values.Clear();
        foreach (var kv in weights) { keys.Add(kv.Key); values.Add(kv.Value); }
    }

    public void OnAfterDeserialize()
    {
        weights = new Dictionary<string, int>();
        for (int i = 0; i < keys.Count; i++) weights[keys[i]] = values[i];
    }
}
```

### ScriptableObjects

`ScriptableObject` is Unity's asset-backed, non-MonoBehaviour data container: it isn't attached to a GameObject, doesn't live in a scene, and is instead a serialized asset on disk (or a runtime-only instance via `ScriptableObject.CreateInstance<T>()`). Use it for shared config, event channels, and inventories/data tables that multiple scenes or objects need to reference identically, instead of a scene-bound singleton/static manager — an SO reference survives scene loads/unloads because it isn't part of any scene. Its lifecycle is asset-based, not scene-based: `OnEnable` fires when the asset is loaded (including on Editor domain reload and when entering Play mode, since the asset gets reloaded/reinitialized), and `OnDisable` fires on unload — this is not the same as a MonoBehaviour "becoming active"; don't assume SO `OnEnable` maps to "GameObject became visible." A critical gotcha: an asset-based `ScriptableObject` instance is shared and persists across Play sessions in the Editor — if a script mutates its fields at runtime (e.g. a "current health" field on a shared `CharacterStatsSO`), that mutation survives after you stop Play mode, corrupting the authored asset for the next session. Either treat SO fields as read-only config and keep mutable runtime state elsewhere, or explicitly clone the SO at runtime (`Instantiate(sourceSO)`) and mutate the clone, or reset fields in `OnEnable`/`OnDisable`. `[CreateAssetMenu]` adds a "Create > ..." Project-window menu entry so designers can create asset instances without code; `fileName` sets the default asset name, `menuName` sets the menu path, `order` controls menu position.

```csharp
[CreateAssetMenu(fileName = "NewCharacterStats", menuName = "Game/Character Stats", order = 0)]
public class CharacterStatsSO : ScriptableObject
{
    public float maxHealth = 100f;
    public float moveSpeed = 5f;
}

// Event-channel pattern: decouples publisher and subscribers via an asset, not a scene ref.
[CreateAssetMenu(menuName = "Game/Events/Void Event Channel")]
public class VoidEventChannelSO : ScriptableObject
{
    public event Action OnRaised;
    public void Raise() => OnRaised?.Invoke();
}

// Runtime-mutable state should be a clone, not the shared asset:
public class Character : MonoBehaviour
{
    [SerializeField] private CharacterStatsSO baseStats; // shared, read-only template
    private CharacterStatsSO runtimeStats;                // per-instance clone

    private void Awake()
    {
        runtimeStats = Instantiate(baseStats); // safe to mutate; doesn't touch the asset
    }
}
```

### Coroutines

A coroutine is a method returning `IEnumerator` that Unity drives incrementally, resuming it once per matching update phase rather than running it to completion synchronously — it is not a thread, and all its code still runs on the main thread. `yield return null` resumes the coroutine on the next frame's `Update`-equivalent point; `yield return new WaitForSeconds(t)` resumes after `t` seconds of scaled time; `yield return new WaitForFixedUpdate()` resumes on the next `FixedUpdate`; `yield return new WaitForEndOfFrame()` resumes after rendering for that frame completes (useful for screenshot capture); `yield return new WaitUntil(predicate)` / `WaitWhile(predicate)` poll a condition each frame; `yield return otherCoroutine` (yielding another coroutine's `IEnumerator` or `Coroutine`) nests execution, resuming the parent only once the child finishes. `StartCoroutine` is a `MonoBehaviour` instance method — the coroutine is bound to that specific GameObject/component, and Unity automatically stops it if the host GameObject is deactivated or destroyed (it does *not* automatically stop merely because the component's `enabled` flag was set false while the GameObject stays active — component-level disable does not implicitly cancel its own coroutines, so explicit `StopCoroutine` in `OnDisable` is still required for correctness). `StartCoroutine` returns a `Coroutine` handle; store it if you need to `StopCoroutine` that specific instance later, since starting the "same" coroutine method twice by name only reliably stops the most recent call when using the string-based overload. `StopAllCoroutines` stops every coroutine started on that MonoBehaviour instance, which is a blunt instrument if the same component runs multiple independent coroutines. For long-lived coroutines that must survive their originating object being destroyed/disabled, host them on a persistent manager object instead. Unity 6 also offers `Awaitable`-based `async`/`await` as a coroutine alternative for a subset of cases — see Advanced Notes.

```csharp
public class Fader : MonoBehaviour
{
    private Coroutine fadeRoutine;

    public void FadeTo(float target, float duration)
    {
        if (fadeRoutine != null) StopCoroutine(fadeRoutine);
        fadeRoutine = StartCoroutine(FadeRoutine(target, duration));
    }

    private IEnumerator FadeRoutine(float target, float duration)
    {
        float start = CanvasGroupAlpha();
        float t = 0f;
        while (t < duration)
        {
            t += Time.deltaTime;
            SetAlpha(Mathf.Lerp(start, target, t / duration));
            yield return null; // resume next frame
        }
        SetAlpha(target);
        fadeRoutine = null;
    }

    private void OnDisable()
    {
        // GameObject deactivation auto-stops coroutines, but explicit stop
        // guards against component-only disable and keeps intent explicit.
        if (fadeRoutine != null) { StopCoroutine(fadeRoutine); fadeRoutine = null; }
    }

    private float CanvasGroupAlpha() => 0f; // placeholder
    private void SetAlpha(float a) { }       // placeholder
}
```

### Events & Delegates

Prefer plain C# `event Action<T>` (or `event EventHandler<T>`) over direct component references for decoupling one script from another's concrete type — subscribers only need to know the delegate signature, not the publisher's class. Always unsubscribe in the mirrored teardown method (`OnDisable` for a subscription made in `OnEnable`) — a missed unsubscription is a common source of `MissingReferenceException`/leaked-object bugs, because the event's invocation list keeps a live reference to the subscriber, preventing GC and firing callbacks on effectively-dead objects. `UnityEvent` (and its generic variants `UnityEvent<T0>` through the 4-type-parameter form) exists specifically so designers can wire method calls from the Inspector without code — use it for GameObject-level hooks meant to be authored visually (e.g. a Button's `onClick`, or a damage-received hook a designer wants to route to different effects per-prefab), but reserve it for that Inspector-wiring use case: `UnityEvent.Invoke()` is measurably slower than a C# delegate invocation (reflection-based persistent-listener resolution) and offers no compile-time type safety on manually-wired listener methods beyond parameter count/type matching at the moment of binding. `UnityEvent` supports up to four generic parameters (`UnityEvent<T0,T1,T2,T3>`); beyond that, or for anything performance-sensitive (e.g. per-frame or per-physics-step events), use a plain delegate. `AddListener`/`RemoveListener` on a `UnityEvent` mirror `+=`/`-=` on a C# event but only affect runtime ("non-persistent") listeners; listeners wired via the Inspector are "persistent" and are managed separately by Unity's serialization of the event.

```csharp
public class Health : MonoBehaviour
{
    // Code-to-code decoupling: fast, type-safe, no Inspector wiring needed.
    public event Action<float> OnDamaged;
    public event Action OnDied;

    [SerializeField] private float currentHealth = 100f;

    public void TakeDamage(float amount)
    {
        currentHealth -= amount;
        OnDamaged?.Invoke(amount);
        if (currentHealth <= 0f) OnDied?.Invoke();
    }
}

public class DamageListener : MonoBehaviour
{
    [SerializeField] private Health health;

    private void OnEnable()  { health.OnDamaged += HandleDamaged; health.OnDied += HandleDied; }
    private void OnDisable() { health.OnDamaged -= HandleDamaged; health.OnDied -= HandleDied; }

    private void HandleDamaged(float amount) { /* flash red, play SFX */ }
    private void HandleDied() { /* ragdoll, disable input */ }
}

// Inspector-wireable version, for designer-facing hooks (e.g. on a UI Button):
public class DamageFlashUI : MonoBehaviour
{
    [SerializeField] private UnityEvent<float> onDamagedUI; // designer assigns listeners in Inspector
    public void Trigger(float amount) => onDamagedUI?.Invoke(amount);
}
```

### Attributes

Built-in attributes fall into two buckets: Inspector-display-only (no runtime effect) and behavior-affecting. `[Header("...")]`, `[Tooltip("...")]`, `[Space]`/`[Space(height)]`, `[Multiline]`/`[Multiline(lines)]`, and `[TextArea(minLines, maxLines)]` only change how a field renders in the Inspector — stripping them changes nothing at runtime. `[Range(min, max)]` both constrains the Inspector to a slider *and* clamps values assigned via the Inspector, but does not clamp values set purely in code — a script can still assign an out-of-range value via `field = 999f`. `[Delayed]` defers committing a text field's edit until Enter/focus-loss rather than on every keystroke — useful for fields that trigger expensive `OnValidate` work. `[SerializeField]`, `[SerializeReference]`, `[HideInInspector]`, and `[NonSerialized]` affect serialization (see above). `[RequireComponent(typeof(T))]` auto-adds a dependency component when this component is added via the Editor's "Add Component" and blocks manual removal of that dependency while this component is present — note it does **not** retroactively add the dependency to GameObjects that already have this script attached from before the attribute existed, and it has no effect on components added purely via code (`AddComponent<T>()` bypasses the auto-add safety net, so a script relying on `[RequireComponent]` should still null-check or `GetComponent` defensively if it can be added programmatically). `[DisallowMultipleComponent]` prevents a GameObject from carrying more than one instance of the component type. `[ExecuteAlways]` (current) and the older `[ExecuteInEditMode]` make lifecycle methods run in Edit mode as well as Play mode — essential for gizmo-driven tooling or live-preview components, but it means `Awake`/`OnEnable`/`Update` etc. can run without ever entering Play mode, so code must not assume Play-mode-only state (e.g. `Time.deltaTime` behaves differently, and singletons/managers may not exist yet). `[ContextMenu("Label")]` on a method adds a right-click Inspector action; `[ContextMenuItem("Label", "MethodName")]` on a field does the same but scoped to that field's context menu — both are Editor/debug conveniences, not shipped-build functionality (they still compile into player builds but are unreachable without the Editor UI).

```csharp
[RequireComponent(typeof(Rigidbody))]
[DisallowMultipleComponent]
public class Projectile : MonoBehaviour
{
    [Header("Flight")]
    [Tooltip("Muzzle velocity in meters/second.")]
    [Range(1f, 200f)] public float speed = 40f;

    [Space(10)]
    [TextArea(2, 4)] public string debugNotes;

    [SerializeField, Delayed] private int poolSize = 32; // commit on Enter, not per keystroke

    private Rigidbody rb; // guaranteed present at runtime because of RequireComponent

    [ContextMenu("Reset Velocity")]
    private void ResetVelocity()
    {
        if (rb == null) rb = GetComponent<Rigidbody>();
        rb.linearVelocity = Vector3.zero;
    }
}

[ExecuteAlways]
public class GizmoRadius : MonoBehaviour
{
    public float radius = 3f;
    private void OnDrawGizmosSelected() => Gizmos.DrawWireSphere(transform.position, radius);
}
```

### Object Pooling

Reusing instances instead of repeated `Instantiate`/`Destroy` avoids GC churn and instantiation overhead for frequently-spawned objects (bullets, particles, enemies). Unity 6 ships a generic `UnityEngine.Pool.ObjectPool<T>` (implementing `IObjectPool<T>`) so hand-rolled pools are rarely needed. Its constructor takes four delegates plus two capacity controls: `createFunc` (`Func<T>`) constructs a brand-new instance when the pool is empty; `actionOnGet` (`Action<T>`) runs when an instance is checked out — activate/reset it here; `actionOnRelease` (`Action<T>`) runs when an instance is returned — deactivate/clean it here; `actionOnDestroy` (`Action<T>`) runs when the pool must discard an instance because it's over `maxSize`; `collectionCheck` (bool) enables a debug-time check that throws if the same instance is released twice; `defaultCapacity` and `maxSize` bound the pool's internal collection size. Code against `IObjectPool<T>` in consumer classes so the concrete pool implementation can be swapped later without touching call sites.

```csharp
public class BulletPool : MonoBehaviour
{
    [SerializeField] private Bullet bulletPrefab;
    private IObjectPool<Bullet> pool;

    private void Awake()
    {
        pool = new ObjectPool<Bullet>(
            createFunc: () => Instantiate(bulletPrefab),
            actionOnGet: b => b.gameObject.SetActive(true),
            actionOnRelease: b => b.gameObject.SetActive(false),
            actionOnDestroy: b => Destroy(b.gameObject),
            collectionCheck: true,
            defaultCapacity: 20,
            maxSize: 100);
    }

    public Bullet Spawn(Vector3 pos, Quaternion rot)
    {
        var bullet = pool.Get();
        bullet.transform.SetPositionAndRotation(pos, rot);
        bullet.Init(this); // bullet calls pool.Release(this) on impact/timeout
        return bullet;
    }

    public void Despawn(Bullet b) => pool.Release(b);
}
```

### Generics

Generic MonoBehaviour subclasses (`public class Foo<T> : MonoBehaviour`) compile but Unity cannot serialize or attach an open generic type directly to a GameObject — the Inspector can't display it and it can't be added via "Add Component." The workaround is a non-generic subclass that closes the type parameter (`public class IntFoo : Foo<int> { }`); only the closed, concrete subclass is addable/serializable. This pattern is genuinely useful for shared base behavior (e.g. a generic `StateMachine<TState>` or a generic `Singleton<T>` base class) as long as every concrete usage goes through a closed subclass. Generic `ScriptableObject` subclasses have the same restriction — `CreateAssetMenu` and asset creation require a closed concrete type. Plain (non-MonoBehaviour, non-ScriptableObject) generic classes/structs have no such restriction and are ordinary C#; they only hit Unity-specific limits if a field of that generic type needs Inspector serialization, in which case the same "must be a closed, `[Serializable]`-marked concrete type" rule applies as for any other serialized field.

```csharp
// Base generic logic — cannot be attached directly to a GameObject.
public abstract class Singleton<T> : MonoBehaviour where T : Singleton<T>
{
    public static T Instance { get; private set; }

    protected virtual void Awake()
    {
        if (Instance != null && Instance != this) { Destroy(gameObject); return; }
        Instance = (T)this;
    }
}

// Closed concrete subclass — this is what actually goes on a GameObject.
public class AudioManager : Singleton<AudioManager>
{
    public void PlayClip(AudioClip clip) { /* ... */ }
}

// Serializable generic field must also be a closed type:
[Serializable]
public class Pair<TA, TB> { public TA first; public TB second; }

public class Config : MonoBehaviour
{
    [SerializeField] private Pair<string, int> nameToScore; // closed: Pair<string,int> — serializes fine
}
```

## Common Mistakes

| Mistake | Root cause | Fix |
|---|---|---|
| Reading another object's state in `Awake` | Sibling `Awake` order across GameObjects is undefined | Do cross-object lookups in `Start`, which runs only after every object's `Awake` has completed |
| Forgetting `StopCoroutine` on disable/destroy | GameObject deactivation stops coroutines, but component-only `enabled = false` does not | Store the `Coroutine` handle and explicitly stop it in `OnDisable` |
| Mutating a shared `ScriptableObject` asset and expecting a reset | SO asset instances persist across Editor Play sessions; a runtime mutation writes back to the authored asset | Treat SO fields as read-only templates, or `Instantiate()` a runtime clone and mutate that instead |
| Expecting bare `public` fields to be Inspector-hidden | Any public field is serialized and shown by default | Use private `[SerializeField]`, or add `[HideInInspector]` if it must stay serialized but unseen |
| `[SerializeField]` on an interface-typed or abstract-typed field | The Unity serializer can't resolve concrete type from an interface/abstract reference | Use `[SerializeReference]` with a concrete `[Serializable]` implementing class |
| Assuming same-GameObject scripts run in list order | Component order in the Inspector is not a guaranteed execution order without explicit configuration | Set Project Settings > Script Execution Order for any hard cross-component ordering dependency |
| Missing `event -=` unsubscription | The publisher's invocation list keeps a live reference to the subscriber, blocking GC and firing on stale objects | Always unsubscribe in the lifecycle method that mirrors the subscription (`OnDisable` for `OnEnable`, `OnDestroy` for `Awake`) |
| Using `UnityEvent` for high-frequency, code-only signaling | `UnityEvent.Invoke()` carries reflection/persistent-listener overhead absent from a plain delegate | Use `event Action<T>` for per-frame or code-to-code events; reserve `UnityEvent` for Inspector-authored hooks |
| Relying on `[RequireComponent]` when adding components purely via code | The attribute's auto-add only fires through the Editor's "Add Component" UI, not `AddComponent<T>()` calls | Still null-check/`GetComponent` defensively, or explicitly `AddComponent<T>()` the dependency in code |
| Assuming `[Range]` clamps values set in code | `[Range]` only clamps edits made through the Inspector slider | Clamp manually in code (`Mathf.Clamp`) wherever the field is assigned at runtime |
| Putting runtime-only logic in `OnValidate` | `OnValidate` runs in the Editor outside Play mode too, where runtime state/managers don't exist yet | Null-guard any runtime references accessed from `OnValidate`, or gate the logic with `Application.isPlaying` |
| Assuming `Dictionary<>` fields serialize and survive domain reload | Unity's serializer has no native `Dictionary` support | Flatten to parallel `List<>`s via `ISerializationCallbackReceiver`, or use a third-party serializable dictionary |
| Attaching a generic MonoBehaviour/ScriptableObject directly | Unity cannot instantiate or serialize an open generic type as a component/asset | Create a closed, non-generic subclass (`class Foo : Base<int>`) and attach/create that instead |
| Double-releasing an object back into an `ObjectPool<T>` | No guard by default; corrupts pool state or causes duplicate active instances | Construct the pool with `collectionCheck: true` to get an exception on double-release during development |
| Doing critical save/cleanup only in `OnDestroy` | `OnDestroy` ordering on quit varies by platform, and scene-unload also triggers it | Also handle `OnApplicationQuit`/`OnApplicationPause` for anything that must run reliably before process exit |

## Quick Reference

| Method / Attribute / Pattern | Fires when / Purpose |
|---|---|
| `Awake()` | Once, on instantiation, before any `Start`, even if the object starts disabled |
| `OnEnable()` | Every activation, including the first (right after `Awake`) and every subsequent re-enable |
| `Reset()` | Editor-only; once, when the component is first added or "Reset" is invoked from its context menu |
| `Start()` | Once, only if enabled, immediately before the object's first `Update` |
| `sceneLoaded` (SceneManager) | After `OnEnable`, before `Start`, for all objects in a newly loaded scene |
| `FixedUpdate()` | Fixed physics timestep, zero-to-many times per rendered frame, before the physics step |
| `Update()` | Once per frame |
| `LateUpdate()` | Once per frame, after all objects' `Update` calls have completed |
| `OnValidate()` | Editor-only; whenever a serialized field changes via Inspector/Undo |
| `OnDisable()` | On deactivation (`SetActive(false)`, `enabled = false`, or destruction) |
| `OnDestroy()` | On destruction or scene unload (for objects not marked `DontDestroyOnLoad`) |
| `OnApplicationPause(bool)` / `OnApplicationFocus(bool)` / `OnApplicationQuit()` | App lifecycle transitions — pause/resume, focus change, shutdown |
| `destroyCancellationToken` | Property; cancellation token that fires when this MonoBehaviour is destroyed — cache it before destruction, use with `Awaitable`/`async` |
| `didAwake` / `didStart` | Properties; query whether `Awake`/`Start` has already executed on this instance |
| `Invoke(name, delay)` / `InvokeRepeating(name, delay, interval)` | Reflection-based delayed/repeating method calls; `CancelInvoke()` stops them, `IsInvoking()` queries |
| `StartCoroutine(IEnumerator)` → `Coroutine` | Begins a coroutine bound to this MonoBehaviour; auto-stops on GameObject deactivation/destruction |
| `StopCoroutine(...)` / `StopAllCoroutines()` | Stops one coroutine handle (or by name) / stops every coroutine on this instance |
| `yield return null` | Resume next frame |
| `yield return new WaitForSeconds(t)` | Resume after `t` seconds of scaled time |
| `yield return new WaitForFixedUpdate()` | Resume on next `FixedUpdate` |
| `yield return new WaitForEndOfFrame()` | Resume after rendering completes for the frame |
| `yield return new WaitUntil(() => cond)` / `WaitWhile(...)` | Resume once predicate becomes true / false |
| `ScriptableObject.CreateInstance<T>()` | Create a runtime SO instance not backed by an asset file |
| `ScriptableObject.OnEnable/OnDisable` | Asset-load/unload lifecycle, not scene-activation lifecycle |
| `[CreateAssetMenu(fileName, menuName, order)]` | Adds a "Create > ..." Project-window entry for a `ScriptableObject` subclass |
| `[SerializeField]` | Expose/force-serialize a private field |
| `[SerializeReference]` | Serialize a polymorphic/interface-typed field by reference to a concrete `[Serializable]` type |
| `[NonSerialized]` | Exclude a field from serialization (and thus from the Inspector) |
| `[HideInInspector]` | Keep serialized, hide from Inspector |
| `ISerializationCallbackReceiver` | `OnBeforeSerialize`/`OnAfterDeserialize` hooks for flattening unserializable types (e.g. `Dictionary`) |
| `[Header("...")]`, `[Tooltip("...")]`, `[Space]`, `[Multiline]`, `[TextArea]` | Inspector display only, zero runtime effect |
| `[Range(min,max)]` | Inspector slider; clamps Inspector edits only, not code assignments |
| `[Delayed]` | Defers Inspector text-field commit until Enter/focus-loss |
| `[RequireComponent(typeof(T))]` | Auto-adds and guards a dependency component, but only through the Editor "Add Component" path |
| `[DisallowMultipleComponent]` | Prevents more than one instance of the component per GameObject |
| `[ExecuteAlways]` / `[ExecuteInEditMode]` | Runs lifecycle methods in Edit mode as well as Play mode |
| `[ContextMenu("Label")]` / `[ContextMenuItem("Label","Method")]` | Adds a right-click Inspector action (method / field-scoped) |
| `event Action<T>` / `event EventHandler<T>` | Type-safe, low-overhead code-to-code pub/sub; must mirror `+=`/`-=` across lifecycle |
| `UnityEvent` / `UnityEvent<T0..T3>` | Inspector-wireable event, up to 4 generic params, higher per-invoke overhead than a delegate |
| `ObjectPool<T>` / `IObjectPool<T>` | Built-in generic pool: `Get()`, `Release()`, `Clear()`, `CountActive/Inactive/All` |
| `Awaitable` / `async`/`await` | Unity 6's pooled awaitable type; alternative to coroutines for async control flow (see Advanced Notes) |
| `Object.Instantiate` / `Object.Destroy` / `DestroyImmediate` / `DontDestroyOnLoad` | Object creation/teardown; `DontDestroyOnLoad` exempts from scene-unload destruction |
| `Object.FindFirstObjectByType<T>` / `FindObjectsByType<T>` | Current, non-obsolete scene-object lookup APIs |
| `Component.TryGetComponent<T>(out var c)` | Non-throwing component lookup, avoids `GetComponent` null-check boilerplate |
| Closed generic subclass pattern | Required to attach/create a generic MonoBehaviour or ScriptableObject; the open generic type itself cannot be |

## Advanced Notes

**Domain reload behavior.** By default, Unity reloads the scripting domain every time you enter Play mode, which wipes all managed state — static fields, cached singletons, subscribed events — back to their compile-time defaults, exactly as a fresh process launch would. This is why static counters/caches "reset" correctly between Play sessions without any explicit cleanup code, and it's a safety net that hides bugs where cleanup logic is actually missing. Domain reload is also comparatively slow, so Unity offers "Enter Play Mode Settings" to disable domain reload (and/or scene reload) for faster iteration — but disabling it means your code is now responsible for manually resetting every piece of static/persistent state on Play-mode entry (typically via `[RuntimeInitializeOnLoadMethod]` or explicit reset calls), because the previous session's static state otherwise leaks into the new one. Domain reload also occurs on script compilation while the Editor is not in Play mode and as part of asset-database refresh in some workflows — code that assumes "domain reload only happens between Play sessions" can be surprised by an Edit-mode reload triggered by simply saving a script.

**Async/await vs coroutines.** Unity 6 supports genuine C# `async`/`await` using its own `Awaitable` type (`UnityEngine.Awaitable`) as a Unity-aware alternative to `System.Threading.Tasks.Task`. Coroutines are Unity's original mechanism: an `IEnumerator`-based method driven frame-by-frame by the engine, cooperative and cancellation-by-convention (stopping requires holding the `Coroutine` handle or being GameObject-scoped). `Awaitable`-based `async` methods integrate with normal C# control flow (`try`/catch, `return` values, composing with `await`), support real cancellation via `CancellationToken` (including the built-in `destroyCancellationToken`, which fires automatically when the hosting MonoBehaviour is destroyed — critical for avoiding "continuing an async method on a destroyed object" bugs), and offer entry points like `Awaitable.NextFrameAsync()`, `Awaitable.WaitForSecondsAsync()`, `Awaitable.FixedUpdateAsync()`, `Awaitable.MainThreadAsync()`, and `Awaitable.BackgroundThreadAsync()` mirroring the common coroutine yield instructions plus explicit thread-affinity control that coroutines never had. The key gotcha, per the `Awaitable` class reference: instances are pooled internally and are **not safe to await more than once** — do not cache an `Awaitable` and re-await it, and do not fire the same async method call's result off to two separate awaiters. Prefer `Awaitable` over `Task` for Unity-facing async code specifically because `Awaitable` is allocation-lean (pooled) and integrates with Unity's player loop and destroy-cancellation, whereas a raw `Task` continuation can resume on a thread-pool thread and touch Unity API objects unsafely from off the main thread. Choose coroutines when the calling code is simple sequential frame-waiting with no need for return values or exception propagation and when working in codebases already standardized on `IEnumerator`; choose `async`/`Awaitable` when you need a return value, proper exception handling, cancellation tied to object lifetime, or composition with other `async` code (including real `Task`-based I/O, which still requires bridging back to the main thread before touching Unity objects).

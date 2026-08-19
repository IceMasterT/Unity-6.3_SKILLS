---
name: unity-testing
description: Use when writing automated tests for Unity code — the Unity Test Framework, Edit Mode vs Play Mode tests, or the Test Runner. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Testing

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Package overview | `Manual/com.unity.test-framework.html` | Package summary, version info |
| Introduction | `Manual/test-framework/test-framework-introduction.html` | What UTF is, custom NUnit vs Unity tests, topic map |
| Get started hub | `Manual/test-framework/getting-started.html` | First-time setup path (assembly → test → run) |
| Edit Mode vs Play Mode | `Manual/test-framework/edit-mode-vs-play-mode-tests.html` | Which mode to pick, asmdef platform requirements for each |
| Create a test assembly | `Manual/test-framework/workflow-create-test-assembly.html` | asmdef scaffolding via Test Runner window / Assets menu |
| Create a test | `Manual/test-framework/workflow-create-test.html` | Generating a `NewTestScript.cs` from the Test Runner window |
| Writing tests hub | `Manual/test-framework/writing-tests.html` | Topic map: setup/teardown, asserting, yield instructions, parameterized, async |
| Before/after tests hub | `Manual/test-framework/before-and-after-tests.html` | Topic map for setup/teardown/outer-action/build-time hooks |
| Unity setup/teardown | `Manual/test-framework/reference-unitysetup-and-unityteardown.html` | `[UnitySetUp]`/`[UnityTearDown]`/`[UnityOneTimeSetUp]`/`[UnityOneTimeTearDown]`, base/derived execution order |
| Build-time setup/cleanup | `Manual/test-framework/reference-setup-and-cleanup.html` | `IPrebuildSetup`/`IPostBuildCleanup`, `[PrebuildSetup]`/`[PostBuildCleanup]` |
| Outer test action | `Manual/test-framework/reference-outerunitytestaction.html` | `IOuterUnityTestAction`, code that wraps a test with Enter/ExitPlayMode |
| Execution order | `Manual/test-framework/reference-actions-outside-tests.html` | Full ordered list of every setup/teardown/action callback |
| Asserting and comparing | `Manual/test-framework/asserting-and-comparing.html` | `LogAssert`, `Assert.That` constraints, custom comparers |
| Yield instructions for Editor | `Manual/test-framework/reference-custom-yield-instructions.html` | `EnterPlayMode`/`ExitPlayMode`/`RecompileScripts`/`WaitForDomainReload`, `MonoBehaviourTest<T>` |
| Parameterized tests | `Manual/test-framework/reference-tests-parameterized.html` | `[ValueSource]` (Unity tests), `ParameterizedIgnoreAttribute` |
| Async tests | `Manual/test-framework/reference-async-tests.html` | `async Task` `[Test]` methods, Task-based awaiting model |
| Running tests hub | `Manual/test-framework/running-tests.html` | Topic map: window, CLI, code, Player, build modifier |
| Run in Test Runner window | `Manual/test-framework/workflow-run-test.html` | Run/filter UI, Rider integration, known duration-reporting limitation |
| Run from command line (workflow) | `Manual/test-framework/run-tests-from-command-line.html` | Minimal `-runTests -batchmode` invocation example |
| Command-line reference | `Manual/test-framework/reference-command-line.html` | Full flag list (`-assemblyNames`, `-testFilter`, `-testCategory`, `-repeat`, `-retry`, `-randomOrderSeed`, `-runSynchronously`, `-testSettingsFile`, …) and `TestSettings.json` schema |
| Run Play Mode tests in a Player | `Manual/test-framework/workflow-run-playmode-test-standalone.html` | On-device runs, network requirement for results to report back, Build/Export-only options |
| Modify Player build for tests | `Manual/test-framework/reference-attribute-testplayerbuildmodifier.html` | `[TestPlayerBuildModifier]` assembly attribute, `ITestPlayerBuildModifier` |
| Run tests from code hub | `Manual/test-framework/running-tests-from-code.html` | `TestRunnerApi` topic map |
| Specify which tests to run | `Manual/test-framework/extension-run-tests.html` | `TestRunnerApi.Execute`, `Filter`, `ExecutionSettings`, multi-filter semantics |
| Retrieve test results | `Manual/test-framework/extension-get-test-results.html` | `ICallbacks`, `RegisterCallbacks` |
| Retrieve the test list | `Manual/test-framework/extension-retrieve-test-list.html` | `RetrieveTestList`, `ITestAdaptor`, `TestMode` |
| Learning materials / course | `Manual/test-framework/course/overview.html`, `course/test-framework-general-introduction.html` | Applied tutorial series (general intro + a "Testing Lost Crypt" sample-game walkthrough under `course/LostCrypt/`) |
| Build flag | `ScriptReference/BuildOptions.IncludeTestAssemblies.html` | Keeps test assemblies in a standalone Play Mode Player build |
| Code Coverage package overview | `Manual/com.unity.testtools.codecoverage.html` | `com.unity.testtools.codecoverage` v1.3.0 — export coverage data/reports, Coverage Recording |
| Coverage API | `ScriptReference/TestTools.Coverage.html`, `TestTools.Coverage-enabled.html` | `UnityEngine.TestTools.Coverage.enabled`, per-method/assembly coverage stats API |
| Exclude from coverage | `ScriptReference/TestTools.ExcludeFromCoverageAttribute.html` | `[ExcludeFromCoverage]` on assembly/class/constructor/method/struct |
| Coverage build flag | `ScriptReference/BuildOptions.EnableCodeCoverage.html` | Enables code coverage in a standalone Player build |

Manual coverage of `test-framework/` is exhaustive — 25 dedicated pages at the top level plus a `course/` subfolder with a full applied tutorial (general-introduction exercises and a "Testing Lost Crypt" sample-game project under `course/LostCrypt/`). Re-confirmed on this pass: the local `ScriptReference/` mirror still has **no** pages for `UnityEngine.TestTools.UnityTestAttribute`, `UnitySetUpAttribute`, `UnityTearDownAttribute`, or any `NUnit.Framework` attribute (`[Test]`, `[TestCase]`, `[SetUp]`, etc.) — a targeted search for `testtools`/`nunit`/`unitytest`/`testcase` under `ScriptReference/` only turns up unrelated hits (`Random.onUnitCircle`, physics `SweepTest*`, `RayTracingInstanceCullingTest*`, iOS Xcode test-target APIs) plus the **Code Coverage** API (`TestTools.Coverage*`, `TestTools.ExcludeFromCoverageAttribute`), which is a real but separate package. Exact attribute signatures and NUnit semantics below are grounded in the Manual pages where they're described in prose/examples, and otherwise rely on accurate pretrained NUnit/UTF knowledge — called out inline where that's the case.

## Key Guidelines

### Edit Mode vs Play Mode Tests

Unity classifies every test as Edit Mode or Play Mode based on which assembly it lives in, not on the attribute used. Edit Mode tests (Editor tests) run entirely inside the Editor's `EditorApplication.update` callback loop, can reference both `UnityEditor` and `UnityEngine`, and can even drive the Editor into and out of Play Mode from within a test — but they cannot run coroutines the normal way, so anything needing to skip a frame must still be a `[UnityTest]`. Their asmdef must target only the `Editor` platform. Play Mode tests run the full engine/frame loop (MonoBehaviours, physics, rendering, coroutines) either inside the Editor or in a built standalone Player; their asmdef must not restrict to Editor-only and must reference `nunit.framework.dll` plus any assembly containing the code under test. Per the docs' own recommendation: default to the plain NUnit `[Test]` attribute and only reach for `[UnityTest]` when you need to yield an Editor instruction (Edit Mode) or skip a frame / wait on time (Play Mode).

```csharp
// Edit Mode test — pure logic, no scene, no frame required.
using NUnit.Framework;

public class InventoryMathTests
{
    [Test]
    public void AddItem_IncreasesStackCount()
    {
        var inventory = new Inventory(capacity: 10);
        inventory.Add("potion", 3);

        Assert.That(inventory.GetCount("potion"), Is.EqualTo(3));
    }
}
```

### Test Assembly Setup (asmdef)

The Test Runner only discovers tests inside a *test assembly* — any asmdef that references `nunit.framework.dll`. Creating one via the Test Runner window ("Create a new Test Assembly Folder") or the `Assets > Create > Testing > Test Assembly Folder` menu produces a `Tests` folder with a `Tests.asmdef` that already references `nunit.framework.dll`, `UnityEngine.TestRunner`, and (for Edit Mode) `UnityEditor.TestRunner` — that reference combination is what marks the assembly as a test assembly, and `UnityEditor.TestRunner` is only available when the assembly targets the Editor platform. The test assembly cannot reference the implicit `Assembly-CSharp.dll`; code you want to test must live in its own custom assembly that the test assembly explicitly references — an unresolved-type error at Test Runner load time almost always means this reference is missing. The Inspector's Platforms checkboxes control which standalone targets a Play Mode test assembly can build/run on; by default new assemblies target Editor only.

```json
// Tests.asmdef — Play Mode test assembly referencing game code
{
    "name": "MyGame.Tests.PlayMode",
    "references": [
        "MyGame.Runtime",
        "UnityEngine.TestRunner",
        "UnityEditor.TestRunner"
    ],
    "optionalUnityReferences": [
        "TestAssemblies"
    ],
    "includePlatforms": [],
    "precompiledReferences": [
        "nunit.framework.dll"
    ],
    "autoReferenced": false,
    "defineConstraints": [
        "UNITY_INCLUDE_TESTS"
    ]
}
```

### Writing Tests (NUnit Attributes)

Unity Test Framework wraps a custom build of NUnit ~3.5. Standard NUnit attributes work as documented upstream: `[TestFixture]` marks a test class (often optional — NUnit will discover `[Test]` methods in any class), `[Test]` marks a synchronous test method, `[SetUp]`/`[TearDown]` run before/after every test in the fixture, `[OneTimeSetUp]`/`[OneTimeTearDown]` run once for the whole fixture, `[Ignore("reason")]` skips a test, `[Category("name")]` tags a test for `-testCategory` filtering, and `[Explicit]` excludes a test from normal runs unless explicitly selected. Assertions use NUnit's constraint model — `Assert.That(actual, constraint)` — which UTF extends with Unity-specific constraints under `UnityEngine.TestTools.Constraints.Is` (disambiguate from `NUnit.Framework.Is` with a fully-qualified reference or explicit `using`) and custom comparers for approximate/Unity-type equality (e.g. float/vector tolerance comparisons). `LogAssert.Expect(LogType, message)` must be called *before* the code that logs, because the expected-log check runs at the end of the frame — a test fails if Unity logs any message above Log/Warning severity that wasn't expected, and also fails if an expected message never appears.

```csharp
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class DamageSystemTests
{
    [Test]
    [Category("Combat")]
    public void ApplyDamage_BelowZeroHealth_ClampsToZero()
    {
        var health = new Health(startingHp: 5);

        health.ApplyDamage(20);

        Assert.That(health.Current, Is.EqualTo(0));
    }

    [Test]
    public void ApplyDamage_LogsWarningOnOverkill()
    {
        var health = new Health(startingHp: 5);

        // Must register the expectation before the call that logs it —
        // the check resolves at end-of-frame.
        LogAssert.Expect(LogType.Warning, "Overkill damage applied");

        health.ApplyDamage(999);
    }
}
```

### Async/UnityTest Coroutine Tests

`[UnityTest]` is the frame-spanning counterpart to `[Test]`: the method returns `IEnumerator`, and Unity runs it as a coroutine in Play Mode or via the `EditorApplication.update` loop in Edit Mode. Beyond ordinary `yield return null` (skip one frame) and `WaitForSeconds`, UTF predefines Editor-only yield instructions — `EnterPlayMode`, `ExitPlayMode`, `RecompileScripts`, `WaitForDomainReload` — for Edit Mode tests that need to drive Editor state transitions. To test a `MonoBehaviour` that needs its own `Update()` loop, `yield return new MonoBehaviourTest<T>()` where `T` implements `IMonoBehaviourTest` (with an `IsTestFinished` property) instantiates the component and waits until it reports completion. Separately, UTF also supports the .NET `async`/`await` model on a plain `[Test]` (not `[UnityTest]`): the method is `async` and returns `Task`, and Unity awaits it by polling task completion each `EditorApplication.update`/frame — this does **not** give you frame-yielding or domain-reload semantics, and any failing log message inside an async test is only evaluated once the whole test completes.

```csharp
// UnityTest — frame-spanning coroutine test
using System.Collections;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class MyMonoBehaviourTest : MonoBehaviour, IMonoBehaviourTest
{
    int frameCount;
    public bool IsTestFinished => frameCount > 10;
    void Update() => frameCount++;
}

public class RespawnTests
{
    [UnityTest]
    public IEnumerator Player_RespawnsAfterThreeSeconds()
    {
        var player = new GameObject().AddComponent<PlayerController>();
        player.Kill();

        yield return new WaitForSeconds(3f);

        Assert.That(player.IsAlive, Is.True);
    }

    [UnityTest]
    public IEnumerator MonoBehaviourTest_RunsToCompletion()
    {
        yield return new MonoBehaviourTest<MyMonoBehaviourTest>();
    }
}
```

### Parameterized Tests

Regular NUnit `[Test]` methods support both `[TestCase(args)]` (inline values) and `[ValueSource(nameof(field))]`. **Unity tests (`[UnityTest]`) only support `[ValueSource]`** — `[TestCase]` is not usable on a `[UnityTest]` method, per the Manual. Use `ParameterizedIgnoreAttribute` to skip specific parameter combinations rather than the whole test.

```csharp
using System.Collections;
using NUnit.Framework;
using UnityEngine.TestTools;

public class SpeedCalculationTests
{
    // [TestCase] works only on plain [Test] methods.
    [TestCase(0f, 0f)]
    [TestCase(10f, 5f)]
    [TestCase(-10f, 5f)]
    public void ClampSpeed_StaysWithinLimit(float input, float maxSpeed)
    {
        Assert.That(SpeedUtil.Clamp(input, maxSpeed), Is.InRange(-maxSpeed, maxSpeed));
    }

    static int[] frameBudgets = { 1, 5, 6 };

    // [UnityTest] must use [ValueSource] instead of [TestCase].
    [UnityTest]
    public IEnumerator LoadAsset_CompletesWithinFrameBudget([ValueSource(nameof(frameBudgets))] int budget)
    {
        var op = AssetLoader.BeginLoad("Enemy");
        for (int i = 0; i < budget && !op.IsDone; i++)
            yield return null;

        Assert.That(op.IsDone, Is.True);
    }
}
```

### Setup/Teardown and Execution Order

Alongside plain NUnit `[SetUp]`/`[TearDown]`/`[OneTimeSetUp]`/`[OneTimeTearDown]`, UTF adds `[UnitySetUp]`/`[UnityTearDown]` and `[UnityOneTimeSetUp]`/`[UnityOneTimeTearDown]`, whose methods must return `IEnumerator` so they can yield Editor instructions across frames. Base-class setup runs before derived-class setup; teardown runs derived-first, base-last — matching ordinary NUnit inheritance rules. The full per-test action order (documented explicitly) is: `IApplyToContext` attributes → `[UnityOneTimeSetUp]` → `[OneTimeSetUp]` → `IOuterUnityTestAction.BeforeTest` → `[UnitySetUp]` → `IWrapSetUpTearDown` → `[SetUp]` → Action-attribute `BeforeTest` → `IWrapTestMethod` → **the test itself** → Action-attribute `AfterTest` → `[TearDown]` → `[UnityTearDown]` → `IOuterUnityTestAction.AfterTest` → `[OneTimeTearDown]` → `[UnityOneTimeTearDown]` — identical for `[Test]` and `[UnityTest]`. If a `[UnityTest]` yields an instruction that triggers a domain reload (e.g. `RecompileScripts`), the non-Unity `[SetUp]` methods re-run after reload before the test continues, but `[UnitySetUp]`/Unity outer actions don't re-run — so don't assume fields set before the reload survive untouched.

```csharp
using System.Collections;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class SceneFixtureTests
{
    GameObject rig;

    [UnitySetUp]
    public IEnumerator UnitySetUp()
    {
        rig = new GameObject("Rig");
        yield return null; // let Awake/Start run this frame
    }

    [UnityTearDown]
    public IEnumerator UnityTearDown()
    {
        Object.Destroy(rig);
        yield return null;
    }

    [UnityTest]
    public IEnumerator Rig_IsActiveAfterSetup()
    {
        Assert.That(rig.activeInHierarchy, Is.True);
        yield return null;
    }
}
```

### Running Tests via Command Line/CI

`Unity.exe -runTests -batchmode -projectPath <path> -testResults <path\results.xml> -testPlatform <EditMode|PlayMode|BuildTarget>` is the baseline CI invocation; `-batchmode` removes the need for manual input. `-testPlatform` defaults to `EditMode` if omitted, accepts `EditMode`, `PlayMode` (in-Editor), or any `BuildTarget` enum value to build-and-run on a standalone Player. Results are written as NUnit-format XML. Useful additional flags: `-assemblyNames "A;B"` restricts which test assemblies run; `-testFilter`/`-testCategory` (semicolon-separated, support `!` negation, combine with AND when both given) narrow by test name/regex or category; `-runSynchronously` forces all tests into a single Editor update call (Edit Mode only — filters out `[UnityTest]`s and anything using `[UnitySetUp]`/`[UnityTearDown]`); `-repeat`/`-retry` control re-running successful/failing tests; `-randomOrderSeed` randomizes execution order deterministically; `-orderedTestListFile` points at a text file pinning exact run order (including parameterized-test argument syntax); `-testSettingsFile` points at a `TestSettings.json` for scripting backend/architecture/API-profile overrides; `-playerHeartbeatTimeout` (default 10 minutes) bounds how long the Editor waits for a Player test run. The regular `-quit` argument is not supported while tests are running. The docs are explicit that there's no common cross-platform exit-code contract for test failures — parse the `-testResults` XML rather than trusting the process exit code in CI.

```bash
Unity -runTests -batchmode -projectPath "$PWD" \
  -testPlatform PlayMode \
  -testResults "$PWD/artifacts/playmode-results.xml" \
  -testCategory "!Flaky" \
  -logFile "$PWD/artifacts/unity.log"
echo "Exit code: $? (informational only — parse the XML for pass/fail)"
```

### Running Tests from Code (TestRunnerApi)

`UnityEditor.TestTools.TestRunner.Api.TestRunnerApi` (a `ScriptableObject`) lets editor tooling or custom CI glue drive test runs programmatically: `Execute(new ExecutionSettings(filter))` starts a run, where `Filter` supports `testMode`, `testNames` (OR within a field), `assemblyNames`, `groupNames`, and `categoryNames` (AND across fields, OR within a field's array); `RegisterCallbacks(ICallbacks)` subscribes to run-start/run-finish/test-start/test-finish events (delivered for *all* runs, not just ones started by that `TestRunnerApi` instance); `RetrieveTestList(TestMode, callback)` returns the test tree as an `ITestAdaptor` without running anything, useful for building custom dashboards or counting cases.

```csharp
using UnityEditor.TestTools.TestRunner.Api;
using UnityEngine;

public static class CiTestKickoff
{
    public static void RunAllPlayMode()
    {
        var api = ScriptableObject.CreateInstance<TestRunnerApi>();
        api.Execute(new ExecutionSettings(new Filter
        {
            testMode = TestMode.PlayMode,
            categoryNames = new[] { "Smoke" }
        }));
    }
}
```

### Decoupling Logic for Testability

Keep gameplay/business logic in plain C# classes that don't derive from `MonoBehaviour` — they can be `new`'d up and asserted on directly inside a fast Edit Mode test, with no scene, no `Instantiate`, and no frame wait. Let `MonoBehaviour`s be thin adapters that own Unity lifecycle hooks (`Update`, `OnCollisionEnter`, serialized fields) and delegate the actual computation to the plain class. Where a `MonoBehaviour` truly must be tested end-to-end (its own `Update` loop, physics callbacks), reach for `MonoBehaviourTest<T>`/`IMonoBehaviourTest` inside a `[UnityTest]` rather than trying to force it into a synchronous `[Test]`.

```csharp
// Testable: pure class, asserted on directly, no scene needed.
public class Health
{
    public int Current { get; private set; }
    public Health(int startingHp) => Current = startingHp;
    public void ApplyDamage(int amount) => Current = Mathf.Max(0, Current - amount);
}

// Thin adapter: MonoBehaviour delegates to the plain class.
public class HealthComponent : MonoBehaviour
{
    Health health;
    void Awake() => health = new Health(startingHp: 100);
    public void TakeDamage(int amount) => health.ApplyDamage(amount);
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Logic lives directly in `MonoBehaviour` methods | Hard to instantiate outside a scene; extract to a plain class the `MonoBehaviour` delegates to |
| `[Test]` used where a frame must pass | Coroutines/physics/async-frame-timing need a yielded frame — use `[UnityTest]` + `IEnumerator` |
| `[TestCase]` applied to a `[UnityTest]` | Unity tests only support `[ValueSource]` for parameterization, not `[TestCase]` — silently ignored or compile-incompatible |
| Test Runner shows no tests | asmdef missing, or missing a reference to `nunit.framework.dll`/`UnityEngine.TestRunner`/`UnityEditor.TestRunner` |
| Test assembly doesn't reference code under test | Unresolved types; add an explicit assembly reference — a test assembly can't reference the implicit `Assembly-CSharp.dll` at all |
| Assuming state persists across Play Mode tests | Scenes/domain can reload between or during tests; use `[SetUp]`/`[UnitySetUp]` instead of relying on statics |
| Forgetting `IncludeTestAssemblies` for standalone runs | Build silently strips test code; Player reports zero tests |
| Calling `LogAssert.Expect` after the logging code runs | The expected-log check resolves at end-of-frame, but call order still matters — register the expectation before invoking the code under test |
| Treating `async Task` tests like `[UnityTest]` | Task-based `[Test]` methods only await Task completion each update; they don't yield frames, Editor instructions, or survive a domain reload the way `[UnityTest]` does |
| Assuming fields survive a domain reload inside a `[UnityTest]` | `RecompileScripts`/similar yields can trigger a domain reload; non-Unity `[SetUp]` re-runs afterward but `[UnitySetUp]`/outer actions don't, so state set before the reload can be lost |
| Building an Edit Mode assembly that also targets a non-Editor platform | `UnityEditor.TestRunner` (required to mark an assembly as Edit Mode-capable) is only available when the platform target is Editor-only |
| Accessing `UnityEditor` APIs from `IPrebuildSetup`/`IPostBuildCleanup` in a Play Mode assembly without guarding | These interfaces run at build time but the containing assembly may be a runtime (non-Editor) assembly — wrap Editor-only calls in `#if UNITY_EDITOR` |
| Trusting the Test Runner window's reported suite duration | Documented known limitation: total duration doesn't include time spent in `OneTimeSetUp`/`UnityOneTimeSetUp`/`OneTimeTearDown`/`UnityOneTimeTearDown`, only the sum of individual test durations |
| Expecting Player test results on-device without a shared network | The Player reports results back to the Editor over the network; if Unity can't establish the connection, tests visibly pass/fail in the running app but no XML results come back |
| Relying on the CI process exit code alone to detect test failure | Unity has no unified cross-platform exit-code contract for test runs — always parse the `-testResults` XML file instead |

## Quick Reference

| Attribute / Concept | Category | Purpose |
|---|---|---|
| `[Test]` | NUnit | Synchronous test method |
| `[UnityTest]` returning `IEnumerator` | Unity | Frame-spanning coroutine test (Play Mode: real coroutine; Edit Mode: driven via `EditorApplication.update`) |
| `[TestFixture]` | NUnit | Marks a test class (often optional) |
| `[TestCase(args)]` | NUnit | Inline parameterization — plain `[Test]` only, not `[UnityTest]` |
| `[ValueSource(nameof(x))]` | NUnit | Parameterize from a static field/property/method — works on both `[Test]` and `[UnityTest]` |
| `ParameterizedIgnoreAttribute` | Unity | Ignore a test for specific parameter values only |
| `[SetUp]` / `[TearDown]` | NUnit | Per-test setup/cleanup |
| `[OneTimeSetUp]` / `[OneTimeTearDown]` | NUnit | Per-fixture setup/cleanup, runs once |
| `[UnitySetUp]` / `[UnityTearDown]` (return `IEnumerator`) | Unity | Setup/teardown that can yield Editor instructions |
| `[UnityOneTimeSetUp]` / `[UnityOneTimeTearDown]` (return `IEnumerator`) | Unity | Fixture-wide setup/teardown that can yield |
| `[Ignore("reason")]` | NUnit | Skip a test |
| `[Explicit]` | NUnit | Excluded from normal runs unless explicitly selected |
| `[Category("name")]` | NUnit | Tag for `-testCategory` / `Filter.categoryNames` filtering |
| `[PrebuildSetup]` / `[PostBuildCleanup]` + `IPrebuildSetup`/`IPostBuildCleanup` | Unity | Work before building tests / cleanup after the run, for standalone Player test builds |
| `IPrebuildSetupWithTestData` / `IPostBuildCleanupWithTestData` | Unity | Build-time hooks with access to test metadata |
| `IOuterUnityTestAction` (`BeforeTest`/`AfterTest`, return `IEnumerator`) | Unity | Wraps a test with code that can yield, outside setup/teardown |
| `[TestPlayerBuildModifier(typeof(X))]` (assembly-level) + `ITestPlayerBuildModifier` | Unity | Customize `BuildPlayerOptions` for the standalone test Player |
| `LogAssert.Expect(LogType, message)` | Unity | Declare an expected log/warning/error before triggering it |
| `UnityEngine.TestTools.Constraints.Is` | Unity | Unity-specific `Assert.That` constraints (disambiguate from `NUnit.Framework.Is`) |
| `EnterPlayMode` / `ExitPlayMode` / `RecompileScripts` / `WaitForDomainReload` | Unity | Predefined yield instructions for Edit Mode `[UnityTest]`s |
| `IEditModeTestYieldInstruction` | Unity | Interface for defining custom Editor yield instructions |
| `MonoBehaviourTest<T>` + `IMonoBehaviourTest` (`IsTestFinished`) | Unity | Instantiate and run a `MonoBehaviour` to completion inside a `[UnityTest]` |
| `TestRunnerApi` / `ExecutionSettings` / `Filter` | Unity | Run tests from code (`Execute`) |
| `ICallbacks` + `RegisterCallbacks` | Unity | Receive run/test start-finish callbacks |
| `RetrieveTestList(TestMode, callback)` / `ITestAdaptor` | Unity | Fetch the test tree without running it |
| Edit Mode vs Play Mode | Concept | No engine loop, Editor-only, fast / full engine loop, in-Editor or on-device |
| asmdef test assembly | Concept | Must reference `nunit.framework.dll` (+`UnityEngine.TestRunner`, +`UnityEditor.TestRunner` for Edit Mode) — required for Test Runner discovery |
| `-runTests -batchmode -testPlatform -testResults` | CLI | Baseline headless CI invocation |
| `-assemblyNames` / `-testFilter` / `-testCategory` | CLI | Narrow which tests run (support `;` lists and `!` negation) |
| `-runSynchronously` / `-repeat` / `-retry` / `-randomOrderSeed` / `-orderedTestListFile` | CLI | Execution-order and repetition controls |
| `-testSettingsFile` (`TestSettings.json`) | CLI | Per-run scripting backend / architecture / API-profile overrides |
| `BuildOptions.IncludeTestAssemblies` | ScriptReference | Keeps tests in standalone Play Mode Players |
| `BuildOptions.EnableCodeCoverage` | ScriptReference | Enables code coverage in a standalone Player build |
| `Coverage.enabled`, `Coverage.GetStatsFor*` | ScriptReference (`UnityEngine.TestTools`) | Code Coverage runtime API |
| `[ExcludeFromCoverage]` | ScriptReference (`UnityEngine.TestTools`) | Exclude assembly/class/constructor/method/struct from coverage |

## Advanced Notes

**CI integration.** The canonical headless invocation is `Unity -runTests -batchmode -projectPath <path> -testPlatform <EditMode|PlayMode|BuildTarget> -testResults <path>.xml`, optionally narrowed with `-assemblyNames`/`-testFilter`/`-testCategory` and made deterministic with `-randomOrderSeed`/`-orderedTestListFile`. Because Unity does not define a common cross-platform exit-code contract for test failures (stated directly in `reference-command-line.html`), a CI pipeline should treat the process exit code as informational only and instead parse the NUnit-format XML written to `-testResults` to determine pass/fail — most CI wrappers (community GitHub Actions, GitLab templates, Jenkins plugins) work this way under the hood. For matrix builds across scripting backends or API compatibility levels, drive per-run configuration through a `-testSettingsFile` pointing at a `TestSettings.json` (`scriptingBackend`, `apiProfile`, `architecture`, `appleEnableAutomaticSigning`, etc.) rather than mutating Player Settings between jobs. For on-device Player runs in CI, remember the Editor and the target device must be able to reach each other over the network for results to stream back — an isolated CI runner or a device on a separate VLAN will pass/fail visibly on-device but produce no XML report, which silently breaks automated gating unless the pipeline explicitly checks for the results file's existence. `-playerHeartbeatTimeout` (default 10 minutes) should be raised for slow device boot times rather than left to fail a healthy but slow run.

**Code coverage.** The Code Coverage package (`com.unity.testtools.codecoverage`, v1.3.0 in this Unity 6.3 doc set) exports coverage data/reports from automated test runs and separately offers a Coverage Recording feature to capture coverage from manual play-testing when there's no automated test for a code path. Coverage is enabled either via the `UnityEngine.TestTools.Coverage.enabled` API at runtime or the `-enableCodeCoverage` command-line argument in batchmode (per `TestTools.Coverage.html`); for standalone Player builds specifically, `BuildOptions.EnableCodeCoverage` must be set so instrumentation is compiled in. The package's keyword list includes `opencover`, indicating OpenCover-compatible report output suitable for ingestion by common coverage-visualization tooling in CI. Use `[ExcludeFromCoverage]` on generated code, editor-only scaffolding, or intentionally untested boundary layers (e.g. thin `MonoBehaviour` adapters that only forward to already-covered plain classes) to keep coverage percentages meaningful rather than penalizing code that's deliberately untestable in isolation. Because coverage instrumentation adds overhead, keep dedicated "coverage" CI jobs separate from fast smoke-test jobs that gate every commit — run coverage on a slower cadence (nightly / pre-merge) rather than on every push.

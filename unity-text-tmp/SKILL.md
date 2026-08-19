---
name: unity-text-tmp
description: Use when displaying or styling text in Unity — TextMeshPro (TMP_Text/TMP_FontAsset), rich text tags, or TextCore font rendering. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Text (TextMeshPro / TextCore)

## Retrieval Sources

**Confirmed gap, verified with multiple `find` globs on this pass:** the local `ScriptReference/` has zero pages under the `TMPro` namespace — no `TMP_Text`, `TMP_FontAsset`, `TextMeshProUGUI`, `TextMeshPro` (the 3D component), `TMP_InputField`, `TMP_Dropdown`, `TMP_StyleSheet`, etc. anywhere (checked `TMPro*.html` and a plain `TextMeshPro*.html` glob — zero hits for both). This mirrors the `unity-ui` skill's finding that `UnityEngine.UI` (uGUI) has no local API pages either: this documentation snapshot only ships the built-in-module ScriptReference, not the `com.unity.textmeshpro`/`com.unity.ugui` package API reference, which Unity hosts on a separate package-docs site. For `TMP_Text`/`TMP_FontAsset`/`TextMeshProUGUI` member-level API, state plainly that local docs don't cover them and fall back to general knowledge, flagged as unverified against local sources. What **is** fully covered locally, and verified against real files, is (1) `UnityEngine.TextCore.*` — the engine-level namespace TextMeshPro itself is built on, which Unity folded into the engine starting in 2021 — 133 ScriptReference pages; (2) UI Toolkit's own text system, which the docs state outright renders through TextCore and is explicitly "based on TextMesh Pro" (`Manual/UIE-get-started-with-text.html`); and (3) the legacy pre-TMP text APIs (`Font`, `TextMesh`, `TextGenerator`) that TMP superseded and that some projects/asset-store code still reference. Every path below was verified with `find`/`grep` against `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` on this pass.

| Source | Path | Use for |
|--------|------|---------|
| TMP/TextCore relationship (hub pages) | `Manual/UIE-get-started-with-text.html`, `Manual/UIE-work-with-text.html` | The explicit statement that "UI Toolkit uses TextCore to render text, a technology based on TextMesh Pro," plus the full topic map for every text sub-page under UI Toolkit |
| TextCore glyph/face metrics | `ScriptReference/TextCore.FaceInfo.html` + its ~19 member pages (`-ascentLine`, `-baseline`, `-capLine`, `-descentLine`, `-familyName`, `-lineHeight`, `-pointSize`, `-scale`, `-styleName`, `-tabWidth`, sub/superscript offset+size, strikethrough/underline offset+thickness), `ScriptReference/TextCore.Glyph.html` + members (`-index`, `-metrics`, `-glyphRect`, `-scale`, `-atlasIndex`, `-classDefinitionType`, `.Compare`, `-ctor`), `ScriptReference/TextCore.GlyphMetrics.html` + members (`-width`, `-height`, `-horizontalAdvance`, `-horizontalBearingX`, `-horizontalBearingY`, `-ctor`), `ScriptReference/TextCore.GlyphRect.html` + members (`-x`, `-y`, `-width`, `-height`, `-zero`, `-ctor`) | The per-glyph/per-face data model shared by every text renderer built on TextCore: what a font asset's atlas actually stores per character (bearing, advance, atlas rect) |
| TextCore.Text namespace | `ScriptReference/TextCore.Text.Character.html`, `ScriptReference/TextCore.Text.AtlasPopulationMode.html` + values (`.Static`, `.Dynamic`, `.DynamicOS`), `ScriptReference/TextCore.Text.FontFeatureTable.html` + its `SortGlyphPairAdjustmentRecords`/`SortMarkToBaseAdjustmentRecords`/`SortMarkToMarkAdjustmentRecords` methods | The `Character` class backing runtime glyph lookup, the `AtlasPopulationMode` enum that drives Static/Dynamic/Dynamic OS font asset behavior, and OpenType feature tables (kerning, mark positioning) |
| TextCore.LowLevel — FontEngine | `ScriptReference/TextCore.LowLevel.FontEngine.html` + methods (`.InitializeFontEngine`, `.DestroyFontEngine`, `.LoadFontFace`, `.UnloadFontFace`, `.UnloadAllFontFaces`, `.GetFaceInfo`, `.GetFontFaces`, `.GetSystemFontNames`, `.SetFaceSize`, `.TryGetGlyphIndex`, `.TryGetGlyphWithIndexValue`, `.TryGetGlyphWithUnicodeValue`) | The low-level FreeType-backed API the Font Asset Creator calls to rasterize glyphs into an atlas — this is what actually runs when you click "Generate Font Atlas" |
| TextCore.LowLevel — engine error/config enums | `ScriptReference/TextCore.LowLevel.FontEngineError.html` + all its named error values (`.Success`, `.Invalid_Face`, `.Invalid_File_Format`, `.Invalid_Pixel_Size`, `.Atlas_Generation_Cancelled`, etc.), `ScriptReference/TextCore.LowLevel.GlyphLoadFlags.html` + flags (`.LOAD_DEFAULT`, `.LOAD_RENDER`, `.LOAD_NO_HINTING`, `.LOAD_NO_BITMAP`, `.LOAD_COLOR`, `.LOAD_COMPUTE_METRICS`, etc.), `ScriptReference/TextCore.LowLevel.GlyphPackingMode.html` + modes (`.BestAreaFit`, `.BestShortSideFit`, `.BestLongSideFit`, `.ContactPointRule`, `.BottomLeftRule`) | Diagnosing font-atlas-generation failures from script, and the bin-packing strategy options behind the Font Asset Creator's "Packing Method" dropdown |
| TextCore.LowLevel — glyph render mode & OpenType adjustment | `ScriptReference/TextCore.LowLevel.GlyphRenderMode.html` + every value (`.SMOOTH`, `.SMOOTH_HINTED`, `.RASTER`, `.RASTER_HINTED`, `.COLOR`, `.COLOR_HINTED`, `.SDF`, `.SDF8`, `.SDF16`, `.SDF32`, `.SDFAA`, `.SDFAA_HINTED`, `.DEFAULT`), `ScriptReference/TextCore.LowLevel.GlyphAdjustmentRecord.html` + members, `ScriptReference/TextCore.LowLevel.GlyphPairAdjustmentRecord.html` + members (`-firstAdjustmentRecord`, `-secondAdjustmentRecord`), `ScriptReference/TextCore.LowLevel.GlyphValueRecord.html` + members (`-xAdvance`, `-yAdvance`, `-xPlacement`, `-yPlacement`) | Every bitmap vs. SDF atlas render mode a Font Asset Creator can target, and the kerning-pair adjustment records that back OpenType kerning |
| Font Assets — concepts & properties | `Manual/UIE-font-asset-landing.html`, `Manual/UIE-font-asset.html`, `Manual/UIE-font-asset-properties.html`, `Manual/UIE-font-creator-properties.html` | Static/Dynamic/Dynamic OS atlas population modes, bitmap vs. SDF atlas render modes, and the full Face Info property table (Point Size, Scale, Line Height, Ascent/Descent Line) plus every Font Asset Creator field (Sampling Point Size, Padding, Packing Method, Atlas Resolution) |
| Font subsetting & static-asset migration | `Manual/UIE-font-subsetting.html`, `Manual/ui-systems/migrate-static-font-assets.html` | Reducing shipped font file size with the external `pyftsubset`/FontTools CLI, and the required migration path off Static font assets once Advanced Text Generator is enabled (ATG doesn't support them) |
| Font/graphic asset prep (best-practice guide) | `Manual/best-practice-guides/ui-toolkit-for-advanced-unity-developers/graphic-and-font-assets-preparation.html` | Broader DCC-pipeline guidance on preparing font files (and adjacent graphic assets) before importing them into a Unity project |
| Fallback fonts | `Manual/UIE-fallback-font.html` | The fallback-list search mechanism when a glyph is missing from the primary font asset, local vs. global fallback chains, and why Dynamic OS assets make good fallback candidates |
| Color emojis | `Manual/UIE-color-emojis.html` | Creating a color-glyph font asset (`Create > Text Core > Font Asset > Color`) and wiring it into a Text Settings asset's Fallback Emoji Text Assets list |
| Rich text tags — concepts & full tag table | `Manual/UIE-rich-text-tags.html`, `Manual/ui-systems/introduction-to-rich-text-tags.html`, `Manual/UIE-supported-tags.html` | Tag syntax (`<tag=value>`, `<tag attribute="value">`), scope/nesting/closing-tag rules, precedence when the same tag repeats, and the full supported-tag reference table (`<a>`, `<align>`, `<allcaps>`, `<alpha>`, `<b>`, `<br>`, `<color>`, `<cspace>`, `<font>`, `<font-weight>`, `<gradient>`, `<i>`, `<indent>`, and more) |
| Hyperlinks in rich text | `Manual/ui-systems/add-hyperlinks-in-text.html` | `<a href="...">` vs. `<link=ID>` tags, and the (experimental) `PointerDownLinkTagEvent`/`PointerUpLinkTagEvent`/`PointerMoveLinkTagEvent` C# events for custom link behavior |
| Color gradients on text | `Manual/UIE-color-gradient.html` | The `<gradient>` tag, creating a Color Gradient preset asset (`Assets > Create > Text Core > Color Gradient`), and the Single/Horizontal/Vertical/Four Corners gradient modes |
| Sprites inside text | `Manual/UIE-sprite-text.html`, `Manual/UIE-sprite.html`, `Manual/UIE-sprite-asset-properties.html` | Building a sprite asset from an atlas texture and referencing entries with `<sprite index=N>`/`<sprite name="...">`, plus the draw-call cost of using more than one sprite atlas per text element |
| Text effects (shadow/outline) | `Manual/UIE-text-effects.html` | `text-shadow` USS shorthand (offset-x/offset-y/blur-radius/color), and why text effects only work with SDF-rendered font assets (padding must be increased to grow the effect radius) |
| Styling text with USS | `Manual/UIB-styling-ui-text.html` | Text-specific USS properties (`font-size`, `-unity-font-style`, `-unity-text-align`, `color`) applied inline in UXML, in a `.uss` file, or via UI Builder, and the fact that (unlike most USS properties) text properties cascade to children |
| Auto-sizing / Best Fit | `Manual/ui-systems/auto-sizing-text-elements.html`, `ScriptReference/UIElements.TextAutoSize.html` + members (`-mode`, `-minSize`, `-maxSize`, `-ctor`, `.None`), `ScriptReference/UIElements.TextAutoSizeMode.html` + values (`.None`, `.BestFit`) | Min/max font-size range with word-wrapping, ellipsis truncation, and alignment support, settable from UI Builder, USS (`-unity-text-auto-size`), or C# |
| Text overflow | `ScriptReference/UIElements.TextOverflow.html` + values (`.Clip`, `.Ellipsis`), `ScriptReference/UIElements.TextOverflowPosition.html` + values (`.Start`, `.Middle`, `.End`) | Clip-vs-ellipsis behavior when text exceeds its element bounds, and where the ellipsis lands in the truncated string |
| Advanced Text Generator (ATG) | `Manual/UIE-advanced-text-generator.html`, `Manual/ui-systems/enable-and-use-atg.html` | The Harfbuzz/ICU/FreeType-backed text-shaping module (Project Settings > UI Toolkit > Enable Advanced Text Generator), and its per-element override via Text Generator Type |
| ATG — bidirectional text & language direction | `Manual/ui-systems/cursor-movement.html`, `Manual/ui-systems/language-direction.html` | Logical vs. Visual cursor-movement models for BIDI (Arabic/Hebrew) input, and the `language-direction`/`LanguageDirection` attribute (Inherited/LTR/RTL) that cascades to children |
| UITK Text Settings asset | `Manual/UIE-text-setting-asset.html` | The project-wide `Assets > Create > UI Toolkit > Text Settings` asset that supplies default font/sprite/style-sheet/gradient-preset paths per Panel |
| UI Toolkit text component API — TextElement | `ScriptReference/UIElements.TextElement.html` + members (`-text`, `-enableRichText`, `-parseEscapeSequences`, `-isElided`, `-displayTooltipWhenElided`, `-emojiFallbackSupport`, `-selection`, `-selectableUssClassName`, `-ussClassName`, `-experimental`, `-ctor`, `.MeasureTextSize`, `.MarkDirtyText`, `.UxmlFactory`, `.UxmlTraits`) | The base class for every text-displaying UI Toolkit control (`Label`, `Button`'s text, `TextField`'s display) — properties for rich text toggling, elision, and measuring rendered text size |
| UI Toolkit — Label / TextField controls | `ScriptReference/UIElements.Label.html` + members (`-ctor`, `-ussClassName`, `.UxmlFactory`, `.UxmlTraits`), `ScriptReference/UIElements.TextField.html` + members (`-ctor`, `-value`, `-multiline`, `-inputUssClassName`, `-labelUssClassName`, `-ussClassName`, `.UxmlFactory`, `.UxmlTraits`) | The concrete display-only (`Label`) and editable (`TextField`) controls built on `TextElement` |
| UI Toolkit — text input base field | `ScriptReference/UIElements.TextInputBaseField_1.html` + its ~35 members (`-text`, `-textEdition`, `-textSelection`, `-cursorIndex`, `-cursorPosition`, `-cursorColor`, `-selectionColor`, `-selectIndex`, `-selectAllOnFocus`, `-selectAllOnMouseUp`, `-doubleClickSelectsWord`, `-tripleClickSelectsLine`, `-maxLength`, `-maskChar`, `-isPasswordField`, `-isReadOnly`, `-isDelayed`, `-autoCorrection`, `-keyboardType`, `-hideMobileInput`, `-hideSoftKeyboard`, `-touchScreenKeyboard`, `.SelectAll`, `.SelectNone`, `.SelectRange`, `.SetValueWithoutNotify`, `.MeasureTextSize`, `.StringToValue`, `.ValueToString`) | The generic editable-text base every input field (`TextField`, numeric fields) inherits — cursor/selection state, password masking, mobile keyboard behavior |
| UI Toolkit — glyph-level vertex access | `ScriptReference/UIElements.TextElement.PostProcessTextVertices.html`, `ScriptReference/UIElements.TextElement.Glyph.html` + `-vertices`, `ScriptReference/UIElements.TextElement.GlyphsEnumerable.html` + `-Count`/`.GetEnumerator`, `ScriptReference/UIElements.TextElement.GlyphsEnumerator.html` + `.Current`/`.MoveNext`/`.Reset` | The `Action<GlyphsEnumerable>` callback fired immediately before UI Toolkit renders each glyph's mesh — the API for per-character animation/tint effects (fade-in, wave, rainbow text) |
| Custom text animation walkthrough | `Manual/ui-systems/create-custom-text-animation.html` | A full worked example (Editor window, glyph fade on Spacebar) driving `PostProcessTextVertices` end to end |
| UI Toolkit — font reference in styles | `ScriptReference/UIElements.FontDefinition.html` + members (`-font`, `-fontAsset`, `.FromFont`, `.FromSDFFont`), `ScriptReference/UIElements.StyleFontDefinition.html` + members (`-value`, `-keyword`, `-ctor`), `ScriptReference/UIElements.IStyle-unityFontDefinition.html`, `ScriptReference/UIElements.IResolvedStyle-unityFontDefinition.html` | How a `VisualElement`'s computed style holds either a legacy bitmap `Font` (`FromFont`) or an SDF `FontAsset` (`FromSDFFont`) under one wrapper type |
| UI Toolkit — text shadow style struct | `ScriptReference/UIElements.TextShadow.html` + members (`-offset`, `-blurRadius`, `-color`) | The C#-side struct backing the `text-shadow` USS property from script (`IStyle.textShadow`) |
| Legacy 3D Text Mesh (superseded by TMP, still shipped) | `Manual/class-TextMesh.html`, `ScriptReference/TextMesh.html` + members (`-text`, `-font`, `-fontSize`, `-fontStyle`, `-characterSize`, `-lineSpacing`, `-offsetZ`, `-anchor`, `-alignment`, `-tabSize`, `-richText`, `-color`) | The old world-space mesh-based text component; Manual explicitly flags it as having "limited functionality" and points to modern UI for anything full-featured — mention only when a project still references it |
| Legacy `Font`/dynamic-font API (predecessor TMP's Font Asset Creator wraps) | `ScriptReference/Font.html` + members (`-ascent`, `-characterInfo`, `-dynamic`, `-fontSize`, `-lineHeight`, `-material`, `-textureRebuilt`, `.CreateDynamicFontFromOSFont`, `.GetCharacterInfo`, `.GetMaxVertsForString`, `.GetOSInstalledFontNames`, `.GetPathsToOSFonts`, `.HasCharacter`, `.RequestCharactersInTexture`), `ScriptReference/FontStyle.html` + values, `ScriptReference/FontRenderingMode.html` + values (`.OSDefault`, `.Smooth`, `.HintedRaster`, `.HintedSmooth`), `ScriptReference/FontTextureCase.html` + values | The `UnityEngine.Font` object every TMP/TextCore font asset is created *from* (Source Font File field) — useful for understanding what a `.ttf`/`.otf`/`.ttc` import actually gives you before atlas generation |
| Legacy uGUI text generation (superseded by TMP) | `ScriptReference/TextGenerator.html` + members (`.Populate`, `.PopulateWithErrors`, `.Invalidate`, `-characters`, `-lines`, `-verts`, `-vertexCount`, `-lineCount`, `-characterCount`, `-characterCountVisible`, `-rectExtents`, `-fontSizeUsedForBestFit`, `.GetVertices`, `.GetVerticesArray`, `.GetLines`, `.GetLinesArray`, `.GetCharacters`, `.GetCharactersArray`, `.GetPreferredWidth`, `.GetPreferredHeight`), `ScriptReference/TextGeneratorType.html` + values (`.Standard`, `.Advanced`) | The legacy uGUI `Text` component's mesh-generation API — historical context for why TMP was built (this generator lacks SDF rendering and rich text depth) |
| Legacy text alignment/anchor enums | `ScriptReference/TextAlignment.html` + values (`.Left`, `.Center`, `.Right`), `ScriptReference/TextAnchor.html` + all 9 values | Shared legacy enums still used by `TextMesh` and other pre-TMP APIs |
| uGUI package landing (context only) | `Manual/com.unity.ugui.html` | Confirms uGUI is a fixed-version core package with no local API reference bundled — `TextMeshProUGUI` (the uGUI-flavored TMP component) is part of this same package and is equally undocumented locally |
| TMP mentions in changelogs | `Manual/WhatsNewUnity6.html`, `Manual/WhatsNew20232.html`, `Manual/WhatsNew20231.html` | Version-by-version TextMeshPro feature additions (basic Emoji support and OpenType kerning added in 2023.2; Color Glyphs and OpenType feature extraction added in 2023.1) — useful for "is feature X available in my Unity version" questions |

## Key Guidelines

### TMP_Text Component Basics — TextMeshPro vs TextMeshProUGUI vs UI Toolkit Text

TextMeshPro ships as three different entry points that all sit on top of the same TextCore rendering engine, and picking the right one depends entirely on which UI system the surrounding screen already uses. `TextMeshPro` (the component, not the package) is the world-space/3D flavor — it renders onto a `MeshRenderer` in the scene like the legacy `TextMesh` it replaces, and is the right choice for floating damage numbers, name tags, or signage. `TextMeshProUGUI` is the uGUI flavor — it derives from `Graphic` the same way `Image`/`RawImage` do, lives on a `RectTransform` inside a `Canvas`, and is what you drop into a uGUI-based HUD or menu in place of the legacy `Text` component. Neither of these two components has local ScriptReference coverage in this doc set (see Retrieval Sources gap note), so exact member names/signatures for `TMP_Text`, `TMP_FontAsset`, `TMP_InputField`, etc. should be treated as general knowledge, not doc-grounded fact. The third entry point is UI Toolkit's own text system — `Label`, `TextField`, and any custom `TextElement` subclass — which is **not** TextMeshPro at all as a component, but is built on the same underlying `TextCore` namespace and, per `Manual/UIE-get-started-with-text.html`, is explicitly described as "a technology based on TextMesh Pro." Practically: if the screen is uGUI, use `TextMeshProUGUI`; if it's UI Toolkit, use `Label`/`TextField`/`TextElement`; if it's world-space 3D, use the `TextMeshPro` component. Don't mix a uGUI `TextMeshProUGUI` and a UI Toolkit `Label` on the same screen expecting shared styling — their font-asset formats, rich-text tag sets, and USS-vs-Inspector styling pipelines are similar in spirit but are two separate implementations layered over TextCore, not one shared component.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

// UI Toolkit: TextElement-derived controls are the closest local-docs-grounded
// equivalent to configuring a TMP_Text instance from script.
public class UitkTextExample : MonoBehaviour
{
    void OnEnable()
    {
        var root = GetComponent<UIDocument>().rootVisualElement;

        var label = new Label("Hello World")
        {
            style =
            {
                fontSize = 14,
                unityFontStyleAndWeight = FontStyle.Bold,
                unityTextAlign = TextAnchor.MiddleCenter,
                color = Color.red,
            }
        };
        label.enableRichText = true; // TextElement property, ScriptReference/UIElements.TextElement-enableRichText.html

        root.Add(label);
    }
}

// uGUI/world-space TextMeshPro members below are NOT locally doc-grounded —
// shown for contrast only, per general TMP knowledge:
// TextMeshProUGUI tmp = GetComponent<TextMeshProUGUI>();
// tmp.text = "Hello World";
// tmp.fontSize = 36;
// tmp.color = Color.red;
```

### Font Assets & Atlas Generation

A font asset is a container wrapping a source font file (`.ttf`/`.otf`/`.ttc`, i.e. a `UnityEngine.Font`) plus a generated atlas texture holding rasterized glyph images and per-glyph metrics (`TextCore.Glyph`/`TextCore.GlyphMetrics`/`TextCore.GlyphRect`). Per `Manual/UIE-font-asset.html`, three **Atlas Population Modes** control when glyphs get baked in: **Static** pre-bakes a fixed character set at creation time (fast, no source font file needed at build time, but any missing character never appears — use for known/finite text like UI labels); **Dynamic** starts with an empty atlas and adds glyphs the first time each character is actually rendered (flexible for unpredictable text like chat/input fields, but the source font file must ship in the build and there's a first-use hitch as new atlas regions get baked); **Dynamic OS** is a dynamic asset that pulls glyphs from a font installed on the runtime operating system instead of a font file bundled in the project — lowest memory footprint, no font file to ship, but only works if the target OS actually has that font installed, which makes it a poor primary choice but a great fallback candidate. Separately, the **Atlas Render Mode** picks bitmap vs. SDF: bitmap modes (`SMOOTH`, `SMOOTH_HINTED`, `RASTER`, `RASTER_HINTED`, `COLOR`, `COLOR_HINTED`) bake fixed-resolution glyph images best suited to pixel art where 1:1 pixel alignment matters; SDF modes (`SDF`, `SDF8`/16/32, `SDFAA`, `SDFAA_HINTED`) bake a signed-distance-field gradient per glyph that stays crisp at any scale/rotation and unlocks shader-driven effects (outline, shadow, glow) without needing separate baked variants — SDF is the default choice for UI text that gets resized, animated, or viewed at varying camera distance. The actual atlas-baking work happens through `TextCore.LowLevel.FontEngine` (`LoadFontFace`, `SetFaceSize`, `TryGetGlyphIndex`, packed via `GlyphPackingMode`), which is the FreeType-backed engine the Font Asset Creator window calls into — you rarely call it directly, but its error enum (`FontEngineError`) is what surfaces as atlas-generation failures in the Inspector.

```csharp
using UnityEngine;
using UnityEngine.TextCore.LowLevel;

// Illustrates the low-level TextCore.LowLevel.FontEngine API that backs the
// Font Asset Creator's "Generate Font Atlas" button — real, locally-documented API.
public static class FontAtlasProbe
{
    public static void PrintGlyphAvailability(Font sourceFont, string testCharacters)
    {
        FontEngineError initResult = FontEngine.InitializeFontEngine();
        if (initResult != FontEngineError.Success)
        {
            Debug.LogError($"FontEngine init failed: {initResult}");
            return;
        }

        FontEngine.LoadFontFace(sourceFont, 90); // 90pt sampling size
        foreach (char c in testCharacters)
        {
            bool found = FontEngine.TryGetGlyphWithUnicodeValue(
                c, GlyphLoadFlags.LOAD_NO_BITMAP, out Glyph glyph);
            Debug.Log(found
                ? $"'{c}' -> glyph index {glyph.index}, advance {glyph.metrics.horizontalAdvance}"
                : $"'{c}' missing from font face");
        }

        FontEngine.DestroyFontEngine();
    }
}
```

### Rich Text Tags

Rich text tags style a substring within one text string using an HTML/XML-like syntax that Unity's text renderers parse at layout time — `<b>bold</b>`, `<color="red">red text</color>`, `<size=150%>bigger</size>`. Per `Manual/ui-systems/introduction-to-rich-text-tags.html`, a tag's **scope** by default runs from where it appears to the end of the string; an explicit closing tag (`</color>`) narrows that scope to just the enclosed text, and tags can nest without needing to close in the same order they opened. If the same tag type appears twice without a closing tag between them, the second instance overrides the first for everything after it — a common source of "why didn't my color change back" confusion. The full supported-tag table (`Manual/UIE-supported-tags.html`) covers formatting (`<b>`, `<i>`, `<s>` strikethrough equivalents), color/appearance (`<color>`, `<alpha>`, `<gradient>`, `<mark>`), layout (`<align>`, `<indent>`, `<br>`, `<pos>`), typography (`<font>`, `<font-weight>`, `<cspace>` character spacing), casing (`<allcaps>`), and interactive/embedded content (`<a href="...">`/`<link=ID>` for hyperlinks, `<sprite index=N>` for inline sprites/emoji). Attribute values accept several unit types in the same tag family — decimals, percentages, pixel values (`5px`), font-relative em units (`1.5em`), and hex color (`#FFFFFF` RGB or `#FFFFFFFF` RGBA) — so `<cspace=1em>` and `<cspace=5px>` are both valid but mean different things at different font sizes. Rich text is enabled by default on text elements that support it and can be disabled per-element (clear "Enable Rich Text" in the Inspector, or `enable-rich-text="false"` in UXML / `enableRichText = false` in C#) — disable it for any string built from untrusted/user-supplied input unless you've sanitized it, since an unescaped `<` from user text will otherwise be parsed as a tag.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class RichTextExample : MonoBehaviour
{
    void OnEnable()
    {
        var label = new Label();
        label.enableRichText = true;

        // Nested + closed scope: only "mostly" is green, rest of the line is red.
        label.text = "<color=red>This text is <color=green>mostly </color>red</color>";

        // Sprite + gradient + hyperlink combined in one string.
        label.text += "\n<gradient=\"Light to Dark Green - Vertical\">Gradient text</gradient> " +
                      "<sprite index=3> <a href=\"https://unity.com\">Visit Unity!</a>";

        GetComponent<UIDocument>().rootVisualElement.Add(label);
    }
}
```

### Runtime Text Manipulation from Script

The UI Toolkit-side, locally-documented pattern for touching text at runtime is `TextElement`'s `text` property plus `MeasureTextSize`/`MarkDirtyText` for layout queries and forced re-render (`ScriptReference/UIElements.TextElement.html`). For per-character/per-glyph effects — fading in letters, a wave animation, rainbow-cycling colors per character — the relevant API is `TextElement.PostProcessTextVertices`, an `Action<GlyphsEnumerable>` callback UI Toolkit invokes immediately before it renders each glyph's mesh; the callback receives a `GlyphsEnumerable` you iterate to reach individual `Glyph` structs and mutate their `vertices` (position/UV/tint) before the frame renders (`Manual/ui-systems/create-custom-text-animation.html` walks a complete fade example). This is analogous in spirit to TMP_Text's (not locally documented) `textInfo.characterInfo[]`/`ForceMeshUpdate()` per-character mesh access pattern that TextMeshPro is well known for in uGUI/3D contexts — same idea (grab per-character vertex data, mutate it, push it back before render), different concrete API surface depending on which text system you're in. For editable text (`TextField`/`TextInputBaseField_1`), script-driven changes to `value`/`text` should go through `SetValueWithoutNotify` when you don't want to re-trigger the field's own change-event listeners (e.g. syncing a display field from a data model), and cursor/selection state is scriptable via `cursorIndex`, `selectIndex`, and `SelectRange`/`SelectAll`/`SelectNone`.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class GlyphFadeAnimator : MonoBehaviour
{
    [SerializeField] private float fadeDuration = 1f;
    private TextElement target;
    private float elapsed;

    void OnEnable()
    {
        target = GetComponent<UIDocument>().rootVisualElement.Q<TextElement>("AnimatedLabel");
        target.experimental.animation.Start(); // schedule pattern per Manual/ui-systems/create-custom-text-animation.html
        target.generateVisualContent += _ => { }; // ensure repaint is scheduled elsewhere in real code
        target.RegisterCallback<AttachToPanelEvent>(_ =>
            target.schedule.Execute(Tick).Every(16));
    }

    void Tick()
    {
        elapsed += 0.016f;
        float t = Mathf.Clamp01(elapsed / fadeDuration);

        target.PostProcessTextVertices = glyphs =>
        {
            foreach (var glyph in glyphs)
            {
                var v = glyph.vertices;
                for (int i = 0; i < v.Length; i++)
                {
                    var c = v[i].tint;
                    c.a = t;
                    v[i].tint = c;
                }
            }
        };
        target.MarkDirtyText(); // force glyph rebuild so PostProcessTextVertices reruns
    }
}
```

### Localization-Friendly Text Sizing & Overflow

Translated strings routinely run 30-200% longer than their English source (German UI labels are a classic example), so hard-coded font sizes and fixed-width containers that "just fit" the English string will clip or overflow once localized. Two complementary, locally-documented tools address this. **Auto Sizing / Best Fit** (`ScriptReference/UIElements.TextAutoSize.html`, `-mode`/`-minSize`/`-maxSize`, driven by `TextAutoSizeMode.BestFit`) lets a text element shrink its rendered font size within a defined range to keep a translated string on one line/within its box, combined with word wrap, ellipsis truncation, and left/center/right alignment support — set via UI Builder (Text > Auto Size), USS (`-unity-text-auto-size: best-fit <min> <max>`), or C#. **Text Overflow** (`ScriptReference/UIElements.TextOverflow.html`: `Clip` vs `Ellipsis`, paired with `ScriptReference/UIElements.TextOverflowPosition.html`: `Start`/`Middle`/`End`) controls what happens when a string still doesn't fit even at minimum auto-size — `Clip` hard-cuts at the box edge, `Ellipsis` truncates with `…` at the configured position. For right-to-left languages (Arabic, Hebrew), `language-direction`/`LanguageDirection` (`Manual/ui-systems/language-direction.html`) needs to be set explicitly per element or inherited from a root — the attribute only gives basic text-reversal RTL support on its own; full line-breaking, word-wrapping, and text-shaping correctness for RTL/complex scripts requires enabling the Advanced Text Generator (`Manual/ui-systems/enable-and-use-atg.html`), which is Harfbuzz/ICU/FreeType-backed and off by default. Never assume a font size or fixed pixel width tuned against English text will hold for German, Finnish, or Arabic without auto-size, overflow handling, and (for RTL) explicit direction plus ATG in the mix.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class LocalizedLabelSetup : MonoBehaviour
{
    void OnEnable()
    {
        var label = new Label("Hello") { name = "TitleLabel" };
        label.style.unityTextOverflowPosition = TextOverflowPosition.End;
        label.style.textOverflow = TextOverflow.Ellipsis;
        label.style.overflow = Overflow.Hidden; // required for textOverflow to take effect
        // Auto-size range is typically set via USS (-unity-text-auto-size: best-fit 12px 24px;)
        // rather than a single scriptable field on style directly.

        GetComponent<UIDocument>().rootVisualElement.Add(label);
    }
}
```

### Performance: Text Rebuild Cost, Static vs. Dynamic Text

Every text mesh rebuild — triggered by changing `.text`, resizing the container, or a font-size auto-size pass — re-runs layout (line breaking, glyph positioning, rich-text tag parsing) and regenerates vertex/UV/color buffers for every visible character. For text that changes every frame (a live FPS counter, a countdown timer with sub-second precision), that's a real per-frame cost multiplied by however many glyphs are on screen; prefer changing only the substring that actually changed when the API allows it, or throttle updates (e.g. update a timer display at 10 Hz instead of 60 Hz) rather than reformatting the entire string every `Update`. Atlas population mode has its own performance profile: **Static** font assets have zero runtime atlas-mutation cost (everything is pre-baked) at the price of build-time inflexibility; **Dynamic** font assets pay a one-time cost the *first* time each new character is encountered (an atlas-texture upload + repack), which is invisible for pre-warmed common text but can cause a visible micro-stutter the first time an unusual/rare character (e.g. an uncommon CJK glyph or emoji) appears in unpredictable text like chat; **Dynamic OS** avoids shipping font data in the build but adds OS-font-lookup overhead and zero guarantee the glyph exists on every target device. Multiple sprite atlases referenced by the same text block increase draw calls (`Manual/UIE-sprite.html` explicitly recommends packing sprites used in text into one atlas), and a long fallback-font chain adds per-missing-glyph search cost plus additional draw calls each time a fallback font actually gets used (`Manual/UIE-fallback-font.html`) — both are classic "invisible until you profile it" costs that scale with how much text-with-mixed-fonts a screen actually shows. As a general rule: pre-warm Dynamic font asset atlases with the expected character set at load time (rendering an off-screen string containing likely characters once) rather than taking the hitch mid-gameplay the first time a player-typed or streamed string introduces a new glyph.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class TimerDisplay : MonoBehaviour
{
    [SerializeField] private float updateIntervalSeconds = 0.1f; // 10 Hz, not every frame
    private Label timerLabel;
    private float elapsed;
    private float remaining = 60f;

    void OnEnable() => timerLabel = GetComponent<UIDocument>().rootVisualElement.Q<Label>("Timer");

    void Update()
    {
        remaining -= Time.deltaTime;
        elapsed += Time.deltaTime;
        if (elapsed < updateIntervalSeconds) return; // throttle text rebuilds
        elapsed = 0f;
        timerLabel.text = Mathf.CeilToInt(Mathf.Max(0, remaining)).ToString(); // only rebuild when the displayed value actually changes
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---------|----------------------|
| Assuming `TMP_Text`/`TMP_FontAsset`/`TextMeshProUGUI` member names are doc-grounded | This local Unity 6 doc set has zero `TMPro` namespace pages (package docs live on a separate site); treat those exact member/signature claims as general knowledge, not verified fact |
| Using a Static font asset for player-typed or streamed chat text | Static assets only render characters baked in at creation time — any character outside that fixed set silently fails to render; use Dynamic (or Dynamic OS) for unpredictable text |
| Shipping a Dynamic font asset without pre-warming its atlas | The first time each new character appears, Unity bakes it into the atlas on the spot, causing a visible micro-stutter; pre-render an off-screen string with the expected character set at load time to pay that cost upfront |
| Choosing bitmap atlas render mode for UI text that gets scaled/animated | Bitmap glyphs are baked at a fixed resolution and go blurry/jagged when magnified or transformed; use an SDF render mode (`SDF`, `SDFAA`, etc.) for any text that isn't rendered at a fixed 1:1 pixel size (pixel-art UI is the one legitimate bitmap use case) |
| Wanting text-shadow/outline effects on a bitmap font asset | Per `Manual/UIE-text-effects.html`, text effects only work on SDF-rendered font assets, and the effect's usable range is capped by the asset's baked padding — increase padding at atlas-generation time if the effect looks clipped |
| Repeating the same rich-text tag mid-string and expecting both instances to apply independently | The later tag instance overrides the earlier one for everything after it (no automatic stacking); use an explicit closing tag (`</color>`) to scope the first instance before opening the second |
| Leaving rich text enabled on strings built from unsanitized user input | An unescaped `<` in player-typed text gets parsed as the start of a tag, which can break layout or inject unintended styling; disable rich text (`enableRichText = false`) on untrusted input, or escape `<`/`>`/`"` first |
| Tuning font size/box width against English text only | Localized strings commonly run 30-200% longer; without Auto Sizing (Best Fit) and Text Overflow (Ellipsis/Clip) configured, translated UI clips or overflows its container in other languages |
| Assuming `language-direction="RTL"` alone gives full Arabic/Hebrew support | The base RTL attribute only reverses text direction; correct line-breaking, word-wrapping, and glyph shaping for complex scripts requires enabling the Advanced Text Generator, which is off by default |
| Referencing font files directly instead of converting them to font assets | TextCore renders through font *assets* (atlas + metrics), not raw `.ttf`/`.otf`/`.ttc` files — a font file must first be converted (Font Asset Creator, or `Create > Text Core > Font Asset`) before any text element can use it |
| Using more than one sprite atlas for sprites inside a single text block | Each additional atlas referenced by the same text element adds draw calls; per `Manual/UIE-sprite.html`, pack all sprites used together in one atlas texture |
| Building a long fallback-font chain without profiling it | Every glyph search that falls through to a fallback font costs extra lookup time plus an additional draw call when that fallback is actually used; keep chains as short as the actual character-set requirements demand |
| Reformatting/reassigning the full text string every frame for fast-changing values (timers, counters) | Every `.text` assignment re-runs layout and rebuilds the glyph mesh for the whole string; throttle updates (e.g. 10 Hz for a countdown) or only touch the substring that changed |
| Using the legacy `TextMesh` component for new 3D world-space text | Its own Manual page (`class-TextMesh.html`) flags it as having "limited functionality" and points to modern UI/TextMeshPro instead — it's still shipped for backward compatibility, not as a recommended path for new work |
| Confusing UI Toolkit's `TextElement`/`Label` rich-text/font-asset system with TextMeshPro-the-component | Both render through TextCore and share tag syntax/font-asset concepts, but they are two separate concrete implementations (UI Toolkit's own text stack vs. the `TMPro` package's `TMP_Text` components) — code/assets aren't drop-in interchangeable between them |

## Quick Reference

| Item | Purpose |
|------|---------|
| `TextCore.Text.AtlasPopulationMode` (`Static`/`Dynamic`/`DynamicOS`) | When a font asset's atlas gets baked: at creation (Static), on first use (Dynamic), or referencing an OS-installed font (DynamicOS) |
| `TextCore.LowLevel.GlyphRenderMode` (`SMOOTH`/`RASTER`/`COLOR`/`SDF`/`SDFAA`/…) | Bitmap vs. Signed Distance Field atlas rendering; SDF stays crisp at any scale/rotation and enables shader-driven outline/shadow effects |
| `TextCore.LowLevel.FontEngine` | Low-level FreeType-backed API (`LoadFontFace`, `TryGetGlyphIndex`, `SetFaceSize`) that the Font Asset Creator calls to generate an atlas |
| `TextCore.LowLevel.GlyphPackingMode` | Bin-packing strategy for laying glyphs into an atlas texture (Best Area/Short Side/Long Side Fit, Contact Point, Bottom Left) |
| `TextCore.Glyph` / `GlyphMetrics` / `GlyphRect` | Per-character atlas data: index, bearing/advance metrics, and atlas-texture rectangle |
| `TextCore.FaceInfo` | Font-face-wide metrics: point size, line height, ascent/descent/baseline, sub/superscript offsets |
| Rich text tags (`<b>`, `<i>`, `<color>`, `<size>`, `<align>`, `<sprite>`, `<gradient>`, `<a href>`/`<link>`, `<cspace>`, `<allcaps>`) | HTML/XML-like inline styling within a text string, scoped from point-of-appearance to end-of-string (or to a closing tag) |
| `UIElements.TextElement` | UI Toolkit base class for any text-displaying control; `text`, `enableRichText`, `MeasureTextSize`, `MarkDirtyText` |
| `UIElements.Label` / `UIElements.TextField` | Concrete display-only and editable UI Toolkit text controls built on `TextElement` |
| `UIElements.TextInputBaseField<T>` | Generic editable-text base: cursor/selection state, password masking, mobile keyboard behavior |
| `UIElements.TextElement.PostProcessTextVertices` | `Action<GlyphsEnumerable>` callback for per-glyph vertex/tint mutation immediately before render — the API for custom text animation |
| `UIElements.TextAutoSize` / `TextAutoSizeMode.BestFit` | Auto-shrinking font size within a min/max range to fit content — key tool for localization-safe layouts |
| `UIElements.TextOverflow` (`Clip`/`Ellipsis`) + `TextOverflowPosition` (`Start`/`Middle`/`End`) | What happens when text still doesn't fit after auto-sizing |
| `UIElements.FontDefinition` (`.FromFont` / `.FromSDFFont`) | Style-level wrapper holding either a legacy bitmap `Font` or an SDF `FontAsset` |
| `UIElements.TextShadow` | C# struct backing the `text-shadow` USS property (offset, blur radius, color) |
| Advanced Text Generator (ATG) | Harfbuzz/ICU/FreeType-backed text-shaping module; required for correct BIDI/complex-script line-breaking and word-wrapping; off by default |
| `language-direction` / `LanguageDirection` | Inherited/LTR/RTL attribute controlling text flow direction, cascades to children |
| Legacy `TextMesh` component | Pre-TMP world-space 3D text mesh; Manual-flagged as limited functionality, superseded |
| Legacy `Font` class | The `UnityEngine.Font` object (TTF/OTF/TTC) a font asset's "Source Font File" wraps |
| Legacy `TextGenerator` | Pre-TMP uGUI `Text` component's mesh-generation API; no SDF support |
| `TMP_Text` / `TMP_FontAsset` / `TextMeshProUGUI` (package API) | Not locally documented in this Unity 6 doc set — general knowledge only, unverified against local sources |

## Advanced Notes

**Font fallback chains.** Per `Manual/UIE-fallback-font.html`, when a character is missing from a text element's primary font asset, TextCore walks a Fallback List (checked in order) until it finds an asset containing that character, then renders using that fallback asset's glyph. Fallback lists can be set locally (per font asset, or per element) or globally (project-wide default chain), and the same character can legitimately appear in multiple fallback entries — TextCore just uses whichever it reaches first. The guidance is to keep a fallback font's style reasonably close to the primary font's style, since a jarring style mismatch mid-sentence (e.g. a serif fallback glyph inside an otherwise sans-serif string) reads as a bug even though it's working as designed. Every fallback search and every additional font actually used adds compute and draw-call cost, so fallback chains should be sized to the actual script/character-set coverage a project needs (e.g. CJK + emoji + base Latin) rather than padded defensively — Dynamic OS assets are called out specifically as good fallback candidates because they add broad OS-level character coverage without bundling extra font files in the build.

**Dynamic atlas generation performance/memory tradeoffs.** The Static/Dynamic/Dynamic OS choice is fundamentally a build-time-vs-runtime tradeoff: Static pays cost once at asset-creation time and ships a fixed, complete atlas (fast at runtime, zero flexibility, and no source font file required in the build); Dynamic defers that cost to runtime on a per-glyph basis (flexible, but every first-encounter of a new character triggers an atlas texture repack, and the source font file itself must be included in the build, which is real disk/memory weight most Static assets avoid); Dynamic OS shifts the cost to "does this glyph exist in an OS-installed font on this exact device," trading guaranteed availability for the smallest possible build footprint. None of these is strictly better — the right choice tracks how *predictable* the text content is: known/finite UI strings (menu labels, section titles) → Static; open-ended input (chat, search, user-generated content) → Dynamic; broad-coverage fallback role → Dynamic OS.

**TMP's relationship to UI Toolkit's built-in text system.** These are not the same component family, but they are not unrelated either: `Manual/UIE-get-started-with-text.html` states directly that UI Toolkit renders text through TextCore, "a technology based on TextMesh Pro," and that TextCore brings SDF font rendering into UI Toolkit the same way it underlies TMP's uGUI/3D components. Concretely, that shared foundation shows up as shared concepts (font assets with the same Static/Dynamic/Dynamic OS population modes, the same bitmap-vs-SDF atlas render modes, an overlapping rich-text tag vocabulary) implemented as two separate concrete systems: `TMPro.TMP_Text` and friends (uGUI/3D component API, not locally documented in this doc set) versus `UnityEngine.UIElements.TextElement`/`Label`/`TextField` (UI Toolkit's native controls, fully documented locally). Beyond that shared TextCore foundation, UI Toolkit also has a second, newer text-shaping layer — the Advanced Text Generator (`Manual/UIE-advanced-text-generator.html`), built on Harfbuzz/ICU/FreeType — which is opt-in (Project Settings > UI Toolkit) and adds correctness for complex/BIDI scripts that the standard TextCore path doesn't fully handle; static font assets are explicitly unsupported once ATG is enabled (`Manual/ui-systems/migrate-static-font-assets.html`), so enabling ATG on a project that leans on Static font assets requires a migration pass, not a flag flip.

---
name: unity-shaders-hlsl
description: Use when hand-authoring Unity shaders in HLSL/ShaderLab rather than Shader Graph — writing .shader files, custom lighting models, or shader passes/variants. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Shaders (HLSL/ShaderLab)

Hand-written shader syntax, includes, and pragmas differ sharply between Built-in Render Pipeline (BIRP) and URP, and between Unity versions. **Prefer the local docs over pre-trained knowledge** for block structure, include paths, and pragma directives — Unity has renamed and moved shader library files across versions, and BIRP/URP syntax is easy to cross-contaminate from memory. This skill covers hand-written `.shader` files only — Shader Graph and render-pipeline selection are owned by the sibling `unity-rendering-pipelines` skill.

## Retrieval Sources

Docs root: `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/`

| Source | Path | Use for |
|--------|------|---------|
| Shader/SubShader/Pass block structure | `SL-Shader.html`, `SL-SubShader.html`, `SL-Pass.html` | Outer nested-brace syntax; what each block can contain |
| Properties block & material properties | `SL-Properties.html`, `writing-shader-material-properties.html`, `SL-PropertiesInPrograms.html` | Property syntax by type, reserved names, attributes, HLSL variable mapping |
| Shader/SubShader/Pass Tags | `SL-PassTags.html`, `SL-SubShaderTags.html`, `writing-shader-tags.html` | `LightMode`, `RenderPipeline`, `Queue`, `RenderType`, `DisableBatching`, etc. |
| LOD, UsePass, GrabPass | `SL-ShaderLOD.html`, `SL-UsePass.html`, `SL-GrabPass.html` | SubShader-level directives: LOD selection, reusing another shader's Pass, screen-grab textures |
| Fallback & PackageRequirements | `SL-Fallback.html`, `SL-PackageRequirements.html` | Fallback shader selection; gating a SubShader/Pass on installed packages/Unity version |
| Render state commands overview | `writing-shader-render-state-commands.html` | Index of all GPU render-state commands and the `Category` block |
| Cull / Offset / Conservative | `SL-Cull.html`, `SL-Offset.html`, `SL-Conservative.html` | Backface culling, depth bias, conservative rasterization |
| ZWrite / ZTest / ZClip | `SL-ZWrite.html`, `SL-ZTest.html`, `SL-ZClip.html` | Depth buffer writes, depth comparison, near/far clip mode |
| Blend / BlendOp / ColorMask / AlphaToMask | `SL-Blend.html`, `SL-BlendOp.html`, `SL-ColorMask.html`, `SL-AlphaToMask.html` | Transparency blending factors/equations, channel write masks, alpha-to-coverage |
| Stencil buffer | `SL-Stencil.html` | Stencil ref/read/write masks, comparison and stencil ops, front/back face variants |
| Adding HLSL program blocks | `writing-shader-add-shader-program.html`, `writing-shader-writing-shader-programs-hlsl.html`, `writing-shader-programs-introduction.html` | Wrapping HLSL in `HLSLPROGRAM`/`ENDHLSL`, vertex/fragment program basics |
| HLSL semantics reference | `SL-HLSLSemantics.html` | `POSITION`, `NORMAL`, `TEXCOORD0-3`, `TANGENT`, `COLOR`, `VPOS`, `VFACE`, `SV_VertexID` |
| Vertex input structures (BIRP) | `SL-VertexProgramInputs.html`, `SL-BuiltinIncludes-Structures.html` | Custom vertex structs; prebuilt `appdata_base`/`appdata_full`/`appdata_tan`/`appdata_img` |
| Pragma directives overview | `SL-PragmaDirectives.html`, `writing-shader-programs-pragma-directives.html` | Full directive list: stages, variants, GPU requirements, graphics APIs, debug flags |
| Pragma target / require (shader models) | `SL-Pragma-target.html`, `SL-Pragma-require.html`, `SL-ShaderCompileTargets.html` | Shader model levels (2.0–5.0), individual GPU feature requirements, per-variant gating |
| Shader keywords & variants | `SL-MultipleProgramVariants.html`, `SL-MultipleProgramVariants-declare.html`, `SL-MultipleProgramVariants-make-conditionals.html`, `SL-MultipleProgramVariants-shortcuts.html` | Declaring keyword sets, branching on them, built-in shortcut macros (`multi_compile_fog`, etc.) |
| Built-in include files (BIRP `.cginc`) | `SL-BuiltinIncludes.html`, `SL-BuiltinIncludes-Lighting.html`, `SL-BuiltinIncludes-PassTags.html` | `UnityCG.cginc`, `Lighting.cginc`, `AutoLight.cginc`; BIRP lighting variables and LightMode values |
| Built-in shader variables (BIRP) | `SL-UnityShaderVariables.html` | Transform matrices, camera/screen/time globals, inline sampler-state suffixes |
| ShaderLab language overview | `SL-landing.html`, `SL-Reference.html`, `SL-ShadingLanguage.html` | Table-of-contents pages tying the whole ShaderLab/HLSL reference together |
| Rendering paths & Surface Shaders (BIRP) | `SL-RenderPipeline.html`, `writing-shaders-birp.html` | `ForwardBase`/`ForwardAdd`/`Deferred`/`Vertex` pass selection; Surface Shader entry points |
| URP basic shader structure & worked examples | `urp/writing-shaders-urp-basic-unlit-structure.html`, `urp/writing-shaders-urp-unlit-color.html`, `urp/writing-shaders-urp-unlit-texture.html` | Minimal compiling URP unlit shader, then adding a color property, then a texture |
| URP Pass tags reference | `urp/urp-shaders/urp-shaderlab-pass-tags.html` | `LightMode` values URP recognizes (`UniversalForward`, `ShadowCaster`, `DepthOnly`, etc.) |
| URP shader library methods | `urp/use-built-in-shader-methods.html`, `urp/use-built-in-shader-methods-transformations.html`, `urp/use-built-in-shader-methods-lighting.html`, `urp/use-built-in-shader-methods-shadows.html`, `urp/use-built-in-shader-methods-additional-lights-fplus.html` | `TransformObjectToHClip`, `GetVertexPositionInputs`, `GetMainLight`, shadow coordinate/attenuation functions, Forward+ light loop |
| URP keywords & macros reference | `urp/urp-shaders/shader-keywords-macros.html` | `_ADDITIONAL_LIGHTS`, `_CLUSTER_LIGHT_LOOP`, `LIGHT_LOOP_BEGIN`/`END` |
| URP custom lighting | `urp/lighting/custom-lighting-introduction.html` | When/how to hand-write a lighting model vs. Shader Graph custom function nodes |
| URP shader variant stripping | `urp/shader-stripping-landing.html`, `urp/shader-stripping-features.html`, `urp/shader-stripping-check.html` | Reducing build size/compile time; logging and exporting variant counts |

## Key Guidelines

### ShaderLab Structure (Properties/SubShader/Pass/Tags)

ShaderLab is the outer declarative wrapper — `Shader`, `Properties`, `SubShader`, `Pass`, `Tags` — around one or more inner HLSL program blocks; ShaderLab itself has no math, just structure, state, and metadata. A `Shader` block can contain a `Properties` block, one or more `SubShader` blocks, an optional custom editor assignment, and an optional `Fallback`. Unity picks the **first** `SubShader` that is compatible with the current GPU/pipeline, so put your most-capable SubShader first and simpler fallbacks after it. Inside a `SubShader`, an optional `LOD` value and a `Tags` block (commonly `RenderPipeline`, `Queue`, `RenderType`) scope which pipeline and render order the SubShader applies to; a `SubShader` then contains one or more `Pass` blocks, each of which can have its own `Name`, its own `Tags` (most importantly `LightMode`, which controls *when* the pipeline runs the pass), render-state commands, and a shader code block.

```hlsl
Shader "Examples/StructureDemo"
{
    Properties
    {
        [MainColor] _BaseColor("Base Color", Color) = (1, 1, 1, 1)
    }

    SubShader
    {
        // SubShader Tags scope this whole SubShader to URP and the opaque queue.
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" "Queue" = "Geometry" }
        LOD 100

        Pass
        {
            Name "ForwardUnlit"
            // Pass Tags control when this specific Pass runs.
            Tags { "LightMode" = "SRPDefaultUnlit" }

            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            struct Attributes { float4 positionOS : POSITION; };
            struct Varyings   { float4 positionHCS : SV_POSITION; };

            CBUFFER_START(UnityPerMaterial)
                half4 _BaseColor;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                return OUT;
            }

            half4 frag() : SV_Target { return _BaseColor; }
            ENDHLSL
        }
    }

    // Used only if no SubShader above is compatible with the current hardware/pipeline.
    Fallback "Universal Render Pipeline/Unlit"
}
```

### HLSLPROGRAM vs CGPROGRAM

URP/SRP shaders use `HLSLPROGRAM`/`ENDHLSL` and include files from `Packages/com.unity.render-pipelines.*`; legacy Built-in Render Pipeline shaders use `CGPROGRAM`/`ENDCG` and include `.cginc` files such as `UnityCG.cginc`. These are **not interchangeable** — mixing `CGPROGRAM` blocks with URP includes, or `HLSLPROGRAM` blocks with `UnityCG.cginc`, produces broken output (often solid pink/magenta) or compile errors, because the macro and function names in each library conflict or are simply absent from the other. A single `Shader` file *can* legitimately contain one SubShader targeting URP (tagged `"RenderPipeline" = "UniversalPipeline"`) and a second SubShader with no `RenderPipeline` tag as a BIRP-compatible fallback, each using its own program-block type and includes — this is the standard way to ship one shader that degrades gracefully across pipelines.

```hlsl
Shader "Examples/DualPipelineUnlit"
{
    Properties
    {
        [MainColor] _BaseColor("Base Color", Color) = (1, 0, 0, 1)
    }

    // Picked when the active pipeline asset is a UniversalRenderPipelineAsset.
    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }
        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            struct Attributes { float4 positionOS : POSITION; };
            struct Varyings   { float4 positionHCS : SV_POSITION; };

            CBUFFER_START(UnityPerMaterial)
                half4 _BaseColor;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                return OUT;
            }
            half4 frag() : SV_Target { return _BaseColor; }
            ENDHLSL
        }
    }

    // No RenderPipeline tag = Built-in-only. Unity falls through to this SubShader
    // when no SRP is active.
    SubShader
    {
        Tags { "RenderType" = "Opaque" }
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            fixed4 _BaseColor;

            struct appdata { float4 vertex : POSITION; };
            struct v2f      { float4 pos : SV_POSITION; };

            v2f vert(appdata v)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(v.vertex);
                return o;
            }
            fixed4 frag(v2f i) : SV_Target { return _BaseColor; }
            ENDCG
        }
    }
}
```

### Vertex/Fragment Function Structure

Structure vertex/fragment stages as input/output structs whose fields carry explicit HLSL semantics connecting them to mesh data — Unity's URP convention names these `Attributes` (vertex input) and `Varyings` (vertex-to-fragment output), while legacy BIRP code and tutorials often use `appdata`/`v2f`. Every `Varyings`-equivalent struct **must** include a `float4` field with the `SV_POSITION` semantic (clip-space position); everything else is up to you, but interpolator counts are limited by target platform, so pack related data into single `float4`s where possible (e.g. two `float2` UV sets into one `float4`). BIRP additionally ships prebuilt vertex-input structs in `UnityCG.cginc` (`appdata_base`, `appdata_tan`, `appdata_full`, `appdata_img`) so you don't have to hand-declare common combinations of position/normal/tangent/UV/color.

```hlsl
Shader "Examples/VertexFragmentStructure"
{
    Properties { _BaseMap("Base Map", 2D) = "white" {} }
    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }
        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            struct Attributes
            {
                float4 positionOS : POSITION;
                float3 normalOS   : NORMAL;
                float2 uv         : TEXCOORD0;
                float4 color      : COLOR;
            };

            struct Varyings
            {
                float4 positionHCS : SV_POSITION;  // Required: clip-space position.
                float3 normalWS    : TEXCOORD0;    // Reuse TEXCOORD slots for custom data.
                float2 uv          : TEXCOORD1;
                float4 color       : COLOR;
            };

            TEXTURE2D(_BaseMap);
            SAMPLER(sampler_BaseMap);
            CBUFFER_START(UnityPerMaterial)
                float4 _BaseMap_ST;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                OUT.normalWS    = TransformObjectToWorldNormal(IN.normalOS);
                OUT.uv          = TRANSFORM_TEX(IN.uv, _BaseMap);
                OUT.color       = IN.color;
                return OUT;
            }

            half4 frag(Varyings IN) : SV_Target
            {
                half4 tex = SAMPLE_TEXTURE2D(_BaseMap, sampler_BaseMap, IN.uv);
                return tex * IN.color;
            }
            ENDHLSL
        }
    }
}
```

### Render State (Cull/ZWrite/Blend/ZTest/Stencil)

Render-state commands (`Cull`, `ZWrite`, `ZTest`, `ZClip`, `Blend`, `BlendOp`, `ColorMask`, `AlphaToMask`, `Offset`, `Stencil`, `Conservative`) configure fixed-function GPU state *before* your shader program runs for that Pass. Placed inside a `Pass` block they apply to that Pass only; placed directly inside a `SubShader` block (outside any Pass) they apply to every Pass within it unless a Pass overrides them locally. Blending defaults to `Off`; when enabled without an explicit `BlendOp`, the operation defaults to `Add`, and the blend equation is `finalValue = sourceFactor * sourceValue OP destinationFactor * destinationValue`. `ZWrite` is conventionally `On` for opaque geometry and `Off` for transparent geometry (since transparent surfaces normally shouldn't occlude what's behind them); disabling it means you may need to sort geometry yourself to avoid ordering artifacts. The `Stencil` block's `Ref`/`ReadMask`/`WriteMask`/`Comp`/`Pass`/`Fail`/`ZFail` fields are all optional and independently overridable per front/back face via `*Front`/`*Back` suffixes.

```hlsl
Shader "Examples/RenderStateDemo"
{
    Properties
    {
        [MainColor] _BaseColor("Base Color", Color) = (1, 1, 1, 0.5)
    }
    SubShader
    {
        Tags { "RenderType" = "Transparent" "RenderPipeline" = "UniversalPipeline" "Queue" = "Transparent" }

        Pass
        {
            Cull Back
            ZWrite Off
            ZTest LEqual
            Blend SrcAlpha OneMinusSrcAlpha
            BlendOp Add
            ColorMask RGB

            Stencil
            {
                Ref 1
                Comp Always
                Pass Replace
            }

            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            struct Attributes { float4 positionOS : POSITION; };
            struct Varyings   { float4 positionHCS : SV_POSITION; };

            CBUFFER_START(UnityPerMaterial)
                half4 _BaseColor;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                return OUT;
            }
            half4 frag() : SV_Target { return _BaseColor; }
            ENDHLSL
        }
    }
}
```

### #pragma Directives & Variants/Keywords

`#pragma vertex <name>` and `#pragma fragment <name>` are required in any regular graphics shader and name the entry-point functions; `#pragma target <model>` and `#pragma require <features>` declare the minimum shader model / GPU features the program needs (Unity warns and can fail to compile on platforms below the target). Shader keywords let one HLSL program compile into multiple variants: `#pragma multi_compile <keywords>` keeps **every** listed variant in the build regardless of whether any material currently uses it (good for things toggled at runtime by script); `#pragma shader_feature <keywords>` behaves the same at compile time but Unity strips variants that no material in the project actually references, shrinking the build (good for material-authored toggles). Add a leading underscore (`_`) as a keyword in the set to create the "all keywords in this set disabled" variant explicitly — without it, that all-off state isn't a distinct compiled variant for `shader_feature`/`multi_compile` sets of two or more keywords. Branch on keywords with a plain HLSL `if` (Unity compiles it to the right static branch or dynamic branch depending on the directive used) or with `#if defined(...)`.

```hlsl
Shader "Examples/KeywordVariants"
{
    Properties
    {
        [MainColor] _BaseColor("Base Color", Color) = (1, 1, 1, 1)
        [Toggle(_RIM_ON)] _UseRim("Enable Rim Light", Float) = 0
    }
    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }
        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #pragma target 3.5

            // shader_feature: variants no material uses are stripped from the build.
            #pragma shader_feature_local _RIM_ON
            // multi_compile: every listed variant is always kept (e.g. toggled at
            // runtime via Shader.EnableKeyword / material.EnableKeyword).
            #pragma multi_compile _ _ADDITIONAL_LIGHTS

            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            struct Attributes { float4 positionOS : POSITION; float3 normalOS : NORMAL; };
            struct Varyings   { float4 positionHCS : SV_POSITION; float3 normalWS : TEXCOORD0; };

            CBUFFER_START(UnityPerMaterial)
                half4 _BaseColor;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                OUT.normalWS = TransformObjectToWorldNormal(IN.normalOS);
                return OUT;
            }

            half4 frag(Varyings IN) : SV_Target
            {
                half4 color = _BaseColor;
            #if defined(_RIM_ON)
                half rim = 1 - saturate(dot(normalize(IN.normalWS), half3(0, 0, 1)));
                color.rgb += rim;
            #endif
                return color;
            }
            ENDHLSL
        }
    }
}
```

### URP Includes vs Built-in cginc Includes

URP shaders include library files from `Packages/com.unity.render-pipelines.universal/ShaderLibrary/` (and `.../render-pipelines.core/ShaderLibrary/`); BIRP shaders include `.cginc` files that ship inside the Unity Editor install itself and are found on the compiler's built-in include path (no package path needed). The two hierarchies are **not** compatible with each other and must never both be included in the same program block.

| BIRP include (`.cginc`) | Contents | URP include (`ShaderLibrary/*.hlsl`) | Contents |
|---|---|---|---|
| `HLSLSupport.cginc` | Cross-platform preprocessor macros (auto-included by `CGPROGRAM`) | `Core.hlsl` | Pulls in `Common.hlsl`, `SpaceTransforms.hlsl`, `Packing.hlsl`, texture/CBUFFER macros |
| `UnityShaderVariables.cginc` | Built-in global variables (auto-included by `CGPROGRAM`) | `Core.hlsl` (via `ShaderVariablesFunction.hlsl`) | Transform/camera/screen helper functions |
| `UnityCG.cginc` | Common helper functions & `appdata_*` structs | `Core.hlsl` | `TransformObjectToHClip`, `GetVertexPositionInputs`, etc. |
| `AutoLight.cginc` | Lighting/shadowing support used internally by Surface Shaders | `Lighting.hlsl` | `GetMainLight`, `GetAdditionalLight`, shadow sampling |
| `Lighting.cginc` | Standard Surface Shader lighting models (auto-included when writing Surface Shaders) | `Lighting.hlsl` | `LightingLambert`, `LightingSpecular` |
| `TerrainEngine.cginc` | Terrain/vegetation shader helpers | — (URP terrain shaders use their own package includes) | — |

```hlsl
Shader "Examples/IncludeChainDemo"
{
    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }
        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            // Core.hlsl pulls in Common.hlsl, SpaceTransforms.hlsl, Packing.hlsl, etc.
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"
            // Lighting.hlsl pulls in RealtimeLights.hlsl, AmbientOcclusion.hlsl, and
            // Shadows.hlsl. It's safe to include alongside Core.hlsl — URP headers
            // use include guards.
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Lighting.hlsl"

            struct Attributes { float4 positionOS : POSITION; };
            struct Varyings   { float4 positionHCS : SV_POSITION; };

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                return OUT;
            }

            half4 frag() : SV_Target
            {
                Light mainLight = GetMainLight();
                return half4(mainLight.color, 1);
            }
            ENDHLSL
        }
    }
}
```

### Writing a Basic Lit Shader

A hand-written lit shader means reimplementing the lighting model yourself — sampling the main light, applying shadow attenuation, and (if needed) looping over additional lights — that Shader Graph's Lit target or BIRP Surface Shaders normally generate for you. In URP, `GetMainLight()` returns a `Light` struct with `direction`/`color`/`distanceAttenuation`/`shadowAttenuation`, but `shadowAttenuation` only reflects real shadowing if you instead call the shadow-coordinate overload `GetMainLight(shadowCoords)`, where `shadowCoords` comes from `GetShadowCoord(VertexPositionInputs)`; you also need a `multi_compile` for the main-light shadow keywords so the correct shadow-sampling variant compiles in. Critically, the object also needs a `ShadowCaster` pass so *other* objects can receive shadows cast by it — rather than hand-writing one, `UsePass` can pull in the built-in Lit shader's, referencing it by uppercased Pass name.

```hlsl
Shader "Examples/BasicLit"
{
    Properties
    {
        [MainTexture] _BaseMap("Base Map", 2D) = "white" {}
        [MainColor] _BaseColor("Base Color", Color) = (1, 1, 1, 1)
    }

    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" "Queue" = "Geometry" }

        Pass
        {
            Name "ForwardLit"
            Tags { "LightMode" = "UniversalForward" }

            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            // Required so this shader can access the main light's shadow map.
            #pragma multi_compile _ _MAIN_LIGHT_SHADOWS _MAIN_LIGHT_SHADOWS_CASCADE _MAIN_LIGHT_SHADOWS_SCREEN
            #pragma multi_compile _ _ADDITIONAL_LIGHTS

            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Lighting.hlsl"

            struct Attributes
            {
                float4 positionOS : POSITION;
                float3 normalOS   : NORMAL;
                float2 uv         : TEXCOORD0;
            };

            struct Varyings
            {
                float4 positionCS   : SV_POSITION;
                float3 normalWS     : TEXCOORD0;
                float2 uv           : TEXCOORD1;
                float4 shadowCoords : TEXCOORD2;
            };

            TEXTURE2D(_BaseMap);
            SAMPLER(sampler_BaseMap);
            CBUFFER_START(UnityPerMaterial)
                float4 _BaseMap_ST;
                half4 _BaseColor;
            CBUFFER_END

            Varyings vert(Attributes IN)
            {
                Varyings OUT;
                VertexPositionInputs posInputs = GetVertexPositionInputs(IN.positionOS.xyz);
                VertexNormalInputs   nrmInputs = GetVertexNormalInputs(IN.normalOS);

                OUT.positionCS   = posInputs.positionCS;
                OUT.normalWS     = nrmInputs.normalWS;
                OUT.uv           = TRANSFORM_TEX(IN.uv, _BaseMap);
                OUT.shadowCoords = GetShadowCoord(posInputs);
                return OUT;
            }

            half4 frag(Varyings IN) : SV_Target
            {
                half4 albedo = SAMPLE_TEXTURE2D(_BaseMap, sampler_BaseMap, IN.uv) * _BaseColor;

                // The shadowCoords overload sets Light.shadowAttenuation from the
                // main light's shadow map instead of always returning 1.
                Light mainLight = GetMainLight(IN.shadowCoords);
                half3 diffuse = LightingLambert(mainLight.color, mainLight.direction, normalize(IN.normalWS))
                                 * mainLight.distanceAttenuation * mainLight.shadowAttenuation;

                return half4(albedo.rgb * diffuse, albedo.a);
            }
            ENDHLSL
        }

        // Reuse the built-in Lit shader's ShadowCaster pass rather than hand-writing
        // one — this is what lets OTHER objects receive shadows cast by this shader.
        UsePass "Universal Render Pipeline/Lit/SHADOWCASTER"
    }
}
```

## Common Mistakes

| Mistake | Why / fix |
|---|---|
| Pink/magenta or fully broken output in URP | `CGPROGRAM`/`UnityCG.cginc` (BIRP) mixed into a URP shader; use `HLSLPROGRAM` + `Core.hlsl`/`Lighting.hlsl` instead |
| Shader compiles but doesn't render in URP | Missing/incorrect `Tags { "RenderPipeline" = "UniversalPipeline" }` on the SubShader; see `urp/urp-shaders/urp-shaderlab-pass-tags.html` |
| Build/compile times balloon | Unbounded `multi_compile` combinations; audit keyword count, switch rarely-used axes to `shader_feature` |
| Lighting looks flat/wrong in a hand-rolled lit shader | Custom lighting model omits GI/shadow terms Shader Graph's Lit target normally supplies; check `urp/lighting/custom-lighting-introduction.html` |
| Shader errors only on some platforms | `#pragma target` too low for the HLSL features used, or platform-specific syntax; check `SL-Pragma-target.html` / `SL-ShaderCompileTargets.html` |
| Material properties silently ignored, SRP Batcher disabled | Per-material properties not declared inside a single `CBUFFER_START(UnityPerMaterial)`/`CBUFFER_END` block in URP; see `SL-VertexProgramInputs.html` "Add a custom buffer" |
| Texture Tiling/Offset fields do nothing | Forgot to declare the matching `_ST` suffix property (e.g. `_BaseMap_ST`) that `TRANSFORM_TEX` depends on |
| `sampler2D _MyTex; tex2D(_MyTex, uv)` used inside a URP `HLSLPROGRAM` | That's BIRP/Cg legacy syntax; URP requires `TEXTURE2D(_MyTex)` / `SAMPLER(sampler_MyTex)` / `SAMPLE_TEXTURE2D(...)` macros |
| "vertex/fragment shader not found" compile error | `#pragma vertex <name>` / `#pragma fragment <name>` doesn't match the actual HLSL function name, or the function isn't in scope in that program block |
| Shader fails to parse / mismatched block error | `HLSLPROGRAM` closed with `ENDCG` (or vice versa) instead of its matching `ENDHLSL`/`ENDCG` |
| Object renders as the pink error shader on some hardware | No `Fallback` set (or `Fallback Off`) and no compatible SubShader for that hardware/pipeline; see `SL-Fallback.html` |
| `shader_feature`/`multi_compile` set can't represent "nothing enabled" | Missing the leading `_` entry in a multi-keyword set declaration (e.g. `shader_feature _ RED GREEN BLUE`); without it there's no compiled variant for the all-off state |
| `GrabPass` used in a URP or HDRP shader | `GrabPass` is BIRP-only (see `SL-GrabPass.html` render-pipeline compatibility table); use the Opaque Texture / Camera Color URP feature instead |
| Shipped build is bloated and slow | `#pragma enable_d3d11_debug_symbols` (or similar debug pragmas) left in the shader for a final build; strip debug pragmas before shipping |
| Transparent object depth-sorts incorrectly against itself/other transparents | `ZWrite On` left enabled on a `Blend`-enabled transparent Pass, or `ZWrite Off` used but geometry isn't sorted back-to-front on the CPU |

## Quick Reference

| Block / Directive | Purpose |
|---|---|
| `Shader "path" { }` | Top-level container: Properties, one or more SubShaders, custom editor, Fallback |
| `Properties { }` | Material-exposed inputs (textures, colors, floats, ints, vectors) |
| `SubShader { }` | A renderable variant set, picked per-pipeline/LOD; first compatible one wins |
| `Pass { }` | One draw submission; holds Name, Tags, render state, and the HLSL program |
| `Tags { "K"="V" }` | Key/value metadata on Shader/SubShader/Pass (`RenderPipeline`, `Queue`, `RenderType`, `LightMode`, `DisableBatching`, …) |
| `LOD <n>` | Assigns a level-of-detail value to a SubShader for `Shader.maximumLOD`/`LODGroup` selection |
| `UsePass "Shader/PASS NAME"` | Inserts a named Pass (uppercase) from another Shader object |
| `GrabPass { }` / `GrabPass { "Name" }` | (BIRP only) Grabs the frame buffer into a texture for later Passes |
| `Fallback "Shader"` / `Fallback Off` | Backup shader when no SubShader matches; `Off` shows the error shader instead |
| `PackageRequirements { }` | Gates a SubShader/Pass on installed package or Unity version ranges |
| `Category { }` | Groups render-state commands so contained SubShaders/Passes inherit them |
| `Cull Back / Front / Off` | Backface/frontface culling (default `Back`) |
| `ZWrite On / Off` | Enables/disables writes to the depth buffer |
| `ZTest <op>` | Depth comparison mode: `Less`, `LEqual` (default), `Greater`, `Always`, etc. |
| `ZClip True / False` | Clip vs. clamp fragments outside near/far planes |
| `Offset <factor>, <units>` | Depth bias to avoid z-fighting |
| `Blend <src> <dst>` | Enables blending, sets RGBA (or separate RGB/alpha) blend factors |
| `BlendOp <op>` | Blend equation: `Add` (default), `Sub`, `RevSub`, `Min`, `Max`, logical/advanced ops |
| `ColorMask <RGBA/0>` | Restricts which color channels the GPU writes |
| `AlphaToMask On / Off` | Enables alpha-to-coverage (MSAA) |
| `Conservative True / False` | Enables conservative rasterization |
| `Stencil { }` | Configures stencil ref/masks/comparison/pass/fail/zfail ops |
| `HLSLPROGRAM` / `ENDHLSL` | SRP (URP/HDRP) shader code block |
| `CGPROGRAM` / `ENDCG` | Legacy BIRP shader code block (also emits HLSL under the hood) |
| `CGINCLUDE` / `ENDCG` | BIRP-only: HLSL shared across all Passes/SubShaders in the file |
| `#pragma vertex <fn>` | Declares the vertex shader entry function (required) |
| `#pragma fragment <fn>` | Declares the fragment shader entry function (required) |
| `#pragma geometry / hull / domain <fn>` | Optional geometry/tessellation stage entry functions |
| `#pragma multi_compile <kw...>` | Keeps every listed variant in the build |
| `#pragma shader_feature <kw...>` | Strips variants unused by any material at build time |
| `#pragma dynamic_branch <kw...>` | One compiled program, runtime branch instead of separate variants |
| `#pragma skip_variants <kw...>` | Removes specific keywords/variants from compilation |
| `#pragma target <model>` | Minimum shader model (`2.0`…`5.0`, `4.5`, `4.6`, `gl4.1`) |
| `#pragma require <feature...>` | Minimum individual GPU features (`geometry`, `tessellation`, `compute`, `interpolators15`, …) |
| `#pragma only_renderers / exclude_renderers <api...>` | Include/exclude compilation for specific graphics APIs |
| `#pragma surface <fn> <lightModel>` | (BIRP only) Declares a Surface Shader entry point |
| `CBUFFER_START(name)` / `CBUFFER_END` | Groups uniform variables into a constant buffer (use `UnityPerMaterial` for SRP Batcher compatibility) |
| `TEXTURE2D(tex)` / `SAMPLER(s)` / `SAMPLE_TEXTURE2D(tex,s,uv)` | URP/SRP texture declaration and sampling macros |
| `sampler2D tex` / `tex2D(tex, uv)` | Legacy BIRP/Cg texture declaration and sampling |
| `TRANSFORM_TEX(uv, tex)` | Applies a texture's Tiling/Offset (`_ST` property) to a UV coordinate |
| **Semantic** | **Meaning** |
| `POSITION` | Object-space vertex position (`float3`/`float4`) |
| `NORMAL` | Vertex normal (`float3`) |
| `TANGENT` | Tangent vector for normal mapping (`float4`) |
| `COLOR` | Per-vertex color (`float4`) |
| `TEXCOORD0`…`TEXCOORD3` | UV/custom interpolator channels (`float2`/`float3`/`float4`) |
| `SV_POSITION` | Required output semantic for clip-space position from the vertex stage |
| `SV_Target` | Fragment shader color output |
| `SV_VertexID` | Per-vertex index (`uint`; not available in Surface Shaders) |
| `VPOS` / `UNITY_VPOS_TYPE` | Fragment-stage screen-space pixel position |
| `VFACE` | Fragment-stage front/back facing indicator (`float`) |

## Advanced Notes

**Shader variant stripping for build size.** URP shaders lean heavily on keywords, so an unaudited shader can compile into thousands of variants, each adding to build size and build time. To check actual variant counts, set a non-`Disabled` **Shader Variant Log Level** under *Project Settings > Graphics > URP > Additional Shader Stripping Settings*, enable **Development Build**, and build — Unity logs a per-shader `Total=<kept>/<total>(<percent>)` breakdown to the Console and `Editor.log` (search for `Shader Stripping`); enabling **Export Shader Variants** additionally writes `Temp/graphics-settings-stripping.json` and `Temp/shader-stripping.json`. To actually reduce the count: enable **Strip Unused Variants** in the same settings panel (URP then strips variants for features disabled across every URP/Universal Renderer asset in the build); disable unused features (post-processing effects, XR/VR modules) at the project-settings level rather than per-shader, since that's what lets the stripper prove a variant is unreachable; and disable individual shader keywords directly via **Shader Build Settings** in *Graphics settings* when you know a feature is unused project-wide. Also strip debug-only pragmas (`#pragma enable_d3d11_debug_symbols`) before a final build — they disable optimizations and bloat every affected variant. See `urp/shader-stripping-features.html` and `urp/shader-stripping-check.html`.

**`multi_compile` vs `shader_feature`, precisely.** Both declare a set of mutually-related keywords and compile one shader variant per combination, but they differ in *when* unused variants get discarded. `multi_compile` variants are always included in the build — use it for keywords toggled by script/runtime logic (quality tiers, platform capability keywords like `UNITY_DEVICE_SUPPORTS_NATIVE_16BIT`) where you can't know ahead of time which variant a given build needs. `shader_feature` variants are stripped at build time unless some material in the build actually references that keyword combination — use it for keywords exposed as material-authored toggles (e.g. a `[Toggle]` property), since most projects only ever use a handful of the possible combinations. Both directives support `_local` suffixes (e.g. `shader_feature_local`) to scope the keyword to the containing shader only rather than declaring it globally, which further reduces global keyword pressure — a shader is limited to roughly 128 total keywords (4 of which Unity reserves) before performance in the Editor degrades. `dynamic_branch` is a third option that compiles a single program with a true runtime branch instead of separate variants — no variant explosion at all, at the cost of a per-fragment/vertex branch; prefer it for keywords that change value frequently per-frame or per-object where variant-count reduction matters more than avoiding a branch. Use `#pragma skip_variants <keyword...>` to prune specific variants out of a built-in keyword shortcut set (e.g. `multi_compile_fwdadd` plus `skip_variants POINT POINT_COOKIE`) when you know some lighting combinations never occur in your project.

---
name: unity-rendering-pipelines
description: Use when choosing or configuring a Unity render pipeline (Built-in, URP, HDRP), authoring Shader Graph shaders, setting up materials/lighting, or configuring post-processing Volumes. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Rendering Pipelines

Pipeline choice, shader authoring, and lighting setup change materially between Unity versions. **Prefer the local docs over pre-trained knowledge** for pipeline APIs, node names, and Volume override lists. Hand-written HLSL/ShaderLab authoring is covered by the sibling skill `unity-shaders-hlsl` — this skill covers pipeline choice, Shader Graph, materials, lighting, and Volumes only.

## Retrieval Sources

Docs root: `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/Manual/` (all paths below are relative to this root unless marked ScriptReference). Every path in this table was verified with `ls`/`find` against the local Unity 6.3 LTS docs before being cited here.

| Source | Path | Use for |
|--------|------|---------|
| Pipeline landing/overview | `render-pipelines.html`, `render-pipelines-overview.html`, `choose-a-render-pipeline-landing.html` | Entry points into the pipeline docs tree |
| Pipeline choice | `choose-a-render-pipeline.html`, `render-pipelines-feature-comparison.html` | Deciding Built-in vs URP vs HDRP; per-feature support matrix |
| Pipeline setup | `render-pipelines-set-up.html`, `srp-setting-render-pipeline-asset.html` | Installing an SRP package, assigning the Render Pipeline Asset in Graphics Settings |
| SRP concept | `scriptable-render-pipeline-introduction.html`, `com.unity.render-pipelines.core.html` | What an SRP is; the shared Core RP Library both URP and HDRP build on |
| Built-in RP | `built-in-render-pipeline.html`, `creating-cookies-built-in-render-pipeline.html` | Legacy pipeline reference, only relevant for maintenance projects |
| URP overview | `universal-render-pipeline.html`, `com.unity.render-pipelines.universal.html` | URP package scope, supported platforms |
| HDRP overview | `high-definition-render-pipeline.html`, `com.unity.render-pipelines.high-definition.html` | HDRP scope; note this local doc set only carries these two HDRP pages — HDRP workflow detail (Diffusion Profiles, Path Tracing, Volumetric Fog specifics) is thinner locally than URP, verify against Unity's hosted HDRP manual before asserting details not present here |
| XR pipeline compatibility | `xr-render-pipeline-compatibility.html` | Which pipeline/feature combos work with XR |
| Shader Graph | `shader-graph.html`, `com.unity.shadergraph.html` | Node-based shader authoring, package scope |
| Shader Graph preferences | `preferences-shader-graph.html` | Editor color-mode / node display settings |
| Shader Graph on Terrain | `terrain-shader-graph.html` | Terrain Lit target authored via Shader Graph |
| Shader Graph + VFX | `svt-use-in-shader-graph.html` | Sampling VFX target output textures inside Shader Graph |
| Lighting modes | `lighting-mode.html`, `lighting-mode-runtime.html`, `lighting-mode-landing.html`, `choose-a-lighting-setup.html` | Baked/Realtime/Mixed/Shadowmask, runtime switching, decision guidance |
| Direct/indirect lighting concept | `direct-and-indirect-lighting.html`, `LightingInUnity.html`, `LightingOverview.html` | Core GI vocabulary before touching bake settings |
| GI configuration | `global-illumination-configure.html`, `lighting-configuration-workflow.html` | Lighting window settings, Lightmapper choice (Progressive CPU/GPU) |
| Lightmapping workflow | `Lightmapping.html`, `Lightmapping-bake.html`, `Lightmapping-baking-before-runtime.html`, `Lightmapping-configure.html` | Baking process, precomputed vs runtime-baked data |
| Lightmap UVs | `LightingGiUvs.html`, `LightingGiUvs-GeneratingLightmappingUVs.html`, `LightingGiUvs-Reference.html` | UV2 generation, causes of blotchy/seamed bakes |
| Realtime GI (Enlighten) | `realtime-gi-using-enlighten.html`, `realtime-gi-using-enlighten-use.html`, `realtime-gi-using-enlighten-optimize.html` | Legacy realtime GI system (Built-in RP only) |
| Lighting window / Explorer | `lighting-window.html`, `LightingExplorer.html`, `LightExplorerExtension.html` | Editor UI for inspecting/bulk-editing lights and bake settings |
| Light Probes | `LightProbes.html`, `LightProbes-Reference.html`, `LightProbes-Placing-Scripting.html`, `LightProbes-TechnicalInformation.html`, `light-probes-troubleshooting.html` | Manual probe placement, scripting probe positions, interpolation math |
| Light Probe Proxy Volume | `LightProbeProxyVolume-landing.html`, `class-LightProbeProxyVolume.html`, `class-LightProbeProxyVolume-configure.html` | Per-texel probe sampling for large dynamic renderers (Built-in RP) |
| Adaptive Probe Volumes (URP) | `urp/probevolumes.html`, `urp/probevolumes-concept.html`, `urp/probevolumes-use.html`, `urp/probevolumes-usebakingsets.html` | URP's automatic-placement probe system, Baking Sets, advantages vs Light Probe Groups |
| APV tuning/troubleshooting | `urp/probevolumes-changedensity.html`, `urp/probevolumes-streaming.html`, `urp/probevolumes-skyocclusion.html`, `urp/probevolumes-troubleshoot-artefacts.html`, `urp/probevolumes-troubleshoot-light-leaks.html` | Density/streaming config, sky occlusion, light-leak fixes |
| Reflection Probes | `class-ReflectionProbe.html`, `ReflectionProbes.html`, `AdvancedRefProbe.html`, `RefProbeTypes.html`, `RefProbePerformance.html`, `UsingReflectionProbes.html` | Baked/realtime/custom probe types, performance cost |
| Reflection Probes (URP) | `urp/lighting/reflection-probes.html`, `urp/lighting/reflection-probes-introduction.html`, `urp/lighting/reflection-probes-troubleshooting.html` | URP-specific reflection probe setup and blending |
| URP lighting overview | `urp/lighting-landing.html`, `urp/lighting/lighting-in-urp.html`, `urp/light-component.html`, `urp/universal-additional-light-data.html`, `urp/lighting/light-limits-in-urp.html` | Light component in URP, per-pixel/per-vertex limits, Forward+ light counts |
| Volumes core workflow | `urp/Volumes.html`, `urp/volumes-landing-page.html`, `urp/set-up-a-volume.html`, `urp/Volume-Profile.html` | Adding a Volume component, creating/sharing a Volume Profile asset, global vs local |
| Volume overrides reference | `urp/VolumeOverrides.html`, `urp/volume-component-reference.html` | Full list of override components and their parameters |
| Volumes troubleshooting | `urp/volumes-troubleshooting.html` | Why an override isn't applying (weight, layer mask, blend distance) |
| Post-processing setup | `urp/add-post-processing.html`, `urp/post-processing-in-urp.html`, `urp/post-processing-and-full-screen-effects-urp.html`, `post-processing-and-full-screen-effects.html` | End-to-end steps: URP Renderer Data asset, Post Processing checkbox, Volume, Profile |
| Post-processing effect matrix | `post-processing-effect-availability-reference.html` | Which overrides exist on which pipeline |
| Bloom / Tonemapping | `urp/post-processing-bloom.html`, `urp/post-processing-tonemapping.html` | Bloom override params; ACES/Neutral/None tonemapper modes |
| Color Adjustments / Curves / Lookup | `urp/Post-Processing-Color-Adjustments.html`, `urp/Post-Processing-Color-Curves.html`, `urp/post-processing-color-lookup.html` | Exposure/contrast/saturation, tone curves, LUT-based grading |
| Split Toning / White Balance / Channel Mixer / Shadows-Midtones-Highlights | `urp/Post-Processing-Split-Toning.html`, `urp/Post-Processing-White-Balance.html`, `urp/Post-Processing-Channel-Mixer.html`, `urp/Post-Processing-Shadows-Midtones-Highlights.html` | Full color-grading override set |
| Vignette / Chromatic Aberration / Lens Distortion / Panini / Film Grain | `urp/post-processing-vignette.html`, `urp/post-processing-chromatic-aberration.html`, `urp/Post-Processing-Lens-Distortion.html`, `urp/Post-Processing-Panini-Projection.html`, `urp/Post-Processing-Film-Grain.html` | Stylistic/camera-artifact overrides |
| Depth of Field / Motion Blur | `urp/depth-of-field-volume-override.html`, `urp/depth-of-field-volume-override-reference.html`, `urp/Post-Processing-Motion-Blur.html` | Camera focus and motion-blur overrides |
| SSAO renderer feature | `urp/post-processing-ssao.html`, `urp/add-ssao-renderer-feature-to-renderer.html`, `urp/ssao-renderer-feature-reference.html` | Screen-space ambient occlusion, added as a Renderer Feature not a Volume override |
| Custom post-processing | `urp/post-processing/custom-post-processing.html`, `urp/post-processing/custom-post-processing-with-volume.html`, `urp/post-processing/post-processing-custom-effect-low-code.html` | Writing a custom full-screen effect driven by a Volume override |
| HDR output | `urp/post-processing/hdr-output.html`, `urp/post-processing/enable-hdr-output-urp.html`, `urp/post-processing/hdr-in-urp.html` | HDR display output pipeline, debug views |
| SRP Batcher | `SRPBatcher.html`, `SRPBatcher-landing.html`, `SRPBatcher-Enable.html`, `SRPBatcher-Materials.html`, `SRPBatcher-Incompatible.html`, `SRPBatcher-Profile.html`, `urp/shaders-in-universalrp-srp-batcher.html` | Draw-call batching mechanism, enabling, shader/material compatibility rules, profiling, intentionally opting out |
| GPU Resident Drawer | `urp/gpu-resident-drawer.html` | BatchRendererGroup-based GPU instancing drawer; Forward+-only, enable steps, analysis tools |
| Renderer Features | `urp/urp-renderer-feature.html`, `urp/urp-renderer-feature-landing.html`, `urp/renderer-features/intro-to-scriptable-render-passes.html`, `urp/renderer-features/custom-rendering-pass-workflow-in-urp.html` | Extending the URP Renderer with custom render passes |
| Built-in Renderer Features | `urp/renderer-feature-decal.html`, `urp/renderer-feature-decal-reference.html`, `urp/renderer-feature-screen-space-shadows.html` | Decal projector and screen-space shadows features shipped with URP |
| Shadows in URP | `urp/Shadows-in-URP.html`, `urp/shadow-resolution-urp.html`, `urp/shadow-cascades-visualize.html`, `urp/shadows-troubleshooting-urp.html` | Cascade shadow maps, resolution tiers, troubleshooting shadow acne/peter-panning |
| Custom lighting in URP | `urp/lighting/custom-lighting-introduction.html`, `urp/lighting/custom-lighting-change-light-falloff.html` | Overriding URP's default light attenuation/falloff model |
| Lighting troubleshooting | `ts-no-baked-global-illum.html`, `ts-objects-missing-lighting.html`, `light-probes-troubleshooting.html` | Common GI/lighting failure symptoms and fixes |

**ScriptReference note (verified):** the core `Rendering.Volume` / `Rendering.VolumeProfile` / `Rendering.VolumeComponent` scripting API is shipped inside the URP/HDRP packages, not the base `ScriptReference/` tree in this doc set — a search for `VolumeProfile` and `Rendering.Volume` in `ScriptReference/` returned no hits. `RenderPipelineAsset`, `GraphicsSettings.currentRenderPipeline`, `ReflectionProbe`, and `LightProbes`/`LightmapSettings` **are** present in `ScriptReference/` (e.g. `Rendering.RenderPipelineAsset-defaultMaterial.html`, `Rendering.GraphicsSettings-currentRenderPipeline.html`, `ReflectionProbe.html`, `LightmapSettings.html`). When writing Volume-manipulation C# code, cross-check method signatures against the installed URP/HDRP package's API docs (Window > Package Manager > in-package documentation) rather than assuming they live in this Manual/ScriptReference set.

## Key Guidelines

### Choosing a Pipeline

Pipeline choice is a project-founding decision, not a setting to revisit casually — `choose-a-render-pipeline.html` and `render-pipelines-feature-comparison.html` are the two pages to read before starting any new project. Built-in Render Pipeline is legacy and receives maintenance fixes only; only pick it for an already-existing Built-in project or a very small scope where migration cost isn't worth it. URP (`universal-render-pipeline.html`) is a single Scriptable Render Pipeline that scales from mobile/VR up through mid-range/high-end desktop and console via one Renderer asset with swappable Renderer Features — it is the default choice for the overwhelming majority of new projects because of its platform reach. HDRP (`high-definition-render-pipeline.html`) targets high-end desktop/console exclusively, trading platform reach for physically based, high-fidelity rendering (volumetrics, path tracing, advanced material models). Check `xr-render-pipeline-compatibility.html` before committing to HDRP for any XR project — support is narrower than URP's. All three pipelines are mutually exclusive at the project-asset level: exactly one Render Pipeline Asset is active at a time (assigned in Graphics Settings, see `srp-setting-render-pipeline-asset.html`), and materials authored for one pipeline do not render correctly under another without running the material upgrader (Edit > Render Pipeline > Upgrade). Switching pipelines mid-project means re-authoring or upgrading every material and shader in the project — budget for this explicitly rather than treating it as a quick asset swap.

### Shader Graph Authoring

Shader Graph (`shader-graph.html`, `com.unity.shadergraph.html`) is Unity's node-based shader editor and the default authoring path for both URP and HDRP — it produces a `.shadergraph` asset that compiles to the same underlying shader variants as hand-written HLSL but is editable visually, previews live, and is approachable for artists/technical artists without a shader-language background. A graph targets a specific pipeline (URP or HDRP) and a specific Master Stack type (Lit, Unlit, Decal, Sprite, etc. — the exact list depends on pipeline and package version, verify current node/target names in `shader-graph.html` rather than assuming they match older Unity versions since Shader Graph's target/stack model has changed release to release). Subgraphs let you package a reusable node network as its own asset and drop it into other graphs as a single node — use them aggressively once a node cluster (e.g. a triplanar UV function, a hash/noise utility) is reused across more than one shader, so an update in one place propagates everywhere. Shader Graph is not a superset of hand-written HLSL: custom lighting models outside the built-in Lit/Unlit stacks, precise register-level control, and some advanced compute-adjacent patterns still require a Custom Function node dropping into HLSL or a fully hand-written shader — that authoring path (writing the HLSL/ShaderLab itself) is covered by the sibling skill `unity-shaders-hlsl`, not here. `preferences-shader-graph.html` controls only editor node-coloring/display preferences, not shader behavior.

### Materials

A material binds a shader (or Shader Graph asset) to a set of property values and is the unit that actually gets assigned to a Renderer. Because materials are pipeline-coupled through their shader, a material authored against the Built-in RP's Standard shader or a URP Lit shader will render as solid magenta/pink under a different active pipeline — this is Unity's visual signal for "shader not found/incompatible," not a lighting bug. The fix is Edit > Render Pipeline > [target pipeline] > Upgrade Project Materials, which remaps built-in shader references where an automatic mapping exists; materials using custom shaders need manual reassignment. When authoring materials for the SRP Batcher (see Performance below), keep per-material property overrides that vary at runtime to material-property-block usage patterns documented in `SRPBatcher-Materials.html` rather than mutating shared material instances per-object, since per-instance shared-material mutation defeats batching and leaks state across objects using the same material asset.

### Lighting & GI

Unity's lighting modes trade runtime cost against flexibility, and the choice is made per-Light via the Mode property plus project-wide Mixed Lighting settings (`lighting-mode.html`, `choose-a-lighting-setup.html`): **Baked** indirect and direct lighting costs nothing at runtime but is fully static — moving a baked light or object at runtime does not update the bake. **Realtime** lighting updates every frame (direct always; indirect only if a realtime GI system such as Enlighten or Adaptive Probe Volumes with runtime updates is in use) at a continuous runtime cost. **Mixed** is the common default: indirect lighting is baked once (cheap, and gives static objects high-quality bounce light) while direct lighting stays dynamic (so shadows and direct illumination respond to moving lights/objects); Mixed further subdivides into Baked Indirect, Shadowmask, and Distance Shadowmask sub-modes, documented in `lighting-mode.html` and `lighting-mode-runtime.html`, which change how runtime shadows for baked-lit dynamic objects are resolved. Baking itself runs through the Lighting window (`lighting-window.html`, `global-illumination-configure.html`) using a Lightmapper (Progressive CPU or Progressive GPU) and writes lightmap textures plus, if Adaptive Probe Volumes or Light Probes are present, probe data. Baked lightmap quality depends on lightmap UVs (a second UV channel, UV2) that don't overlap and have reasonable texel margins — enable "Generate Lightmap UVs" on the mesh's Model Import Settings, or hand-author UV2 for hero assets; `LightingGiUvs.html` and `LightingGiUvs-GeneratingLightmappingUVs.html` cover the generation parameters (Hard Angle, Pack Margin, etc.) and are the first stop when a bake looks blotchy or seamed. Realtime GI via Enlighten (`realtime-gi-using-enlighten.html`) is a Built-in-RP-era system; URP's realtime-updating indirect lighting path is Adaptive Probe Volumes with runtime updates enabled, not Enlighten.

### Reflection & Light Probes

Dynamic (non-lightmapped) objects need a separate mechanism to receive plausible ambient/reflected lighting, since they have no lightmap UVs. **Light Probes** (`LightProbes.html`, `LightProbes-Reference.html`) sample baked spherical-harmonics indirect lighting at fixed points in the scene and interpolate between the nearest probes per-object (or, with a Light Probe Proxy Volume, per-texel — `LightProbeProxyVolume-landing.html` — useful for large dynamic renderers like terrain vehicles where per-object interpolation looks wrong). URP's newer alternative is **Adaptive Probe Volumes (APV)** (`urp/probevolumes-concept.html`, `urp/probevolumes.html`): Unity auto-places probes based on scene geometry density instead of requiring manual Light Probe Group placement, samples per-pixel rather than per-GameObject (better consistency, fewer seams between adjacent objects), supports streaming for open-world scenes, and supports blending between multiple baked lighting states via Baking Sets (`urp/probevolumes-usebakingsets.html`) — trade-offs are documented in the feature-comparison table inside `urp/probevolumes-concept.html` (APV: no manual placement, yes streaming, yes multi-bake blending; Light Probe Groups: manual placement only, no streaming, no blending). **Reflection Probes** (`class-ReflectionProbe.html`, `ReflectionProbes.html`, `RefProbeTypes.html`) capture a cubemap of the surrounding environment for use as specular/reflection input on nearby objects; probe Type is Baked (static, cheapest), Realtime (updates at runtime, most expensive — avoid unless the reflected environment actually changes), or Custom (a manually assigned cubemap, cheapest but static and hand-authored). `RefProbePerformance.html` is the reference for realtime probe cost before enabling more than a handful in a scene, and URP has its own reflection-probe setup/troubleshooting pages under `urp/lighting/reflection-probes.html` and `urp/lighting/reflection-probes-troubleshooting.html`.

### Post-Processing Volumes

Post-processing in URP/HDRP is entirely Volume-driven — there is no per-camera effect stack outside this system. The chain is: a Volume component (Global, or Local with a trigger Collider) → a Volume Profile asset assigned to it → one or more Override components added to that profile (Bloom, Tonemapping, Color Adjustments, Vignette, Depth of Field, etc. — the full list is in `urp/VolumeOverrides.html` / `urp/volume-component-reference.html`). No Volume in the scene, no Profile assigned, an override left disabled, or an override's blend **weight left at 0** are the four ways an override silently does nothing (`urp/volumes-troubleshooting.html`); a Global Volume with weight 1 is the simplest working baseline. Local Volumes participate in a distance-weighted blend as the camera crosses their trigger Collider's bounds and Blend Distance — multiple overlapping Volumes with different Layer/priority let you build region-specific looks (e.g. an underwater color grade) that blend smoothly rather than popping. Setting this up from C# (`urp/Volume-Profile.html`, note the ScriptReference caveat above — these types ship in the URP package):

```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

public class RuntimeVolumeSetup : MonoBehaviour
{
    void Start()
    {
        var volume = gameObject.AddComponent<Volume>();
        volume.isGlobal = true;
        volume.weight = 1f;

        var profile = ScriptableObject.CreateInstance<VolumeProfile>();
        if (profile.TryAdd(out Bloom bloom))
        {
            bloom.intensity.overrideState = true;
            bloom.intensity.value = 0.6f;
        }
        if (profile.TryAdd(out Tonemapping tonemap))
        {
            tonemap.mode.overrideState = true;
            tonemap.mode.value = TonemappingMode.ACES;
        }
        volume.profile = profile;
    }
}
```

Every override parameter has its own `overrideState` bool that must be set true for the value to actually apply when blending — a common bug is setting `.value` without setting `.overrideState`, which leaves the parameter inert. The individually documented override pages (Bloom `urp/post-processing-bloom.html`, Tonemapping `urp/post-processing-tonemapping.html`, Color Adjustments/Curves/Lookup, Split Toning, White Balance, Channel Mixer, Shadows-Midtones-Highlights for grading; Vignette, Chromatic Aberration, Lens Distortion, Panini Projection, Film Grain for stylization; Depth of Field, Motion Blur for camera/lens simulation) each list their own parameter set and valid ranges — check the specific page rather than guessing values, since ranges and defaults differ per override. SSAO is the one common "post-processing-looking" effect that is **not** a Volume override — it's added as a Renderer Feature on the URP Renderer asset (`urp/add-ssao-renderer-feature-to-renderer.html`), because it needs access to renderer-internal depth/normal buffers before the Volume stack runs. `post-processing-effect-availability-reference.html` lists which overrides exist on which pipeline before assuming HDRP/URP feature parity.

### Performance (SRP Batcher / GPU Resident Drawer)

The **SRP Batcher** (`SRPBatcher.html`) reduces per-object CPU overhead by caching per-material GPU state and issuing a lighter-weight draw-call setup for consecutive objects sharing SRP Batcher–compatible shaders, without requiring identical materials the way GPU instancing does. Enable it in the Render Pipeline Asset inspector (`SRPBatcher-Enable.html`) — in Unity 6 it's on by default for new URP/HDRP projects. Compatibility is per-shader: a shader must declare its per-material properties inside a single `CBUFFER` matching the SRP Batcher's expected layout (`SRPBatcher-Materials.html`); shaders that don't (including some custom/legacy shaders) silently fall back to the older batching path with no error, so verify actual batching via the Frame Debugger or the dedicated SRP Batcher profiling view (`SRPBatcher-Profile.html`) rather than assuming it's active. You can intentionally opt a GameObject out of SRP Batcher compatibility (`SRPBatcher-Incompatible.html`) — either by making the shader incompatible or the renderer incompatible — specifically when you want GPU instancing instead (via `Graphics.RenderMeshInstanced` or per-material "Enable GPU Instancing"), because GPU instancing and the SRP Batcher are alternative, not additive, batching strategies for the same draw calls; profile both before choosing. The **GPU Resident Drawer** (`urp/gpu-resident-drawer.html`) is a newer, more automatic layer on top of `BatchRendererGroup` that GPU-instances MeshRenderer GameObjects with no per-object opt-in required, but has hard requirements verified from the doc: it only works with the **Forward+** rendering path, requires a compute-shader-capable Graphics API/platform (excludes OpenGL ES), and only accelerates GameObjects with a Mesh Renderer (others silently fall back to normal drawing). Enabling it costs build time (all BatchRendererGroup shader variants get compiled in) and, when both Forward+ and the GPU Resident Drawer are active, switches Probe Atlas Blending on by default. Enable steps per the doc: Project Settings > Graphics > Shader Stripping > set BatchRendererGroup Variants to Keep All; confirm SRP Batcher is enabled on the active URP asset; set GPU Resident Drawer to Instanced Drawing on the URP asset; set the Universal Renderer's Rendering Path to Forward+. Analyze results via the Frame Debugger (batched draws appear grouped under "Hybrid Batch Group"), the Rendering Debugger, Rendering Statistics (FPS / CPU time / SetPass call count), and the Profiler — it pays off most on large scenes with many GameObjects sharing meshes, and less in Scene/Game view than in an actual Play/build target.

## Common Mistakes

| Mistake | Why / fix |
|---|---|
| Pink/magenta materials after a pipeline switch | Shader built for the wrong SRP; run Edit > Render Pipeline > [target] > Upgrade Project Materials, or reassign a pipeline-correct shader |
| Post-processing has no visible effect | No Volume in scene, Profile not assigned, override component disabled, or override weight/blend weight is 0; see `urp/volumes-troubleshooting.html` |
| Volume override parameter set in C# but not applying | Set `.value` without setting the parameter's `overrideState = true`; both must be set for the blend to pick it up |
| Baked lighting looks blotchy or seamed | Missing/overlapping lightmap UVs (UV2); enable "Generate Lightmap UVs" on mesh import or hand-unwrap; see `LightingGiUvs.html` |
| Draw calls higher than expected | SRP Batcher off, or shader isn't SRP-Batcher-compatible (missing per-material CBUFFER layout) and silently fell back; check Frame Debugger and `SRPBatcher-Materials.html` |
| GPU Resident Drawer enabled but nothing is batching | Rendering Path isn't Forward+, platform/Graphics API doesn't support compute shaders (e.g. OpenGL ES), or the GameObject lacks a Mesh Renderer — all documented hard requirements in `urp/gpu-resident-drawer.html` |
| SRP Batcher and GPU instancing fighting each other | They're alternative batching strategies, not additive; a shader/material can't benefit from both on the same draw simultaneously — profile which one actually wins for the workload before enabling both |
| Dynamic objects have flat/wrong ambient lighting | No Light Probes or Adaptive Probe Volume covering that area; static-only lighting setups leave moving objects unlit by indirect light |
| Realtime Reflection Probes tanking framerate | Realtime probes re-render the scene from the probe's position every update; default to Baked or Custom probes and reserve Realtime for probes whose environment genuinely changes, per `RefProbePerformance.html` |
| SSAO added as a Volume override and it does nothing | SSAO is a URP Renderer Feature, not a Volume override — add it via the Renderer asset's Renderer Features list, not the Volume Profile |
| HDRP running on a target it doesn't support | HDRP requires compute-shader-capable, high-end desktop/console-class GPUs; check `xr-render-pipeline-compatibility.html` and the feature comparison before targeting mobile/VR/low-end with HDRP |
| Choosing Shader Graph nodes/targets from memory of an older Unity version | Shader Graph's Master Stack/target model has changed release to release; verify current node and target names against the local `shader-graph.html` rather than assuming parity with a remembered older version |
| Mixing Light Probe Groups and Adaptive Probe Volumes without understanding the trade-off | APV auto-places probes, supports streaming and multi-bake blending but not manual placement; Light Probe Groups are the reverse; pick per the comparison table in `urp/probevolumes-concept.html`, don't default to whichever is familiar |
| Assuming Volume/VolumeProfile C# API lives in core ScriptReference | These types ship inside the URP/HDRP package, not the base Unity ScriptReference set; consult the installed package's own API docs for exact method signatures |
| Treating "Mixed" as a single lighting mode | Mixed lighting has sub-modes (Baked Indirect, Shadowmask, Distance Shadowmask) that change how dynamic-object shadows resolve against baked lighting; picking the wrong sub-mode gives incorrect shadow behavior on dynamic objects near baked-lit static geometry |

## Quick Reference

| Asset/Feature | Purpose | Where configured |
|---|---|---|
| Render Pipeline Asset | Project-wide graphics settings, one active per project | Project Settings > Graphics > Scriptable Render Pipeline Settings |
| URP Renderer Data asset | Per-camera render pass list, Renderer Features, rendering path (Forward/Forward+/Deferred) | Assigned on the URP Render Pipeline Asset |
| Renderer Feature | Extensible custom/built-in render pass plugged into a Renderer (e.g. Decal, SSAO, Screen Space Shadows) | Renderer Data asset inspector |
| Shader Graph asset | Node-based shader, compiles to a pipeline-specific Master Stack target | Create > Shader Graph |
| Subgraph | Reusable node network embedded as a single node in other graphs | Create > Shader Graph > Sub Graph |
| Material | Binds a shader/Shader Graph + property values to a Renderer | Create > Material |
| Volume | Scene component that carries a Volume Profile, Global or Local (trigger Collider) | Add Component > Volume |
| Volume Profile | Reusable asset holding a set of Overrides | Create > Volume Profile, or auto-created on a Volume |
| Volume Override | Individual effect settings block (Bloom, Tonemapping, Color Adjustments, Vignette, DoF, Motion Blur, etc.) | Add Override inside a Volume Profile |
| Light Probe Group | Manually placed probe set for baked indirect lighting on dynamic objects | Add Component > Light Probe Group |
| Light Probe Proxy Volume | Per-texel probe sampling for large dynamic renderers | Add Component > Light Probe Proxy Volume |
| Adaptive Probe Volume (APV) | Auto-placed, streamable, per-pixel indirect lighting probes (URP) | Add Component > Adaptive Probe Volume; configure via Baking Sets |
| Reflection Probe | Captured cubemap for specular/reflection approximation; Baked/Realtime/Custom | Add Component > Reflection Probe |
| Lighting Mode | Per-Light Baked/Realtime/Mixed setting; Mixed has Baked Indirect / Shadowmask / Distance Shadowmask sub-modes | Light component inspector + Lighting window (Mixed Lighting section) |
| Lightmapper | Progressive CPU or Progressive GPU bake engine | Lighting window > Scene tab |
| Lightmap UV (UV2) | Second UV channel required for non-overlapping lightmap texel packing | Model Import Settings > Generate Lightmap UVs, or hand-authored |
| SRP Batcher | Reduces per-object CPU draw-call setup cost for compatible shaders | Render Pipeline Asset > Rendering (toggle) |
| GPU Resident Drawer | BatchRendererGroup-based automatic GPU instancing (Forward+ only) | Render Pipeline Asset > Rendering, + Renderer's Rendering Path = Forward+ |
| GPU Instancing | Per-material draw-call batching for identical meshes, alternative to SRP Batcher | Material inspector checkbox, or `Graphics.RenderMeshInstanced` |
| Rendering Path | Forward / Forward+ / Deferred, set per URP Renderer | Universal Renderer asset inspector |
| Frame Debugger | Inspect per-draw-call state, batching groups ("Hybrid Batch Group" for GPU Resident Drawer) | Window > Analysis > Frame Debugger |
| Rendering Debugger | Runtime visualization of rendering internals (probes, batching, overdraw, etc.) | Window > Analysis > Rendering Debugger |

## Advanced Notes

**HDRP hardware/platform requirements.** HDRP targets compute-shader-capable GPUs on high-end desktop and console only; it is not a viable target for mobile or most VR hardware — always check `render-pipelines-feature-comparison.html` and `xr-render-pipeline-compatibility.html` per-target before scoping an HDRP project. This local doc set carries only two HDRP-specific Manual pages (`high-definition-render-pipeline.html`, `com.unity.render-pipelines.high-definition.html`); deep HDRP topics (Diffusion Profiles, Path Tracing, Volumetric Fog/Clouds, Decal Projector specifics, Camera physical settings) are not present here in the depth URP enjoys in this archive — treat pretrained knowledge as a starting point for those topics but flag it as unverified against local docs, and prefer Unity's hosted HDRP manual for anything beyond pipeline-choice-level detail.

**Custom render passes / Renderer Features.** Beyond the built-in Renderer Features (Decal, SSAO, Screen Space Shadows), URP supports fully custom render passes via `ScriptableRendererFeature` + `ScriptableRenderPass`, documented starting at `urp/renderer-features/intro-to-scriptable-render-passes.html` and walked through end-to-end in `urp/renderer-features/custom-rendering-pass-workflow-in-urp.html`. This is the mechanism for injecting custom full-screen or per-object passes into the URP frame (e.g. outline effects, custom depth/normal passes, a custom post-processing effect that needs to run at a specific injection point) without forking the Universal Renderer source. `urp/post-processing/custom-post-processing-with-volume.html` specifically covers wiring a custom Renderer Feature pass to react to Volume override parameters, which is the standard pattern for shipping a custom post-processing effect that fits into the existing Volume-blending workflow rather than bypassing it.

**SRP Batcher vs GPU instancing vs GPU Resident Drawer — picking one.** These three sit at different levels and aren't simply "pick the best one": the SRP Batcher reduces CPU per-object state-change cost for shader-compatible draws regardless of whether meshes/materials are identical; GPU instancing collapses many draws of the *same* mesh+material into one GPU-side call and is mutually exclusive with SRP Batcher per-draw; the GPU Resident Drawer is a higher-level automatic system built on `BatchRendererGroup` that effectively generalizes instancing-style batching across Forward+ scenes without manual per-object setup, at the cost of longer build times and a hard Forward+/compute-shader requirement. For a new Forward+ URP project on supported hardware, enabling both SRP Batcher and the GPU Resident Drawer together is the documented recommended baseline; GPU instancing remains the right manual tool specifically for large counts of one mesh+material where you're intentionally opting out of SRP Batcher compatibility.

**Baking Sets and multi-scene APV.** Adaptive Probe Volumes bake per Baking Set (`urp/probevolumes-usebakingsets.html`), which groups scenes that share a probe-lighting bake — this is the mechanism for supporting multiple lighting states (e.g. day/night) or multi-scene open-world streaming setups without re-baking probes per scene load. Sky Occlusion (`urp/probevolumes-skyocclusion.html`) additionally lets APV update sky-contributed indirect light at runtime without a full re-bake, which Light Probe Groups cannot do at all.

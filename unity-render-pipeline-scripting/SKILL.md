---
name: unity-render-pipeline-scripting
description: Use when writing low-level rendering code — CommandBuffer, ScriptableRenderPass/Renderer Features, RenderPipelineManager callbacks, or compute shaders/ComputeBuffer/GraphicsBuffer. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Render Pipeline Scripting

This skill covers the C# API surface that *drives* rendering rather than the assets/nodes that describe *what* to render. **Prefer the local docs over pre-trained knowledge**, especially for the render graph API (`RecordRenderGraph`, `TextureHandle`, `AddRasterRenderPass`/`AddComputePass`), which is new relative to the older Execute/RTHandle pattern many tutorials and pretrained knowledge still assume. Pipeline choice, Shader Graph, materials, lighting, and Volumes are owned by the sibling skill `unity-rendering-pipelines` — not duplicated here. Hand-written `.shader`/HLSL authoring (vertex/fragment code, ShaderLab blocks, keywords) is owned by `unity-shaders-hlsl` — not duplicated here. This skill is specifically about the C# that issues GPU commands and injects custom passes: `CommandBuffer`, `ScriptableRenderPass`/`ScriptableRendererFeature`, `RenderPipelineManager` callbacks, `ComputeShader`/`ComputeBuffer`/`GraphicsBuffer` dispatch from script, and `RenderTexture` management.

## Retrieval Sources

Docs root: `/media/artiq/FRESH_DRIVE/Books/Unity6/Documentation/en/` — `Manual/` and `ScriptReference/` subtrees, as noted per row. Every path below was verified with `find`/`ls` against the local Unity 6.3 LTS docs before being cited here.

| Source | Path | Use for |
|--------|------|---------|
| CommandBuffer fundamentals (BIRP) | `Manual/GraphicsCommandBuffers.html`, `Manual/GraphicsCommandBuffers-landing.html` | What a CommandBuffer is; `Camera.AddCommandBuffer`/`Light.AddCommandBuffer` + `CameraEvent`/`LightEvent` scheduling model (Built-in RP framing, but the CommandBuffer object itself is the same type used inside URP passes) |
| CameraEvent/LightEvent ordering | `Manual/GraphicsCommandBuffers-order.html` | Execution order reference for BIRP-scheduled command buffers |
| CommandBuffer constructor & core methods | `ScriptReference/Rendering.CommandBuffer-ctor.html`, `ScriptReference/Rendering.CommandBuffer.Clear.html`, `ScriptReference/Rendering.CommandBuffer.SetRenderTarget.html` (verify further members via search — 149 CommandBuffer-prefixed pages exist) | Constructing and populating a buffer of GPU commands |
| CommandBuffer draw commands | `ScriptReference/Rendering.CommandBuffer.DrawMesh.html`, `ScriptReference/Rendering.CommandBuffer.DrawMeshInstanced.html`, `ScriptReference/Rendering.CommandBuffer.DrawMeshInstancedIndirect.html`, `ScriptReference/Rendering.CommandBuffer.DrawProcedural.html`, `ScriptReference/Rendering.CommandBuffer.DrawRenderer.html`, `ScriptReference/Rendering.CommandBuffer.DrawRendererList.html` | Recording geometry draws into a command buffer |
| CommandBuffer texture/target commands | `ScriptReference/Rendering.CommandBuffer.Blit.html`, `ScriptReference/Rendering.CommandBuffer.ClearRenderTarget.html`, `ScriptReference/Rendering.CommandBuffer.CopyTexture.html`, `ScriptReference/Rendering.CommandBuffer.GenerateMips.html` | Blitting, clearing, copying render targets (see Common Mistakes re: URP-safe blitting) |
| CommandBuffer compute commands | `ScriptReference/Rendering.CommandBuffer.DispatchCompute.html`, `ScriptReference/Rendering.CommandBuffer.SetComputeBufferParam.html` | Dispatching a compute shader from inside a command buffer, e.g. in a render pass's `Execute`/render function |
| CommandBuffer render-pass/native RP commands | `ScriptReference/Rendering.CommandBuffer.BeginRenderPass.html`, `ScriptReference/Rendering.CommandBuffer.EndRenderPass.html` | Native render pass API (tile-based GPU optimization), used internally by URP's render graph execution |
| Graphics.ExecuteCommandBuffer | `ScriptReference/Graphics.ExecuteCommandBuffer.html`, `ScriptReference/Graphics.ExecuteCommandBufferAsync.html` | Running a command buffer immediately outside the camera-event scheduling model |
| Camera/Light command buffer attachment | `ScriptReference/Camera.AddCommandBuffer.html`, `ScriptReference/Camera.RemoveCommandBuffer.html`, `ScriptReference/Camera.RemoveAllCommandBuffers.html`, `ScriptReference/Light.AddCommandBuffer.html` | Scheduling a buffer to run at a `CameraEvent`/`LightEvent` (BIRP path) |
| Scriptable Render Pass concept | `Manual/urp/renderer-features/intro-to-scriptable-render-passes.html` | What a Scriptable Render Pass is and why it exists (injection points, per-scene customization) |
| Custom render pass workflow overview | `Manual/urp/renderer-features/custom-rendering-pass-workflow-in-urp.html` | End-to-end map of the custom-pass authoring docs (render graph vs legacy) |
| Writing a render pass with the render graph system | `Manual/urp/render-graph-write-render-pass.html` | Full worked `PassData`/`RecordRenderGraph`/`AddRasterRenderPass`/`SetRenderFunc` example — the canonical Unity 6 pattern |
| Render graph introduction | `Manual/urp/render-graph-introduction.html` | Recording vs execution stages; automatic resource/pass optimization the render graph performs |
| Render graph: create/import textures | `Manual/urp/render-graph-create-a-texture.html`, `Manual/urp/render-graph-import-a-texture.html` | `TextureDesc`/`renderGraph.CreateTexture`, and `ImportTexture` for wrapping an existing `RenderTexture`/RTHandle as a `TextureHandle` |
| Render graph: frame data / camera history | `Manual/urp/render-graph-frame-data.html`, `Manual/urp/render-graph-frame-data-reference.html`, `Manual/urp/render-graph-add-texture-to-frame-data.html`, `Manual/urp/render-graph-add-textures-to-camera-history.html`, `Manual/urp/render-graph-get-previous-frames.html` | `ContextContainer`/`UniversalResourceData`/`UniversalCameraData`; sharing textures across passes/frames |
| Render graph: pass textures between passes / read-write | `Manual/urp/render-graph-pass-textures-between-passes.html`, `Manual/urp/render-graph-read-write-texture.html` | `builder.UseTexture`, `SetRenderAttachment`, read vs write access declarations |
| Render graph: draw objects in a pass | `Manual/urp/render-graph-draw-objects-in-a-pass.html` | `CreateRendererList`/`DrawRendererList` inside a raster pass |
| Render graph: blit | `Manual/urp/render-graph-blit.html`, `Manual/urp/customize/blit-overview.html` | `AddBlitPass`/`AddCopyPass`, `Blitter.BlitCameraTexture`; explicit warning against `CommandBuffer.Blit`/`Graphics.Blit`/`RenderingUtils.Blit` in URP |
| Render graph: compute shaders | `Manual/urp/render-graph-compute-shader.html`, `Manual/urp/render-graph-compute-shader-run.html`, `Manual/urp/render-graph-compute-shader-input.html` | `AddComputePass`, `ComputeGraphContext`, `BufferHandle`/`ImportBuffer`/`UseBuffer`, `DispatchCompute` inside the render graph |
| Render graph: unsafe pass | `Manual/urp/render-graph-unsafe-pass.html` | `AddUnsafePass`/`UnsafeCommandBuffer` — escape hatch for legacy-style commands the raster/compute pass APIs don't expose |
| Render graph: optimize / debugging | `Manual/urp/render-graph-optimize.html`, `Manual/urp/render-graph-view.html`, `Manual/urp/render-graph-viewer-reference.html` | `AllowPassCulling`, diagnosing why a pass/resource was culled or reordered, the Render Graph Viewer window |
| Render graph: framebuffer fetch | `Manual/urp/render-graph-framebuffer-fetch.html` | Tile-local framebuffer read (mobile bandwidth optimization) inside a render graph pass |
| Inject a pass with a Scriptable Renderer Feature | `Manual/urp/renderer-features/scriptable-renderer-features/inject-a-pass-using-a-scriptable-renderer-feature.html` | Full worked `ScriptableRendererFeature` example: `Create()`, `AddRenderPasses()`, `EnqueuePass`, `Dispose()` |
| Inject a pass via RenderPipelineManager | `Manual/urp/customize/inject-render-pass-via-script.html` | Full worked example subscribing to `RenderPipelineManager.beginCameraRendering` and calling `EnqueuePass` from a MonoBehaviour instead of a Renderer Feature |
| Injection points reference | `Manual/urp/customize/custom-pass-injection-points.html` | Complete `RenderPassEvent` enum table (Before/AfterRenderingShadows, Opaques, Skybox, Transparents, PostProcessing, etc.) with what's set up at each point |
| Restrict a pass to a scene area | `Manual/urp/customize/restrict-render-pass-scene-area.html` | Scoping a custom pass to specific cameras/volumes/layers rather than the whole frame |
| Modify URP source code | `Manual/urp/customize/modify-urp-source-code.html` | When/how to fork the URP package source itself, as a last resort beyond Renderer Features |
| Built-in Renderer Feature examples | `Manual/urp/renderer-features/renderer-feature-render-objects.html`, `Manual/urp/renderer-features/how-to-custom-effect-render-objects.html`, `Manual/urp/renderer-features/renderer-feature-full-screen-pass.html` | Worked examples of shipped Renderer Features (Render Objects, custom full-screen pass) as authoring references |
| RenderPipelineManager callbacks | `ScriptReference/Rendering.RenderPipelineManager.html`, `-beginCameraRendering.html`, `-endCameraRendering.html`, `-beginContextRendering.html`, `-endContextRendering.html`, `-beginFrameRendering.html`, `-endFrameRendering.html` | Per-camera/per-context/per-frame render loop hook points (delegate signatures, when each fires) |
| RenderPipelineManager pipeline-lifecycle events | `ScriptReference/Rendering.RenderPipelineManager-activeRenderPipelineAssetChanged.html`, `-activeRenderPipelineCreated.html`, `-activeRenderPipelineDisposed.html`, `-activeRenderPipelineTypeChanged.html`, `-pipelineSwitchCompleted.html`, `-currentPipeline.html` | Reacting to the active `RenderPipelineAsset`/pipeline type changing at runtime |
| ComputeShader manual overview | `Manual/class-ComputeShader-introduction.html`, `Manual/class-ComputeShader.html` | What a compute shader is; kernels, thread groups, GPGPU use cases |
| Creating a compute shader | `Manual/class-ComputeShader-create.html` | `.compute` asset creation, `#pragma kernel`, default template |
| Running a compute shader | `Manual/class-ComputeShader-run.html` | `ComputeShader.Dispatch`, pairing with `ComputeBuffer`, `RenderTexture.enableRandomWrite` for UAV write access |
| HLSL/ShaderLab specifics in compute shaders | `Manual/class-ComputeShader-hlsl-shaderlab.html` | Texture/sampler naming rules, keywords/variants, `CGPROGRAM`/`GLSLPROGRAM` platform-specific blocks |
| Compute shaders cross-platform | `Manual/class-ComputeShader-crossplatform.html` | Platform support matrix and portability caveats for compute across DX11/12, Metal, Vulkan, consoles |
| ComputeShaderImporter | `Manual/class-ComputeShaderImporter.html` | Import settings for `.compute` asset files |
| ComputeShader ScriptReference members | `ScriptReference/ComputeShader.html`, `ComputeShader.Dispatch.html`, `ComputeShader.FindKernel.html`, `ComputeShader.GetKernelThreadGroupSizes.html`, `ComputeShader.SetTexture.html`, `ComputeShader.SetBuffer.html` | Exact C#-side compute dispatch API: finding a kernel, querying its declared `numthreads`, binding textures/buffers, dispatching |
| ComputeBuffer | `ScriptReference/ComputeBuffer.html`, `ComputeBuffer-ctor.html`, `ComputeBuffer.SetData.html`, `ComputeBuffer.GetData.html`, `ComputeBuffer.Release.html`, `ComputeBuffer.IsValid.html`, `ComputeBufferType.html`, `ComputeBufferMode.html` | Structured/raw/append/counter/indirect-args buffer creation, data transfer, disposal, and the buffer-mode enum (Immutable/Dynamic/Circular/SubUpdates/StreamOut) |
| GraphicsBuffer | `ScriptReference/GraphicsBuffer.html`, `GraphicsBuffer-ctor.html`, `GraphicsBuffer.SetData.html`, `GraphicsBuffer.GetData.html`, `GraphicsBuffer.Release.html`, `GraphicsBuffer.Target.html`, `GraphicsBuffer.UsageFlags.html`, `GraphicsBuffer.LockBufferForWrite.html`, `GraphicsBuffer.IndirectDrawArgs.html`, `GraphicsBuffer.IndirectDrawIndexedArgs.html` | Unity's newer unified GPU buffer type — superset of `ComputeBuffer`'s targets (Vertex/Index/Structured/Raw/Append/Counter/IndirectArguments/Constant), used for compute *and* draw-indirect data |
| RenderTexture manual | `Manual/class-RenderTexture.html`, `Manual/class-RenderTexture-create.html` | Format, dimension, depth-buffer, MSAA options; creating a RenderTexture and assigning it as a camera's `targetTexture` |
| RenderTexture ScriptReference | `ScriptReference/RenderTexture.html`, `RenderTexture-ctor.html`, `RenderTexture.GetTemporary.html`, `RenderTexture.ReleaseTemporary.html`, `RenderTexture.Release.html`, `RenderTexture-enableRandomWrite.html` | Persistent vs temporary (pooled) RenderTexture lifecycle; `enableRandomWrite` for UAV compute-shader write access |
| CustomRenderTexture | `Manual/class-CustomRenderTexture-landing.html`, `Manual/class-CustomRenderTexture-introduction.html`, `Manual/class-CustomRenderTexture-create.html`, `Manual/class-CustomRenderTexture-configure.html`, `Manual/class-CustomRenderTexture-write-shader.html` | Self-updating RenderTexture driven by a shader each frame/on-demand (double-buffered simulation textures, procedural textures) |
| Async GPU readback | `ScriptReference/Rendering.AsyncGPUReadback.html`, `Rendering.AsyncGPUReadback.Request.html`, `Rendering.AsyncGPUReadback.RequestAsync.html`, `Rendering.AsyncGPUReadbackRequest.html`, `Rendering.AsyncGPUReadbackRequest.GetData.html`, `Rendering.AsyncGPUReadbackRequest-done.html`, `Rendering.AsyncGPUReadbackRequest-hasError.html`, `SystemInfo-supportsAsyncGPUReadback.html` | Reading GPU buffer/texture/RenderTexture data back to CPU without a hard stall |
| ScriptableRenderContext | `ScriptReference/Rendering.ScriptableRenderContext.DrawRenderers.html`, `.Cull.html`, `.CreateRendererList.html`, `.BeginRenderPass.html`, `.Submit.html` (verify further members via search) | Low-level SRP context object a fully custom `RenderPipeline.Render` override submits work through (below the URP Renderer Feature layer) |

**ScriptReference gap (verified):** `ScriptableRenderPass`, `ScriptableRendererFeature`, `RenderGraph`, `TextureHandle`, and `BufferHandle` return **no hits** under `ScriptReference/` in this doc set — like `Volume`/`VolumeProfile` in the sibling `unity-rendering-pipelines` skill, these types ship inside the URP/Core SRP packages, not the base Unity API. Their authoritative reference is the installed package's in-Package Manager API docs; the `Manual/urp/` pages cited above are this doc set's primary source for their usage patterns and are what this skill treats as ground truth.

## Key Guidelines

### CommandBuffer Basics

A `CommandBuffer` (`UnityEngine.Rendering.CommandBuffer`) is an ordered list of GPU commands — clear, set render target, draw mesh, blit, dispatch compute, copy texture — that you record on the CPU and hand to Unity to execute on the GPU as a unit, without those commands going through Unity's normal per-`Renderer` draw loop. In the Built-in Render Pipeline, the classic pattern is `Camera.AddCommandBuffer(CameraEvent, CommandBuffer)` or `Light.AddCommandBuffer(LightEvent, CommandBuffer)` (`GraphicsCommandBuffers.html`), which schedules the buffer to run automatically at a named point in Unity's fixed BIRP render loop (`GraphicsCommandBuffers-order.html` has the full event ordering) — this pattern still works but is legacy and not how URP custom passes are structured. In URP/SRP code, you normally don't attach a raw `CommandBuffer` to a camera event yourself; instead you obtain one (via `CommandBufferPool.Get()` in the legacy `ScriptableRenderPass.Execute` pattern, or via `context.cmd` inside a render graph `SetRenderFunc`) and record commands into it inside a `ScriptableRenderPass`, and URP schedules *the pass* at the `RenderPassEvent` you choose — the CommandBuffer itself is the same underlying type either way. `Graphics.ExecuteCommandBuffer` runs a buffer immediately, outside any scheduling system, for one-off GPU work triggered directly from script (e.g. from `OnRenderImage` in BIRP or ad hoc tooling code) rather than as part of a per-frame pipeline pass.

```csharp
using UnityEngine;
using UnityEngine.Rendering;

public class SimpleBlurCommandBuffer : MonoBehaviour
{
    Camera cam;
    CommandBuffer cmd;
    Material blurMaterial; // assign a material using a blur shader

    void OnEnable()
    {
        cam = GetComponent<Camera>();
        cmd = new CommandBuffer { name = "Simple Blur" };

        int tempId = Shader.PropertyToID("_TempBlurTex");
        cmd.GetTemporaryRT(tempId, -1, -1, 0, FilterMode.Bilinear);
        cmd.Blit(BuiltinRenderTextureType.CameraTarget, tempId, blurMaterial, 0);
        cmd.Blit(tempId, BuiltinRenderTextureType.CameraTarget);
        cmd.ReleaseTemporaryRT(tempId);

        // BIRP-only scheduling: run this buffer after opaque+transparent geometry,
        // before image effects.
        cam.AddCommandBuffer(CameraEvent.AfterImageEffectsOpaque, cmd);
    }

    void OnDisable()
    {
        if (cam != null) cam.RemoveCommandBuffer(CameraEvent.AfterImageEffectsOpaque, cmd);
        cmd?.Dispose();
    }
}
```

### Custom ScriptableRenderPass / ScriptableRendererFeature (URP, Render Graph)

In Unity 6, the recommended way to write a custom URP pass is through the **render graph system** (`render-graph-introduction.html`), not the older direct-`Execute`-with-`CommandBuffer.Blit` pattern many tutorials still show. A render graph pass has two separated stages: a **recording** stage (`RecordRenderGraph`), where you declare which textures/buffers the pass reads and writes and register a rendering function, but issue *no* GPU commands; and an **execution** stage, where the render graph actually runs your rendering function with those resources resolved. This separation is what lets URP automatically cull unused passes, reuse texture memory across the frame, and synchronize compute/graphics queues (`render-graph-write-render-pass.html`, `render-graph-introduction.html`). A `ScriptableRenderPass` subclass overrides `RecordRenderGraph(RenderGraph renderGraph, ContextContainer frameContext)`; inside it you pull shared frame resources from `frameContext.Get<UniversalResourceData>()` (camera color/depth textures) or `UniversalCameraData`, open a pass scope with `renderGraph.AddRasterRenderPass<PassData>(name, out var passData)`, declare texture usage with `builder.UseTexture(...)`/`builder.SetRenderAttachment(...)`, and finally call `builder.SetRenderFunc(static (PassData data, RasterGraphContext context) => ExecutePass(data, context))` — a static method/lambda, since instance state must flow through the `PassData` struct rather than closures, to avoid per-frame allocations.

```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.RenderGraphModule;
using UnityEngine.Rendering.Universal;

// A pass that inverts the camera's color output as a full-screen effect.
public class InvertColorPass : ScriptableRenderPass
{
    class PassData
    {
        public TextureHandle source;
        public Material invertMaterial;
    }

    Material m_InvertMaterial;

    public InvertColorPass(Material invertMaterial)
    {
        m_InvertMaterial = invertMaterial;
        renderPassEvent = RenderPassEvent.AfterRenderingTransparents;
    }

    public override void RecordRenderGraph(RenderGraph renderGraph, ContextContainer frameContext)
    {
        UniversalResourceData resourceData = frameContext.Get<UniversalResourceData>();

        using (var builder = renderGraph.AddRasterRenderPass<PassData>("Invert Color", out var passData))
        {
            passData.source = resourceData.activeColorTexture;
            passData.invertMaterial = m_InvertMaterial;

            // Read the camera color texture, then write it back as the render target.
            builder.UseTexture(passData.source);
            builder.SetRenderAttachment(resourceData.activeColorTexture, 0);

            builder.SetRenderFunc(static (PassData data, RasterGraphContext context) =>
            {
                Blitter.BlitTexture(context.cmd, data.source, new Vector4(1, 1, 0, 0), data.invertMaterial, 0);
            });
        }
    }
}

public class InvertColorFeature : ScriptableRendererFeature
{
    public Material invertMaterial;
    InvertColorPass m_Pass;

    public override void Create()
    {
        m_Pass = new InvertColorPass(invertMaterial);
    }

    public override void AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData)
    {
        if (invertMaterial == null) return;
        if (renderingData.cameraData.cameraType != CameraType.Game) return;
        renderer.EnqueuePass(m_Pass);
    }
}
```

`ScriptableRendererFeature.Create()` runs when the feature first loads, when it's toggled on/off, and whenever an Inspector property on it changes — never allocate per-frame resources there and never skip it for one-time setup (`inject-a-pass-using-a-scriptable-renderer-feature.html`). `AddRenderPasses()` runs every frame, once per camera — it must stay cheap (no allocation, no resource creation) and just call `EnqueuePass` conditionally. Add the feature to the active URP Renderer asset's Renderer Features list to activate it project-wide, or gate it per-camera-type/per-layer inside `AddRenderPasses` as shown above.

### RenderPipelineManager Callbacks

`RenderPipelineManager` (`UnityEngine.Rendering.RenderPipelineManager`, static class) exposes C# events that fire at fixed points in *any* active Scriptable Render Pipeline's frame — `beginFrameRendering`/`endFrameRendering` (once per `Camera.Render`-triggering call, potentially covering multiple cameras/contexts), `beginContextRendering`/`endContextRendering` (once per `ScriptableRenderContext`, i.e. per batch of cameras rendered together), and `beginCameraRendering`/`endCameraRendering` (once per individual camera) — plus pipeline-lifecycle events (`activeRenderPipelineAssetChanged`, `activeRenderPipelineCreated`, `activeRenderPipelineDisposed`, `activeRenderPipelineTypeChanged`, `pipelineSwitchCompleted`) for reacting when the project's active `RenderPipelineAsset` changes at runtime. This is the mechanism for injecting a custom `ScriptableRenderPass` from a plain `MonoBehaviour` without writing a `ScriptableRendererFeature` at all (`inject-render-pass-via-script.html`) — useful for a pass that's scoped to one specific camera or GameObject (e.g. a portal/surveillance camera rendering to a `RenderTexture`) rather than every camera in every scene, which is what a Renderer Feature applies to. The docs note events only fire while at least one camera is actively rendering, and explicitly recommend a Renderer Feature instead when the pass should apply across multiple cameras, scenes, or the whole project.

```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

public class InjectPassViaRenderPipelineManager : MonoBehaviour
{
    ExampleRenderPass m_Pass;

    void OnEnable()
    {
        m_Pass = new ExampleRenderPass { renderPassEvent = RenderPassEvent.BeforeRenderingTransparents };
        RenderPipelineManager.beginCameraRendering += OnBeginCameraRendering;
    }

    void OnDisable()
    {
        RenderPipelineManager.beginCameraRendering -= OnBeginCameraRendering;
    }

    void OnBeginCameraRendering(ScriptableRenderContext context, Camera cam)
    {
        // Only inject for this specific camera, not every camera in the scene.
        if (cam != GetComponent<Camera>()) return;

        var urpCameraData = cam.GetUniversalAdditionalCameraData();
        urpCameraData.scriptableRenderer.EnqueuePass(m_Pass);
    }
}
```

Always pair a subscription in `OnEnable` with an unsubscription in `OnDisable` — a `MonoBehaviour` that subscribes to a static event and is destroyed without unsubscribing leaks a delegate reference that keeps firing (and can NullReferenceException against the destroyed object) until domain reload.

### Compute Shaders from C# (Dispatch, Thread Groups, Buffer Binding)

A compute shader (`.compute` asset, `class-ComputeShader-introduction.html`) is a GPU program with no fixed vertex/fragment role — it runs arbitrary parallel work organized into **thread groups**, each containing a fixed number of **threads** declared in HLSL via `[numthreads(x, y, z)]` on the kernel function. From C#, you look up a kernel index by name with `ComputeShader.FindKernel("KernelName")`, bind its inputs/outputs with `SetBuffer`/`SetTexture`/`SetInt`/`SetFloat` etc., and invoke it with `ComputeShader.Dispatch(kernelIndex, threadGroupsX, threadGroupsY, threadGroupsZ)` (`class-ComputeShader-run.html`, `ScriptReference/ComputeShader.Dispatch.html`) — critically, `Dispatch`'s arguments are **thread *group* counts**, not thread counts: if a kernel declares `[numthreads(8,8,1)]` and you need to cover a 256x256 texture, you dispatch `Mathf.CeilToInt(256/8f) = 32` groups in X and Y, not 256. `ComputeShader.GetKernelThreadGroupSizes(kernelIndex, out x, out y, out z)` lets you read the declared `numthreads` back from C# instead of hardcoding it twice, keeping the divide-and-ceil math in sync with the shader automatically. Writing into a `RenderTexture` from a compute shader requires `RenderTexture.enableRandomWrite = true` (a DX11-terminology "unordered access view") set *before* the texture is created, and the texture must actually be `.Create()`d (or otherwise realized) before you `SetTexture` it onto the kernel.

```hlsl
// GradientCompute.compute
#pragma kernel CSMain

RWTexture2D<float4> Result;
float Time;

[numthreads(8, 8, 1)]
void CSMain(uint3 id : SV_DispatchThreadID)
{
    uint width, height;
    Result.GetDimensions(width, height);
    float2 uv = float2(id.xy) / float2(width, height);
    Result[id.xy] = float4(uv.x, uv.y, 0.5 + 0.5 * sin(Time), 1.0);
}
```

```csharp
using UnityEngine;

public class GradientComputeDispatcher : MonoBehaviour
{
    public ComputeShader computeShader;
    RenderTexture m_Result;
    int m_KernelIndex;
    uint m_ThreadGroupSizeX, m_ThreadGroupSizeY, m_ThreadGroupSizeZ;

    void OnEnable()
    {
        m_KernelIndex = computeShader.FindKernel("CSMain");
        computeShader.GetKernelThreadGroupSizes(
            m_KernelIndex, out m_ThreadGroupSizeX, out m_ThreadGroupSizeY, out m_ThreadGroupSizeZ);

        m_Result = new RenderTexture(512, 512, 0, RenderTextureFormat.ARGBFloat)
        {
            enableRandomWrite = true // must be set before Create()
        };
        m_Result.Create();
    }

    void Update()
    {
        computeShader.SetTexture(m_KernelIndex, "Result", m_Result);
        computeShader.SetFloat("Time", Time.time);

        int groupsX = Mathf.CeilToInt(512f / m_ThreadGroupSizeX);
        int groupsY = Mathf.CeilToInt(512f / m_ThreadGroupSizeY);
        computeShader.Dispatch(m_KernelIndex, groupsX, groupsY, (int)m_ThreadGroupSizeZ);
    }

    void OnDisable()
    {
        if (m_Result != null)
        {
            m_Result.Release();
            m_Result = null;
        }
    }
}
```

Inside URP's render graph, the equivalent pattern uses `renderGraph.AddComputePass<PassData>(name, out passData)` (instead of `AddRasterRenderPass`) and `ComputeGraphContext` (instead of `RasterGraphContext`) in the render function, and `context.cmd.DispatchCompute(...)`/`context.cmd.SetComputeBufferParam(...)` inside `SetRenderFunc` rather than calling `ComputeShader.Dispatch` directly — the render graph then handles synchronizing the compute queue against the graphics queue automatically (`render-graph-compute-shader-run.html`, and see the CommandBuffer-based Compute Buffer example in that section below).

### RenderTexture Management

A `RenderTexture` is a GPU-resident texture Unity can render into, rather than the CPU-authored pixel data of a `Texture2D`. There are two lifecycle patterns: **persistent** — `new RenderTexture(...)` (or the manual constructor overloads) followed by an explicit `.Create()`, held for the object's lifetime, and explicitly `.Release()`d when done (`class-RenderTexture-create.html`, `ScriptReference/RenderTexture-ctor.html`) — and **temporary/pooled** — `RenderTexture.GetTemporary(...)` pulls a RenderTexture from Unity's internal pool matching the requested descriptor, and `RenderTexture.ReleaseTemporary(...)` returns it to the pool rather than destroying it, which is far cheaper for a texture only needed within a single frame or a single render pass (e.g. a scratch blur target). Never let a persistent `RenderTexture` go out of scope without calling `Release()` first — unlike most managed objects, the underlying GPU memory is not automatically reclaimed by the .NET garbage collector on a predictable schedule, so an unreleased RenderTexture is a real GPU memory leak, not just a soon-to-be-collected managed object. Key creation-time parameters: `width`/`height`, `depth` (depth-buffer bits: 0/16/24/32, distinct from texture depth), `format` (`RenderTextureFormat`, e.g. `ARGB32`, `ARGBHalf`, `RFloat`, `Depth`), and `antiAliasing`. `enableRandomWrite` (see Compute Shaders above) and `useMipMap`/`autoGenerateMips` must be set before `.Create()` — mutating most `RenderTexture` properties after creation either throws or silently does nothing until the texture is recreated.

```csharp
using UnityEngine;

public class ScratchBlurTarget : MonoBehaviour
{
    void RenderScratchPass(Camera cam)
    {
        // Pooled: cheap to acquire/release every frame, no leak risk if released.
        RenderTexture temp = RenderTexture.GetTemporary(
            cam.pixelWidth / 2, cam.pixelHeight / 2, 0, RenderTextureFormat.Default);

        Graphics.Blit(null, temp); // fill temp with something, e.g. via a shader pass

        // ... consume temp ...

        RenderTexture.ReleaseTemporary(temp);
    }
}
```

### GraphicsBuffer for GPU Data

`GraphicsBuffer` is the newer, unified GPU buffer type that supersedes `ComputeBuffer` for most new code — where `ComputeBuffer` only exposes `ComputeBufferType` (Default/Structured/Raw/Append/Counter/IndirectArguments/Constant), `GraphicsBuffer.Target` adds `Vertex` and `Index` to that same list, meaning a single buffer type can back compute I/O, indirect draw arguments, *and* mesh vertex/index data, letting a compute shader write directly into a buffer later consumed as a mesh's geometry without a CPU round-trip (e.g. GPU-driven procedural geometry or particle systems). Construction takes a `Target`, a `count` (element count), and a `stride` (bytes per element, must match the HLSL struct's layout); `UsageFlags.LockBufferForWrite` combined with `LockBufferForWrite`/`UnlockBufferAfterWrite` gives direct-mapped CPU write access for streaming data in without a full `SetData` copy. `GraphicsBuffer.IndirectDrawArgs`/`IndirectDrawIndexedArgs` are the argument-struct layouts consumed by indirect draw calls sourced from GPU-computed instance/vertex counts.

```csharp
using UnityEngine;
using UnityEngine.Rendering;

public class ParticlePositionBuffer : MonoBehaviour
{
    public ComputeShader particleCompute;
    GraphicsBuffer m_PositionBuffer;
    int m_Kernel;
    const int ParticleCount = 1024;

    void OnEnable()
    {
        // 3 floats per particle position, matching a HLSL float3 with 12-byte stride.
        m_PositionBuffer = new GraphicsBuffer(GraphicsBuffer.Target.Structured, ParticleCount, sizeof(float) * 3);
        m_Kernel = particleCompute.FindKernel("CSMain");
        particleCompute.SetBuffer(m_Kernel, "_Positions", m_PositionBuffer);
    }

    void Update()
    {
        particleCompute.SetFloat("_DeltaTime", Time.deltaTime);
        particleCompute.Dispatch(m_Kernel, Mathf.CeilToInt(ParticleCount / 64f), 1, 1);
        // m_PositionBuffer now holds updated positions on the GPU, ready for a
        // DrawMeshInstancedIndirect / DrawProceduralIndirect call or a compute readback.
    }

    void OnDisable()
    {
        m_PositionBuffer?.Release();
        m_PositionBuffer = null;
    }
}
```

Reading buffer/texture data back to the CPU with `GetData` stalls the CPU until the GPU catches up if called too soon after the GPU work that produced the data — prefer `AsyncGPUReadback.Request`/`RequestAsync` (see Advanced Notes) when the read doesn't need to happen this exact frame.

## Common Mistakes

| Mistake | Why / fix |
|---|---|
| RenderTexture/ComputeBuffer/GraphicsBuffer never released | GPU memory isn't reclaimed by .NET GC on a predictable schedule; always pair `new RenderTexture(...)`/`new ComputeBuffer(...)`/`new GraphicsBuffer(...)` with `.Release()` (or `.Dispose()`) in `OnDisable`/`OnDestroy`, and prefer `GetTemporary`/`ReleaseTemporary` for single-frame RenderTextures |
| Compute thread group math off by orders of magnitude | `Dispatch(x, y, z)` takes **thread group counts**, not thread counts; divide the total work size by the kernel's declared `numthreads` (via `GetKernelThreadGroupSizes` or the literal in the shader) and round up with `Mathf.CeilToInt`, or the compute misses/duplicates coverage at the edges |
| Renderer Feature injected at the wrong `RenderPassEvent` | e.g. reading a fully-lit scene color at `BeforeRenderingOpaques` (nothing's drawn yet) or trying to modify opaque geometry after `AfterRenderingTransparents` (too late); pick the event from `custom-pass-injection-points.html` that matches what buffers you need available |
| Blitting in URP with `CommandBuffer.Blit`/`Graphics.Blit`/`RenderingUtils.Blit` | Manual explicitly warns these APIs can break XR rendering and aren't compatible with native render passes in URP; use `Blitter.BlitCameraTexture`/`Blitter.BlitTexture`, or `renderGraph.AddBlitPass`/`AddCopyPass` inside a render graph pass, with a hand-coded (not Shader Graph) shader |
| `RenderTexture.enableRandomWrite` set after `.Create()` | Must be set before the texture is created (or the texture recreated) — setting it afterward has no effect, and the compute shader's UAV write silently fails or throws |
| `ScriptableRendererFeature.Create()` used to allocate per-frame resources | `Create()` runs on load/toggle/property-change, not every frame; put per-frame-safe logic in `AddRenderPasses()` instead, and keep `AddRenderPasses()` itself allocation-free since it runs once per camera every frame |
| Forgetting to unsubscribe a `RenderPipelineManager` event | A `MonoBehaviour` that subscribes in `OnEnable` without unsubscribing in `OnDisable`/`OnDestroy` leaks the delegate and can throw against a destroyed object; always pair the subscribe/unsubscribe |
| Using `RenderPipelineManager.beginCameraRendering` for an effect that should apply everywhere | Docs recommend a `ScriptableRendererFeature` for anything spanning multiple cameras/scenes/the whole project; `RenderPipelineManager` events are the right tool only for a targeted, script-driven single-camera/GameObject case |
| Mixing legacy `Execute`/RTHandle-based pass code with render graph APIs in the same pass | The render graph's recording/execution separation (`RecordRenderGraph` declares resources, the render function executes) is a different contract from the older direct `Execute(context, ref renderingData)` pattern — don't call `cmd.SetRenderTarget`/`Blit` directly inside `RecordRenderGraph` (that stage must not issue GPU commands) |
| `PassData` capturing instance state via closure instead of fields | `SetRenderFunc` should take a static method/lambda; capturing `this`/local variables in a non-static closure allocates a delegate every frame instead of reusing the `PassData` struct, and defeats one of the render graph's stated GC-allocation optimizations |
| Compute buffer `stride` not matching the HLSL struct's actual byte layout | HLSL struct packing rules (16-byte alignment boundaries) don't always match a naive sum of C# field sizes; a mismatched stride silently reads garbage/misaligned data rather than throwing |
| Assuming `ScriptableRenderPass`/`ScriptableRendererFeature`/`TextureHandle`/`RenderGraph` are documented in base Unity ScriptReference | These ship inside the URP/Core SRP packages, not `ScriptReference/`; use the in-Package Manager API docs or the `Manual/urp/` pages for authoritative signatures |
| Calling `GetData()` on a GPU buffer/texture right after the GPU work that wrote it | Forces a CPU-GPU sync stall waiting for the GPU to finish; use `AsyncGPUReadback.Request`/`RequestAsync` when the result isn't needed the same frame |
| Reusing one `CommandBuffer` instance across multiple cameras/passes without `Clear()`-ing it | Commands accumulate across `AddCommandBuffer` calls if not cleared, silently re-executing stale draws; call `cmd.Clear()` before re-recording a reused buffer |

## Quick Reference

| Type / Method | Purpose |
|---|---|
| `CommandBuffer` | Ordered list of GPU commands recorded on CPU, executed as a unit |
| `Camera.AddCommandBuffer(CameraEvent, CommandBuffer)` | Schedules a buffer at a fixed BIRP camera-event point |
| `Graphics.ExecuteCommandBuffer(CommandBuffer)` | Runs a buffer immediately, outside event scheduling |
| `CommandBuffer.Blit` / `.DrawMesh` / `.DrawProcedural` / `.CopyTexture` / `.DispatchCompute` | Core recorded commands: blit, draw, copy, compute dispatch |
| `ScriptableRenderPass` | Base class for a custom URP render pass; override `RecordRenderGraph` (render graph) or `Execute` (legacy) |
| `ScriptableRendererFeature` | Base class packaging one or more passes; `Create()` on load/change, `AddRenderPasses()` every frame/camera |
| `ScriptableRenderer.EnqueuePass(ScriptableRenderPass)` | Injects a pass instance into this frame's render loop |
| `RenderPassEvent` | Enum of injection points (BeforeRenderingShadows … AfterRendering) controlling *when* a pass runs |
| `RenderGraph.AddRasterRenderPass<T>` | Opens a render-graph raster pass scope, returns an `IRasterRenderGraphBuilder` |
| `RenderGraph.AddComputePass<T>` | Opens a render-graph compute pass scope, returns a compute builder |
| `RenderGraph.AddUnsafePass<T>` | Escape hatch pass type using `UnsafeCommandBuffer` for commands the raster/compute builders don't expose |
| `RenderGraph.CreateTexture` / `.ImportTexture` | Creates a new graph-managed texture, or wraps an existing RenderTexture/RTHandle as a `TextureHandle` |
| `RenderGraph.ImportBuffer` | Wraps an existing `GraphicsBuffer` as a `BufferHandle` for graph use |
| `builder.UseTexture` / `.SetRenderAttachment` / `.UseBuffer` | Declares a pass's texture/buffer inputs, color/depth attachments, and buffer read/write access |
| `builder.SetRenderFunc(static (data, context) => ...)` | Registers the static execution function the graph calls with resolved resources |
| `builder.AllowPassCulling(false)` | Opts a pass out of automatic dead-pass removal (debug/dev use, not production) |
| `Blitter.BlitCameraTexture` / `Blitter.BlitTexture` | URP-safe, XR-compatible blit using a hand-written shader (not Shader Graph) |
| `RenderPipelineManager.beginCameraRendering` / `endCameraRendering` | Per-camera SRP hook, fires for any active SRP |
| `RenderPipelineManager.beginContextRendering` / `endContextRendering` | Per-`ScriptableRenderContext` hook (batch of cameras) |
| `RenderPipelineManager.beginFrameRendering` / `endFrameRendering` | Per-frame hook |
| `RenderPipelineManager.activeRenderPipelineAssetChanged` etc. | Pipeline-lifecycle hooks (asset swap, pipeline created/disposed) |
| `ScriptableRenderContext` | Low-level SRP submission object (`Cull`, `DrawRenderers`, `Submit`) used by a fully custom `RenderPipeline` |
| `ComputeShader.FindKernel(name)` | Resolves a kernel function to its integer index |
| `ComputeShader.GetKernelThreadGroupSizes` | Reads a kernel's declared `[numthreads(x,y,z)]` back into C# |
| `ComputeShader.SetBuffer` / `.SetTexture` / `.SetInt` / `.SetFloat` | Binds data to a kernel before dispatch |
| `ComputeShader.Dispatch(kernel, x, y, z)` | Invokes a kernel across `x*y*z` thread groups |
| `ComputeBuffer` | GPU buffer type: Structured/Raw/Append/Counter/IndirectArguments/Constant targets |
| `GraphicsBuffer` | Newer unified GPU buffer type; adds Vertex/Index targets to ComputeBuffer's set |
| `GraphicsBuffer.LockBufferForWrite` / `.UnlockBufferAfterWrite` | Direct-mapped CPU write access without a full `SetData` copy |
| `RenderTexture` | GPU-resident render target texture |
| `RenderTexture.GetTemporary` / `.ReleaseTemporary` | Pooled RenderTexture acquire/release, cheap for single-frame use |
| `RenderTexture.enableRandomWrite` | Enables UAV write access for compute shaders; must be set before `.Create()` |
| `CustomRenderTexture` | Self-updating RenderTexture driven by a shader pass each update cycle |
| `AsyncGPUReadback.Request` / `.RequestAsync` | Non-stalling CPU readback of GPU buffer/texture data |
| `AsyncGPUReadbackRequest.done` / `.hasError` / `.GetData<T>()` | Poll/consume an async readback's completion and result |

## Advanced Notes

**RTHandles vs raw RenderTexture in modern URP.** URP internally wraps camera-scoped render targets in `RTHandle`, a resizable/reusable handle layer over `RenderTexture` that lets URP resize and reuse GPU memory across cameras and frames (e.g. for dynamic resolution and XR eye-texture handling) without you juggling raw `RenderTexture` instances by hand — this is why writing a custom pass "the old way" against `RenderTexture` directly is fragile in a URP context (a hardcoded resolution/format breaks under dynamic resolution or XR). In Unity 6's render graph system, `RTHandle` itself is largely superseded as the pass-authoring surface: you interact with `TextureHandle` (via `renderGraph.CreateTexture`/`ImportTexture` and `builder.UseTexture`/`SetRenderAttachment`) instead, and the render graph manages the underlying `RTHandle`/`RenderTexture` allocation and reuse for you (`render-graph-create-a-texture.html`, `render-graph-import-a-texture.html`). Direct `RTHandle` manipulation still exists for lower-level/legacy (non-render-graph) `ScriptableRenderPass.Execute`-style code and inside URP's own source, but new custom passes should default to the `TextureHandle`/render graph surface described in this skill's Key Guidelines rather than hand-managing `RTHandle`s.

**Async GPU readback.** `AsyncGPUReadback.Request(texture-or-buffer, callback)` / `.RequestAsync(...)` (`ScriptReference/Rendering.AsyncGPUReadback.html`) queue a GPU-to-CPU data transfer that completes over one or more subsequent frames instead of blocking the calling thread — check `SystemInfo.supportsAsyncGPUReadback` first, since not every platform/graphics API supports it (`ScriptReference/SystemInfo-supportsAsyncGPUReadback.html`), and have a synchronous `GetData` fallback path for those. Poll a pending request's `AsyncGPUReadbackRequest.done`/`.hasError`, or use the callback form, then call `.GetData<T>()` to get a `NativeArray<T>` view onto the result — that view is only valid until the next frame, so copy out of it immediately if you need the data to persist. This is the correct way to pull compute-shader results (e.g. a GPU-side histogram, physics simulation state, or a screenshot) back to the CPU for gameplay logic or file I/O without stalling the render thread; `RequestIntoNativeArray`/`RequestIntoNativeSlice` variants let you supply your own persistent destination buffer instead of a per-request allocation, useful for a readback that repeats every frame (e.g. streaming compute results into a `NativeArray` reused across frames rather than reallocating one each request).

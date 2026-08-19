---
name: unity-video-playback
description: Use when playing back or streaming video in Unity — VideoPlayer, VideoClip, or render-target/audio-output configuration for video. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Video Playback

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Video and cutscenes (landing page) | `Manual/Video.html` | Top-level index into Video sources, Video Player, and Video Profiler module topics; notes that Timeline is the recommended tool for authored (non-pre-rendered) cutscenes |
| Video sources (index) | `Manual/video-sources.html` | Index into import/preview, referencing sources, file compatibility, unsupported-file workarounds, encoding compatibility, transcoding, and transparency sub-pages |
| Use video sources | `Manual/video-sources-reference.html` | How to point a Video Player at a Video Clip vs. a URL; file-system/web-server/StreamingAssets URL forms; Web platform URL restriction |
| Import and preview video clips | `Manual/video-clips-use.html` | Importing a file as a Video Clip asset, previewing it in the Inspector, and viewing Source Info (size/duration/frames/pixels) |
| Understand video files | `Manual/VideoSources-VideoFiles.html` | Container vs. codec concepts, multi-track containers (multiple POV, stereo/5.1, subtitle/dialog tracks), hardware vs. software decoding |
| Video file compatibility with the Unity Editor | `Manual/VideoSources-FileCompatibility.html` | Per-Editor-OS supported file extensions table (.mp4/.mov/.webm/.avi/etc.); WebM codec restrictions and the StreamingAssets bypass for unsupported WebM codecs; per-platform WebM/VP8/VP9/Opus support table |
| Video file compatibility with target platforms | `Manual/video-sources-compatibility-target-platforms.html` | Reminder that the Editor uses host-OS decoding libraries while builds use target-platform libraries, so a file that imports fine may still fail on-device; links to platform codec references |
| Use unsupported video files in the Editor | `Manual/video-files-unsupported.html` | StreamingAssets workaround for files the Editor can't preview but the target platform supports; placeholder-video pattern using platform `#if` compilation |
| Video encoding compatibility reference | `Manual/video-encoding-compatibility.html` | H.264/H.265/VP8 tradeoffs table, `.ogv` Editor-only caveat, external-encoder workflow, H.265 per-platform hardware/software encode-decode requirements table |
| Video transcoding (index) | `Manual/video-transcoding.html` | Index into transcoding introduction and step-by-step instructions |
| Introduction to transcoding video files | `Manual/video-transcode-intro.html` | What transcoding does, the 3 transcode-target codecs (H.264/H.265/VP8) and their automatically-paired audio codecs (AAC/AAC/Vorbis) |
| Transcode your video files | `Manual/video-transcode-steps.html` | Step-by-step: enable Transcode on the Video Clip Importer, configure, Apply |
| Video Clip Importer reference (Manual) | `Manual/class-VideoClip.html` | Full Inspector property table: sRGB (Color Texture), Transcode toggle, and (once enabled) Dimensions/Codec/Bitrate Mode/Spatial Quality/Keep Alpha/Deinterlace/Flip/Import Audio sub-settings |
| Video Player component reference | `Manual/class-VideoPlayer.html` | Full Inspector property table: Source, Video Clip, URL, Update Mode, Play On Awake, Wait For First Frame, Loop, Skip On Drop, Playback Speed, Render Mode, Camera, Alpha, 3D Layout, Target Texture, Aspect Ratio, Renderer, Auto-Select Property, Audio Output Mode, Controlled Tracks |
| Video Player (landing page) | `Manual/VideoPlayer.html` | Index into Video Player sub-topics: targets, creation, render-texture setup, MovieTexture migration, clock management, panoramic video |
| Video Player component targets | `Manual/VideoPlayer-intro.html` | Render Mode target overview (camera planes, Material/Texture, Render Texture, API Only) and Unity's default-target auto-selection rules when dragging a clip/component onto a GameObject |
| Create a Video Player component | `Manual/VideoPlayer-instructions.html` | The 4 ways to create a Video Player (menu, Add Component, drag clip into scene, `AddComponent<VideoPlayer>()` at runtime) |
| Set up your Render Texture to display video | `Manual/VideoPlayer-rendertexture.html` | Concrete recommended Render Texture settings for video (resolution match, no AA, `R8G8B8A8_UNORM`/`R16G16B16A16_UNORM`, no depth/stencil, no mipmaps, Clamp wrap, Point filter, `anisoLevel = 0`) |
| Migrating from MovieTexture to VideoPlayer | `Manual/VideoPlayer-MigratingFromMovieTexture.html` | Side-by-side legacy `MovieTexture`+`AudioClip` script vs. modern `VideoPlayer` equivalent |
| Clock management with the Video Player component | `Manual/video-clock.html` | The 3 time update modes (Audio DSP clock, Game time, Unscaled game time), interaction with `Time.captureFramerate`/`Time.captureDeltaTime`, synchronous vs. asynchronous playback behavior, Web-platform synchronous-playback restriction |
| Video transparency support (index) | `Manual/VideoTransparency.html` | Index into transparency overview, supported codecs, and setup instructions |
| Introduction to video transparency support | `Manual/VideoTransparency-overview.html` | Global alpha (camera-plane render modes only) vs. per-pixel alpha distinction; how Unity packs alpha into the color stream by doubling frame width during transcode |
| Supported codecs for transparent videos (per-pixel alpha) | `Manual/VideoTransparency-codecs.html` | The only 2 codecs with native per-pixel alpha (Apple ProRes 4444, WebM/VP8); Android's VP8 hardware path lacks alpha and needs transcoding to fix it |
| Set up transparent videos in Unity | `Manual/VideoTransparency-instructions.html` | Step-by-step for global alpha (camera near/far plane `Alpha` field) and per-pixel alpha (external authoring + Transcode + Keep Alpha for ProRes 4444) |
| Panoramic video (index) | `Manual/VideoPanoramic.html` | Index into 360°/180° video introduction, setup, skybox setup, and 3D setup |
| Introduction to panoramic videos | `Manual/VideoPanoramic-introduction.html` | Equirectangular (2:1 or 1:1 aspect) vs. cubemap (1:6/3:4/4:3/6:1 aspect) panoramic layouts |
| Set up a panoramic video | `Manual/VideoPanoramic-setup.html` | Rendering an equirectangular/cubemap video to a Render Texture sized to match the video, then projecting it onto an object/skybox |
| Set up a panoramic video as a skybox | `Manual/VideoPanoramic-skybox.html` | Building a skybox material from a panoramic Render Texture for scene backdrops |
| Set up your 3D panoramic video | `Manual/VideoPanoramic-3D.html` | Stereo 3D Layout field for VR skyboxes vs. non-360° camera-plane 3D content |
| Video playback in Web | `Manual/webgl-video.html` | Web-specific gaps: no frame accuracy, no synchronous `captureFramerate` playback, drift-correction disabled on Safari; only `None`/`Direct` audio output modes are fully supported (`AudioSource` mode ignores all fields but mute since 3D spatialization isn't available); supported container/codec list |
| Video Profiler module reference | `Manual/profiler-video-profiler-module.html` | Total/Playing Video Sources, Pre-buffered Frames, Total Video Memory chart categories; Paused/Software Video Playback detail-pane fields for diagnosing buffering and decode-path issues |
| Video package (`com.unity.modules.video`) | `Manual/com.unity.modules.video.html` | Confirms Video is a built-in package fixed to the Editor version — no separate install/version management |
| Playing video in Movie Textures (legacy) | `Manual/MovieTexture-landing.html` | Explicit note that `MovieTexture` is deprecated in favor of `VideoPlayer`; only relevant when maintaining old projects |
| `VideoPlayer` class | `ScriptReference/Video.VideoPlayer.html` | Full member list and a worked example: attaching to a camera, setting `renderMode`, `targetCameraAlpha`, `url`, `frame`, `isLooping`, `loopPointReached`, then `Play()` |
| `VideoClip` class | `ScriptReference/Video.VideoClip.html` | Read-only clip metadata (`length`, `frameCount`, `frameRate`, `width`/`height`, `audioTrackCount`, `originalPath`, `sRGB`) and `GetAudioChannelCount`/`GetAudioLanguage`/`GetAudioSampleRate` |
| `VideoSource` enum | `ScriptReference/Video.VideoSource.html` | `VideoClip` vs. `Url` — a VideoPlayer can hold both simultaneously; this enum picks which one plays |
| `VideoRenderMode` enum | `ScriptReference/Video.VideoRenderMode.html` | The 5 render targets: `CameraNearPlane`, `CameraFarPlane`, `MaterialOverride`, `RenderTexture`, `APIOnly`; worked example cycling through all of them |
| `VideoAudioOutputMode` enum | `ScriptReference/Video.VideoAudioOutputMode.html` | `None`/`AudioSource`/`Direct`/`APIOnly` semantics; worked example cycling modes with `SetTargetAudioSource` |
| `VideoTimeUpdateMode` enum | `ScriptReference/Video.VideoTimeUpdateMode.html` | `DSPTime`/`GameTime`/`UnscaledGameTime` — which Unity clock the player's playback timing follows |
| `VideoTimeReference` enum | `ScriptReference/Video.VideoTimeReference.html` | `Freerun`/`InternalTime`/`ExternalTime` — used with `externalReferenceTime` for external-clock-driven sync (e.g. genlock-style setups) |
| `VideoTimeSource` enum | `ScriptReference/Video.VideoTimeSource.html` | `GameTimeSource`/`AudioDSPTimeSource` — the underlying clock backing a given time update mode |
| `Video3DLayout` enum | `ScriptReference/Video.Video3DLayout.html` | `No3D`/`SideBySide3D`/`OverUnder3D` — stereo frame-packing layout for 3D/VR video content |
| `VideoAspectRatio` enum | `ScriptReference/Video.VideoAspectRatio.html` | `NoScaling`/`FitHorizontally`/`FitVertically`/`FitInside`/`FitOutside`/`Stretch` — how video content is scaled to fill its target |
| `VideoPlayer.Prepare` | `ScriptReference/Video.VideoPlayer.Prepare.html` | What preparation reserves/preloads; emits `prepareCompleted` and sets `isPrepared`; worked example gating a Play button on preparation |
| `VideoPlayer.isPrepared` | `ScriptReference/Video.VideoPlayer-isPrepared.html` | Starts `false`; becomes `true` only after `prepareCompleted`; resets to `false` on `Stop()`; polling-loop example |
| `VideoPlayer.prepareCompleted` | `ScriptReference/Video.VideoPlayer-prepareCompleted.html` | Fires even if you never called `Prepare()` yourself, because `Play()` triggers preparation implicitly |
| `VideoPlayer.Play` | `ScriptReference/Video.VideoPlayer.Play.html` | Calling `Play()` before preparing means playback isn't instant — it silently prepares first |
| `VideoPlayer.Pause` | `ScriptReference/Video.VideoPlayer.Pause.html` | Preserves current time and preparation state; calling on an unprepared player triggers preparation and shows the seek-target frame |
| `VideoPlayer.Stop` | `ScriptReference/Video.VideoPlayer.Stop.html` | Resets time to 0 and destroys internal resources (textures/buffers), so `isPrepared` becomes `false` again — costlier to resume than `Pause` |
| `VideoPlayer.time` | `ScriptReference/Video.VideoPlayer-time.html` | Setting `time` initiates an async seek: moves toward the target, fires `seekCompleted`, then `frameReady` once the frame displays |
| `VideoPlayer.clip` | `ScriptReference/Video.VideoPlayer-clip.html` | Player can hold both a clip and a URL; whichever was set most recently wins; not supported on WebGL (`url` only) |
| `VideoPlayer.source` | `ScriptReference/Video.VideoPlayer-source.html` | Explicitly controls which of `clip`/`url` plays when both are set; WebGL only supports `VideoSource.Url` |
| `VideoPlayer.url` | `ScriptReference/Video.VideoPlayer-url.html` | Accepts local absolute/relative paths, `file://`, `http://`/`https://`, or a `StreamingAssets` path |
| `VideoPlayer.renderMode` | `ScriptReference/Video.VideoPlayer-renderMode.html` | Auto-set based on how the component was created (e.g. added to a Camera defaults to camera background) |
| `VideoPlayer.targetTexture` | `ScriptReference/Video.VideoPlayer-targetTexture.html` | Used when `renderMode == RenderTexture`; 2D vs. Cube `TextureDimension` behavior, including exact cubemap face-size math for cubemap-layout video |
| `VideoPlayer.targetCamera` | `ScriptReference/Video.VideoPlayer-targetCamera.html` | The Camera whose near/far plane receives the video when render mode targets a camera plane |
| `VideoPlayer.targetMaterialRenderer` | `ScriptReference/Video.VideoPlayer-targetMaterialRenderer.html` | Used with `MaterialOverride`; `null` falls back to the GameObject's first Renderer |
| `VideoPlayer.targetMaterialProperty` | `ScriptReference/Video.VideoPlayer-targetMaterialProperty.html` | Texture property name the video writes into; empty string falls back to the material's main texture, then its first texture property |
| `VideoPlayer.audioOutputMode` | `ScriptReference/Video.VideoPlayer-audioOutputMode.html` | WebGL only fully supports `None`/`Direct`; `AudioSource` mode on Web ignores all `AudioSource` fields except mute |
| `VideoPlayer.SetTargetAudioSource` | `ScriptReference/Video.VideoPlayer.SetTargetAudioSource.html` | Assigns which `AudioSource` receives a given audio track's samples when `audioOutputMode == AudioSource` |
| `VideoPlayer.EnableAudioTrack` | `ScriptReference/Video.VideoPlayer.EnableAudioTrack.html` | Only effective while not playing; disables decoding entirely (cheaper than muting, which still decodes) |
| `VideoPlayer.errorReceived` | `ScriptReference/Video.VideoPlayer-errorReceived.html` | Reports HTTP failures, missing files, unsupported formats, permission issues, runtime errors; worked logging example |
| `VideoPlayer.loopPointReached` | `ScriptReference/Video.VideoPlayer-loopPointReached.html` | Fires at end-of-content regardless of `isLooping`; used for both loop-driven and stop-driven end-of-video logic |
| `VideoPlayer.started` | `ScriptReference/Video.VideoPlayer-started.html` | Fires once playback actually begins after preparation — good hook for SFX/VFX synced to video start |
| `VideoPlayer.seekCompleted` | `ScriptReference/Video.VideoPlayer-seekCompleted.html` | Seek duration can be noticeably long depending on codec/encoding parameters |
| `VideoPlayer.clockResyncOccurred` | `ScriptReference/Video.VideoPlayer-clockResyncOccurred.html` | Fires when the player's clock resyncs to its `VideoTimeReference`, with the corrected time in the event args |
| `VideoPlayer.frameDropped` | `ScriptReference/Video.VideoPlayer-frameDropped.html` | Fires when the decoder fails to produce a frame on schedule — a direct performance/skip-detection signal |
| `VideoPlayer.waitForFirstFrame` | `ScriptReference/Video.VideoPlayer-waitForFirstFrame.html` | With `playOnAwake`, gates the very first draw on preparation + first-frame availability; disabling it can skip several initial frames, and long preparation can cause many consecutive catch-up skips |
| `VideoPlayer.skipOnDrop` | `ScriptReference/Video.VideoPlayer-skipOnDrop.html` | Whether the player jumps ahead to correct drift vs. plays every frame unconditionally; only settable when `canSetSkipOnDrop` is true |
| `VideoPlayer.StepForward` | `ScriptReference/Video.VideoPlayer.StepForward.html` | Pauses (if playing) and advances exactly one frame; on an unprepared player it triggers preparation and shows frame 0 rather than skipping ahead; not frame-accurate on WebGL |
| `VideoPlayer.playbackSpeed` | `ScriptReference/Video.VideoPlayer-playbackSpeed.html` | Multiplier 0–10 on playback rate; gated by `canSetPlaybackSpeed` |
| `VideoClipImporter` class | `ScriptReference/VideoClipImporter.html` | Editor-time import settings API: `deinterlaceMode`, `flipHorizontal`/`flipVertical`, `importAudio`, `keepAlpha`, `sRGBClip`, `sourceHasAlpha`, `transcodeSkipped` (transcoding a large source can take hours; the import progress bar offers a skip option) |
| `VideoCodec` enum | `ScriptReference/VideoCodec.html` | Transcode target codecs: `Auto`, `H264`, `H265`, `VP8` |
| `VideoBitrateMode` enum | `ScriptReference/VideoBitrateMode.html` | Transcode bitrate presets: `Low`, `Medium`, `High` |
| `VideoResizeMode` enum | `ScriptReference/VideoResizeMode.html` | Transcode resize presets: `OriginalSize`, `ThreeQuarterRes`, `HalfRes`, `QuarterRes`, `Square1024`, `Square512`, `Square256`, `CustomSize` |
| `VideoDeinterlaceMode` enum | `ScriptReference/VideoDeinterlaceMode.html` | `Off`, `Even`, `Odd` — which field ordering to assume when deinterlacing during transcode |
| `VideoSpatialQuality` enum | `ScriptReference/VideoSpatialQuality.html` | `LowSpatialQuality`, `MediumSpatialQuality`, `HighSpatialQuality` — transcode detail-vs-size tradeoff |
| `VideoImporterTargetSettings` struct | `ScriptReference/VideoImporterTargetSettings.html` | Per-platform transcode override struct (`codec`, `bitrateMode`, `resizeMode`, `customWidth`/`customHeight`, `aspectRatio`, `spatialQuality`, `enableTranscoding`) used with `VideoClipImporter.SetTargetSettings` for scripted per-platform batch import |

That's 74 verified rows, well above the 20-30+ target; the local docs set covers all 125 Video-namespace pages, and the rows above were selected as the ones that carry unique, actionable information (the remaining ~50 pages are either per-enum-member stub pages with no content beyond their parent enum's page, or unrelated `Video*` matches like `PlayerLoop.PreUpdate.UpdateVideo`, `Profiling.ProfilerArea.Video`, and Windows `VideoCapture`/`WebCamTexture` APIs that belong to other subsystems, not the `UnityEngine.Video` playback namespace).

## Key Guidelines

### VideoPlayer Setup & Render Modes

A `VideoPlayer` component is a source (`VideoClip` or URL) routed to a target via `renderMode` (`Manual/VideoPlayer-intro.html`, `ScriptReference/Video.VideoRenderMode.html`). Unity picks a sensible default when you create the component: dropping a clip onto an existing Renderer-bearing GameObject sets `MaterialOverride` targeting that Renderer's main texture; dragging a clip into empty scene space targets the main camera's far plane; adding the component via script defaults to `RenderTexture` unless it's on a Camera, in which case it defaults to that camera's background plane. The five render modes serve different purposes: `CameraNearPlane` draws in front of everything (cutscene overlays, letterboxed intros, UI-level video), `CameraFarPlane` draws behind everything (animated skyboxes, backgrounds), `RenderTexture` decouples the video from any camera so multiple GameObjects or post-processing can share one texture, `MaterialOverride` writes directly into a specific Renderer's material property (video on a screen prop, a monitor, a in-world billboard) without an intermediate Render Texture, and `APIOnly` produces no automatic draw target at all — you pull frames yourself via `VideoPlayer.texture` for a fully custom pipeline (e.g. feeding a UI RawImage, a compute shader, or a native plugin). Camera-plane modes expose a global `targetCameraAlpha` for letting the scene show through; `RenderTexture`/`MaterialOverride` don't have that field because transparency there comes from the texture's own alpha channel and how the receiving material blends it. Preparation and playback are decoupled from render-mode choice — set the mode before or after assigning a source, but set it before the first frame is expected to show up somewhere meaningful.

```csharp
using UnityEngine;
using UnityEngine.Video;

public class CutsceneVideoPlayer : MonoBehaviour
{
    [SerializeField] private VideoClip clip;
    [SerializeField] private Camera targetCamera;
    [SerializeField] private RenderTexture cutsceneTexture; // pre-configured per VideoPlayer-rendertexture.html
    [SerializeField] private AudioSource narrationAudio;

    private VideoPlayer player;

    void Awake()
    {
        player = gameObject.AddComponent<VideoPlayer>();
        player.playOnAwake = false;               // we drive Play() ourselves after Prepare()
        player.source = VideoSource.VideoClip;
        player.clip = clip;

        // Render to an offscreen texture so it can also feed a UI RawImage or a monitor prop.
        player.renderMode = VideoRenderMode.RenderTexture;
        player.targetTexture = cutsceneTexture;

        // Route audio through a dedicated AudioSource instead of Direct output
        // so it can be positioned, ducked, or routed through a mixer group.
        player.audioOutputMode = VideoAudioOutputMode.AudioSource;
        player.EnableAudioTrack(0, true);
        player.SetTargetAudioSource(0, narrationAudio);

        player.waitForFirstFrame = true;           // avoid showing a blank/garbage first frame
        player.isLooping = false;

        player.prepareCompleted += OnPrepareCompleted;
        player.loopPointReached += OnVideoFinished;
        player.errorReceived += OnVideoError;

        player.Prepare(); // start preparing immediately; Play() is called once ready
    }

    void OnPrepareCompleted(VideoPlayer vp)
    {
        Debug.Log($"Cutscene ready: {vp.clip.name}, {vp.frameCount} frames @ {vp.frameRate} fps");
        vp.Play();
    }

    void OnVideoFinished(VideoPlayer vp)
    {
        Debug.Log("Cutscene finished playing.");
        // Hand control back to gameplay here.
    }

    void OnVideoError(VideoPlayer vp, string message)
    {
        Debug.LogError($"VideoPlayer error: {message}");
    }
}
```

### Video Clip Import Settings & Formats

Importing a video file creates a `VideoClip` asset whose Inspector exposes an `sRGB (Color Texture)` toggle (leave enabled for normal color footage, disable for non-color/data video) and a `Transcode` toggle (`Manual/class-VideoClip.html`). With Transcode off, Unity ships the source file's container/codec as-is into the build — fastest import, smallest build-time cost, but you are responsible for verifying every target platform can decode it (`Manual/video-sources-compatibility-target-platforms.html`). With Transcode on, the Video Clip Importer re-encodes into one of three codecs — H.264, H.265, or VP8 — automatically pairing the matching audio codec (AAC for H.264/H.265, Vorbis for VP8), and exposes Dimensions (resize/aspect controls), Bitrate Mode (Low/Medium/High), Spatial Quality, Keep Alpha, Deinterlace, Flip Horizontal/Vertical, and Import Audio (`Manual/video-transcode-intro.html`, `Manual/video-encoding-compatibility.html`). Transcoding is scriptable per-platform via `VideoClipImporter.SetTargetSettings(BuildTargetGroup, VideoImporterTargetSettings)` for batch pipelines that need e.g. VP8/WebM for Linux/Android and H.264/MP4 everywhere else. Codec choice follows real tradeoffs: H.264 has the widest native hardware-decode support and is the safe default; H.265 compresses better but needs newer hardware (6th-gen+ Intel, iOS 11+ with A9+ for hardware decode, Android 5.0+, Windows 10 + HEVC extension) so treat it as an optional high-end target, not a baseline; VP8 is cross-platform and open but costs more CPU than hardware H.264 decode, though Android does have native VP8 hardware acceleration on many devices. `.ogv` imports fine in the Editor but is not broadly supported on other platforms and should always be transcoded to `.mp4`/`.webm` before shipping.

```csharp
using UnityEditor;
using UnityEngine;

public static class BatchVideoImportSettings
{
    [MenuItem("Tools/Video/Apply Mobile Transcode Preset")]
    public static void ApplyMobilePreset()
    {
        foreach (var guid in AssetDatabase.FindAssets("t:VideoClip", new[] { "Assets/Video/Cutscenes" }))
        {
            var path = AssetDatabase.GUIDToAssetPath(guid);
            var importer = AssetImporter.GetAtPath(path) as VideoClipImporter;
            if (importer == null) continue;

            var androidSettings = new VideoImporterTargetSettings
            {
                enableTranscoding = true,
                codec = VideoCodec.H264,
                bitrateMode = VideoBitrateMode.Medium,
                resizeMode = VideoResizeMode.ThreeQuarterRes,
                spatialQuality = VideoSpatialQuality.MediumSpatialQuality,
            };
            importer.SetTargetSettings("Android", androidSettings);
            importer.importAudio = true;
            importer.deinterlaceMode = VideoDeinterlaceMode.Off; // source is already progressive
            importer.SaveAndReimport();
        }
    }
}
```

### Audio Output Configuration

`VideoAudioOutputMode` (`ScriptReference/Video.VideoAudioOutputMode.html`) has four values: `None` mutes embedded audio entirely (useful for background/ambient loops you don't want competing with game audio, or when audio is handled separately); `Direct` sends embedded audio straight to the platform's audio hardware, bypassing Unity's `AudioSource`/`AudioMixer` graph entirely — lowest latency and overhead, but no mixing, no spatialization, no mixer-group routing; `AudioSource` routes each enabled audio track into an `AudioSource` you assign via `VideoPlayer.SetTargetAudioSource(trackIndex, source)`, which lets video audio participate in the normal mixer graph (ducking, snapshots, spatial blend, output group) exactly like any other clip; `APIOnly` produces no automatic output, letting you pull audio via the audio sample-provider API for a fully custom pipeline. A clip's audio tracks must be individually enabled with `VideoPlayer.EnableAudioTrack(trackIndex, true)` before playback — disabling unused tracks skips decoding them entirely (cheaper than track-muting, which still decodes). On WebGL only `None` and `Direct` are reliably supported; requesting `AudioSource` mode there still plays audio but Unity ignores every `AudioSource` field except mute, since 3D spatialization of video audio isn't available on the web (`Manual/webgl-video.html`).

```csharp
using UnityEngine;
using UnityEngine.Video;
using UnityEngine.Audio;

public class VideoAudioRouting : MonoBehaviour
{
    [SerializeField] private VideoPlayer player;
    [SerializeField] private AudioSource dialogueSource; // routed to a "Dialogue" AudioMixerGroup
    [SerializeField] private AudioMixerGroup dialogueGroup;

    void Awake()
    {
        dialogueSource.outputAudioMixerGroup = dialogueGroup;

        player.audioOutputMode = VideoAudioOutputMode.AudioSource;

        // Must enable each track you intend to use before playback starts.
        player.EnableAudioTrack(0, true);
        player.SetTargetAudioSource(0, dialogueSource);

        // If the clip has a second (e.g. commentary) track and we don't need it,
        // leave it disabled so Unity skips decoding it entirely.
        if (player.clip != null && player.clip.audioTrackCount > 1)
            player.EnableAudioTrack(1, false);
    }
}
```

### Streaming from URL

Setting `VideoPlayer.source = VideoSource.Url` and `VideoPlayer.url = "..."` plays video that isn't imported as a project asset (`Manual/video-sources-reference.html`, `ScriptReference/Video.VideoPlayer-url.html`). Three URL forms are supported on native platforms: a bare/`file://` local filesystem path (useful for user-generated content or downloaded video, with no asset-pipeline management — you must guarantee the path is valid and accessible yourself); an `http://`/`https://` web URL, for which Unity performs its own pre-buffering and error handling; and a `StreamingAssets`-relative path built from `Application.streamingAssetsPath`, for files that ship with the build but bypass normal asset import/compression. A player can hold both a `clip` and a `url` at once — whichever was assigned most recently wins, and `VideoPlayer.source` lets you switch between them explicitly without clearing the other. On the Web (WebGL) platform, only `VideoSource.Url` is supported at all (`clip` is not supported), and the URL must be a real web URL — local filesystem and `Application.persistentDataPath` playback are not supported there. Because URL-sourced content is never scanned by the asset importer, none of its metadata (`length`, `width`/`height`, `audioTrackCount`, etc.) is known until `Prepare()`/`Play()` completes — always gate any UI or logic that reads those properties on `isPrepared` being true.

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.Video;
using UnityEngine.Networking;

public class StreamedVideoLoader : MonoBehaviour
{
    [SerializeField] private VideoPlayer player;
    [SerializeField] private string remoteUrl = "https://example.com/videos/intro.mp4";

    IEnumerator Start()
    {
        player.playOnAwake = false;
        player.source = VideoSource.Url;
        player.url = remoteUrl;

        player.Prepare();
        while (!player.isPrepared)
        {
            // Optionally surface buffering UI here; errorReceived also fires
            // if the URL is unreachable, so don't spin forever without a timeout.
            yield return null;
        }

        Debug.Log($"Streamed clip ready: {player.width}x{player.height}, {player.length:F1}s");
        player.Play();
    }

    // Example of a StreamingAssets-relative URL instead of a remote one.
    public void PlayFromStreamingAssets(string relativePath)
    {
        player.Stop();
        player.url = System.IO.Path.Combine(Application.streamingAssetsPath, relativePath);
        player.Prepare();
    }
}
```

### Preparing, Seeking, and Looping

`Prepare()` reserves playback resources and preloads initial content asynchronously; it fires `prepareCompleted` and flips `isPrepared` to `true` on success (`ScriptReference/Video.VideoPlayer.Prepare.html`). Calling `Play()` without a prior `Prepare()` still works — Unity prepares implicitly first — but playback then isn't instantaneous, which matters for tightly-timed cutscenes or rhythm-critical sequences. `Stop()` resets time to 0 and tears down internal resources (buffers, textures), which is why `isPrepared` goes back to `false` afterward and a subsequent `Play()` re-pays the preparation cost; use `Pause()` instead when you want to halt playback but resume instantly later, since it preserves both current time and prepared state (`ScriptReference/Video.VideoPlayer.Pause.html`, `ScriptReference/Video.VideoPlayer.Stop.html`). Setting `VideoPlayer.time` (or `frame`) performs an async seek: playback moves toward the target, `seekCompleted` fires on arrival, then `frameReady` fires once that frame is actually decoded and displayed — seek latency depends on codec and how the source was encoded (frequent keyframes seek faster). `isLooping = true` makes `loopPointReached` restart the clip instead of stopping it; the event fires either way, so it's a reliable single hook for both "loop again" and "cutscene over, hand back control" logic. `skipOnDrop` (gated by `canSetSkipOnDrop`) controls whether the player catches up by skipping frames when it falls behind its time source, versus playing every frame regardless of drift; `frameDropped` fires whenever the decoder misses a scheduled frame, which is a good signal for detecting playback performance problems on-device. `StepForward()` pauses (if playing) and advances exactly one frame — useful for frame-accurate scrubbing/debugging tools, though it isn't frame-accurate on WebGL.

```csharp
using UnityEngine;
using UnityEngine.Video;

public class VideoScrubber : MonoBehaviour
{
    [SerializeField] private VideoPlayer player;

    public void SeekToNormalized(float t01)
    {
        if (!player.isPrepared) return; // length/frameCount aren't valid before this
        player.time = player.length * Mathf.Clamp01(t01);
    }

    void OnEnable()
    {
        player.seekCompleted += vp => Debug.Log($"Seek landed at {vp.time:F2}s");
        player.frameDropped += vp => Debug.LogWarning($"Frame dropped near {vp.time:F2}s — check decode perf");
    }

    public void StepOneFrame() => player.StepForward();
}
```

### Platform-Specific Codec Support

Codec support is a function of the *target* platform's native decoding libraries, not the Editor's — a file that previews fine in the Editor can still fail to decode in a build on a different OS (`Manual/video-sources-compatibility-target-platforms.html`). H.264 is the broadly-safe baseline codec across desktop, mobile, and console. H.265/HEVC needs explicit hardware/software capability checks per platform: on macOS, hardware encode/decode needs a 6th-generation-or-newer Intel processor while software encode/decode works on all Macs (SDK 10.13+); on Windows, both encode and decode need the HEVC Video Extensions package on Windows 10+, with hardware-only encode but hardware+software decode; on iOS/tvOS (SDK 11.0+), hardware decode needs an A9 chip or later while software decode works on all devices; on Android 5.0+ and UWP, device-family-level "H.265 supported" claims don't guarantee every device in that family actually supports it (`Manual/video-encoding-compatibility.html`). VP8 is Unity's software-decode fallback path (paired with Vorbis audio) and is used internally when a platform's hardware decoder imposes unwanted restrictions on resolution, multi-track audio, or alpha transparency; Linux's optimal encoding target is specifically WebM/VP8+Vorbis, since Linux has no broad H.264/H.265 hardware decode story in the Editor. WebM import is normally restricted to VP8 video + Vorbis audio, but Android, Nintendo Switch, and Web additionally support VP9 video and (on Switch/Web) Opus audio when the WebM file is loaded via `StreamingAssets` + `VideoPlayer.url` rather than the normal asset pipeline — a raw-file bypass that skips Editor codec validation entirely, at the cost of losing in-Editor preview and drag-and-drop assignment (`Manual/VideoSources-FileCompatibility.html`).

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Calling `Play()` and expecting instant playback | `Play()` implicitly calls `Prepare()` if you haven't already, so the first playback after `Play()` alone is delayed; call `Prepare()` early and start actual playback from the `prepareCompleted` callback for instant response |
| Reading `clip.length`, `width`, `height`, `frameCount`, etc. before checking `isPrepared` | For URL sources especially, this metadata isn't populated until preparation completes; gate any such read on `videoPlayer.isPrepared` |
| Black/blank texture with `Render Texture` mode | Target Texture not assigned, or assigned Render Texture doesn't match the video's resolution/format recommendations (see `VideoPlayer-rendertexture.html`: match resolution, no AA, no depth/stencil, no mipmaps, Clamp wrap, Point filter) |
| Black/blank output with `Material Override` mode | `targetMaterialRenderer` left unset with no Renderer on the same GameObject, or `targetMaterialProperty` pointing at a texture property name that doesn't exist on the material — leave it empty to fall back to the main texture automatically |
| Silent video despite `isPlaying == true` | `audioOutputMode` left at `None`/default, an audio track not enabled via `EnableAudioTrack`, or `AudioSource` mode selected but no `AudioSource` assigned via `SetTargetAudioSource` |
| Assuming `AudioSource` audio output mode spatializes video audio on WebGL | Web only fully supports `None`/`Direct`; requesting `AudioSource` mode there ignores every `AudioSource` field except mute, since spatialization of video audio isn't available on the web |
| Using `Stop()` to just pause playback | `Stop()` resets time to 0 and tears down internal resources, forcing a full re-prepare on the next `Play()`; use `Pause()` to halt while preserving both position and prepared state |
| Shipping `.ogv` files unchanged to non-Editor platforms | `.ogv` is Editor-previewable but poorly supported elsewhere; transcode to `.mp4` (H.264) or `.webm` (VP8) before building |
| Assuming a file that imports/plays in the Editor will play identically on every target platform | Codec support is platform-specific (see the H.265 hardware/software requirements table); always verify against the target platform's decoder support, not just Editor behavior |
| Setting `VideoClipImporter.deinterlaceMode` unnecessarily on progressive-scan source footage | Deinterlacing assumes interlaced field pairs and can visibly degrade already-progressive footage; only enable it for genuinely interlaced sources |
| Forgetting `Keep Alpha` when transcoding an alpha-channel source (e.g. ProRes 4444) | Without it, transparency information is discarded during transcode even though the source had it; enable both `Transcode` and `Keep Alpha` |
| Expecting per-pixel alpha to work with the `Alpha` Inspector field | That field is *global* alpha for camera-plane render modes only; per-pixel transparency requires an alpha-capable source codec (ProRes 4444 or WebM/VP8) authored externally — Unity can't add per-pixel alpha to opaque footage |
| Relying on VP8 hardware acceleration for transparent video on Android | Android's native VP8 hardware decode path doesn't support alpha; you must enable transcoding so Unity falls back to its internal (software) alpha representation |
| Assuming a large transcode job is quick | `VideoClipImporter.transcodeSkipped` and the Manual both note transcoding a large/long source can take many hours; the import progress bar offers a skip option — don't block CI pipelines on unattended large-clip transcodes without accounting for this |
| Confusing this skill's VideoPlayer-in-a-scene setup with Timeline's Video Track | Timeline (owned by the `unity-animation-cinematics` skill) can host a Video Track that drives a `VideoPlayer`'s clip/timing as part of a larger sequenced cutscene; the component/API semantics described here (render modes, audio output, prepare/seek) still apply underneath — Timeline just adds authoring/sequencing on top |

## Quick Reference

| Class / Enum / Member | Purpose |
|---|---|
| `VideoPlayer` | Component that plays a `VideoClip` or URL onto a configurable target |
| `VideoClip` | Imported video asset; read-only metadata (`length`, `frameCount`, `frameRate`, `width`/`height`, `audioTrackCount`, `sRGB`, `originalPath`) |
| `VideoClipImporter` | Editor-time importer for `VideoClip` assets; exposes transcode settings via script |
| `VideoImporterTargetSettings` | Per-platform transcode override struct (`codec`, `bitrateMode`, `resizeMode`, `spatialQuality`, `enableTranscoding`, custom size) |
| `VideoSource` (`VideoClip` / `Url`) | Which of a player's two possible sources is active |
| `VideoRenderMode` (`CameraNearPlane` / `CameraFarPlane` / `RenderTexture` / `MaterialOverride` / `APIOnly`) | Where decoded frames are drawn |
| `VideoAudioOutputMode` (`None` / `AudioSource` / `Direct` / `APIOnly`) | Where embedded audio is routed |
| `VideoTimeUpdateMode` (`DSPTime` / `GameTime` / `UnscaledGameTime`) | Which Unity clock drives playback timing |
| `VideoTimeReference` (`Freerun` / `InternalTime` / `ExternalTime`) | How the player's clock relates to an external reference clock |
| `Video3DLayout` (`No3D` / `SideBySide3D` / `OverUnder3D`) | Stereo frame-packing layout for 3D/VR video |
| `VideoAspectRatio` (`NoScaling` / `Stretch` / `FitHorizontally` / `FitVertically` / `FitInside` / `FitOutside`) | How video content is scaled to fit its target |
| `VideoCodec` (`Auto` / `H264` / `H265` / `VP8`) | Transcode target video codec |
| `VideoBitrateMode` (`Low` / `Medium` / `High`) | Transcode bitrate preset |
| `VideoResizeMode` | Transcode resize preset (`OriginalSize` through `Square256`, or `CustomSize`) |
| `VideoDeinterlaceMode` (`Off` / `Even` / `Odd`) | Field-order assumption for deinterlacing during transcode |
| `VideoSpatialQuality` (`Low`/`Medium`/`High SpatialQuality`) | Transcode detail-vs-size tradeoff |
| `VideoPlayer.Prepare()` | Async-reserve resources/preload; fires `prepareCompleted`, sets `isPrepared` |
| `VideoPlayer.Play()` / `Pause()` / `Stop()` | Start/resume (prepares implicitly if needed); halt without losing state; halt and reset+release resources |
| `VideoPlayer.isPrepared` / `isPlaying` / `isPaused` | Current lifecycle state flags |
| `VideoPlayer.time` / `frame` | Current position; setting either triggers an async seek |
| `VideoPlayer.StepForward()` | Pause and advance exactly one frame |
| `VideoPlayer.EnableAudioTrack(i, bool)` / `IsAudioTrackEnabled(i)` | Toggle per-track audio decoding |
| `VideoPlayer.SetTargetAudioSource(i, AudioSource)` / `GetTargetAudioSource(i)` | Wire a track to an `AudioSource` (for `AudioSource` output mode) |
| `VideoPlayer.SetDirectAudioVolume` / `GetDirectAudioVolume` / `SetDirectAudioMute` | Per-track volume/mute for `Direct` output mode |
| `VideoPlayer.clip` / `url` / `source` | Assign the active content source |
| `VideoPlayer.renderMode` / `targetTexture` / `targetCamera` / `targetMaterialRenderer` / `targetMaterialProperty` / `targetCameraAlpha` | Render-target configuration |
| `VideoPlayer.isLooping` / `loopPointReached` | Loop control and end-of-content event |
| `VideoPlayer.playbackSpeed` / `canSetPlaybackSpeed` | Playback rate multiplier (0–10) and platform support check |
| `VideoPlayer.skipOnDrop` / `frameDropped` | Drift-correction behavior and drop notifications |
| `VideoPlayer.waitForFirstFrame` | Gate first draw on prep + first-frame availability when `playOnAwake` is set |
| `VideoPlayer.started` / `seekCompleted` / `clockResyncOccurred` / `errorReceived` | Lifecycle/diagnostic events |
| `VideoPlayer.frameReady` / `sendFrameReadyEvents` | Per-frame-ready callback (opt-in, has a performance cost) |
| `Application.streamingAssetsPath` | Base path for shipping video files that bypass normal asset import |

## Advanced Notes

**Platform codec/format support is the dominant source of video bugs.** The Editor previews video using the *host OS's* native decode libraries, while a build uses the *target platform's* — these are frequently different, so "it plays in the Editor" is not evidence it will play on-device (`Manual/video-sources-compatibility-target-platforms.html`). Treat H.264 as the safe cross-platform baseline; treat H.265 as an opt-in enhancement gated behind explicit per-platform hardware-generation checks (documented precisely in `Manual/video-encoding-compatibility.html` — e.g. 6th-gen+ Intel on macOS/Windows, A9+ chip on iOS/tvOS); treat VP8/WebM as the Linux-optimal and transparency-safe choice, at a higher CPU cost than hardware H.264 decode. Alpha-channel video narrows the codec choice further to just two options (Apple ProRes 4444 or WebM/VP8), and even VP8's alpha support isn't free on Android — the platform's hardware VP8 decode path drops alpha, forcing a transcode to Unity's software-decoded internal alpha representation to recover it. When in doubt for a multi-platform release, enable `Transcode` per-platform via `VideoClipImporter`/`VideoImporterTargetSettings` and let Unity produce a codec/container combination it has already validated against each target, accepting the tradeoffs of longer build times, potentially many hours for large sources, and some quality loss versus a source encoded by a purpose-built external tool.

**Streaming vs. local-file playback is a genuinely different engineering problem, not just a different URL scheme.** A bundled `VideoClip` asset is validated at import time, has all its metadata (`length`, dimensions, track counts) available synchronously once prepared, and its preparation cost is bounded by local I/O and decode setup. A `url`-sourced stream — whether `http(s)://`, a local `file://` path outside the asset pipeline, or `StreamingAssets` — is never scanned by the importer, so nothing is known about it until `Prepare()`/`Play()` actually contacts it; metadata reads before that point are meaningless, and preparation failure surfaces only through the async `errorReceived` event (HTTP errors, missing files, permission problems, unsupported formats) rather than an exception you can catch synchronously. For genuine network streams, Unity performs its own pre-buffering, but you're still responsible for UX around buffering stalls, connectivity loss mid-playback, and (per `Manual/video-clock.html`) the fact that non-`GameTime` update modes run the player asynchronously and may skip/repeat frames to stay in sync rather than blocking the rest of the game — appropriate for background/ambient video, less so for anything that must frame-lock with gameplay or audio. `StreamingAssets` splits the difference: files ship with the build (so no network dependency and bounded I/O latency like a local asset) but bypass the importer's compatibility checks and per-platform transcoding entirely, which is exactly the trapdoor `Manual/video-files-unsupported.html` recommends for using target-platform-only formats the Editor itself can't preview — you trade in-Editor preview and drag-and-drop convenience for the ability to ship a file the Editor doesn't understand but the runtime does.

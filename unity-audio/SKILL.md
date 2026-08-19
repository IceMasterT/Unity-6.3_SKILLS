---
name: unity-audio
description: Use when implementing audio in Unity — AudioSource/AudioMixer setup, spatial audio, or mixer snapshots. Grounds answers in the local Unity 6.3 docs over pretrained knowledge.
---

# Unity Audio

## Retrieval Sources

| Source | Path | Use for |
|--------|------|---------|
| Audio Source component reference | `Manual/AudioSource-reference.html` | Full Inspector property reference: Audio Generator, Output, Mute, Spatialize, Bypass flags, Priority, Volume, Pitch, Stereo Pan, Spatial Blend, Reverb Zone Mix, 3D sound settings (Doppler, Spread, Min/Max Distance, Volume Rolloff), and the per-property distance curve editor |
| Audio Source (index) | `Manual/class-AudioSource.html` | Landing page linking Introduction, component reference, and setup instructions for the Audio Source |
| `AudioSource` scripting API | `ScriptReference/AudioSource.html` | Runtime members: `Play`, `PlayOneShot`, `PlayDelayed`, `PlayScheduled`, `PlayClipAtPoint`, `Pause`/`UnPause`, `Stop`, `isPlaying`, `isVirtual`, `outputAudioMixerGroup` |
| Audio Clip Import Settings reference | `Manual/class-AudioClip.html` | Import Inspector fields: Force To Mono, Normalize, Load In Background, Ambisonic, and per-platform overrides for Load Type, Compression Format, Sample Rate Setting, Quality |
| `AudioClip` scripting API | `ScriptReference/AudioClip.html` | Procedural clips via `AudioClip.Create`, `GetData`/`SetData`, `LoadAudioData`/`UnloadAudioData`, `loadState` |
| Audio file compatibility | `Manual/AudioFiles-compatibility.html` | Supported source formats: MP3, AIFF, WAV, OGG Vorbis, FLAC, plus tracker modules (`.mod`, `.xm`, `.it`, `.s3m`); channel limits (mono/stereo/up to 8 channels) |
| Import audio files into Unity | `Manual/AudioFiles-import.html` | Import workflow (menu import vs. drag-and-drop) |
| Audio file compression in Unity | `Manual/AudioFiles-compression.html` | PCM vs. Vorbis/MP3 vs. ADPCM tradeoffs; module file handling; Linux `ffmpeg` dependency |
| Introduction to the Audio Mixer | `Manual/AudioMixerOverview.html` | Core concepts: groups, routing vs. scene graph, sound categories, ducking, moods/snapshots, the "global mix" |
| Specifics on the Audio Mixer window | `Manual/AudioMixerSpecifics.html` | Mixers panel, Hierarchy panel (add/reparent/duplicate groups), AudioGroup strip view (Solo/Mute/Bypass), Snapshot panel, Views panel |
| AudioGroup Inspector | `Manual/AudioMixerInspectors.html` | Attenuation Unit, Effect Units, Send/Receive Units, Duck Volume Units, exposing parameters to script, transition overrides, Auto Mixer Suspend/Threshold Volume |
| Audio Mixer (landing page) | `Manual/audio-mixer-landing.html` | Index into all Audio Mixer manual sub-topics |
| `AudioMixer.SetFloat` | `ScriptReference/Audio.AudioMixer.SetFloat.html` | Exact signature (`bool SetFloat(string name, float value)`) for driving an exposed parameter at runtime; return value signals a bad/unexposed name |
| `AudioMixerSnapshot.TransitionTo` | `ScriptReference/Audio.AudioMixerSnapshot.TransitionTo.html` | Blend every exposed parameter toward a snapshot over `timeToReach` seconds |
| Introduction to ambisonic audio | `Manual/AmbisonicAudio.html` | B-format WAV import (ACN ordering, SN3D normalization), selecting a decoder in Project Settings, disabling `Spatialize` on ambisonic sources, reverb zones being disabled for ambisonic clips |
| Develop an ambisonic audio decoder | `Manual/AudioDevelopAmbisonicDecoder.html` | Implementing and registering a custom ambisonic decoder plug-in |
| Ambisonic audio (landing page) | `Manual/audio-ambisonic.html` | Index into ambisonic sub-topics |
| Audio effects | `Manual/audio-effects.html` | DSP units usable **inside an Audio Mixer group**: Low/High Pass, Echo, Flange, Distortion, Normalize, Parametric EQ, Pitch Shifter, Chorus, Compressor, SFX Reverb |
| Audio filters | `Manual/audio-filters.html` | Filter **components attachable directly to an Audio Source or Audio Listener GameObject**: Low Pass, High Pass, Echo, Distortion, Reverb, Chorus |
| Audio Reverb Filter | `Manual/class-AudioReverbFilter.html` | Reverb parameters (room, decay time/HF ratio, diffusion, density, reflections) and presets used by both `AudioReverbFilter` and `AudioReverbZone` |
| Reverb Zones | `Manual/class-AudioReverbZone.html` | Min/Max Distance gizmo radii, Reverb Preset, position-based environmental reverb that blends as the listener moves |
| Audio Listener | `Manual/class-AudioListener.html` | Single-listener-per-scene rule, typical attachment to Main Camera, interaction with Reverb Zones and Audio Effects |
| Audio Settings | `Manual/class-AudioSettings.html` | Runtime reconfiguration of speaker mode, sample rate, DSP buffer size, and voice counts via `AudioSettings.Reset`; `OnAudioConfigurationChanged` callback; note that all audio objects reload on reconfiguration |
| `AudioConfiguration.dspBufferSize` | `ScriptReference/AudioConfiguration-dspBufferSize.html` | Latency-controlling field; code example changing buffer size via `AudioSettings.Reset` |
| `AudioSettings.GetDSPBufferSize` | `ScriptReference/AudioSettings.GetDSPBufferSize.html` | Ring buffer `bufferLength`/`numBuffers` semantics; explicit guidance that smaller = lower latency but higher CPU, larger = smoother parameter changes |
| Audio Spatializer SDK | `Manual/AudioSpatializerSDK.html` | Custom HRTF/binaural spatializer plug-ins, per-source `Spatialize`/`Spatialize Post Effect`, `SetSpatializerFloat`/`GetSpatializerFloat` |
| Audio playlist randomization (Audio Random Container) | `Manual/AudioRandomContainer.html` + `Manual/AudioRandomContainer-fundamentals.html` | Weighted/randomized playlists for footsteps, impacts, weapons; Manual vs. Automatic playback mode differences from a plain `AudioClip` |
| Audio Profiler module reference | `Manual/ProfilerAudio.html` | Playing/Paused/Virtual voice counts, DSP CPU vs. Streaming CPU vs. Other CPU, per-source Audibility/Distance/Plays columns for diagnosing voice-stealing and CPU spikes |
| XR audio | `Manual/xr-audio.html` | Spatializer + ambisonic decoder usage for VR/AR immersion |
| Audio in Web (WebGL) | `Manual/webgl-audio.html` | Web Audio API backend (no FMOD threads), AAC transcoding, Chrome autoplay policy, iOS Silent Mode bug with `DecompressOnLoad`, unsupported Scriptable Audio Pipeline |

## Key Guidelines

### AudioSource Fundamentals (2D vs 3D)

`AudioSource.spatialBlend` is the single most consequential property to set deliberately: `0` is fully 2D (UI, menu music — constant volume regardless of listener position), `1` is fully 3D positional (world SFX, attenuated and panned by distance/direction), and values in between blend both for near-field effects that shouldn't fully vanish at range. 3D sources are further shaped by `rolloffMode` (`Logarithmic` — physically accurate, loud up close then tapers slowly; `Linear` — even falloff to `maxDistance`; `Custom` — driven by the Volume distance curve), `minDistance` (the "stays at max volume" radius), and `priority` (0 = never stolen, 256 = stolen first; default 128). Route output through an `AudioMixerGroup` instead of leaving it at the default Audio Listener output so a category volume slider can affect it later.

```csharp
using UnityEngine;

public class GunshotSource : MonoBehaviour
{
    [SerializeField] private AudioSource worldSfx;   // 3D
    [SerializeField] private AudioSource uiClickSfx;  // 2D

    void Awake()
    {
        // World SFX: fully 3D, physically-plausible falloff, never stolen for gameplay-critical cues.
        worldSfx.spatialBlend = 1f;
        worldSfx.rolloffMode = AudioRolloffMode.Logarithmic;
        worldSfx.minDistance = 2f;
        worldSfx.maxDistance = 40f;
        worldSfx.priority = 64; // more important than default 128

        // UI SFX: fully 2D, unaffected by camera position.
        uiClickSfx.spatialBlend = 0f;
    }
}
```

### AudioMixer & Snapshots

An `AudioMixer` sits between `AudioSource`s and the `AudioListener`, mixing signals through a hierarchy of `AudioGroup`s independent of the scene graph. Distance-based 3D attenuation, Doppler, and reverb-zone mixing are applied at the Audio Source *before* the signal enters the mixer, so mixer-side effects should be reserved for category-wide mastering (ducking dialogue under music, a global underwater low-pass, a pause-menu low-pass-everything effect). Never drive category volume through each `AudioSource.volume` field — right-click a group parameter (Volume, Pitch, Send Level, Wet Level, or any effect parameter) in the AudioGroup Inspector and choose **Expose to script**, then drive it at runtime with `AudioMixer.SetFloat(name, value)`. Exposed values on groups are in decibels, not linear 0–1, so convert a linear UI slider with `Mathf.Log10(value) * 20`. `SetFloat` returns `false` if the name doesn't match an exposed parameter — check it, since a typo fails silently otherwise. For discrete mood changes (paused, underwater, low-health) use **Snapshots** instead of scripting individual parameter writes: `AudioMixerSnapshot.TransitionTo(timeToReach)` (or `AudioMixer.TransitionToSnapshots` to blend multiple) interpolates every exposed parameter in that snapshot over the given duration, using each snapshot's configured transition curve (linear by default, overridable per-parameter). Note that a manually-set `SetFloat` value on a parameter is locked and stops responding to snapshot transitions until cleared with `AudioMixer.ClearFloat`.

```csharp
using UnityEngine;
using UnityEngine.Audio;

public class MixerVolumeController : MonoBehaviour
{
    [SerializeField] private AudioMixer masterMixer;
    [SerializeField] private AudioMixerSnapshot normalSnapshot;
    [SerializeField] private AudioMixerSnapshot underwaterSnapshot;

    // Called from a UI slider (0..1 linear).
    public void SetMusicVolume(float linear01)
    {
        float dB = linear01 > 0.0001f ? Mathf.Log10(linear01) * 20f : -80f;
        if (!masterMixer.SetFloat("MusicVolume", dB))
            Debug.LogWarning("MusicVolume is not an exposed AudioMixer parameter.");
    }

    public void EnterWater() => underwaterSnapshot.TransitionTo(1.5f);
    public void ExitWater() => normalSnapshot.TransitionTo(1.5f);
}
```

### Audio Import & Compression

Choose compression by clip role, not blanket policy: **PCM** for short, frequent SFX (uncompressed, cheapest on CPU, largest on disk/memory); **ADPCM** for mid-length, frequently-triggered SFX like footsteps and impacts (fixed ~3.5x compression, more CPU than PCM but far less than Vorbis, less suited to smooth music due to audible artifacts); **Vorbis/MP3** for music and long ambience (best size/quality ratio via the Quality slider, higher CPU cost). Pair compression with **Load Type**: `Decompress On Load` for small frequent sounds (avoids per-play decode cost, but decompressing Vorbis uses ~10x more memory than staying compressed, ADPCM only ~3.5x — never use it for large files); `Compressed In Memory` when memory is the constraint over CPU (decompression happens on the mixer thread — watch DSP CPU in the Audio Profiler); `Streaming` for long music/dialogue (minimal memory, decode on a separate streaming thread, but every streamed clip carries a fixed ~200KB overhead even unloaded). Import settings can be scripted per-platform via `AudioImporter`/`AudioImporterSampleSettings` for batch pipelines.

```csharp
using UnityEditor;
using UnityEngine;

public static class BatchAudioImportSettings
{
    [MenuItem("Tools/Audio/Apply SFX Compression Preset")]
    public static void ApplySfxPreset()
    {
        foreach (var guid in AssetDatabase.FindAssets("t:AudioClip", new[] { "Assets/Audio/SFX" }))
        {
            var path = AssetDatabase.GUIDToAssetPath(guid);
            var importer = AssetImporter.GetAtPath(path) as AudioImporter;
            if (importer == null) continue;

            var settings = new AudioImporterSampleSettings
            {
                loadType = AudioClipLoadType.CompressedInMemory,
                compressionFormat = AudioCompressionFormat.ADPCM,
                sampleRateSetting = AudioSampleRateSetting.PreserveSampleRate,
            };
            importer.SetOverrideSampleSettings("Standalone", settings);
            importer.SaveAndReimport();
        }
    }
}
```

### Spatial / Ambisonic Audio

Standard 3D positional audio in Unity uses simple stereo panning driven by distance/angle — enough for most games. For higher-fidelity binaural output (VR/XR headphones), install an Audio Spatializer SDK plug-in, select it in **Project Settings > Audio**, then enable `Spatialize` (and optionally `Spatialize Post Effect`, to apply the spatializer after other Source effects) per `AudioSource`. Ambisonic audio is a *different* mechanism for 360° soundfields (skyboxes, VR ambience): import a multi-channel B-format WAV with ACN component ordering and SN3D normalization, enable **Ambisonic** on the clip's import settings, assign it to an `AudioSource`, select an Ambisonic Decoder Plugin project-wide, and **disable** `Spatialize` on that source — the ambisonic decoder already performs spatialization as part of decoding, so leaving both on double-processes or conflicts. Reverb zones are disabled for ambisonic clips by design.

```csharp
using UnityEngine;

public class VrAmbientAudio : MonoBehaviour
{
    [SerializeField] private AudioSource pointSfx;      // regular 3D SFX, wants HRTF
    [SerializeField] private AudioSource ambisonicBed;  // 360 ambisonic skybox audio

    void Start()
    {
        // Binaural point source: rely on the installed spatializer plug-in.
        pointSfx.spatialize = true;
        pointSfx.spatializePostEffects = false;

        // Ambisonic bed: decoder handles spatialization — must NOT also spatialize.
        ambisonicBed.spatialize = false;
        ambisonicBed.Play();
    }
}
```

### Pooling AudioSources

Never `Instantiate` a GameObject+AudioSource per sound effect — the GC churn and Instantiate/Destroy overhead is wasteful for anything that fires often (gunshots, footsteps, impacts). For truly fire-and-forget one-shots that share a source's output settings, `AudioSource.PlayOneShot` layers a clip on an existing source without interrupting what it's already playing. For overlapping, independently-controlled sounds (need their own volume/pitch/position or need to be stopped individually), maintain a small pool of reusable `AudioSource` components instead.

```csharp
using System.Collections.Generic;
using UnityEngine;

public class AudioSourcePool : MonoBehaviour
{
    [SerializeField] private AudioSource sourcePrefab;
    [SerializeField] private int poolSize = 16;

    private readonly Queue<AudioSource> available = new();

    void Awake()
    {
        for (int i = 0; i < poolSize; i++)
        {
            var src = Instantiate(sourcePrefab, transform);
            src.gameObject.SetActive(false);
            available.Enqueue(src);
        }
    }

    public AudioSource PlayAt(AudioClip clip, Vector3 position, float volume = 1f)
    {
        if (available.Count == 0) return null; // all voices busy; drop or steal lowest-priority here

        var src = available.Dequeue();
        src.transform.position = position;
        src.clip = clip;
        src.volume = volume;
        src.gameObject.SetActive(true);
        src.Play();
        StartCoroutine(ReclaimWhenDone(src, clip.length));
        return src;
    }

    private System.Collections.IEnumerator ReclaimWhenDone(AudioSource src, float delay)
    {
        yield return new WaitForSeconds(delay);
        src.gameObject.SetActive(false);
        available.Enqueue(src);
    }
}
```

### Audio Filters vs. Audio Effects (don't conflate them)

Unity documents two distinct DSP mechanisms that are easy to confuse. **Audio Filters** (`AudioLowPassFilter`, `AudioHighPassFilter`, `AudioEchoFilter`, `AudioDistortionFilter`, `AudioReverbFilter`, `AudioChorusFilter`) are *components* you attach directly to the same GameObject as an `AudioSource` or the `AudioListener` — they process only that one source's signal (or, on the Listener, everything reaching it). **Audio Effects** (Low/High Pass, Echo, Flange, Distortion, Normalize, Parametric EQ, Pitch Shifter, Chorus, Compressor, SFX Reverb) are DSP units added inside an **AudioMixer group's** effect chain — they process every source routed through that group at once, which is almost always the better tool for a category-wide change (e.g. "everyone sounds muffled underwater") since it's one mixer edit instead of toggling a filter component on every active source.

```csharp
using UnityEngine;

public class UnderwaterMuffle : MonoBehaviour
{
    [SerializeField] private AudioLowPassFilter playerVoiceFilter; // per-source filter, narrow scope
    [SerializeField] private UnityEngine.Audio.AudioMixer worldMixer; // category-wide via exposed effect param

    public void EnterWater()
    {
        // Narrow: only this one source (e.g. local player's own breathing SFX).
        playerVoiceFilter.enabled = true;
        playerVoiceFilter.cutoffFrequency = 800f;

        // Broad: every SFX/ambience routed through the "World" mixer group.
        worldMixer.SetFloat("WorldLowPassCutoff", 800f);
    }
}
```

## Common Mistakes

| Mistake | Why it happens / fix |
|---|---|
| Setting `AudioSource.volume` for a global mute/volume slider | Bypasses the mixer; instead expose a mixer parameter and call `AudioMixer.SetFloat` so it affects every source on that bus |
| Forgetting `spatialBlend = 1` on 3D world sounds | Default new AudioSource is 2D in some templates; sound plays at constant volume regardless of distance |
| `Instantiate`-ing a GameObject+AudioSource per gunshot/footstep | Garbage-collection churn and Instantiate overhead; use `PlayOneShot` or a pooled AudioSource instead |
| Using linear volume interpolation via `Mathf.Lerp` on `AudioSource.volume` | Sounds non-linear to human hearing; use mixer snapshot transitions (decibel-based) instead |
| Importing all clips as PCM "for quality" | Bloats build size and memory; use Vorbis/ADPCM per the compression guidance above |
| Too many simultaneous AudioSources active at once | Triggers Unity's voice-stealing, causing audible pops/cutoffs; cap concurrent instances and set `priority` |
| Confusing Audio Filters with Audio Effects | Filter components (`AudioLowPassFilter`, etc.) process one Source/Listener; Effects live inside an AudioMixer group and process every routed source — use Effects for category-wide changes instead of toggling filters on many sources |
| Leaving `Spatialize` enabled on an ambisonic-encoded AudioSource | The ambisonic decoder already performs spatialization as part of decoding; also enabling the source's `Spatialize` conflicts with/duplicates that processing — disable `Spatialize` for ambisonic clips |
| Ignoring the `bool` return value of `AudioMixer.SetFloat` | A typo'd or unexposed parameter name fails silently — the call returns `false` and nothing happens; check it during development |
| Setting a mixer parameter with `SetFloat` and expecting snapshots to still animate it | A scripted `SetFloat` locks that parameter until `AudioMixer.ClearFloat` is called; it stops responding to `AudioMixerSnapshot.TransitionTo` |
| Using `Decompress On Load` for long music tracks | Decompressing Vorbis-encoded audio uses ~10x more memory than compressed; use `Streaming` for long music, reserve `Decompress On Load` for small frequent clips |
| Reducing DSP Buffer Size project-wide "for lower latency" without profiling | Smaller buffers mean lower latency but more CPU overhead (cache misses, DSP network overhead) and higher risk of audible glitches on constrained hardware; leave it on Unity's default unless a measured latency problem demands it |
| Calling `AudioListener.pause = true` as a "mute everything" toggle | Pauses all audio uniformly, including UI/menu feedback sounds that should keep playing over a paused game; mute a dedicated mixer group instead |
| Assuming `AudioSource.Play()` behaves identically on an Audio Random Container as on a plain `AudioClip` | In `Manual` mode it selects a clip per the container's Playback Mode (e.g. `Random`) instead of just replaying the assigned clip — read the Audio Random Container fundamentals before scripting against one |
| Adding more than one `AudioListener` to a scene (e.g. after an additive scene load) | Unity only supports one active Listener per scene; extras cause warnings and unpredictable audio — disable/destroy the redundant one on scene load |
| Setting `AudioSource.priority` uniformly (or never) | Default 128 for everything means Unity's voice-stealing has no signal to prioritize by; set 0 for critical music/dialogue and higher values for disposable SFX so voice-stealing drops the right sounds first |

## Quick Reference

| Component/Setting | Purpose |
|---|---|
| `AudioSource` | Plays an `AudioClip` (or Audio Random Container); volume, pitch, loop, spatialBlend, priority, output group |
| `AudioListener` | Receives 3D audio at a position (usually the camera); exactly one active per scene |
| `AudioClip` | Imported/procedural audio data; Force To Mono, Ambisonic flag, per-platform Load Type/Compression/Sample Rate overrides |
| `AudioMixer` / `AudioMixerGroup` | Routes and processes sources through a hierarchy of buses independent of the scene graph |
| Exposed Parameter | Mixer float (dB) controllable at runtime via `AudioMixer.SetFloat`/`GetFloat`/`ClearFloat` |
| `AudioMixerSnapshot` | Saved state of all exposed parameters; blend between states with `TransitionTo` or `AudioMixer.TransitionToSnapshots` |
| Send / Receive Unit | Diverges a (possibly attenuated) copy of a group's signal to another Effect Unit for side-chaining |
| Duck Volume Unit | Side-chain compressor driven by a Send, for automatic ducking (e.g. music under dialogue) |
| Auto Mixer Suspend | Per-mixer CPU-saving setting that suspends processing after output falls below a loudness Threshold Volume |
| Audio Filter (`AudioLowPassFilter`, `AudioHighPassFilter`, `AudioEchoFilter`, `AudioDistortionFilter`, `AudioReverbFilter`, `AudioChorusFilter`) | Per-Source or per-Listener DSP component |
| Audio Effect (Mixer DSP unit) | Per-group DSP unit inside an AudioMixer (Low/High Pass, Echo, Flange, Distortion, Normalize, Parametric EQ, Pitch Shifter, Chorus, Compressor, SFX Reverb) |
| `AudioReverbZone` | Position-based environmental reverb with Min/Max Distance falloff and a Reverb Preset |
| Load Type (`DecompressOnLoad` / `Streaming` / `CompressedInMemory`) | Controls memory vs. CPU tradeoff per clip |
| Compression Format (`PCM` / `ADPCM` / `Vorbis`/`MP3`) | Controls CPU vs. disk/memory tradeoff per clip |
| Audio Random Container | Weighted/randomized playlist asset for footsteps, impacts, props; Manual vs. Automatic playback modes |
| Audio Spatializer SDK plug-in | Custom HRTF/binaural processing, selected in Project Settings > Audio, toggled per-source via `Spatialize` |
| Ambisonic Decoder Plugin | Project-wide decoder for B-format (ACN/SN3D) soundfield clips; disable `Spatialize` on sources using it |
| `AudioConfiguration` / `AudioSettings` | Runtime speaker mode, sample rate, DSP buffer size, and voice-count control via `AudioSettings.Reset` |
| DSP Buffer Size (Project Settings > Audio) | `Best Latency` / `Good Latency` / `Best Performance` presets trading responsiveness for CPU/stability |
| Max Real Voices / Virtual Voices | Caps concurrent audible ("real") sources; excess sources become inaudible "virtual" voices instead of hard-cutting |
| Audio Profiler module | Playing/Paused/Virtual voice counts, DSP/Streaming/Other CPU, per-source Audibility for diagnosing overload |

## Advanced Notes

**Audio format choice per platform.** The default `Compressed` mode picks Vorbis for desktop/mobile standalone builds; you can override per-platform in the Audio Clip Import Settings (e.g. force MP3 or ADPCM on a specific target) when a platform's decoder is cheaper or its licensing differs. On **WebGL/Web**, Unity cannot use its normal FMOD-based pipeline because FMOD relies on threads the WebGL API doesn't support — audio there runs through an internal Web Audio API implementation instead, and Unity transcodes `AudioClip`s to AAC regardless of source format. Two Web-specific gotchas: Chrome's autoplay policy blocks any audio (including background music set to autoplay) until the user interacts with the page via click/tap/keypress, so gate audio behind a loading/start screen; and on iOS Silent Mode, `DecompressOnLoad` clips are silenced because WebKit routes that audio-node type differently than the `MediaElementSourceNode` used for `CompressedInMemory` clips — set affected clips to `CompressedInMemory` to keep them audible in Silent Mode. Also note Linux builds require the `ffmpeg` package installed for audio clip import/support, and the Scriptable Audio Pipeline is unsupported on Web builds entirely.

**DSP buffer size and latency tuning.** The audio engine mixes into a ring buffer defined by `bufferLength` (samples per block) × `numBuffers` (block count), configurable via **Project Settings > Audio > DSP Buffer Size** (`Best Latency`, `Good Latency`, `Best Performance`) or at runtime through `AudioSettings.Reset(AudioConfiguration)`. Smaller buffers lower the latency between a script's volume/pitch/pan change and its audible effect, but increase CPU usage (more frequent DSP graph evaluation, more cache misses) and raise the risk of audible glitches/underruns on constrained hardware; anything above roughly 20ms of buffer latency makes parameter changes noticeably steppy rather than smooth. Unity's default is tuned per output type and driver and is deliberately conservative — the docs explicitly recommend leaving it alone unless you have a measured latency requirement (e.g. music/rhythm games needing tight sync). Any runtime change via `AudioSettings.Reset` reloads every audio object: disk-based `AudioClip`s and Audio Mixers survive automatically, but procedurally-generated or script-modified `AudioClip`s are lost and must be recreated in the `AudioSettings.OnAudioConfigurationChanged` callback, along with any play-state you need to restore. Use the Audio Profiler's DSP CPU / Streaming CPU / Other CPU breakdown to determine whether a performance problem is actually buffer-size-related before touching this setting — a high "Other CPU" or excessive Playing/Virtual voice counts usually point to voice management (priority, Max Real Voices) rather than DSP latency.

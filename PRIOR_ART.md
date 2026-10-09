# Amiga-Family Prior Art

Snapshot date: **9 October 2026**

Purpose: record existing AROS, classic AmigaOS, AmigaOS 4 and MorphOS application-port work that may reduce the cost or risk of AmigaChrome ports.

This is not a claim that historical source can always be reused directly. Licence, source availability, age, architecture and provenance must be checked per application.

## AROS Contrib / AROS Ports

AROS is especially valuable because it often contains explicit Amiga-like portability patches rather than only finished binaries.

### Multimedia and codec foundations

Confirmed in AROS Contrib:

- **FFmpeg 8.1.2**
  - explicit AROS target support;
  - static build;
  - m68k-specific configuration;
  - m68k recipe disables networking and HEVC decode;
  - patch file: `MultiMedia/libs/ffmpeg/ffmpeg-8.1.2-aros.diff`.
- **FLAC**
- **Ogg**
- **Vorbis**
- **Opus**
- **Speex**
- **Theora**
- **FAAD2**
- **OpenAL**
- **freealut**
- **MikMod**
- **ModPlug**
- **PTPlay**
- **SoX 12.17.4** portability patch.

These are high-value because FFmpeg/OpenMedia, game audio, media players, transcoding and audio-editing all depend on overlapping pieces of this stack.

### Applications

Confirmed AROS application prior art includes:

- **MilkyTracker 1.05.01**
- **ScreenRecorder**
- **Radium**
- **GrafX2 2.9**
- **LodePaint**
- **ZunePaint**
- **ZuneView**
- **Netpbm**
- **Potrace**
- **POV-Ray**
- **XaoS**
- **c-ray**
- **MUIbase 3.3**
- **AROSPDF**
- **VPDF**
- **Text2PDF**
- **FryingPan**
- **Vim**
- **Antiword**
- **GOCR**
- **Mathomatic**

### Emulation

Confirmed AROS emulation prior art includes:

- **MAME 0.36 RC1** — very old, but useful as evidence of a small-generation MAME port on an Amiga-family OS.
- **DOSBox**
- **ScummVM** — AROS patch history includes 1.8.0, 1.9.0 and 2.2.0.
- **VBA-M**
- **VICE**
- **E-UAE / UAE**

The old AROS MAME README is particularly interesting because it documents platform constraints directly: 8-bit rendering assumptions, manual frameskip and substantial memory use. It should be treated as architectural archaeology, not as our release target.

## Aminet / classic AmigaOS

The classic 68k ecosystem is the most important prior-art source when code or patches are available because it is closest to our actual ABI/CPU target.

High-value discoveries from the current sweep:

- **RetroArch 1.20 for 68k AmigaOS**
  - frontend already ported to classic AmigaOS;
  - separate cores available;
  - dramatically reduces uncertainty around a RetroArch revival.
- **ScummVM 2.5.1 AGA/68060 lineage**
  - confirms a much newer ScummVM generation has run on classic 68k than the AROS patch history alone suggests.
- **MAME 0.106 MiniMix lineage**
  - potentially a much more appropriate AC090 starting generation than current monolithic MAME.
- **DOSBox** classic Amiga RTG/AGA lineages.
- **FFmpeg** classic m68k history.
- **Amico8 / PICO-8 style emulator** on 020+ AmigaOS.

For classic ports, source availability must be checked carefully. A binary package is useful evidence but not automatically reusable implementation work.

## AmigaOS 4 / OS4Depot

OS4 is valuable for relatively modern upstream versions and for seeing how large Unix/SDL/C++ applications were adapted to Amiga APIs.

High-value findings:

- **FFmpeg 9.0.1** — modern Amiga-family FFmpeg evidence.
- **FFmpegGUI** — proves the small-native-GUI-over-FFmpeg application pattern.
- **VideoClipper** — another useful FFmpeg-driven workflow/UI reference.
- **MilkyTracker** SDL2 lineage.
- **ScummVM 2026.2.0** with a large engine set.
- **Mednafen** including Sony PlayStation support.
- **mGBA**.
- **ImageMagick 6.8.9** binary/source port lineage.
- **Blender 2.48** lineage.
- **GIMP 2.6** via AmiCygnix/X11.
- subtitle/editor utilities such as **SimpleSub**.

The main architectural lesson is that OS4 proves many large applications can be made Amiga-aware, but AmigaChrome should avoid copying heavyweight X11/GTK/Cygnix frontend strategies when an OpenGadTools/Open-family solution is cleaner.

## MorphOS Storage

MorphOS is especially valuable for maintained SDL2/OpenGL/FFmpeg-era applications and native non-SDL backends.

High-value findings:

- **FinalBurn Neo**
  - current MorphOS SDL2 port;
  - strong arcade-emulation candidate;
  - likely better near-term return than attempting full modern MAME.
- **ScummVM 2026.3.0**
  - modern dependency stack including SDL2, FluidSynth, FLAC, Theora, FAAD, VPX and MikMod.
- **MPlayer 1.5.2 / FFmpeg 6.1.6 lineage**
  - valuable source of Amiga-family video/audio/output portability ideas.
- **GrafX2 2.9**
  - current-ish graphics application precedent.
- **MilkyTracker 1.05**
  - corroborates AROS/OS4 tracker portability.
- **MAME 0.148**
  - native MUI-oriented port with non-SDL architecture;
  - useful backend/UI reference even if we choose a different MAME core generation.
- **DOSBox**
  - native backend/JIT work worth studying.
- **FPSE**
  - PlayStation emulator evidence; code reuse depends on licence/source availability.
- native audio-editor applications such as **WaveEdit / SoundFX** for UI/feature reference.

## Implications for AmigaChrome

### RetroArch

Status should be **revive/adapt**, not greenfield port.

Best route:

1. recover classic 68k frontend portability work;
2. modernise against a suitable current RetroArch baseline;
3. replace historical video/audio/input/threading glue with:
   - OpenGPU
   - OpenAudio
   - OpenInput
   - OpenMulticore
4. bring up one lightweight core first;
5. use the frontend to unlock additional emulator cores.

### Arcade emulation

Do not assume current MAME is the correct first target.

Evaluate three routes:

1. **FinalBurn Neo** via modern MorphOS SDL2 prior art;
2. **MAME 0.106-ish** via classic 68k prior art;
3. selected **libretro MAME/FBNeo cores** under RetroArch.

Current full MAME can remain a later capability target.

### PlayStation

Use RetroArch as the preferred frontend layer.

Candidates:

- PCSX-ReARMed interpreter core;
- Mednafen PSX knowledge from OS4;
- FPSE only as behavioural/performance prior art unless its source/licence permits more.

### FFmpeg / transcoding

FFmpeg is now a **foundation project**, not a speculative application port.

Recommended architecture:

`OpenTranscode UI -> FFmpeg/libav* -> OpenMedia hardware acceleration`

Retain ffmpeg/ffprobe CLI tools for scripting and testing.

Use OS4 FFmpegGUI/VideoClipper as workflow references rather than copying their UI wholesale.

### Audio editing

No verified direct Audacity port was found in the official AROS sweep.

Recommended architecture:

`OpenAudioEdit UI -> SoX / libsndfile / FFmpeg / codec libs -> OpenAudio`

Use MorphOS audio editors and Audacity itself as feature/workflow references.

### Graphics

GrafX2 should be treated as a high-confidence revival candidate because multiple Amiga-family systems already have ports.

ImageMagick, Netpbm and Potrace are complementary CLI foundations rather than competitors to a paint application.

## Standard research record

For every new application candidate, capture:

- upstream project and exact source pin;
- licence;
- last upstream release/activity;
- AROS prior art;
- classic AmigaOS/Aminet prior art;
- AmigaOS 4 prior art;
- MorphOS prior art;
- GUI/toolkit dependencies;
- audio/video/graphics/input/threading dependencies;
- likely Open-family mappings;
- expected RAM/CPU footprint;
- redistributable versus user-supplied data;
- build status;
- AC090 runtime status.

The aim is to stop solving solved Amiga portability problems twice.

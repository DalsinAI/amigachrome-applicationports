# Unified Application Port Backlog

Snapshot date: **9 October 2026**

Primary target: **AmigaChrome AC090 / AmigaOS 3.x / 68040 + FPU**

This is the single non-game application-port backlog for AmigaChrome. The **Category** column groups ports by application type so the catalogue can be reviewed by domain as well as priority. Emulators, emulator frontends and compatibility runtimes live with games in `amigachrome-gameports`. It combines fresh candidates with AROS, classic AmigaOS, AmigaOS 4 and MorphOS prior art so priority decisions can be made from the whole field.

Priority scale:

- **P0** — finish / enable now
- **P1** — next delivery wave
- **P2** — planned
- **P3** — stretch
- **PARK** — research / low return for now

## Governing porting rule

Application-port work should be preceded by one question:

> **What need does this remove to use another platform for?**

The highest-value ports are the ones that let an AmigaChrome user complete a real task end-to-end without switching to Linux, Windows, macOS or another machine. A port should therefore justify itself in terms of the external workflow it replaces, not merely because the upstream application is technically interesting.

Examples:

- FFmpeg / OpenTranscode -> removes the need to use another platform for media conversion and delivery encoding.
- OpenAudioEdit / SoX -> removes the need to use another platform for recording, trimming and restoring audio.
- GrafX2 / image tools -> removes the need to use another platform for creating and processing graphics.
- OpenRecorder -> removes the need to use another platform for screen and audio capture.
- Vim / developer tooling -> removes the need to use another platform for editing and maintaining source code.
- PDF/document tools -> removes the need to use another platform for routine document viewing, conversion and output.

This **platform-substitution value** should be assessed before engineering effort begins.

A successful compile is not a release. RELEASE still requires reproducible provenance, the approved GCC stove, packaging and AC090 runtime qualification.

| Application / tool | Category | Platform need replaced | Current state | Amiga-family prior art | Intended AmigaChrome treatment | Priority | Next meaningful gate |
|---|---|---|---|---|---|---|---|
| FFmpeg / ffprobe | Media Infrastructure | Media conversion, probing, filtering and scripted processing beyond datatype playback | FOUNDATION CANDIDATE | AROS Contrib has FFmpeg 8.1.2 with an m68k configuration; AmigaOS 4 and MorphOS also have modern FFmpeg ports | Keep CLI tools and libraries as creation/editing/transcoding infrastructure; OpenMedia owns hardware codec acceleration and DataTypes own routine playback/viewing | P0 | Reproduce the AROS m68k build with the fixed GCC16 baseline, then define the clean OpenMedia acceleration boundary |
| MediaInfo / libmediainfo | Media Infrastructure | Inspect codecs, containers, streams and media metadata beyond current datatype probe fields | CANDIDATE / POSSIBLE THIN TOOL | Cross-platform upstream; existing media.decode/1 PROBE already supplies basic kind/format/frame/size/rate metadata | First assess extending OpenMedia/OpenService probe metadata; port libmediainfo only if that is materially better | P2 | Gap analysis against media.decode/1 PROBE and OpenPlay About-this-file before importing a new dependency |
| TagLib | Media Infrastructure | Read/write common audio metadata and tags locally | CANDIDATE | Cross-platform upstream; Amiga-family prior art to be checked during intake | Shared metadata library for players, editors and music tools | P2 | Pin source and GCC16 library build |
| libsndfile | Media Infrastructure | Common PCM/audio-file I/O for editors, converters and analysis tools | CANDIDATE | Cross-platform upstream; Amiga-family prior art to be checked during intake | Shared audio-file library beneath OpenAudioEdit and utilities | P1 | Pin source, supported-format audit and GCC16 build |
| libsamplerate | Media Infrastructure | High-quality sample-rate conversion without external tooling | CANDIDATE | Cross-platform upstream; Amiga-family prior art to be checked during intake | Shared resampling engine; expose OpenMulticore for batch/offline work where useful | P1 | Pin source, GCC16 build and quality/performance test |
| SoX | Audio / Processing | Audio conversion, DSP and batch processing | REVIVAL CANDIDATE | AROS Contrib has SoX 12.17.4 portability patch | Revive a current SoX; CLI first; later DSP engine for audio editor | P1 | Diff old AROS portability patch against current SoX |
| SoundTouch | Audio / Processing | Change tempo, pitch and playback rate without leaving AmigaChrome | CANDIDATE | Portable LGPL C++ library; upstream designed for cross-platform and embedded use | Shared OpenAudioEdit processing library; parallelise offline/batch jobs where useful | P2 | Pin source, GCC16 build and reference quality/performance test |
| RNNoise | Audio / Processing | Speech/noise cleanup without an external workstation | CANDIDATE | BSD-licensed reusable Xiph noise-suppression library | OpenAudioEdit/OpenAudio processing backend; prefer host-side/OpenMulticore execution for expensive inference | P2 | Prove portable scalar build, define offload boundary and process a reference WAV |
| OpenAudioEdit | Audio / Editing | Record, trim, repair and edit audio | NEW APPLICATION | No direct Audacity port found in official AROS Contrib/Ports sweep; MorphOS has native audio editor prior art | Audacity-like waveform editor built from portable DSP/audio engines with OpenAudio + OpenGadTools | P1 | Define MVP: record, waveform view, cut/copy/paste, fades, normalize, save/export |
| MilkyTracker | Music / Tracker | Tracker music creation and module editing | REVIVAL CANDIDATE | AROS Ports has 1.05.01 recipe; OS4 and MorphOS ports also exist | Rebuild on OpenGPU/OpenAudio/OpenInput; keep tracker UI | P1 | Import AROS patch set, clean GCC16 build, audio first light |
| Schism Tracker | Music / Tracker | Create and edit Impulse Tracker-style module music without another platform | REVIVAL CANDIDATE | Current SDL2/GCC project; upstream notes portability to architectures including m68k | Rebuild on OpenGPU/OpenAudio/OpenInput; retain tracker UI and keyboard workflow | P1 | Pin current release and prove GCC16/Open SDL2 build |
| libopenmpt / openmpt123 | Music / Module Playback | Play, render and inspect a wide range of tracker module formats locally | FOUNDATION CANDIDATE | Current cross-platform library/CLI with actively maintained 2026 releases | Shared module decoder beneath players/editors plus native CLI renderer/player | P1 | Pin 0.8.x, GCC16 build and MOD/XM/S3M/IT playback tests |
| Furnace | Music / Chiptune | Compose and export multi-system chiptune without leaving AmigaChrome | CANDIDATE | Current GPL tracker with SDL/OpenGL/software-renderer support | Prefer OpenGPU/OpenAudio backend; assess software-renderer path first | P2 | Pin current source, dependency audit and first UI/audio build |
| Radium | Music / Tracker | Advanced music composition/tracker workflow | INVESTIGATE | Present in AROS Contrib | Assess as higher-end tracker/music workstation | P2 | Licence/dependency/current-upstream review |
| OpenTranscode | Video / Transcoding | Video conversion, resizing, codec/container delivery jobs | NEW APPLICATION | OS4 FFmpegGUI / VideoClipper style prior art proves GUI-over-FFmpeg model | Native OpenGadTools transcoder using FFmpeg + OpenMedia hardware acceleration | P1 | UI/job model and one H.264 -> H.265 hardware-assisted transcode proof |
| OpenVideoEdit | Video / Editing | Cut, arrange, trim, filter and export video projects without another platform | NEW APPLICATION | MLT and existing Amiga-family FFmpeg work provide reusable engine prior art | Native AmigaChrome NLE UI over MLT/FFmpeg/OpenMedia; background render/export through OpenMulticore | P1 | Define MVP timeline, two video/audio tracks, trim/cut, transitions, preview and export |
| MLT Framework | Video / Editing Infrastructure | Reusable non-linear editing engine for OpenVideoEdit and automation | FOUNDATION CANDIDATE | Current cross-platform multimedia framework; upstream explicitly targets NLE/broadcast/transcode use | Build engine without heavyweight desktop UI assumptions; map decode/encode to FFmpeg/OpenMedia | P2 | Pin current 7.x source, dependency audit and headless melt/render proof |
| MKVToolNix CLI | Video / Container Tools | Create, alter, inspect and repair Matroska files locally | CANDIDATE | Mature cross-platform CLI suite; Amiga-family prior art to be checked during intake | Port CLI tools first; native GUI only if a real workflow gap remains | P2 | Pin current source, isolate CLI dependencies and build mkvmerge/mkvinfo subset |
| GPAC / MP4Box | Video / Container Tools | Package, inspect and manipulate MP4/HEIF/DASH/HLS media locally | CANDIDATE | Mature cross-platform media framework/tool suite; Amiga-family prior art to be checked | Start with MP4Box CLI; integrate OpenMedia/FFmpeg only where it materially helps | P2 | Pin current 26.x source and build minimal MP4Box feature set |
| OpenRecorder | Video / Capture | Screen and audio capture | NEW APPLICATION | AROS Ports contains ScreenRecorder; OpenMedia already targets encode services | Native screen/audio capture recorder using ACRTG/OpenAudio/OpenMedia | P1 | Capture AC090 display + audio and encode one clip |
| GrafX2 | Graphics / Paint | Pixel art and bitmap graphics creation | REVIVAL CANDIDATE | AROS Ports has 2.9 patch; OS4/MorphOS ports exist | Native pixel-art/paint app using OpenGPU/OpenInput | P1 | Clean GCC16/OpenGPU build |
| LodePaint | Graphics / Paint | General image editing | REVIVAL CANDIDATE | AROS Ports contains port | Assess lightweight image editor role | P2 | Current-source and dependency review |
| ZunePaint | Graphics / Paint | General image editing | PRIOR ART / REVIVAL OPTION | AROS Ports contains port | Mine native Amiga UI ideas; consider revival if useful beside GrafX2 | P2 | Feature/source review |
| ZuneView | Graphics / Viewer | Image viewing | PRIOR ART / REVIVAL OPTION | AROS Ports contains port | Lightweight image viewer candidate | P2 | Source review and OpenGFX/OpenGPU mapping |
| ImageMagick | Graphics / Processing | Scripted/batch image manipulation | REVIVAL CANDIDATE | AmigaOS 4 binary/source port exists | Native CLI image-processing suite | P1 | Recover portability changes and build a small command subset |
| GraphicsMagick | Graphics / Processing | Scripted/batch image manipulation | CANDIDATE | No confirmed Amiga-family port in current sweep | Alternative lighter image-processing suite | P2 | Compare footprint/build complexity with ImageMagick |
| Netpbm | Graphics / Processing | Format conversion and simple image processing | REVIVAL CANDIDATE | AROS Contrib port exists | CLI image conversion toolkit | P1 | GCC16 clean build |
| Potrace | Graphics / Vectorisation | Bitmap-to-vector conversion | REVIVAL CANDIDATE | AROS Ports port exists | Bitmap-to-vector CLI utility | P1 | GCC16 clean build and SVG/EPS output test |
| POV-Ray | 3D / Rendering | Offline 3D rendering | REVIVAL CANDIDATE | AROS Contrib port exists | Useful renderer and CPU/OpenMulticore benchmark | P2 | Recover build, render reference scene |
| c-ray | 3D / Rendering | Lightweight rendering/benchmarking | REVIVAL CANDIDATE | AROS Contrib source exists | Small renderer/benchmark | P2 | GCC16 build and reference image |
| XaoS | Graphics / Visualisation | Interactive mathematical visualisation | REVIVAL CANDIDATE | AROS Contrib port exists | Lightweight graphics/compute showcase | P2 | Clean build and interactive runtime |
| Blender 2.4x lineage | 3D / Authoring | 3D modelling/authoring reference path | PRIOR ART / STRETCH | AmigaOS 4 Blender 2.48 lineage exists | Historical source/portability reference; not the immediate 3D authoring target | P3 | Study OS4 changes and decide if a scoped legacy Blender revival has value |
| GIMP lineage | Graphics / Editing | Advanced image-editing reference path | PRIOR ART / STRETCH | AmigaOS 4 ran GIMP through AmiCygnix/X11 | Mine feature expectations only; avoid adopting X11/Cygnix as Open app architecture | PARK | No work until a native UI/engine strategy exists |
| MUIbase | Office / Database | Desktop database work / OpenBase prior art | PRIOR ART | AROS Ports has MUIbase 3.3 patch | Feed lessons into OpenBase rather than create a competing product | P2 | Compare data model/UI/import features with OpenBase design |
| AROSPDF / VPDF | Documents / PDF | PDF viewing | PRIOR ART / CANDIDATE | AROS Contrib contains both | PDF viewing/printing prior art | P2 | Decide whether to revive or build around existing OpenPrint/document stack |
| Text2PDF | Documents / PDF | Generate PDF from text | REVIVAL CANDIDATE | AROS Contrib port exists | Small OpenPrint companion utility | P2 | GCC16 build and print/export smoke |
| FryingPan | Storage / Optical Media | Optical-disc mastering/burning | REVIVAL CANDIDATE | AROS Contrib contains optical-disc authoring app | Optical media utility for AmigaChrome | P2 | Source/licence/device-layer review |
| Vim | Developer / Editor | Source/text editing | REVIVAL CANDIDATE | AROS Ports port exists | Developer-tool backlog | P2 | Build current sensible Vim version and test terminal/editor paths |
| Antiword | Documents / Conversion | Extract/import legacy Word documents | REVIVAL CANDIDATE | AROS Ports port exists | Document text-extraction/import utility | P2 | GCC16 build and DOC extraction test |
| GOCR | Documents / OCR | OCR from scanned/bitmap documents | REVIVAL CANDIDATE | AROS Ports port exists | Lightweight OCR utility | P2 | GCC16 build and simple OCR sample |
| Mathomatic | Scientific / Maths | Symbolic mathematics | REVIVAL CANDIDATE | AROS Ports port exists | Scientific/education utility | P3 | Clean build |
| OpenAL / freealut | Audio / Compatibility | Compatibility for ports needing OpenAL-style APIs | PRIOR ART / COMPATIBILITY | AROS Contrib ports exist | Compatibility option only; OpenAudio remains preferred native target | P3 | Use only where it materially reduces port cost |

### Media playback ownership

Routine media playback is not an application-port backlog item. **OpenPlay** is the native datatype-driven player and **OpenAmigaVLC** is a separate existing project that drives OpenMedia. Applicationports should only add another playback application when it closes a workflow that those two cannot reasonably cover.

### Video product rule

**OpenTranscode**, **OpenRecorder**, **OpenVideoEdit** and **OpenAmigaVLC** are the user-facing video products. HandBrake/libhb, historical AROS ScreenRecorder code and MPlayer are reference/engine prior art rather than separate shipping targets unless a later gap justifies reviving them directly.

### Audio editing product rule

**OpenAudioEdit** is the shipping audio-editor target. Audacity is a workflow and feature reference, not a port target. Reuse portable algorithms/libraries where licences and architecture make sense, but do not import a heavyweight desktop GUI/toolkit merely to preserve the Audacity application shell.

### Audio codec scope rule

Do not turn every codec supported by FFmpeg into a separate port project. Add a standalone codec/library only when it provides at least one of:

- a materially smaller/lighter native dependency than FFmpeg;
- a common upstream dependency for several applications;
- useful native CLI tooling;
- significantly better encode/decode quality or performance for its format;
- existing Amiga-family prior art worth reviving.

Formats such as WavPack, ALAC and other less-common codecs should normally be served through FFmpeg unless a consuming application gives us a concrete reason to maintain their native libraries separately.

## Platform ownership: OpenMedia and DataTypes

Before adding a media port, check whether the requirement is already satisfied by the AmigaChrome media platform:

- **OpenMedia** owns hardware video codec sessions and zero-copy surfaces: H.264, HEVC, VP9, AV1, MPEG-2, VC-1 and JPEG, with decode/encode capability depending on the provider.
- **OpenAmigaMediaLibrary** already has working AC090-tested libwebp 1.6.0 and libvpx 1.17.0 decoder builds.
- **openpicture.datatype** covers AVIF, HEIC/HEIF, JPEG XL, RAW, PSD, XCF, EXR, HDR, QOI, DDS, JPEG 2000, OpenRaster, Krita, CBZ, SVG and more through `media.decode/1`.
- **opensound.datatype** covers FLAC, Ogg/Vorbis, Opus, AAC/M4A, ALAC, WMA, MP3, MIDI, SID and module formats through `media.decode/1`.
- **openmodule.datatype** plays module formats locally using libxmp, with an OpenMulticore/offload ladder.
- **openvideo.datatype** covers MP4/MOV, MKV, AVI, WMV, MPEG, FLV and APNG through `media.decode/1`.
- **webp.datatype** and **webm.datatype** provide local WebP and VP8/VP9 paths.
- **OpenPlay** is already the user-facing datatype-driven media player.

Therefore, a codec/library does **not** get a standalone application-port row merely because a media format exists. Add it only when creation/editing/encoding, a concrete consuming application, or a material performance/compatibility reason requires a native library.

## Scale assumption: 64-192 host cores

AmigaChrome application ports should assume that the surrounding host environment may expose **64 to 192 CPU cores** through OpenMulticore.

This does not mean every legacy application should be naively threaded. It means the port intake should explicitly identify work that can be parallelised or offloaded safely:

- media encode/decode pipelines;
- rendering and ray tracing;
- audio analysis, conversion and restoration;
- compression/decompression;
- image batch processing;
- build/compile workloads;
- transcoding queues and independent jobs;
- background indexing, OCR and document conversion.

Where upstream already uses pthreads, worker pools, task graphs or job queues, prefer mapping those onto OpenMulticore rather than serialising them for AmigaOS. Where a workload is embarrassingly parallel, the Amiga UI process should remain responsive while the heavy work is distributed across the available host cores.

The target is therefore **Amiga semantics at the application boundary, modern parallel execution underneath**.

## Priority lens

Before assigning engineering time, rank each candidate on:

- **Platform substitution value** — how much it removes the need to use another platform.
- **User value** — how useful the completed workflow is.
- **Probability of success** — likelihood of reaching useful first light on AC090.
- **Reuse value** — whether the port unlocks multiple applications or workflows.
- **Open-family value** — whether it proves or strengthens OpenMedia, OpenAudio, OpenGPU, OpenInput, OpenMulticore, OpenPrint or related services.
- **Parallel scale value** — whether useful work can exploit the expected 64-192 core host environment through OpenMulticore or independent job execution.
- **Effort** — engineering and maintenance cost.
- **Dependency friction** — external libraries/toolchains required first.
- **Workflow completeness** — whether the result solves the whole user task or only one fragment of it.

A technically impressive port that still forces the user onto another platform to finish the job should normally rank below a smaller port that closes a complete workflow.

## Standard intake rule

Before writing Amiga portability code for any application:

1. inspect AROS Contrib and AROS Ports;
2. inspect Aminet and classic AmigaOS source/patches;
3. inspect OS4Depot;
4. inspect MorphOS Storage;
5. inspect current upstream;
6. identify which historical patches are still relevant;
7. map graphics/audio/input/media/threading to Open-family interfaces;
8. pin source and produce a reproducible GCC16 build recipe.

## Porting styles

Every backlog item should eventually be classified as one of:

- **Native revival** — retain the original application's UI and port it cleanly.
- **Engine + Open UI** — reuse portable core functionality but build a native AmigaChrome UI.
- **CLI foundation** — expose a command-line engine/tool first.
- **Open service client** — make expensive work use OpenMedia/OpenGPU/OpenAudio/OpenMulticore.
- **Prior-art only** — mine old Amiga-family patches without shipping that historical application.

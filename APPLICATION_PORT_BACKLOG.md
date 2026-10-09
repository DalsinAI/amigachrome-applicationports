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

A successful compile is not a release. RELEASE still requires reproducible provenance, the approved GCC stove, packaging and AC090 runtime qualification.

| Application / tool | Category | Current state | Amiga-family prior art | Intended AmigaChrome treatment | Priority | Next meaningful gate |
|---|---|---|---|---|---|---|
| FFmpeg / ffprobe | FOUNDATION CANDIDATE | AROS Contrib has FFmpeg 8.1.2 with an m68k configuration; AmigaOS 4 and MorphOS also have modern FFmpeg ports | Native GCC16 build; feed OpenMedia decode/encode paths; keep CLI tools | P0 | Reproduce AROS m68k build assumptions with the GCC16 stove and inventory patches worth carrying |
| OpenMedia FFmpeg bridge | DESIGN / PLATFORM CLIENT | AROS FFmpeg prior art plus existing OpenMedia service design | Hardware encode/decode acceleration bridge for FFmpeg clients | P0 | Define AVCodec/AVHWDevice-style boundary to openmedia.library |
| FLAC tools / libFLAC | PORT PRIOR ART | AROS Contrib port exists | Native codec/tool foundation | P0 | GCC16 clean build and CLI encode/decode smoke |
| Ogg / Vorbis tools | PORT PRIOR ART | AROS Contrib ports exist | Native codec/tool foundation | P0 | GCC16 clean build and round-trip test |
| Opus tools / libopus | PORT PRIOR ART | AROS Contrib port exists | Native codec/tool foundation | P0 | GCC16 clean build and encode/decode smoke |
| FAAD2 / AAC decode | PORT PRIOR ART | AROS Contrib port exists | Codec foundation | P1 | Clean build and decode test |
| Speex | PORT PRIOR ART | AROS Contrib port exists | Codec / voice utility foundation | P2 | Clean build and smoke |
| Theora | PORT PRIOR ART | AROS Contrib port exists | Legacy/open video codec support | P2 | Clean build and sample decode |
| SoX | REVIVAL CANDIDATE | AROS Contrib has SoX 12.17.4 portability patch | Revive a current SoX; CLI first; later DSP engine for audio editor | P1 | Diff old AROS portability patch against current SoX |
| OpenAudioEdit | NEW APPLICATION | No direct Audacity port found in official AROS Contrib/Ports sweep; MorphOS has native audio editor prior art | Audacity-like waveform editor built from portable DSP/audio engines with OpenAudio + OpenGadTools | P1 | Define MVP: record, waveform view, cut/copy/paste, fades, normalize, save/export |
| Audacity | INVESTIGATE | No verified AROS/Aminet/OS4/MorphOS port found in current sweep | Mine algorithms/workflows; avoid dragging current desktop GUI stack onto 68k unless proven sensible | P3 | Dependency audit and identify reusable non-UI components |
| MilkyTracker | REVIVAL CANDIDATE | AROS Ports has 1.05.01 recipe; OS4 and MorphOS ports also exist | Rebuild on OpenGPU/OpenAudio/OpenInput; keep tracker UI | P1 | Import AROS patch set, clean GCC16 build, audio first light |
| Radium | INVESTIGATE | Present in AROS Contrib | Assess as higher-end tracker/music workstation | P2 | Licence/dependency/current-upstream review |
| OpenTranscode | NEW APPLICATION | OS4 FFmpegGUI / VideoClipper style prior art proves GUI-over-FFmpeg model | Native OpenGadTools transcoder using FFmpeg + OpenMedia hardware acceleration | P1 | UI/job model and one H.264 -> H.265 hardware-assisted transcode proof |
| HandBrake / libhb | INVESTIGATE | No direct Amiga-family port found in current sweep | Mine job/preset/engine ideas; do not assume full desktop GUI port | P2 | Identify libhb portability boundary and dependencies versus OpenTranscode needs |
| OpenRecorder | NEW APPLICATION | AROS Ports contains ScreenRecorder; OpenMedia already targets encode services | Native screen/audio capture recorder using ACRTG/OpenAudio/OpenMedia | P1 | Capture AC090 display + audio and encode one clip |
| ScreenRecorder (AROS lineage) | PRIOR ART / REVIVAL OPTION | AROS Ports has complete ScreenRecorder source patch set | Mine capture, pointer, scaling and AVI/MNG/PNG logic | P2 | Portability review against ACRTG/OpenMedia capture path |
| VLC / OpenAmigaVLC | EXISTING SEPARATE PROJECT | Existing DalsinAI repo; Amiga-family media-player precedent exists | Keep in its own repo; consume OpenMedia | P1 | Continue OpenMedia-backed decode/display qualification |
| MPlayer prior-art review | RESEARCH | MorphOS has maintained FFmpeg-based MPlayer work | Mine Amiga-family video-output/audio/network patches | P2 | Review MorphOS portability changes worth generalising |
| GrafX2 | REVIVAL CANDIDATE | AROS Ports has 2.9 patch; OS4/MorphOS ports exist | Native pixel-art/paint app using OpenGPU/OpenInput | P1 | Clean GCC16/OpenGPU build |
| LodePaint | REVIVAL CANDIDATE | AROS Ports contains port | Assess lightweight image editor role | P2 | Current-source and dependency review |
| ZunePaint | PRIOR ART / REVIVAL OPTION | AROS Ports contains port | Mine native Amiga UI ideas; consider revival if useful beside GrafX2 | P2 | Feature/source review |
| ZuneView | PRIOR ART / REVIVAL OPTION | AROS Ports contains port | Lightweight image viewer candidate | P2 | Source review and OpenGFX/OpenGPU mapping |
| ImageMagick | REVIVAL CANDIDATE | AmigaOS 4 binary/source port exists | Native CLI image-processing suite | P1 | Recover portability changes and build a small command subset |
| GraphicsMagick | CANDIDATE | No confirmed Amiga-family port in current sweep | Alternative lighter image-processing suite | P2 | Compare footprint/build complexity with ImageMagick |
| Netpbm | REVIVAL CANDIDATE | AROS Contrib port exists | CLI image conversion toolkit | P1 | GCC16 clean build |
| Potrace | REVIVAL CANDIDATE | AROS Ports port exists | Bitmap-to-vector CLI utility | P1 | GCC16 clean build and SVG/EPS output test |
| POV-Ray | REVIVAL CANDIDATE | AROS Contrib port exists | Useful renderer and CPU/OpenMulticore benchmark | P2 | Recover build, render reference scene |
| c-ray | REVIVAL CANDIDATE | AROS Contrib source exists | Small renderer/benchmark | P2 | GCC16 build and reference image |
| XaoS | REVIVAL CANDIDATE | AROS Contrib port exists | Lightweight graphics/compute showcase | P2 | Clean build and interactive runtime |
| Blender 2.4x lineage | PRIOR ART / STRETCH | AmigaOS 4 Blender 2.48 lineage exists | Historical source/portability reference; not the immediate 3D authoring target | P3 | Study OS4 changes and decide if a scoped legacy Blender revival has value |
| GIMP lineage | PRIOR ART / STRETCH | AmigaOS 4 ran GIMP through AmiCygnix/X11 | Mine feature expectations only; avoid adopting X11/Cygnix as Open app architecture | PARK | No work until a native UI/engine strategy exists |
| MUIbase | PRIOR ART | AROS Ports has MUIbase 3.3 patch | Feed lessons into OpenBase rather than create a competing product | P2 | Compare data model/UI/import features with OpenBase design |
| AROSPDF / VPDF | PRIOR ART / CANDIDATE | AROS Contrib contains both | PDF viewing/printing prior art | P2 | Decide whether to revive or build around existing OpenPrint/document stack |
| Text2PDF | REVIVAL CANDIDATE | AROS Contrib port exists | Small OpenPrint companion utility | P2 | GCC16 build and print/export smoke |
| FryingPan | REVIVAL CANDIDATE | AROS Contrib contains optical-disc authoring app | Optical media utility for AmigaChrome | P2 | Source/licence/device-layer review |
| Vim | REVIVAL CANDIDATE | AROS Ports port exists | Developer-tool backlog | P2 | Build current sensible Vim version and test terminal/editor paths |
| Antiword | REVIVAL CANDIDATE | AROS Ports port exists | Document text-extraction/import utility | P2 | GCC16 build and DOC extraction test |
| GOCR | REVIVAL CANDIDATE | AROS Ports port exists | Lightweight OCR utility | P2 | GCC16 build and simple OCR sample |
| Mathomatic | REVIVAL CANDIDATE | AROS Ports port exists | Scientific/education utility | P3 | Clean build |
| OpenAL / freealut | PRIOR ART / COMPATIBILITY | AROS Contrib ports exist | Compatibility option only; OpenAudio remains preferred native target | P3 | Use only where it materially reduces port cost |

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

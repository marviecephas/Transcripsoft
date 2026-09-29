<div align="center">

# Transcripsoft

### Local Offline Video Transcriber

*A standalone, zero-dependency desktop application for Windows. Transcribes video collections locally into structured `transcription.txt` documents with zero cloud reliance.*

<br/>

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-182020?style=flat-square&logo=windows&logoColor=e2e8e8)](https://github.com/marviecephas/Transcripsoft)
[![Mode](https://img.shields.io/badge/Architecture-100%25%20Offline-182020?style=flat-square&logoColor=e2e8e8)](https://github.com/marviecephas/Transcripsoft)
[![Inference](https://img.shields.io/badge/Inference-ONNX%20Runtime%20INT8-182020?style=flat-square&logoColor=e2e8e8)](https://github.com/marviecephas/Transcripsoft)
[![Speech Model](https://img.shields.io/badge/Model-Whisper%20Tiny-182020?style=flat-square&logoColor=e2e8e8)](https://github.com/marviecephas/Transcripsoft)
[![Contributions](https://img.shields.io/badge/Contributions-Open-182020?style=flat-square&logoColor=e2e8e8)](https://github.com/marviecephas/Transcripsoft)
[![License](https://img.shields.io/badge/License-MIT-182020?style=flat-square&logoColor=e2e8e8)](LICENSE)

</div>

---

## Overview

Transcribing batches of video lessons, interviews, presentations, or meetings traditionally requires either sending sensitive audio to cloud servers or setting up heavyweight Python environments with PyTorch, CUDA, and FFmpeg dependencies.

**Transcripsoft** provides a completely self-contained Windows desktop application. All neural inference, speech recognition models, audio demuxers, and runtime libraries are packaged directly within the binary.

- **100% Offline & Private**: Video files and transcribed text never leave your workstation. No telemetry, no cloud APIs, and no internet connection required.
- **Zero Configuration**: Ready to run upon extraction. Requires no Python, Node.js, or external codec installations.
- **Sub-Second Latency**: Leverages quantized INT8 ONNX Runtime for rapid speech decoding on standard x64 CPUs.
- **Automated Document Generation**: Aggregates all video transcripts into a single, standardized `transcription.txt` file placed directly in your source folder.

---

## Tech Stack

| Component | Framework / Technology | Role & Rationale |
| :--- | :--- | :--- |
| **Application Runtime** | **Electron v31** | Isolated cross-platform desktop shell with secure IPC architecture |
| **Neural Execution** | **ONNX Runtime (C++ bindings)** | Low-overhead INT8 neural inference execution on CPU |
| **Speech Recognition** | **OpenAI Whisper (Tiny.en)** | Quantized 39M-parameter English acoustic model (~77 MB) |
| **Media Demuxer** | **Static FFmpeg (v8.1.1)** | Direct 16kHz mono audio extraction streamed via memory pipes |
| **Sample Normalization** | **WaveFile.js** | 16-bit PCM to Float32Array conversion within `[-1.0, 1.0]` |
| **User Interface** | **HTML5 / CSS3 / Vanilla JS** | Low-latency glassmorphic HUD in Charleston Green & Silver palette |

---

## Installation & Usage

Transcripsoft is distributed as a single portable archive (`Transcripsoft.zip`).

### Option 1: Direct Download (Recommended)

1. Download **[`Transcripsoft.zip`](https://github.com/marviecephas/Transcripsoft/raw/main/Transcripsoft.zip)** from this repository.
2. Right-click the `.zip` file and select **Extract All...**.
3. Open the unzipped folder and double-click **`Transcripsoft.exe`** to launch.

---

### Option 2: PowerShell Terminal One-Liner

Run the following command in PowerShell to automatically download, unpack, and launch the application:

```powershell
# Download and extract Transcripsoft
curl.exe -L "https://github.com/marviecephas/Transcripsoft/raw/main/Transcripsoft.zip" -o "$env:USERPROFILE\Downloads\Transcripsoft.zip"
tar.exe -xf "$env:USERPROFILE\Downloads\Transcripsoft.zip" -C "$env:USERPROFILE\Desktop\Transcripsoft"

# Launch Application
Start-Process "$env:USERPROFILE\Desktop\Transcripsoft\Transcripsoft.exe"
```

---

## Package Directory Structure

The extracted `Transcripsoft` directory contains the complete standalone environment:

```
Transcripsoft/
├── Transcripsoft.exe          # Main application executable (Double-click to run)
├── resources/
│   └── app/                   # Self-contained application bundle
│       ├── bin/
│       │   └── ffmpeg.exe     # Bundled static FFmpeg media processor
│       ├── models/            # Bundled Whisper ONNX INT8 speech model
│       ├── node_modules/      # Embedded ONNX Runtime & Transformers engine
│       ├── renderer/          # Dark metallic UI, fonts, styling, and icon assets
│       │   ├── icon.png       # Silver & Charleston Studio Microphone app icon
│       │   ├── icon.svg       # Vector icon source
│       │   ├── index.html     # Application interface structure
│       │   ├── style.css      # Dark metallic styling & animations
│       │   └── renderer.js    # Interface controller & IPC handlers
│       ├── main.js            # Electron background engine & file compiler
│       ├── preload.js         # Context-isolated security bridge
│       └── package.json       # Metadata & engine dependencies
├── locales/                   # Multi-language interface packages
├── chrome_100_percent.pak     # Chromium visual interface resource pack
├── chrome_200_percent.pak     # High-DPI UI scaling bundle
├── resources.pak              # Chromium core resource package
├── icudtl.dat                 # Unicode internationalization data library
├── snapshot_blob.bin          # V8 JavaScript snapshot cache
├── v8_context_snapshot.bin    # Pre-compiled V8 execution context
├── d3dcompiler_47.dll         # Direct3D shader compiler for hardware acceleration
├── ffmpeg.dll                 # Internal audio decoding library
├── libEGL.dll                 # Embedded graphics library rendering interface
├── libGLESv2.dll              # OpenGL ES 2.0 graphics acceleration library
├── vk_swiftshader.dll         # High-performance software Vulkan rasterizer
├── vk_swiftshader_icd.json    # Vulkan driver configuration metadata
├── vulkan-1.dll               # Vulkan graphics runtime loader
├── LICENSES.chromium.html     # Open-source license acknowledgments
└── README.md                  # Project documentation & user guide
```

---

## Workflow & Features

```
[ Step 1: Select Folder ]
          │
          ▼
[ Step 2: Real-time Queue ] ──► (Animated Wireframe Skeletons / Bouncing Guide Arrow)
          │
          ├──► FFmpeg Extracts 16kHz Mono Stream via Memory Pipe
          ├──► WaveFile converts PCM -> Float32Array Samples
          └──► ONNX Runtime executes Whisper Tiny (Offline INT8)
          │
          ▼
[ Step 3: Success Result ]  ──► Automatically saves 'transcription.txt' in video folder
```

1. **Folder Scan**: Select any folder containing video files (`.mp4`, `.mkv`, `.avi`, `.mov`, `.webm`, `.flv`, `.wmv`, etc.).
2. **Dynamic Queue HUD**:
   - **Wireframe Skeleton Rows**: Displays animated placeholder rows with a liquid shimmer wave prior to folder selection.
   - **Inline Bouncing Guide (`↑`)**: A subtle metallic silver arrow guides the user to select a folder from Step 1.
   - **Live 5-Band Audio Equalizer**: An animated wave visualizer pulses during active transcription.
   - **Real-Time Progress**: Individual file progress bars and overall batch percentage indicators.
3. **Automated Export**: Generates `transcription.txt` inside the selected folder and displays a live in-app preview with quick-open buttons.

---

## Output Specification: `transcription.txt`

Transcripts are written to `transcription.txt` in the root of the selected video folder with clear header delimiters:

```text
========================================
FILE: 01_introduction_and_setup.mp4
========================================
Welcome to the course. In this first lesson, we will cover the initial environment setup.

========================================
FILE: 02_core_architecture.mp4
========================================
Now let's explore the core architectural patterns and state management structure.
```

> [!NOTE]
> If a video file contains no spoken audio (e.g. background music or silence), the application writes `(No speech detected)` and continues processing the queue without interruption.

---

## Color Palette & Theme

The user interface is built on a dark industrial palette:

| Swatch | Color Name | Hex Code | Role |
| :---: | :--- | :--- | :--- |
| ![#182020](https://img.shields.io/badge/-%23182020-182020?style=flat-square) | **Charleston Green** | `#182020` | Base atmospheric background radial gradient & table headers |
| ![#121616](https://img.shields.io/badge/-%23121616-121616?style=flat-square) | **Dark Grey** | `#121616` | Card panels, input displays, and console HUD |
| ![#505858](https://img.shields.io/badge/-%23505858-505858?style=flat-square) | **Davy's Grey** | `#505858` | Hairline borders, dividers, and skeleton animation base |
| ![#6e7878](https://img.shields.io/badge/-%236e7878-6e7878?style=flat-square) | **Nickel** | `#6e7878` | Metadata labels, step badge outlines, and scrollbars |
| ![#9aa4a4](https://img.shields.io/badge/-%239aa4a4-9aa4a4?style=flat-square) | **Aluminium** | `#9aa4a4` | Button borders, progress tracks, and table headers |
| ![#ffffff](https://img.shields.io/badge/-%23ffffff-ffffff?style=flat-square) | **Silver** | `#ffffff` | Primary button, brand text, progress beam, and app icon |

---

## Open to Contributions & Roadmap

**Transcripsoft is open to contributions.** We welcome pull requests, optimizations, bug reports, and feature proposals.

### Planned Roadmap
- [ ] **Multilingual Speech Models**: Support for multilingual Whisper checkpoints (`whisper-base`, `whisper-small`, or multilingual ONNX models) with an in-app language dropdown.
- [ ] **Subtitle Generators**: Automated export of `.srt` and `.vtt` timestamped subtitle files alongside `transcription.txt`.
- [ ] **DirectML / WebGPU GPU Acceleration**: Support for GPU execution providers in ONNX Runtime for accelerated transcription.
- [ ] **Pure Audio Ingestion**: Scanner support for `.mp3`, `.wav`, `.m4a`, `.aac`, `.flac`, and `.ogg` files.
- [ ] **Speaker Diarization & Timestamps**: Segment transcripts with speaker turn detection (`[00:01:23] Speaker 1: ...`).

### Contributing
1. Fork the repository and create your branch (`git checkout -b feature/your-feature-name`).
2. Adhere to existing code conventions, keep dependencies zero-setup, and preserve the 100% offline guarantee.
3. Test locally using `npm start`.
4. Open a Pull Request with a clear summary of your changes.

---

## License & Acknowledgments

- **License**: MIT License. See [LICENSE](LICENSE) for details.
- **Dependencies**: Electron, Transformers.js, ONNX Runtime, and static FFmpeg.
- **Speech Model**: OpenAI Whisper.

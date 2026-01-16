# Amethyst-Offline Architecture

## Overview

Amethyst-Offline is an offline-capable version of [Amethyst](https://wiki.angelauramc.dev/), a Minecraft launcher derived from PojavLauncher. It enables users to run Minecraft on Android and iOS devices without requiring online connectivity for core functionality, by applying patches to handle resource downloading and launching offline. The project provides prebuilt releases via GitHub Actions and supports building from source for both platforms. Its purpose is to deliver a portable, self-contained Minecraft launcher for mobile users, emphasizing offline usability while maintaining compatibility with Minecraft's ecosystem.

## Tech Stack

- **Languages**: Java, Kotlin (Android), Objective-C/Swift (iOS)
- **Build Tools**: Gradle (Android), Make (iOS), Bash (scripts), gpatch (iOS patching)
- **Scripts**: Batch (Windows), Shell (Unix-like)
- **Version Control**: Git (with submodules, as indicated by `update-submodules.bat`)
- No specific versions are declared in root-level manifests (e.g., no `package.json` or explicit version files provided). Platform-specific versions are managed within `Amethyst-Android` (Gradle) and `Amethyst-iOS` (Make).

## Project Structure

The repository is organized as a monorepo containing platform-specific source code, patches, build scripts, and documentation. Submodules are used (updated via `update-submodules.bat`), likely pulling in base code from upstream projects like PojavLauncher.

```
Amethyst-Offline/
├── Amethyst-Android/          # Android app source (Kotlin/Java, Gradle-based)
├── Amethyst-iOS/              # iOS app source (Objective-C/Swift, Make-based)
├── MinecraftDownloader.patch  # Patch for offline Minecraft downloader modifications
├── MinecraftResourceDownloadTask.patch  # Patch for resource download task handling
├── README.md                  # Project documentation and build instructions
├── Tools.patch                # General tools patch
├── android-build.bat          # Windows batch script for Android build
├── android-build.sh           # Unix shell script for Android build
├── patches/
│   └── JavaLauncherMachFix.patch  # iOS-specific Mach-O launcher fix patch
└── update-submodules.bat      # Script to update Git submodules
```

- **Amethyst-Android/**: Contains the full Android project structure, with output APKs generated at `Amethyst-Android/app_pojavlauncher/build/outputs/apk/debug/`.
- **Amethyst-iOS/**: Contains the iOS Xcode project or equivalent, requiring manual patching with `gpatch` and compilation via `gmake`.
- **Patches**: Collection of `.patch` files applied during build to modify base code (e.g., for offline resource handling and platform fixes).
- **Build Scripts**: Cross-platform scripts automate patching and building for Android; iOS relies on manual steps mirroring the GitHub Actions workflow.
- **Root Files**: Documentation and utilities for setup.

## Architecture Diagram

```
+-----------------------------------+
|        Amethyst-Offline Repo      |
|  (Git + Submodules)               |
+-----------------------------------+
                |
                | (git clone --recursive)
                v
+---------------+---------------+
| Amethyst-Android             | Amethyst-iOS     |
| (Java/Kotlin/Gradle)         | (ObjC/Swift/Make)|
+---------------+---------------+
         |                       |
         | (./android-build.sh)  | (gpatch + gmake)
         v                       v
+---------------+---------------+
|   APK Output  |  iOS App      |
| (debug/release)| (via workflow)|
+---------------+---------------+
         ^                       ^
         | Patches Applied:      | Patches Applied:
         | - MinecraftDownloader | - JavaLauncherMachFix
         | - MinecraftResource...| - Tools
         | - Tools               |
+---------------------------------+
```

The diagram illustrates the build-time flow: Repository cloning initializes submodules, patches are applied to platform sources, and platform-specific build tools produce deployable artifacts. Runtime architecture is encapsulated within each platform's app (launcher logic from PojavLauncher base).

## Key Components

| Component              | Description |
|------------------------|-------------|
| **Amethyst-Android**  | Core Android application source, based on PojavLauncher. Handles Minecraft launching, offline resource management via patches. Built with Gradle. |
| **Amethyst-iOS**      | Core iOS application source. Supports Minecraft execution on iOS via patched launcher components. Built with Make after patching. |
| **Patches**           | Diff files (`MinecraftDownloader.patch`, `MinecraftResourceDownloadTask.patch`, `Tools.patch`, `JavaLauncherMachFix.patch`) modifying base code for offline functionality, resource tasks, tools, and iOS Mach-O compatibility. |
| **android-build.sh/bat** | Cross-platform scripts to automate Android patching and Gradle build. |
| **update-submodules.bat** | Initializes/updates Git submodules, pulling dependencies like PojavLauncher base. |
| **README.md**         | Entry-point documentation for releases, building, and credits. |

## Data Flow

1. **Build-Time Flow**:
   - Clone repo with submodules → Run `update-submodules.bat` (if needed).
   - For Android: Execute `android-build.sh/bat` → Applies patches (e.g., `MinecraftDownloader.patch`) → Gradle build → APK output.
   - For iOS: Manual `gpatch` on patches (e.g., `JavaLauncherMachFix.patch`) → `gmake` compile → App binary (as per workflow).

2. **Runtime Flow** (Inferred from Patches and Base Launcher):
   - App launch → Patched downloader/task logic checks local cache first (offline mode).
   - Minecraft resources/assets loaded from offline storage (modified via `MinecraftResourceDownloadTask.patch`).
   - Launcher invokes Java/Mach-O runtime (fixed via `JavaLauncherMachFix.patch`) to start Minecraft instance.
   - No external servers required post-setup; data persists locally.

Data movement is primarily local (file I/O for assets) with patches enabling offline bypassing of online downloads.

## Configuration

- No root-level config files or environment variables are defined.
- Platform-specific configs:
  - Android: Gradle properties in `Amethyst-Android/` (standard Android project).
  - iOS: Make variables or Xcode schemes in `Amethyst-iOS/`.
- Build scripts (`android-build.sh/bat`) use default behaviors; customize via script editing.
- iOS build references GitHub workflow for details (e.g., `.github/workflows/ios.yml`).

## Dependencies

| Dependency          | Purpose | Usage |
|---------------------|---------|-------|
| **PojavLauncher**  | Base Minecraft launcher framework (via submodules in `Amethyst-Android`/`Amethyst-iOS`). | Provides core launching, Java runtime, and Minecraft integration; patched for offline. |
| **Git Submodules** | Embeds upstream sources (e.g., PojavLauncher repos). | Managed by `update-submodules.bat`; essential for complete source. |
| **gpatch**         | Applies `.patch` files for iOS. | Required for manual iOS patching (Homebrew formula). |
| **Gradle**         | Android build automation. | Compiles `Amethyst-Android`. |
| **gmake**          | iOS compilation. | Builds `Amethyst-iOS` post-patching. |

All dependencies are fetched via Git or standard tools; no package managers like npm/Maven wrappers at root. Upstream licenses/credits noted in README.
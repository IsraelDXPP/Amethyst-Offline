# Amethyst-Offline API Documentation

## Overview

Amethyst-Offline is an offline-capable Minecraft launcher for Android and iOS, forked and modified from the original Amethyst project (based on PojavLauncher). It does not expose a public HTTP/REST API or server-side endpoints. Instead, it provides a set of **command-line interface (CLI) tools and scripts** for repository management, submodule updates, and building the mobile applications from source.

### Design Philosophy and Conventions
- **Modular Build System**: The project uses Git submodules (requiring `--recursive` clone), patches for upstream modifications (e.g., Minecraft downloader, resource tasks, Java launcher fixes), and platform-specific build scripts.
- **Cross-Platform Scripts**: Bash (`.sh`) for Unix-like systems and batch (`.bat`) for Windows.
- **Patch-Based Customization**: Core modifications are applied via `.patch` files in the root and `patches/` directory, enabling offline functionality without upstream dependencies.
- **No Runtime API**: As an offline launcher, there are no network-based APIs, authentication endpoints, or webhooks. All interfaces are build-time CLI tools.
- **Dependencies**: Git, platform-specific build tools (e.g., Android SDK/NDK for Android, Xcode/gpatch/gmake for iOS).

**Note**: This documentation is derived strictly from the provided file structure and README.md. No runtime code or internal app APIs are analyzed or documented here.

## Authentication

Not applicable. Amethyst-Offline is designed for **offline use**, bypassing online Minecraft authentication (e.g., Mojang/Microsoft accounts). Builds produce standalone APKs/IPAs that support offline launches. No auth tokens, sessions, or credentials are required for building or running.

## Base URL

Not applicable. No HTTP API or network services.

## CLI Commands

The project exposes the following CLI commands/scripts for repository setup and building. Run them from the project root after cloning.

### 1. Repository Initialization
- **Command**: `git clone --recursive https://github.com/DumDum192/Amethyst-Offline.git`
- **Description**: Clones the repository with all Git submodules (e.g., Amethyst-Android, Amethyst-iOS).
- **Platform**: All (Git required).
- **Parameters**: None.
- **Output**: Fully populated project directory.
- **Example**:
  ```
  git clone --recursive https://github.com/DumDum192/Amethyst-Offline.git
  cd Amethyst-Offline
  ```
- **Errors**:
  | Code/Exit | Description |
  |-----------|-------------|
  | Git errors (e.g., network failure) | Check internet connectivity and Git installation. |
  | Submodule fetch failure | Run `git submodule update --init --recursive` manually. |

### 2. Update Submodules
- **Command**: `./update-submodules.bat` (Windows) or equivalent `git submodule update --init --recursive` (cross-platform).
- **Description**: Updates and initializes Git submodules (Amethyst-Android, Amethyst-iOS).
- **Platform**: Windows (via `.bat`); manual Git for others.
- **Parameters**: None.
- **Output**: Updated submodules.
- **Example** (Windows):
  ```
  update-submodules.bat
  ```
- **Errors**:
  | Code/Exit | Description |
  |-----------|-------------|
  | Submodule conflicts | Resolve manually with `git submodule foreach git pull origin main`. |

### 3. Android Build
- **Command**: `./android-build.sh` (Unix/macOS/Linux) or `android-build.bat` (Windows).
- **Description**: Applies patches (e.g., MinecraftDownloader.patch, MinecraftResourceDownloadTask.patch, Tools.patch) and builds the debug APK.
- **Platform**: Android build environment (Android SDK/NDK, Gradle).
- **Parameters**: None (script handles patching and Gradle build).
- **Output**: APK at `Amethyst-Android/app_pojavlauncher/build/outputs/apk/debug/`.
- **Example** (Unix):
  ```
  ./android-build.sh
  ```
- **Errors**:
  | Code/Exit | Description |
  |-----------|-------------|
  | Patch rejection | Manually apply patches (e.g., `patch -p1 < MinecraftDownloader.patch`). |
  | Gradle failure | Ensure Android SDK/NDK paths are set (ANDROID_HOME, etc.). |

### 4. iOS Build (Manual CLI Steps)
iOS build is not fully scripted; use `gpatch` and `gmake` manually. Reference the [iOS workflow](https://github.com/DumDum192/Amethyst-Offline/blob/main/.github/workflows/ios.yml#L75) for details.

- **Prerequisites**: Install `gpatch` (via Homebrew: `brew install gpatch`), Xcode, and dependencies.
- **Steps**:
  1. Apply patches: `gpatch -p1 < JavaLauncherMachFix.patch` (and others as needed).
  2. Build: `gmake` (in Amethyst-iOS directory).
- **Description**: Patches source files and compiles the iOS app.
- **Platform**: macOS with Xcode.
- **Parameters**: Standard `gpatch`/`gmake` flags (e.g., `-p1` for patch level).
- **Output**: Compiled IPA (location per Xcode/gmake output).
- **Example**:
  ```
  gpatch -p1 < patches/JavaLauncherMachFix.patch
  cd Amethyst-iOS
  gmake
  ```
- **Errors**:
  | Code/Exit | Description |
  |-----------|-------------|
  | Patch failure (e.g., "hunk failed") | Check file versions; rebase upstream changes. |
  | gmake errors | Verify Xcode command-line tools (`xcode-select --install`). |

## Rate Limiting

Not applicable. No network APIs.

## Webhooks

Not applicable. No event-driven webhooks or integrations.

## Additional Notes
- **Patches**: Root-level `.patch` files (MinecraftDownloader.patch, MinecraftResourceDownloadTask.patch, Tools.patch) and `patches/JavaLauncherMachFix.patch` modify upstream PojavLauncher/Amethyst for offline support (e.g., local resource downloading).
- **Releases**: Prebuilt artifacts available via GitHub Actions ([Android](https://github.com/DumDum192/Amethyst-Offline/actions/workflows/android.yml), [iOS](https://github.com/DumDum192/Amethyst-Offline/actions/workflows/ios.yml)).
- **License**: See Amethyst-Android and Amethyst-iOS repositories for details.

For app runtime behavior (e.g., Minecraft launching), refer to the built APK/IPA or upstream PojavLauncher docs.
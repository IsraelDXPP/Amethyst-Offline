# SETUP.md

## Prerequisites

Before building or running Amethyst-Offline, ensure the following software is installed. Minimum versions are not explicitly specified in the codebase; use recent stable releases compatible with the tech stack (Java, Kotlin, Objective-C/Swift, Gradle, Make, Bash).

### Common
- **Git**: For cloning the repository (`git clone --recursive`).
- **Bash**: Unix-like shell for build scripts (use Git Bash or WSL on Windows).

### Android
- **Java JDK**: Required for Gradle builds (OpenJDK recommended, version 11+).
- **Gradle**: Handled via wrapper in the project ( `./gradlew` ).
- **Android SDK**: Implicitly required for Gradle Android builds (install via Android Studio or command-line tools).

### iOS (macOS only)
- **Xcode**: Command Line Tools and full Xcode for Objective-C/Swift compilation.
- **Homebrew**: Package manager (`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`).
  - `gpatch`: `brew install gpatch`.
  - `gmake`: Available via Homebrew or Xcode Command Line Tools.
- **macOS**: Ventura (13+) recommended for GitHub Actions compatibility.

**Windows Users**: Use `android-build.bat` for Android. WSL2 recommended for full Unix compatibility.

## Quick Start

The fastest way to get Amethyst-Offline running (prebuilt binaries):

1. Download the latest **Android APK** from [GitHub Actions - Android](https://github.com/DumDum192/Amethyst-Offline/actions/workflows/android.yml).
2. Download the latest **iOS IPA** from [GitHub Actions - iOS](https://github.com/DumDum192/Amethyst-Offline/actions/workflows/ios.yml).
3. **Android**: Enable "Install from unknown sources" on your device, then install the APK.
4. **iOS**: Sideload the IPA using tools like AltStore, Sideloadly, or Xcode (requires Apple ID provisioning).
5. Launch the app.

## Detailed Installation

### Clone Repository
```bash
git clone --recursive https://github.com/DumDum192/Amethyst-Offline.git
cd Amethyst-Offline
```

The `--recursive` flag is required to fetch PojavLauncher submodules.

### Install Dependencies
- **Android**: No additional steps; Gradle wrapper fetches dependencies.
- **iOS**:
  ```bash
  brew install gpatch
  # Ensure gmake is available (brew install make if needed)
  ```

No `.env.example` or environment variables are defined. No database setup is required (not present in codebase).

### Platform-Specific Setup
- **Android**: Review `android-build` script for any custom patches applied to PojavLauncher.
- **iOS**: Manually apply patches using `gpatch` (details in [.github/workflows/ios.yml](https://github.com/DumDum192/Amethyst-Offline/blob/main/.github/workflows/ios.yml#L75)).

## Running the Application

Amethyst-Offline produces native mobile apps (APK for Android, app bundle/IPA for iOS). There are no server-side, development/production modes, or Docker support in the codebase.

### Android
1. Run `./android-build` (Linux/macOS) or `android-build.bat` (Windows).
2. Find the APK: `Amethyst-Android/app_pojavlauncher/build/outputs/apk/debug/`.
3. Install on device/emulator: `adb install <apk-file>` or sideload manually.
4. Launch via app drawer.

### iOS
1. Manually patch source files with `gpatch` (follow [ios.yml workflow](https://github.com/DumDum192/Amethyst-Offline/blob/main/.github/workflows/ios.yml#L75)).
2. Compile: `gmake`.
3. Build produces an iOS app bundle; archive in Xcode for IPA.
4. Sideload to device using Xcode or third-party tools.
5. Launch on device.

## Running Tests

No test suite or testing instructions are present in the codebase.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `git clone` fails with submodule errors | Use `git clone --recursive`; if issues persist, run `git submodule update --init --recursive`. |
| `./android-build` fails on Windows | Use `android-build.bat`. Ensure Java JDK and Android SDK are in PATH. |
| Gradle build errors (Android) | Run `./gradlew clean` then retry. Check Java version (11+). |
| `gpatch` or `gmake` not found (iOS) | Install via `brew install gpatch make` on macOS. |
| iOS build fails | Review [.github/workflows/ios.yml](https://github.com/DumDum192/Amethyst-Offline/blob/main/.github/workflows/ios.yml) for exact patch/compile sequence. Ensure Xcode Command Line Tools: `xcode-select --install`. |
| APK/IPA crashes on launch | Verify against official PojavLauncher compatibility; this is an offline fork. |

## IDE Setup

### Android (Recommended: Android Studio)
- **Android Studio**: Hedgehog (2023.1.1+) or later.
- Plugins/Extensions:
  - Kotlin (built-in).
  - Gradle (built-in).
- Open `Amethyst-Android/` as project; use Gradle tasks panel for build.

### iOS (Recommended: Xcode)
- **Xcode**: 15+.
- No additional plugins needed; open the iOS project directory.
- Use `gmake` from Terminal for custom builds.

For cross-platform editing: VS Code with Java/Kotlin/Swift extensions, but full builds require platform IDEs.

## License & Credits
See [README.md](README.md) for details. Based on [Amethyst-Android](https://github.com/AngelAuraMC/Amethyst-Android) and [Amethyst-iOS](https://github.com/AngelAuraMC/Amethyst-iOS).
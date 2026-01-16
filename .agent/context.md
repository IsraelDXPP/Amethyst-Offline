```markdown
# Amethyst-Offline Context

## Project Summary
Amethyst-Offline is an offline-enabled fork of Amethyst, a Minecraft launcher based on PojavLauncher for running Minecraft Java Edition on mobile. It provides Android and iOS apps with patches for offline resource downloading and Java launching. Build from source or use CI releases; focuses on patching upstream PojavLauncher repos.

## Tech Stack
- **Android**: Java/Kotlin, Gradle, PojavLauncher base (Amethyst-Android submodule)
- **iOS**: Objective-C/Swift, Xcode/gmake, gpatch for patching (Amethyst-iOS submodule)
- **Patching**: Custom .patch files (e.g., MinecraftDownloader.patch) applied via build scripts
- **Build Tools**: Bash/Bat scripts, git submodules, GitHub Actions for CI
- **Minecraft**: Java Edition assets/version handling, offline mode support

## Key Files (Read First)
- `README.md`: Overview, build instructions, releases
- `android-build.sh` / `android-build.bat`: Android patching/build automation
- `patches/*.patch`: Core modifications (e.g., `JavaLauncherMachFix.patch`, `MinecraftResourceDownloadTask.patch`)
- `Amethyst-Android/`: Android source (submodule; focus on `app_pojavlauncher/`)
- `Amethyst-iOS/`: iOS source (submodule; check `.github/workflows/ios.yml` for patch details)
- `update-submodules.bat`: Sync submodules

## Architecture
- **Monorepo with Submodules**: Root holds patches/scripts; `Amethyst-Android/iOS` are PojavLauncher forks.
- **Components**:
  - **Patching Layer**: Build scripts apply root patches to submodules for offline MC downloads/launch.
  - **Android**: Gradle-based app in `Amethyst-Android/app_pojavlauncher`; outputs APK to `build/outputs/apk/debug/`.
  - **iOS**: Manual gpatch + gmake; see iOS workflow for steps.
- **Flow**: Clone recursive → Run build script → Patched app launches MC offline.

## Patterns & Conventions
- **Naming**: CamelCase (Java/Kotlin), snake_case patches; PojavLauncher conventions (e.g., `PojavApplication`).
- **Structure**: Patches are diff-based; edits target MC downloaders, launchers (e.g., fix mach-O for iOS).
- **Modularity**: Changes via patches, not direct submodule edits (rebase-friendly).
- **Commits**: Descriptive, platform-prefixed (e.g., "[Android] Fix offline auth").

## Common Tasks
- **Add Feature**: Create .patch for submodule changes; test in build script; update README/CI.
- **Fix Bug**: Reproduce in built APK/IPA; diff against upstream; add targeted patch.
- **Build Android**: `./android-build.sh` (Linux/Mac) or `android-build.bat` (Win); requires JDK/Android SDK.
- **Build iOS**: `git clone --recursive`; manual `gpatch -p1 < patchfile`; `gmake`.
- **Update Submodules**: `./update-submodules.bat` or `git submodule update --recursive`.
- **Release**: Trigger GitHub Actions (android.yml/ios.yml).

## Testing
- **No Dedicated Tests**: Platform emulators/devices primary.
- **Android**: Build APK → Install on device/emulator → Launch MC offline (check logs via `adb logcat`).
- **iOS**: Build IPA → Sideload via AltStore/Xcode → Test on device/simulator.
- **Write Tests**: Add to submodule's existing unit/UI tests (JUnit/XCTest); run via Gradle/Xcode.
- **CI Validation**: Push triggers workflows; check artifacts/logs.

## Important Notes
- **Recursive Clone Required**: `git clone --recursive` for submodules; else patches fail.
- **iOS Quirks**: Needs `gpatch` (brew install); follow ios.yml exactly; no auto-build script.
- **Upstream Sync**: Submodules track AngelAuraMC/Amethyst-*; rebase patches after updates.
- **Offline Focus**: Patches enable no-internet MC assets/launch; avoid breaking online fallback.
- **Licenses**: Check submodule READMEs; attribute credits.
- **Gotchas**: Windows line endings in patches; Android needs env vars (ANDROID_HOME); total size ~500MB cloned.
```

*(298 lines; optimized for quick AI parsing)*
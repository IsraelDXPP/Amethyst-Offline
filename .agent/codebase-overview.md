```markdown
# Amethyst-Offline Codebase Overview

## File Index
- **README.md**: Project setup, build instructions, and usage guide.
- **android-build.bat / android-build.sh**: Scripts to build Android APK.
- **update-submodules.bat**: Initializes/updates Git submodules (e.g., platform dirs).
- **MinecraftDownloader.patch**: Patch for offline Minecraft version downloading.
- **MinecraftResourceDownloadTask.patch**: Patch for resource pack/asset downloading.
- **Tools.patch**: Patch for utility tools in Minecraft launcher.
- **patches/JavaLauncherMachFix.patch**: macOS-specific fix for Java launcher.

## Directory Map
- **Amethyst-Android/**: Android app source (Java/Kotlin, Gradle-based).
- **Amethyst-iOS/**: iOS app source (Swift/Objective-C, Xcode-based).
- **patches/**: Supplemental patches for platform-specific fixes.

## Entry Points
- Android: `android-build.bat` or `android-build.sh` (runs Gradle build).
- Submodules: `update-submodules.bat` (git submodule update --init --recursive).
- iOS: Xcode project in Amethyst-iOS (open via README).

## Key Functions/Classes
- **MinecraftDownloader**: Handles offline MC version fetching (patched).
- **MinecraftResourceDownloadTask**: Manages asset/resource downloads (patched).
- **JavaLauncher**: macOS Java launch fixes (in patches/JavaLauncherMachFix.patch).
- Platform mains: Amethyst-Android/app/src/main/*Activity, Amethyst-iOS/AppDelegate.swift.

## Dependencies
- **Git submodules**: Platform sources (Amethyst-Android/iOS).
- **Gradle/Android SDK**: Android builds.
- **Xcode/Swift**: iOS builds.
- **Minecraft libs**: Patched into launcher (versions/assets via manifests).
```
```
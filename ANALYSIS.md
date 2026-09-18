# Squire VR - Codebase Analysis

## Overview
**Squire VR** is a native Android application designed specifically for the Meta Quest ecosystem. It acts as a standalone sideloading client that allows users to browse, download, install, and manage VR games directly on their headset without the need for a PC or phone.

## Architectural Summary
The application follows a monolithic Android structure primarily written in Java.
- **UI Layer**: Composed of Activities (`MainActivity`, `SplashActivity`, `SquireManagerActivity`, `DeviceStorageActivity`) and custom adapters (`GameAdapter`). It uses standard XML layouts.
- **Service Layer**: The core downloading logic is encapsulated in a background service (`StreamingService`). This service executes native binaries (`rclone` for downloading and `7z` for extraction) using `ProcessBuilder` to ensure high performance and reliability.
- **Privilege Management**: The app integrates with the **Shizuku API** to gain elevated privileges without root. This allows it to perform operations such as moving files to restricted directories (e.g., `/Android/obb`) and silently installing APKs (`InstallReceiver`, `AutoInstaller`).
- **Data Persistence**: Uses `SharedPreferences` for user configurations, favorites, and cached ratings. A local text file (`cached_games.txt`) acts as a fast cache for the game metadata.

## Key Features
- **Standalone Sideloading**: Direct device-to-HMD game installations.
- **Resumable Downloads**: Utilizes `rclone`'s robust downloading capabilities to resume interrupted transfers safely.
- **Pre-flight Storage Checks**: Uses `StatFs` to validate that there is sufficient storage (2x game size) before initiating a download and extraction process.
- **Version Management**: Automatically compares locally installed app versions against the remote repository to highlight available updates.
- **Community Ratings**: Integrated rating system syncing with a remote cloud server.
- **Squire Manager**: Dedicated Activity for Shizuku setup, managing wireless debugging pairing, and verifying ADB access.

## Technology Stack
- **Language**: Java
- **Build System**: Gradle (Android SDK 34)
- **Native Binaries**: `librclone.so` (Data synchronization), `lib7z.so` (Archive extraction)
- **Networking**: OkHttp3 (API communication, updates, rating submission)
- **Image Loading**: Glide (Asynchronous thumbnail loading)
- **Privilege Escalation**: Shizuku API (`dev.rikka.shizuku:api`, `dev.rikka.shizuku:provider`)
- **UI Components**: Material Components, ConstraintLayout, RecyclerView

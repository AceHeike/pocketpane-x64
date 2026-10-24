# PocketPane -- Intel Mac Edition

**Your Android, right here.**

This repository is an independent fork of
[PocketPane](https://github.com/tobwil/pocketpane), a desktop app for
mirroring and controlling Android devices using
[scrcpy](https://github.com/Genymobile/scrcpy) and Android Debug Bridge
(ADB).

## About this fork

This fork is intentionally focused on **a native macOS build for Intel
Macs (`x86_64`)**. Compared with the upstream project, the main change
made here is a custom application icon, giving the macOS executable a
distinct identity. The app's functionality is otherwise based on the
upstream PocketPane project.

-   **Platform distributed by this fork:** macOS Intel (`x86_64`)
-   **Minimum macOS version:** macOS 14 Sonoma or newer
-   **Tested on:** Intel MacBook Pro (16-inch, 2019), running macOS
    Sequoia 15.8.1
-   **Application:** native SwiftUI app bundle, with ADB and scrcpy
    runtime files included
-   **Windows builds:** not distributed or supported by this fork. For
    the original project and its platform offerings, visit [the upstream
    PocketPane repository](https://github.com/tobwil/pocketpane).

This is an independent community fork, not an official release of the
original project. Compatibility beyond the tested Mac and macOS version
has not been exhaustively verified.

## Download

Download the macOS Intel build from this repository's
[Releases](https://github.com/AceHeike/pocketpane-x64/releases) page
when a packaged release is available.

The package is intended for **Intel Macs (`x86_64`)**. Do not use the
upstream Apple Silicon package as a substitute for this build.

### Install and open

1.  Download the macOS Intel ZIP from this fork's Releases page and
    extract it.
2.  Move **PocketPane.app** to your **Applications** folder.
3.  Open PocketPane and follow the [First connection](#first-connection)
    guide below.

The application is currently **ad-hoc signed and not notarized by
Apple**. If macOS blocks the first launch, open **System Settings →
Privacy & Security** and use **Open Anyway** if that option is offered.
Only do this if you trust the copy you downloaded and its source.

## What PocketPane does

-   **Cable-free mirroring and control** --- pair Android 11 or newer
    using its Wireless debugging pairing code, then reconnect over your
    local network.
-   **Low-latency video and audio** --- scrcpy provides the mirroring
    engine while PocketPane manages discovery, pairing, options, and
    recovery.
-   **Android apps as desktop windows** --- search installed apps and
    open an app in its own scrcpy virtual display.
-   **Files and captures** --- drop files into PocketPane, save PNG
    screenshots, and record MP4 presentations.
-   **Presentation controls** --- show touches, keep the device awake,
    hide window borders, or turn the physical phone screen off.
-   **Local and private** --- no user account, telemetry,
    advertisements, cloud relay, or phone companion app.

### macOS extras

-   Native SwiftUI interface with a Light/Dark Mode toggle.
-   Menu bar mode for nearby-device status and one-click actions.
-   Automatic clipboard synchronization while mirroring.
-   Keyboard-driven app launcher with **⌘K**.

## Screenshots

![PocketPane dashboard in Light Mode](https://github.com/user-attachments/assets/538afcc4-6eeb-40f1-9c38-5683e18bf2e7)
![PocketPane dashboard in Dark Mode](https://github.com/user-attachments/assets/1af9e27c-0321-4fa9-81c5-ea139d8e1822)

### Mirroring Options & Real mirroring

Search the apps installed on the connected phone and launch one in a
separate desktop window.

![PocketPane dashboard in Dark Mode - Start Mirroring](https://github.com/user-attachments/assets/765389fc-12d5-4299-940d-f6e621b2aaaf)
![PocketPane Real Mirroring in Dark Mode](https://github.com/user-attachments/assets/7cf84a71-d6e2-4dde-b13b-e3af081805f5)

## First connection

1.  On Android, open **Settings → System → Developer options → Wireless
    debugging**. Menu names can vary by Android manufacturer.
2.  Enable **Wireless debugging** and ensure the phone and Mac are
    connected to the same local network.
3.  Tap **Pair device with pairing code** and keep that dialog open.
4.  Enter the displayed pairing IP address, port, and six-digit code in
    PocketPane.
5.  Close the pairing dialog on the phone after pairing succeeds.
6.  PocketPane discovers the separate connection endpoint. If necessary,
    enter the IP address and port shown on the main Wireless debugging
    screen.
7.  Select the phone and start mirroring.

The temporary pairing port and the normal connection port are different.
Use the pairing port for pairing and the connection endpoint shown on
the main Wireless debugging screen for the subsequent connection.

A one-time authorized USB connection is also available as an optional
fallback for switching ADB to Wi-Fi.

## Requirements

### Mac

-   Intel Mac with an `x86_64` processor.
-   macOS 14 Sonoma or newer.
-   Network connectivity between the Mac and Android device for wireless
    use.

This fork has been built and tested on an Intel MacBook Pro (16-inch,
2019) running macOS Sequoia 15.8.1. Other Intel Mac models and macOS
versions have not all been individually tested.

### Android

-   Android 11 or newer for wireless pairing.
-   Developer options and **Wireless debugging** enabled.
-   Phone and Mac connected to the same local network.

The underlying scrcpy engine supports older Android versions, but
PocketPane's wireless pairing workflow requires Android 11 or newer.

## Build from source

The native macOS app has no third-party Swift package dependencies. The
build script fetches the official scrcpy tools for the current CPU
architecture, verifies the published SHA-256 checksum, and packages the
required runtime files into the app bundle. The custom icon is included
from `assets/macos/AppIcon.icns`.

### Prerequisites

-   An Intel Mac (`x86_64`) running macOS 14 or newer.
-   Xcode Command Line Tools, including Swift and the macOS SDK.
-   Git and a working internet connection to download the required
    scrcpy archive.

### Build

``` zsh
git clone https://github.com/AceHeike/pocketpane-x64.git
cd pocketpane-x64
zsh scripts/build-app.sh
open dist/PocketPane.app
```

The resulting application bundle is located at `dist/PocketPane.app`.

To build a distributable DMG, if needed:

``` zsh
zsh scripts/build-dmg.sh
```

The build downloads the official scrcpy 4.0 archive for the current CPU
architecture, verifies its published SHA-256 checksum, and embeds the
required runtime files. The application is ad-hoc signed by the build
process; this does not constitute Apple notarization.

## Technology

| Layer | macOS | Windows | Purpose |
| --- | --- | --- | --- |
| Desktop UI | Swift 6, SwiftUI | PowerShell 5.1, WPF | Pairing, device selection, options, and launch flows |
| OS integration | AppKit, Foundation | .NET Framework, Windows dialogs | Menu bar, processes, files, save panels, and drag-and-drop |
| Mirroring | scrcpy 4.0 | scrcpy 4.0 | Video, audio, input control, recording, and virtual displays |
| Device transport | Android Debug Bridge | Android Debug Bridge | Secure pairing, TCP/IP connections, screenshots, and file transfer |
| Discovery | ADB mDNS / Bonjour | ADB mDNS | Finds wireless pairing and connection services |
| Packaging | Swift Package Manager, zsh | C# launcher, PowerShell, GitHub Actions | Self-contained platform downloads |

PocketPane launches scrcpy as a managed subprocess rather than
reimplementing its codec, audio, and Android control stack.

## Repository layout

``` text
Sources/PocketPane/             SwiftUI macOS application
Sources/PocketPaneCore/         Swift ADB/scrcpy parsing and process utilities
Sources/PocketPaneCoreChecks/   Executable core checks
assets/macos/AppIcon.icns       Custom macOS application icon
scripts/build-app.sh            macOS app-bundle build and packaging
scripts/build-dmg.sh            macOS DMG and SHA-256 generation
scripts/fetch-tools.sh          Verified scrcpy download for macOS
```

The fork may retain other files inherited from upstream, but this README
documents only the macOS Intel edition distributed by this repository.
Windows downloads and Windows build instructions belong to the [original
PocketPane project](https://github.com/tobwil/pocketpane).

## Privacy and security

-   Screen, audio, clipboard, and file data travel locally between the
    Mac and Android device through ADB/scrcpy.
-   PocketPane does not include an analytics SDK, user account, ad
    framework, cloud relay, or remote service.
-   ADB stores its normal authentication keys in the current user's
    profile.
-   Android provides a **Forget** action for paired computers under
    Wireless debugging.
-   Only pair devices on networks you trust.

## License and acknowledgements

This fork retains the upstream project's licensing and notices.

PocketPane source code is available under the [MIT License](LICENSE).

PocketPane bundles or downloads the official scrcpy 4.0 distribution.
scrcpy is licensed under the [Apache License
2.0](https://github.com/Genymobile/scrcpy/blob/master/LICENSE). See
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for included components
and notices.

The original PocketPane project was created by [tobwil](https://github.com/tobwil/pocketpane). This fork contains a custom macOS application icon and an Intel (x86_64) build, created primarily for personal use on Intel-based Macs. It is an independent, personal adaptation of the original project and is not an official release. There is no commitment to provide future updates, additional builds, or ongoing maintenance. For the original project, upstream development, and Windows versions, please visit the [original repository](https://github.com/tobwil/pocketpane).

Android is a trademark of Google LLC. macOS is a trademark of Apple
Inc. PocketPane is an independent project and is not affiliated with
Google, Apple, Genymobile, or the scrcpy maintainers.

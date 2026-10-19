# Upset Client

**A Windows launcher and companion Fabric mod for a more personal Minecraft Java Edition setup.**

Upset Client brings profiles, game setup, mod discovery, and client customization into one desktop app. Choose a Minecraft version, give each profile its own mods and settings, browse projects from Modrinth, and shape an in-game crosshair with a live preview. The launcher is built with .NET and WebView2; the companion mod runs on Fabric.

> **Development status:** Upset Client is an early project. The launcher and editor are under active development. Its Microsoft application can complete Microsoft and Xbox authentication, but Minecraft Services currently rejects its app ID with HTTP 403. The app ID requires Minecraft application approval. Account verification and normal game launch will remain unavailable until that approval is granted. No ownership check is bypassed.

## What is here today

| Area | Current capability |
| --- | --- |
| Profiles | Separate Minecraft version, Fabric or Vanilla loader, memory allocation, game files, mods, and client settings per profile. |
| Content browser | Search Modrinth for compatible mods, shaders, and resource packs; install supported files into the selected profile. Fabric mod installs resolve required Modrinth dependencies. |
| Crosshair editor | Preview and adjust crosshair shape, size, gap, thickness, color, and outline; choose a preset or draw an 11 × 11 custom design. The bundled Fabric mod applies changes in-game for Minecraft 1.21.11. |
| Launcher settings | Controls for interface feedback, notifications, server status, Discord activity, storage access, and game display preferences. |
| Discord activity | Optional Rich Presence through the locally running Discord desktop app, with the Upset Client artwork and launcher or game activity. |
| Account flow | Microsoft sign-in followed by Xbox and Minecraft Java profile and entitlement checks. The Minecraft Services step is pending app approval. |

The module screen includes controls for other HUD and visual ideas. **The custom crosshair is the in-game module implemented at present**; the other switches should be treated as work in progress.

## How it is built

- **Launcher:** C# on .NET 10 for Windows, with a WinForms host and WebView2 interface.
- **Game integration:** CmlLib.Core handles Minecraft installation and launch preparation. Fabric is installed for Fabric profiles, and the companion mod is bundled for the supported version.
- **Companion mod:** Java 21 and Fabric for Minecraft **1.21.11**.
- **Local data:** Profiles and settings live under `%LOCALAPPDATA%\upset client`. Microsoft account tokens, when available, are protected with Windows DPAPI for the current user.
- **External services:** Official Microsoft/Xbox/Minecraft endpoints for authentication and ownership checks; Modrinth for content discovery; Discord desktop IPC for Rich Presence.

Upset Client does not ask users to enter a personal Microsoft or Discord application ID. The public application IDs are part of the launcher configuration. Microsoft credentials are entered through Microsoft's sign-in page, not into the launcher.

## Build from source

You need Windows, the .NET 10 SDK, the WebView2 Runtime, and Java 21 to build the companion mod.

```powershell
# Build the Fabric mod first.
cd client
.\gradlew.bat build
cd ..
Copy-Item client\build\libs\upset-client-0.1.0.jar bundled\upset-client-0.1.0.jar -Force

# Publish the Windows launcher.
dotnet publish launcher\UpsetLauncher.csproj -c Release -r win-x64 --self-contained false -o dist\latest
```

The launcher project copies `launcher/ui` and `bundled` into its output. Publishing the launcher does not grant Minecraft API approval or make account verification available.

## Roadmap

- Complete Minecraft Services app approval, then verify the full sign-in, ownership, and launch flow.
- Implement the remaining in-game HUD and visual modules shown in the launcher.
- Continue polishing profile management, the crosshair editor, and launcher transitions.
- Expand customization while keeping profile data local and easy to manage.

## Project note

Upset Client is an independent Minecraft Java Edition project. It is not affiliated with or endorsed by Microsoft, Mojang Studios, Fabric, Modrinth, or Discord. Minecraft access requires a legitimate account with the appropriate entitlement or active access, plus the pending approval of this launcher's Microsoft application.

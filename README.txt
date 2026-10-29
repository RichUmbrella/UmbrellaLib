# UmbrellaLib

UmbrellaLib is a Windows game-library launcher that brings games from several PC launchers and emulators into one library. It is built with Tauri 2, Rust, and a lightweight HTML/CSS/JavaScript interface, with a bundled Node.js service for scanning games and processing artwork.

## Download and install

1. Open the repository’s **Releases** page and download the latest `UmbrellaLib-...zip` asset.
2. Extract the **entire ZIP** to a folder. Keep `UmbrellaLib.exe` and the `resources` folder together.
3. Open `UmbrellaLib.exe`.

The portable build targets **64-bit Windows** and requires the Microsoft Edge WebView2 Runtime. Node.js does not need to be installed because the scanner runtime is bundled. The ZIP does not include games, emulators, or launcher installations.

## Features

- Scans Steam, Epic Games, GOG, EA app, Ubisoft Connect, Xbox, Windows shortcuts, Cemu, and Dolphin.
- Lets you add a regular PC game manually by selecting its main `.exe`.
- Saves game-specific settings and keeps manually added games across scans.
- Caches scan results, game icons, and artwork to make later launches and scans faster.
- Supports searching, sorting, favorites, game details, and artwork refresh.
- Click a game's cover to launch it. Right-click a game tile to open that game's settings.

## Add games

Open **Settings**, enable the sources you want to scan, and choose **Save and rescan**.

### Add a regular PC game

In **Add a game manually**, browse to the game's main `.exe`. You can enter a display name or let UmbrellaLib use the executable's name. The game remains in your library across scans.

### Add a Cemu game

1. In **Settings → Emulator locations**, browse to `Cemu.exe`.
2. Make sure the **Cemu** scan source is enabled. Cemu's configured game paths and standard installed titles are scanned automatically.
3. If the game is still missing, choose **Add folder** under **Extra emulator game folders**. Select the parent folder that contains the game's folder—not its `meta` subfolder or the game folder itself.

For an extracted Wii U game, the game folder should contain `meta/meta.xml` and a `code` or `content` folder. For `.wua` or `.wux` games, select the folder containing those files. Then choose **Save and rescan** and look under the **Cemu** filter.

### Add a Dolphin game

Set the Dolphin executable in **Settings → Emulator locations**. Dolphin's configured ISO paths are scanned automatically. You can also use **Add folder** for a directory containing `.iso`, `.wbfs`, `.rvz`, `.ciso`, `.gcm`, or `.wia` files.

## Troubleshooting

- **The app does not open:** confirm WebView2 Runtime is installed and that you extracted the whole ZIP, including `resources`.
- **A launcher game is missing:** check that its source is enabled in Settings, then choose **Save and rescan**.
- **A Cemu game is missing:** check the Cemu path and folder layout above. Select the directory containing the game folder.
- **A game launches without appearing:** check whether its launcher or emulator opened behind UmbrellaLib.

## Local data

UmbrellaLib stores its settings, library cache, and artwork/icon cache in the current Windows user's application-data folder. It scans local launcher and emulator information to find games. Artwork lookup may use online sources; a SteamGridDB key is optional and can be entered in Settings.

## Build from source

Building on Windows requires Node.js, Rust, Microsoft C++ build tools, and the Windows SDK.

```powershell
npm install
npm run tauri -- dev

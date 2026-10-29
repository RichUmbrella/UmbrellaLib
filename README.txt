# UmbrellaLib (Tauri edition)

Windows game library launcher built with Tauri 2 and the system WebView2 runtime. The existing Electron project remains available alongside this migration.

## Run the portable build

Extract the downloaded ZIP to a folder, then open `UmbrellaLib.exe` from that folder. Keep the `resources` directory beside it; it contains the bundled scanner runtime. Node.js is not required to run the app.

The portable ZIP targets 64-bit Windows PCs. It uses the Microsoft Edge WebView2 Runtime for its window; if that runtime is missing, install WebView2 on the PC before launching. The bundled Node runtime means Node.js does not need to be installed. The app stores its own settings and game-library cache in the current Windows user's app-data folder, and scans that PC for installed games. Game files and emulator installs are not included in the ZIP. This build is not for macOS or Linux.

## Development

Install Node.js, Rust, and the Microsoft C++ build tools with the Windows 10/11 SDK. Then run:

```powershell
npm install
npm run tauri -- dev
```

Build an installer with `npm run tauri -- build`.

Tauri runs the library scanner in a short-lived bundled Node process, so the Node runtime is present in the app bundle. The scanner discovers games from Steam, Epic, GOG, EA app, Ubisoft Connect, Xbox, shortcuts, Cemu, and Dolphin while replacing Electron's desktop shell with Tauri/WebView2. Settings can also add a regular PC game by its executable; these entries persist across scans.

## Controls

- Click a game's cover to launch it.
- After a successful launch, the launcher minimizes so the game can come to the front.
- Cemu launches show the emulator window; if no window appears, the launcher stays open and reports that state.
- Right-click a game tile to open that game's settings.
- Click a game's title to open its detail page.

## Adding games

- EA app and Ubisoft Connect games are discovered during scans when their source toggles are enabled.
- For a regular PC game, open Settings, enter a display name if desired, browse to the game's main `.exe`, then choose **Add game**.
- For Cemu, set the Cemu executable and save. The app checks Cemu's configured game paths and installed titles. You can also add a folder directly containing Wii U title folders (`meta/meta.xml` plus `code` or `content`) or `.wua` / `.wux` files.
- For Dolphin, set Dolphin's executable and add a folder containing `.iso`, `.wbfs`, `.rvz`, `.ciso`, `.gcm`, or `.wia` files. Dolphin's configured ISO paths are scanned too.

Scanning runs in the background and checks sources in parallel, leaving the cached library usable while it works. The list refreshes once at completion only if visible contents changed, and keeps the current scroll anchor. Unchanged games reuse cached artwork. The first scan after this update refreshes the older icon cache once, then saves fingerprints so later scans can skip unchanged art. A successful scan is reused for 10 minutes on the next launch, while **Scan now** always forces a fresh check.

Windows shell icons are cached by source file and modification time, including icons resolved from shortcuts and file associations.

The app stores its library cache and settings in Tauri's per-user app data directory. Artwork is resized and cached to reduce image memory use.

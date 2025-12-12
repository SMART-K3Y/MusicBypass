# MusicBypass (Local-First Music Dashboard)

Local-first music dashboard with shared listening, streaming engines, and a desktop shell for hosting and controlling playback. The app runs a local Node.js service and can be wrapped into desktop executables (launchers and portable build).

## Requirements
- Windows
- Node.js **v24.3.0** (launchers auto-install/upgrade if missing)
- .NET Framework 4.x `csc.exe` (included with Windows/VS build tools) for building launchers

## Project layout
- `main.js` – boots the Node service and dashboard
- `MUSIC_ENGINES/` – streaming engines (YouTube/Spotify resolver, Lavalink client, etc.)
- `SETTINGS/`, `DATA/`, `UTILS/`, `DASHBOARD/`, `MUSIC_CACHE/` – app configuration, data, and assets
- `MusicBypass-nativefier/` – packaged desktop shell for the dashboard (Nativefier output)
- `launcher/StartMusicBypass.cs` – main launcher (hidden server + opens Nativefier app)
- `launcher/StartTunnel.cs` – console launcher to start the server and a Cloudflare quick tunnel
- `launcher/PortableBootstrapper.cs` – self-extracting portable wrapper
- `build-launcher.ps1`, `build-tunnel-launcher.ps1`, `build-portable.ps1` – build scripts
- `build-all.bat` – interactive one-shot builder for all executables

## Running (without rebuild)
- `StartMusicBypass.exe` – starts `npm start` (hidden by default) and opens the Nativefier window. Use `--debug` to keep the console visible.
- `StartTunnel.exe` – starts `npm start` and a Cloudflare quick tunnel; prints the tunnel URL. Use `--debug` to see full logs.
- `MusicBypassPortable.exe` – self-extracts to `%TEMP%`, enforces Node v24.3.0, then runs the launcher. Use `--debug` for visible consoles.

## Building executables
1) Install Node.js v24.3.0 and ensure `csc.exe` is available.
2) From the repo root:
   - `build-all.bat` → choose what to build (launcher, tunnel, portable, or all)
   - or run individually:
     - `powershell -ExecutionPolicy Bypass -File build-launcher.ps1`
     - `powershell -ExecutionPolicy Bypass -File build-tunnel-launcher.ps1`
     - `powershell -ExecutionPolicy Bypass -File build-portable.ps1`
3) Outputs land in the repo root: `StartMusicBypass.exe`, `StartTunnel.exe`, `MusicBypassPortable.exe`.

## Auto-update (stub)
- Launchers/portable include a placeholder manifest URL (`https://example.com/musicbypass/manifest.json`). Replace it with your hosted manifest (e.g., GitHub Releases or a public bucket).
- Manifest example:
  ```json
  {
    "version": "1.3.0",
    "launcher_url": "https://your.host/StartMusicBypass.exe",
    "portable_url": "https://your.host/MusicBypassPortable.exe"
  }
  ```
- On startup, if a newer version is found, the app prompts to download, replaces the EXE, and restarts. No secrets/tokens are embedded; host the manifest and binaries publicly or supply an auth layer externally.

## Tunneling (quick share)
- `StartTunnel.exe` uses Cloudflare Quick Tunnel: requires `cloudflared` on PATH. It prints a `*.trycloudflare.com` URL and keeps the tunnel until you close it.

## Notes and tips
- Node version is enforced at `v24.3.0`; launchers prompt to install/upgrade automatically.
- Portable build no longer tries to rebuild native modules; ensure Node matches to avoid native ABI issues (e.g., `better-sqlite3`).
- Icons/metadata are applied from `MusicBypass-nativefier/logo.ico` and `launcher/AssemblyInfo.cs`.
- If builds fail with locked files (e.g., SQLite db), close running app/node processes and retry.

## Releasing updates
1) Build new executables.
2) Publish them to your hosting (e.g., GitHub Releases).
3) Update `manifest.json` with the new `version` and URLs.
4) Update the `ManifestUrl` constants in launchers/portable to point to your hosted manifest.

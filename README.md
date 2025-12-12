# MusicBypass

A local-first music dashboard with shared listening and a desktop shell.

## What you get
- `MusicBypassPortable.exe`: one-file portable app. Double-click to run. Uses Node.js v24.3.0 (installer will prompt if needed). Use `--debug` to see consoles.
- `StartMusicBypass.exe`: starts the server and opens the desktop window. Use `--debug` to see the server console.
- `StartTunnel.exe`: starts the server and a Cloudflare quick tunnel; prints a public URL. Use `--debug` for full logs.

## Quick start
1) Install Node.js **v24.3.0** (launchers prompt to install/upgrade if missing).
2) Double-click `MusicBypassPortable.exe` (or `StartMusicBypass.exe` if already extracted).
3) Optional: run `StartTunnel.exe` to share via a quick tunnel (requires `cloudflared` on PATH).

## Auto-update (stub)
- The app can check a manifest URL for updates; replace the placeholder manifest URL in code with your hosted manifest (e.g., GitHub Releases).
- On a new version, it will prompt, download, replace the EXE, and restart.

## Notes
- Portable build no longer rebuilds native modules; matching Node version avoids ABI issues.
- Node and tunnel tools are required on the host if you use tunneling.

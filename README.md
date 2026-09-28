# Session Manager

A local desktop app that tracks your parallel [Claude Code](https://docs.anthropic.com/en/docs/claude-code) sessions and turns their questions and results into one inbox of to-dos. This repo holds the installers. Download the latest one from the [Releases](../../releases/latest) page.

## Install

1. Install Claude Code and sign in once, so `claude` works in a terminal.
2. Download the installer for your platform:
   - **macOS, Apple Silicon** (M1 and newer, including every current MacBook Pro): the `aarch64.dmg`
   - **macOS, Intel**: the `x64.dmg`
   - **Windows**: the `.msi` or `-setup.exe`
   - **Linux**: the `.AppImage`, `.deb`, or `.rpm`
3. Install and open Session Manager. The first time it opens it starts its background service automatically.
4. An alert strip under the status bar says hooks are not installed. Click **Install hooks**. This adds Session Manager to the hooks in `~/.claude/settings.json`, leaving your other hooks alone.
5. Start or continue any Claude Code session in a terminal. It appears in the Sessions panel on its next prompt.

That is the whole setup. The service keeps running after you close the window, so hook events are never missed while the app is open at least once per login.

### macOS: "app is damaged" or "cannot be opened"

The builds are not signed with an Apple certificate, so Gatekeeper refuses them the first time. Drag the app to Applications, then clear the quarantine flag once in Terminal and open it again:

```sh
xattr -dr com.apple.quarantine "/Applications/Session Manager.app"
```

### Where things live

| What | Path |
|---|---|
| Database and logs | `~/Library/Application Support/session-manager` (macOS), `~/.local/share/session-manager` (Linux), `%APPDATA%\session-manager` (Windows) |
| Service log | `<data dir>/daemon.log` |
| Service address | `http://127.0.0.1:7717` |

To uninstall, delete the app and that data directory, and remove the `session-manager` entries from the hooks in `~/.claude/settings.json`.

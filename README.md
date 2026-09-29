# Session Manager

A local desktop app that tracks your parallel [Claude Code](https://docs.anthropic.com/en/docs/claude-code) sessions and turns their questions and results into one inbox of to-dos. This repo holds the installers. Download the latest one from the [Releases](../../releases/latest) page.

## Which file to download

Each release lists one installer per platform. Pick the row that matches your machine. The two **Source code** links (`zip` and `tar.gz`) at the bottom of every release are added by GitHub automatically and contain only this README. Ignore them.

| Your machine | Download this file | How to tell |
|---|---|---|
| **Mac with Apple Silicon** (M1, M2, M3, M4 and newer) | `Session.Manager_<version>_aarch64.dmg` | Apple menu → **About This Mac** shows a chip named *Apple M…*. Every Mac sold since late 2020 except a few Intel models. |
| **Mac with an Intel processor** | `Session.Manager_<version>_x64.dmg` | Apple menu → **About This Mac** shows *Intel Core…*. |
| **Windows 10 or 11** (64-bit) | `Session.Manager_<version>_x64-setup.exe` | The `.msi` is the same installer in Windows Installer format, for people who deploy with Group Policy or prefer MSI. Either works. |
| **Linux, Ubuntu or Debian** | `Session.Manager_<version>_amd64.deb` | Install with `sudo apt install ./Session.Manager_<version>_amd64.deb`. |
| **Linux, Fedora, RHEL or openSUSE** | `Session.Manager-<version>-1.x86_64.rpm` | Install with `sudo dnf install ./Session.Manager-<version>-1.x86_64.rpm`. |
| **Linux, any other distribution** | `Session.Manager_<version>_amd64.AppImage` | No install. Run `chmod +x` on the file, then double-click or run it. |

If you pick the wrong Mac build the app will still open, but the Intel build runs slower on Apple Silicon and the Apple Silicon build will not open at all on an Intel Mac.

## Install

1. Install Claude Code and sign in once, so `claude` works in a terminal.
2. Download the installer for your machine from the table above.
3. Install and open Session Manager. The first time it opens it starts its background service automatically.
4. An alert strip under the status bar says hooks are not installed. Click **Install hooks**. This adds Session Manager to the hooks in `~/.claude/settings.json`, leaving your other hooks alone.
5. Start or continue any Claude Code session in a terminal. It appears in the Sessions panel on its next prompt.

That is the whole setup. The service keeps running after you close the window, so hook events are never missed while the app is open at least once per login.

### macOS: "app is damaged" or "cannot be opened"

The builds are not signed with an Apple certificate, so Gatekeeper refuses them the first time. Open the `.dmg`, drag the app to Applications, then clear the quarantine flag once in Terminal and open it again:

```sh
xattr -dr com.apple.quarantine "/Applications/Session Manager.app"
```

### Windows: "Windows protected your PC"

The builds are not signed with a Windows certificate either, so SmartScreen shows this on first run. Click **More info**, then **Run anyway**. It only asks once per installer.

### Linux: the AppImage does nothing when clicked

Make it executable first:

```sh
chmod +x Session.Manager_*_amd64.AppImage
./Session.Manager_*_amd64.AppImage
```

## Updating

Download the new installer and run it over the old one. The database, themes, and window layout live in the data directory below and are kept. Installed hooks keep working because they point at a copy of the hook binary in that directory, not at the app.

## Where things live

| What | Path |
|---|---|
| Database and logs | `~/Library/Application Support/session-manager` (macOS), `~/.local/share/session-manager` (Linux), `%APPDATA%\session-manager` (Windows) |
| Service log | `<data dir>/daemon.log` |
| Service address | `http://127.0.0.1:7717` |

To uninstall, delete the app and that data directory, and remove the `session-manager` entries from the hooks in `~/.claude/settings.json`.

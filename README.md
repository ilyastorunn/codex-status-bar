<img width="672" height="80" alt="Codex Status Bar screen recording placeholder" src="https://github.com/user-attachments/assets/REPLACE_WITH_STATUS_BAR_GIF" />
<br><br>

<a href="https://github.com/ilyastorunn/codex-status-bar/releases/latest/download/CodexStatusBar.dmg"><img src="assets/download.png" alt="Download CodexStatusBar.dmg for macOS" width="260"></a>
<br>

## Codex Status Bar

A tiny macOS menu bar app that shows **Codex's live status**: an animated Codex icon while it's thinking or running a tool, a yellow dot when it's awaiting your permission, and the elapsed time of the current turn. Lightweight, no window, no dock icon, no usage dashboards.

> Built so you can tab away during a long "thinking" stretch and still see, at a glance, whether Codex is working, waiting on you, or done.

Inspired by [Claude Status Bar](https://github.com/m1ckc3s/claude-status-bar) by [@m1ckc3s](https://github.com/m1ckc3s). This project follows the same small local-file + menu-bar idea, adapted for Codex hooks.

<img width="710" height="714" alt="Codex Status Bar demo placeholder" src="https://github.com/user-attachments/assets/REPLACE_WITH_DEMO_GIF" />
<br>

> [!IMPORTANT]
> **Multi-session support.** This is built for one active Codex session at a time. If you
> run multiple sessions at once (several terminals, or a terminal plus the desktop app), the menu
> bar follows the currently active user turn. Background/internal events are ignored when possible,
> and quiet thinking states auto-clear after a short idle window.

---

## What it shows

- **Thinking / working** - the icon animates, with a live `1m 1s` timer.
- **Running a tool** - a short label (`Editing`, `Reading`, `Running command`, `Using tool`, ...).
- **Awaiting permission** - a paused yellow dot when Codex is waiting for your approval.
- **Idle / done** - rests on the Codex icon.

Everything is controlled from the menu:

- **Show timer:** toggle the elapsed `1m 1s` clock.
- **Use system icon color:** switch between the adaptive macOS menu bar glyph and a state-tinted Codex glyph.
- **Reveal State File:** open the local status JSON in Finder.
- **Reset Status:** clear the current menu bar state.

## Where it works

| Surface | Tracked? |
|---|---|
| Codex CLI / terminal sessions | ✅ |
| Codex Desktop app projects | ✅ |
| Permission requests | ✅ |
| Multiple simultaneous Codex sessions | Follows the active turn |
| ChatGPT / unrelated OpenAI apps | ❌ |

## Requirements

- macOS 12+
- Codex with hooks enabled
- Node.js

## Install

### Option A - DMG (recommended)

Open it, drag the app to Applications, launch once, then install hooks.

1. Download the latest `CodexStatusBar.dmg` from [Releases](../../releases).
2. Open it and drag **Codex Status Bar** into Applications.
3. Launch **Codex Status Bar** once.
4. Install the Codex hooks:

```bash
node "/Applications/CodexStatusBar.app/Contents/Resources/install-codex-statusbar.js"
```

5. Start a new Codex session. The icon appears whenever Codex is running.

### Updating

Download the latest DMG and drag it into Applications (choose **Replace**).
Then run the installer once so hooks point at the new app copy:

```bash
node "/Applications/CodexStatusBar.app/Contents/Resources/install-codex-statusbar.js"
```

## Build From Source

```bash
git clone https://github.com/ilyastorunn/codex-status-bar.git
cd codex-status-bar
./build.sh
open -g build/CodexStatusBar.app
node scripts/install-codex-statusbar.js
```

`./build.sh` creates:

- `build/CodexStatusBar.app`
- `build/CodexStatusBar.dmg`

## How it works

The app is stateless. Codex hooks write the current status to `~/.codex/statusbar/state.json`; the app polls that file every 0.4s and renders the icon and label.

The installer merges its hooks into `~/.codex/hooks.json` and backs it up first. The status writer stores only minimal display metadata: state, label, tool category, project basename, sanitized session id, and timestamps. It does not store prompts, command output, transcript contents, or secrets.

Codex Desktop can emit lifecycle/tool events differently from the CLI, so Codex Status Bar guards events by active `session_id` / `turn_id`, expires tool labels quickly, and clears quiet thinking states after a short idle window.

## Uninstall

```bash
node "/Applications/CodexStatusBar.app/Contents/Resources/uninstall-codex-statusbar.js"   # removes only our hooks
```

Then drag the app to the Trash.

## Trademark / Not Affiliated

This is an unofficial, open-source side project. **It is not affiliated with, endorsed by, or sponsored by OpenAI.** "OpenAI", "ChatGPT", and "Codex" are trademarks of OpenAI, used here nominatively. This project is MIT licensed, but that covers the source code only and conveys no rights to OpenAI's trademarks or brand.

If this project violates or impedes any trademark or brand usage, please open an issue.

## Attribution

This project is heavily inspired by [Claude Status Bar](https://github.com/m1ckc3s/claude-status-bar) by [@m1ckc3s](https://github.com/m1ckc3s). Thank you for the original idea and implementation pattern.

The download button image is adapted from the MIT-licensed Claude Status Bar repository.

## License

MIT

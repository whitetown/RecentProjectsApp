# RecentProjects

A macOS menubar app that unifies recent project folders from your dev tools
(VS Code, Xcode, Android Studio, Sourcetree, …) into a single grouped menu —
solving the "which `webapp` is this?" problem when you work across multiple
companies or clients with similar folder names.

## The problem it solves

If you bounce between several projects with names like `webapp`, `ios`,
`android`, or `api`, the OS's built-in recent lists become useless:

- The Dock's "Recent Folders" shows two entries called `webapp` with no
  parent context — indistinguishable.
- VS Code's "Open Recent" gives you full paths but is buried, capped, and
  doesn't include projects from Xcode or Android Studio.
- Each tool keeps its own short list, so a project rolls off as soon as you
  open ~10 newer ones.

RecentProjects pulls from all your dev tools' recent lists, groups them by
**parent folder** (effectively by company), remembers items long after the
source tools forget them, and lets you open each one in whichever editor
makes sense.

## Install

1. Unzip the download. Move `RecentProjects.app` to `/Applications`.
2. Double-click it. A folder-with-gear icon appears in your menubar.
3. (Optional) **System Settings → General → Login Items & Extensions →
   Open at Login → +** and pick RecentProjects, so it starts on every boot.

There is no Dock icon — RecentProjects is a menubar-only app.

### First-launch prompts

On macOS 13+ you may see a one-time alert: _"RecentProjects wants to access
data managed by another application."_ Click **Allow** — the app needs this
to read VS Code / Xcode / Android Studio's own recent-files records.

If a particular source stops working, granting **Full Disk Access** in
**System Settings → Privacy & Security → Full Disk Access** fixes it.

## Usage

Click the menubar icon. You'll see something like:

```
★ Pinned
  webapp                · today
  receipts-android — finstat   · today

whitetown ▸   castles          · yesterday
              webapp-api       · 3d ago
              castles/astro    · Dec 12

tribo     ▸   web              · today
              backend          · 2d ago
              ios              · 12.06.2025

temaron   ▸   webapp           · yesterday
              ios              · 5d ago

Refresh
Preferences…
Quit
```

- **Click a project** → opens in your default editor (configurable).
- **⌥-click a project** → reveals an action submenu:
  - _Open in VS Code / Xcode / Android Studio / …_ — every app from your
    Preferences list
  - _Reveal in Finder_
  - _Open in Terminal_
  - _Pin_ — keeps it at the top, regardless of recency
  - _Hide_ — removes it from the menu. Auto-restores if you reopen the
    project in any tracked editor.
  - _Copy Path_
- **Refresh** — re-reads all source apps' recents on demand.

The list under each company submenu is sorted most-recent-first. Items
"forgotten" by their source apps still appear because RecentProjects keeps
its own rolling history.

## Preferences

Menubar icon → **Preferences…** (or ⌘, when the menu is open).

The window shows the editor list used in the action submenu. The
**starred** app is the default — clicking a project (without ⌥) opens it in
that one. Reorder by dragging. **Add App…** lets you point to any `.app`
bundle on your Mac. **Remove** drops the selected row.

On first launch the list is auto-populated with whichever of these are
installed: VS Code, Xcode, Android Studio, Sourcetree, Cursor, Zed.

## How it discovers projects

| Source         | Where it reads from                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| VS Code        | `~/Library/Application Support/Code/User/globalStorage/storage.json`                                                                                                                                                               |
| Xcode          | `~/Library/Preferences/com.apple.dt.Xcode.plist` (best-effort — modern Xcode versions store recents in varying locations)                                                                                                          |
| Android Studio | `~/Library/Application Support/Google/AndroidStudio*/options/recentProjects.xml`                                                                                                                                                   |
| Sourcetree     | `~/Library/Application Support/SourceTree/openWindowList`                                                                                                                                                                          |
| Discovery      | Scans the parent folders you already work in (depth 2) for git repos and project markers (`package.json`, `Cargo.toml`, `pyproject.toml`, `Package.swift`, `*.xcodeproj`, etc.). Surfaces projects with activity in the last year. |

All reads are local and read-only.

## Where your data is stored

All in `~/Library/Application Support/RecentProjects/`:

- `apps.json` — your editor preferences (the "Open in…" list and default)
- `history.json` — rolling memory of every project ever observed (cap: 100)
- `pins.json` — pinned project paths
- `hidden.json` — paths you've hidden from the menu

No data leaves your machine. No telemetry, no network calls.

## Requirements

- macOS 14 or later
- Apple Silicon or Intel

## Uninstall

1. Drag `/Applications/RecentProjects.app` to the Trash.
2. (Optional) Remove its settings: `rm -rf ~/Library/Application\ Support/RecentProjects`
3. (Optional) Remove it from **System Settings → Login Items**.

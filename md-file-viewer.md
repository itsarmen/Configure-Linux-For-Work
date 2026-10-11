# MD File Viewer — Native GitHub-Style Markdown Viewer/Editor

A small native Linux desktop app for **viewing and editing Markdown** with the
same formatting GitHub uses. It rounds out this productivity setup: read
READMEs, notes and docs without opening a browser, and keep `.md` files in a
fast, familiar editor.

- **Repository:** https://github.com/itsarmen/MD-File-Viewer-Software
- **Latest release:** https://github.com/itsarmen/MD-File-Viewer-Software/releases/latest
- Built with **Flutter** and the **Yaru (GNOME)** theme.

## Why it helps

- **GitHub-flavoured preview** — headings, tables, task lists, blockquotes and
  fenced code blocks with syntax highlighting, plus local, remote and HTML
  `<img>` images.
- **Three view modes** — `Preview`, `Split` (editor + live preview) and `Edit`.
- **Folder browsing** and **recent files** between sessions.
- **Selectable, copyable text** everywhere, with smart links: external links open
  in your browser, links to local `.md` files open inside the app.
- **Native look & feel** — Yaru theme with automatic light/dark.

## Install (Debian / Ubuntu) — recommended

Install the latest `.deb` straight from the releases page:

```bash
curl -LO https://github.com/itsarmen/MD-File-Viewer-Software/releases/latest/download/md-file-viewer_1.0.0_amd64.deb
sudo apt install ./md-file-viewer_1.0.0_amd64.deb
```

Then launch **MD File Viewer** from the application menu, or open a file directly:

```bash
md-file-viewer path/to/README.md
```

## Install (user-level, no root)

A helper script installs the app, icons and desktop entry into `~/.local`:

```bash
git clone https://github.com/itsarmen/MD-File-Viewer-Software.git
cd MD-File-Viewer-Software
flutter pub get
flutter build linux --release
./scripts/install.sh
```

## Build a Debian package (.deb)

```bash
flutter build linux --release
./scripts/build-deb.sh
sudo apt install ./dist/md-file-viewer_*.deb
```

## Set as the default `.md` handler

Open **Quick Settings** (gear icon in the toolbar) to see whether the app is
installed and to set it as the default Markdown handler with one click (it runs
the `xdg-mime` registration). Double-clicking a `.md` file then opens it here.

## Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl+O` | Open file |
| `Ctrl+S` | Save |
| `Ctrl+Shift+S` | Save as… |
| `↑` / `↓` | Scroll preview by one line |
| `Page Up` / `Page Down` | Scroll preview by one page |
| `Home` / `End` | Jump to top / bottom of preview |

## Notes

- Supported extensions: `md`, `markdown`, `mdx`, `mdown`, `mkd`, `txt`.
- Requirements: GTK 3, GLib and libstdc++6 (present on most desktops).
- If the start-menu icon reports `not found in $PATH` right after switching from
  a `~/.local` install to the `.deb`, **log out and back in** — GNOME Shell
  caches the old launcher path for the lifetime of the session.
- License: **MIT** — free and open source. Original author and copyright holder:
  **Amiran Kimadze (itsarmen)**, who must remain credited in copies and forks.

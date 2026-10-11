# Firefox (snap): "Save As" Silently Fails / Desktop Not Reachable

Fixes for two related problems with saving files in the Firefox **snap** on
Ubuntu (Wayland):

1. Right-click → **Save Image As** (or any download) reported success but no
   file was written — the file never appeared on disk or in the downloads list.
2. After fixing (1), the Save dialog could not navigate to **Desktop** or its
   subfolders.

## Cause

- **Problem 1:** Since Firefox 149, the file picker is routed through the XDG
  desktop portal by default (`widget.use-xdg-desktop-portal.file-picker = 2`).
  On this setup the portal save path silently fails to write the file.
- **Problem 2:** The snap runs with `HOME=/home/xrow/snap/firefox/common`, not
  the real home. The built-in GTK picker resolves `Desktop`, `Documents`,
  `Downloads`, etc. relative to that sandbox home, where those folders do not
  exist. (The portal picker ran outside the snap, which is why it showed the
  real Desktop.)

Confirm the snap home:

```bash
FP=$(pgrep -f "/snap/firefox/.*/firefox$" | head -1)
tr '\0' '\n' < /proc/$FP/environ | grep -E '^(HOME|SNAP_REAL_HOME)='
```

## Fix 1 — Use the built-in GTK picker instead of the portal

Created `~/snap/firefox/common/.mozilla/firefox/<profile>/user.js`:

```bash
P=~/snap/firefox/common/.mozilla/firefox/8snii5ps.default
cat > "$P/user.js" <<'EOF'
user_pref("widget.use-xdg-desktop-portal.file-picker", 0);
EOF
```

`0` = disabled (built-in GTK picker), `1` = enabled, `2` = auto (default).

## Fix 2 — Point the sandbox home at the real folders

Symlink the real XDG user dirs into the snap home. These live in
`SNAP_USER_COMMON`, so they survive Firefox updates:

```bash
C=~/snap/firefox/common
for d in Desktop Documents Downloads Music Pictures Videos Public Templates; do
  [ -e "$HOME/$d" ] && ln -sfn "$HOME/$d" "$C/$d"
done
```

## Apply

Fully quit Firefox and reopen it (both the pref and the folder list are cached
per process):

```bash
pkill firefox
firefox
```

## Verify

- Right-click an image → Save Image As → choose any folder (including Desktop
  subfolders) → the file is written.
- Inside the sandbox, the sandbox home now resolves to the real folders:

```bash
snap run --shell firefox -c 'ls "$HOME/Desktop"'
```

## Notes

- The snap's `home` interface already allows absolute access to `/home/xrow/**`;
  only the `$HOME`-relative shortcuts were broken.
- Absolute paths always worked: in the file dialog press `Ctrl+L` and type the
  path, e.g. `/home/xrow/Desktop/project/`.
- Leftover partial downloads (`~/Downloads/*.part`) from failed saves are safe
  to delete.
- To stop overriding the pref later, delete the `user.js` file and set
  `widget.use-xdg-desktop-portal.file-picker` in `about:config` instead.
- `/proc/<pid>/environ`, `snap connections firefox`, and
  `journalctl -k | grep apparmor` are useful for diagnosing snap issues.

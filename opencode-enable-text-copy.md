# OpenCode: Enable Text Copying in the TUI

The OpenCode terminal UI captures mouse events by default (`mouse: true`), which
blocks the terminal's native click-drag text selection. Disabling mouse capture
restores normal select-and-copy behavior.

## Commands used

Create the TUI config file with mouse capture disabled:

```bash
cat > ~/.config/opencode/tui.json <<'EOF'
{
  "$schema": "https://opencode.ai/tui.json",
  "mouse": false
}
EOF
```

Contents of `~/.config/opencode/tui.json`:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "mouse": false
}
```

## Apply

Quit and restart `opencode` for the change to take effect. Config is loaded once
at startup and is not hot-reloaded.

## Verify

```bash
cat ~/.config/opencode/tui.json
```

Then relaunch `opencode` and drag-select text with the mouse; the terminal's
own selection and copy shortcut now work.

## Alternatives (without changing config)

- Hold `Shift` while dragging to select text — bypasses mouse capture in most
  Linux terminals (GNOME Terminal, GNOME Console, etc.).
- Press `ctrl+x` then `y` to copy the last assistant message to the clipboard.
- Run `/export` to write the whole conversation to Markdown and open it in
  `$EDITOR`.

## Notes

- `tui.json` is separate from `opencode.json`; it configures only the TUI.
- Trade-off: with `mouse: false`, mouse-wheel scrolling inside the TUI is
  handled by the terminal instead of OpenCode.
- Location of the global config: `~/.config/opencode/`.
- A custom path can be used via the `OPENCODE_TUI_CONFIG` environment variable.
- Docs: https://opencode.ai/docs/tui

# Show Hidden Files & Folders Globally (GNOME)

Instructions to always show hidden files and folders system-wide on GNOME.

## 1. Nautilus (GNOME Files app)

```bash
gsettings set org.gnome.nautilus.preferences show-hidden-files true
```

## 2. GTK file chooser dialogs (Open/Save in all apps)

```bash
gsettings set org.gtk.Settings.FileChooser show-hidden true
```

## Verify

```bash
gsettings get org.gnome.nautilus.preferences show-hidden-files
gsettings get org.gtk.Settings.FileChooser show-hidden
```

Both should return `true`.

## Notes

- Applies to the current user only.
- No restart required; new windows pick up the change immediately.
- Hidden entries are files/folders whose name starts with a dot (`.`).

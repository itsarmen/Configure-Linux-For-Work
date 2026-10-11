# npm: Always Show Full Install Logs

npm 11 hides install output behind a progress spinner when `loglevel=notice`.
To always see the full step-by-step logs, raise the log level and disable the spinner.

## Commands used

```bash
printf 'loglevel=verbose\nprogress=false\nfund=false\naudit=false\n' > ~/.npmrc
```

## Verify

```bash
npm config get loglevel
npm config get progress
```

## Notes

- `loglevel=verbose` -> prints full install logs.
- `loglevel=silly` -> even more detail (debug-level).
- `progress=false` -> disables the spinner animation.
- `fund=false`, `audit=false` -> remove the funding/audit summary noise.
- The config lives in `~/.npmrc` and applies to every npm command.

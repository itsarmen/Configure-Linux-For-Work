# GitHub PAT Workflow — Clone, Push, and Release

Everything needed to authenticate with a Personal Access Token (PAT), push
markdown docs to GitHub, and publish a release — without typing the token every
time.

## 1. Create a classic token

In the browser (logged into GitHub): https://github.com/settings/tokens

1. **Generate new token** -> **Generate new token (classic)**.
2. **Note:** e.g. `laptop-git`.
3. **Expiration:** pick a date (prefer expiring).
4. **Scopes:** check only **`repo`** — it covers private + public push/pull and
   releases. Add `workflow` only if a repo has `.github/workflows` you push to.
5. **Generate token** and copy it now (`ghp_...`) — shown only once.

## 2. Configure git identity (once)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## 3. Store the token globally (no prompts)

```bash
git config --global credential.helper store
printf "https://<your-username>:<token>@github.com\n" > ~/.git-credentials
chmod 600 ~/.git-credentials
```

Every `git push` / `git pull` now authenticates automatically. When the token is
rotated, re-run the `printf` line with the new value.

## 4. Initialize and push the repo

```bash
cd ~/Documents/Configure-Linux-For-Work
git init
git add .
git commit -m "Add Linux configuration instructions"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo>.git
git push -u origin main
```

## 5. Tag and create a release

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

Then publish the release via the GitHub API (or **Releases -> Draft a new
release** in the browser), which needs no scope beyond `repo`:

```bash
curl -sS -X POST \
  https://api.github.com/repos/<your-username>/<repo>/releases \
  -H "Authorization: Bearer <token>" \
  -H "Accept: application/vnd.github+json" \
  -d '{"tag_name":"v1.0.0","name":"v1.0.0","body":"Initial release.","draft":false,"prerelease":false}'
```

## Verify

```bash
git config --global --get credential.helper   # -> store
git ls-remote https://github.com/<your-username>/<repo>.git   # no prompt
```

## Notes / Security

- `store` keeps the token in plaintext in `~/.git-credentials` (`chmod 600`).
- Prefer an expiring token, and never commit or paste the token anywhere.
- Revoke a token anytime at https://github.com/settings/tokens.
- Always use HTTPS clone URLs for PATs, not SSH (`git@github.com:...`).
- To keep the token in memory only (expires after a timeout) instead:

  ```bash
  git config --global credential.helper 'cache --timeout=86400'
  ```

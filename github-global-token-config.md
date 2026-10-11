# Configure GitHub Token Globally (no prompt every time)

Store your Personal Access Token (PAT) once so `git push` / `git pull` never ask
for a username and password again.

## 1. Create the token

In the browser (logged into GitHub), create a classic token with the **`repo`**
scope: https://github.com/settings/tokens

- **Note:** e.g. `laptop-git`
- **Expiration:** pick a date (prefer expiring)
- **Scopes:** check `repo` (clone/pull/push, private + public, releases)
- Copy the token now (`ghp_...`) — GitHub shows it only once.

## 2. Use the `store` credential helper globally

```bash
git config --global credential.helper store
```

## 3. Store the token without typing it each time

Instead of entering username/token at the prompt on every operation, write the
credential directly to the store file (replace `<your-username>` and `<token>`):

```bash
printf "https://<your-username>:<token>@github.com\n" > ~/.git-credentials
chmod 600 ~/.git-credentials
```

This is a global setting, so every `git push` / `git pull` now authenticates
automatically with no prompt.

## 4. Rotating the token

When the token expires or is revoked, generate a new one and re-run the command
from step 3 with the new value. That's the only change needed.

## Verify

```bash
git config --global --get credential.helper   # -> store
git ls-remote https://github.com/<your-username>/<repo>.git   # no prompt
```

## Notes / Security

- `store` keeps the token in plaintext in `~/.git-credentials` (`chmod 600`
  limits it to your user).
- Prefer an expiring token over a non-expiring one.
- Never commit or paste the token anywhere. Revoke it anytime at
  https://github.com/settings/tokens.
- Always use HTTPS clone URLs for PATs, not SSH (`git@github.com:...`).
- To keep the token in memory only (expires after a timeout) instead:

  ```bash
  git config --global credential.helper 'cache --timeout=86400'
  ```

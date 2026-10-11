# Cloning Private GitHub Repos with a Personal Access Token (PAT)

GitHub no longer accepts your account password for Git operations. To clone
private repos over HTTPS, authenticate with a Personal Access Token.

## 1. Create a Personal Access Token

This must be done in the browser while logged into GitHub — it cannot be done
from the terminal.

### Option A — Classic token (simplest)

1. Go to https://github.com/settings/tokens
2. Click **Generate new token** -> **Generate new token (classic)**.
3. Confirm your password / 2FA if GitHub asks.
4. **Note:** enter a label, e.g. `laptop-git`.
5. **Expiration:** pick e.g. 90 days (or "No expiration").
6. **Select scopes:** check the top-level **`repo`** box
   (full control of private repositories — clone/pull/push).
7. Scroll down and click **Generate token**.
8. **Copy the token now** (starts with `ghp_...`). GitHub shows it only once.

### Option B — Fine-grained token (least privilege)

1. Go to https://github.com/settings/personal-access-tokens/new
2. **Token name:** e.g. `laptop-git`.
3. **Expiration:** pick a date.
4. **Repository access:** choose **Only select repositories** and pick the
   private repos you clone.
5. **Permissions** -> **Repository permissions** -> **Contents** ->
   **Read-only** (or **Read and write** if you also push).
6. Click **Generate token**, then **copy** it (starts with `github_pat_...`).

Copy the token immediately — GitHub only shows it once.

> Tip: if the token page won't load, make sure you are logged in and have
> confirmed your email address on GitHub.

## 2. Configure Git to store credentials

```bash
git config --global credential.helper store
```

### Store the token without typing it each time

Rather than entering username/token at the prompt on every operation, write the
credential directly to the store file (replace `<your-username>` and `<token>`):

```bash
printf "https://<your-username>:<token>@github.com\n" > ~/.git-credentials
chmod 600 ~/.git-credentials
```

This is a global setting, so every `git push` / `git pull` now authenticates
automatically with no prompt. When the token is rotated, just re-run the command
above with the new token.

## 3. Clone over HTTPS

```bash
git clone https://github.com/<your-username>/<repo>.git
```

When prompted:

- **Username:** your GitHub username
- **Password:** paste the **token** (not your account password)

After the first successful clone, the token is saved to `~/.git-credentials`
and you will not be prompted again.

## Verify

```bash
git config --global --get credential.helper   # -> store
```

## Notes / Security

- `store` keeps the token in plaintext in `~/.git-credentials`.
- Prefer an expiring token (set an expiration date) over a non-expiring one.
- To keep the token in memory only (expires after a timeout) instead:

  ```bash
  git config --global credential.helper 'cache --timeout=86400'
  ```

- Revoke a token anytime at https://github.com/settings/tokens.
- Always use HTTPS clone URLs for PATs, not SSH (`git@github.com:...`).

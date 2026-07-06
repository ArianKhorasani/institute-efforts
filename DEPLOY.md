# Deploying the CAISI dashboard to GitHub Pages (password-protected)

You have **one file**: `index.html` — a fully self-contained, encrypted dashboard.
Nothing else is needed (no data files, no build step, no server).

## How the privacy works
GitHub Pages on a free account serves from a **public** repo, so the *file* is public —
but its contents are **AES-256-GCM encrypted**. Without the password the page shows only
a prompt; the underlying numbers/charts are unreadable. Anyone you give **the URL + the
password** can view it, and they can pass both along to others. (This is a client-side
gate — good for keeping a draft out of public view, not a guarantee against a determined
attacker. Don't put anything you couldn't tolerate leaking.)

- **URL (after deploy):** https://ariankhorasani.github.io/institute-efforts/
- **Password:** `AISafety-Jinesis`

## Option A — GitHub website (no terminal)
1. Create a new **public** repo named **`institute-efforts`** under your account.
2. Click **Add file → Upload files**, upload `index.html`, commit to `main`.
3. **Settings → Pages →** Source: *Deploy from a branch* → Branch: `main` / `/ (root)` → Save.
4. Wait ~1 minute, then open the URL above and enter the password.

## Option B — command line (on a machine where you're logged into GitHub)
```bash
# with GitHub CLI:
gh repo create ArianKhorasani/institute-efforts --public --clone
cd institute-efforts
cp /path/to/index.html .
git add index.html && git commit -m "CAISI AI-safety dashboard (encrypted draft)"
git push -u origin main
gh api -X POST repos/ArianKhorasani/institute-efforts/pages -f source[branch]=main -f source[path]=/ 2>/dev/null || \
  echo "If that errors, enable Pages once in Settings → Pages (branch: main, /root)."
```

## Only commit `index.html`
Do **not** add the raw data, `index_inner.html`, or the collection scripts to a public
repo — those contain the unencrypted dashboard. `index.html` alone is safe to publish.

## Updating later
Re-generate `index.html` (same password or a new one) and push over the old file —
the URL stays the same. When your advisor approves, the same `index.html` (or the
unencrypted `index_inner.html`) drops straight into the EuroSafeAI repo.

# Keisaal Smithy — hosting & access setup

The calculator now opens behind an **access gate**. Users need a code you sell; you manage
everything from a built-in **admin panel**. Access lives in `status.json`, published next to
the page on GitHub. Fail-closed: no reachable `status.json` = no access.

---

## 1. Put it on GitHub Pages (free)

1. Create a **public** GitHub repo (e.g. `keisaal`).
2. Upload two files to the repo root:
   - `keisaal-calculator.html` — **rename it to `index.html`** so the URL is clean.
   - `status.json` (the starter file — week 1, no codes).
3. Repo **Settings → Pages** → *Build and deployment* → **Deploy from a branch** → `main` / `/root` → Save.
4. Wait ~1 min, then open `https://YOUR-NAME.github.io/keisaal/`. You should see the access gate.

> Public repo is fine: the page only ever ships **hashes**, never real codes or your token.

## 2. Make a GitHub token (one time)

1. GitHub → **Settings → Developer settings → Fine-grained tokens → Generate new token**.
2. **Repository access → Only select repositories →** pick just this repo.
3. **Repository permissions → Contents → Read and write**. (Nothing else.)
4. Generate, copy the `github_pat_…` string.

The token is entered into the admin panel and stored **only in your browser** — it is never in
the file, so a visitor can never see it.

## 3. First run of the admin panel

Do admin **on the live https site** (GitHub's API blocks writes from a local file).

1. Open your Pages URL → click **admin** at the bottom of the gate.
2. **Create an admin passphrase** (first time — remembered in this browser).
3. Fill in: Owner = your GitHub name, Repository = repo name, Branch = `main`, Path = `status.json`,
   and paste your **token**. Click **Save settings**, then **Load from GitHub**.
4. **Generate** a code for yourself → **Publish to GitHub**.
5. Close admin, **Unlock** with that code. You're in.

---

## Weekly running

| Task | Steps |
|------|-------|
| **Weekly reset** | admin → **Advance week ▸** → **Publish**. Every code paid only through last week dies. |
| **Sell access** | admin → type a buyer note + weeks paid → **Generate** → copy code to buyer → **Publish**. |
| **Revoke a leaker** | admin → **✕** on their row → **Publish**. Locked out within a minute. |
| **Lock everyone** | admin → **Kill switch** → **Publish**. |
| **Backup** | admin → **Download backup** (your codes + private buyer notes). |

Buyer notes stay **local to your browser** (never published), so no player names leak into the
public file. Keep a backup, and always run admin from the same browser.

## Honest limits

- This is friction for an honest game community, **not** unbreakable DRM. Because everything runs
  in the browser, a determined person who reads the source can bypass the gate. The design keeps
  your **secret (token) off the page** and stores only **un-reversible hashes**, so nobody can mint
  valid codes or read others' codes — but the tool's logic itself is visible in a public repo.
- The clock-rollback trick doesn't work: validity is decided by the week number in *your* hosted
  file, not the user's PC clock.

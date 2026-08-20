# Keisaal Smithy — live site & running it

**Live URL:** https://ipaintz.github.io/keisaal-calculator/
**Repo:** https://github.com/IPaintZ/keisaal-calculator (public)

The calculator opens behind an **access gate**. Users need a weekly code you sell. You manage
everything from the built-in **admin panel**. Access lives in `status.json` next to the page.
Fail-closed: if `status.json` can't be reached, nobody gets in.

The shipped database has the full item/recipe catalog but **zero pricing** — every visitor fills
in their own prices, saved in their own browser (`localStorage`). Nothing they enter touches anyone else.

---

## Admin passphrase

    frost-cinder-frost-94

Only its hash is in the source, so this is safe to ship publicly. To change it, tell Claude a new
passphrase and it'll re-bake the hash. Note: even the passphrase is a courtesy lock — the panel is
**powerless without your GitHub token**, which never leaves your browser.

## One-time: make a GitHub token

1. GitHub → **Settings → Developer settings → Fine-grained tokens → Generate new token**.
2. **Repository access → Only select repositories →** `keisaal-calculator`.
3. **Repository permissions → Contents → Read and write**. Nothing else.
4. Generate and copy the `github_pat_…` string.

## One-time: connect the admin panel

Do this **on the live https site** (GitHub blocks writes from a local file).

1. Open the live URL → click **admin** at the bottom of the gate.
2. Passphrase: `frost-cinder-frost-94`.
3. Fill in: Owner = `IPaintZ`, Repository = `keisaal-calculator`, Branch = `main`,
   Path = `status.json`, and paste your **token**. Click **Save settings** → **Load from GitHub**.
4. **Generate** a code for yourself → **Publish to GitHub**.
5. Close admin and **Unlock** with that code.

---

## Weekly running

| Task | Steps |
|------|-------|
| **Weekly reset** | admin → **Advance week ▸** → **Publish**. Codes paid only through last week die. |
| **Sell access** | admin → buyer note + weeks paid → **Generate** → send code to buyer → **Publish**. |
| **Revoke a leaker** | admin → **✕** on their row → **Publish**. Locked out within a minute. |
| **Lock everyone** | admin → **Kill switch** → **Publish**. |
| **Backup** | admin → **Download backup** (your codes + private buyer notes). |

Buyer notes stay local to your browser (never published), so no names leak into the public file.
Always run admin from the same browser, and keep a backup.

## Updating the tool later

Edit `index.html` and push:

```bash
git add index.html && git commit -m "update" && git push
```

## Honest limits

- Friction for an honest game community, not unbreakable DRM: the tool's logic is visible in a
  public repo, so someone who reads the source could bypass the gate. What's protected: your token
  never ships, and only un-reversible SHA-256 hashes are published — nobody can mint valid codes,
  read anyone's code, or use the admin panel against your repo.
- Clock-rollback doesn't work: validity is decided by the week number in your hosted file, not the
  user's PC clock.

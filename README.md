# Countdown — marketing & legal site

Static pages served from this folder, paired to the iOS app. The four
documents Apple wants for App Store Connect submission live here:

| Page | URL once deployed | App Store Connect field |
|---|---|---|
| Marketing landing | `/` | App Store Connect → App Information → Marketing URL |
| Privacy policy (EN) | `/privacy.html` | App Store Connect → App Privacy → Privacy Policy URL |
| Privacy policy (RU) | `/privacy.ru.html` | Locale-specific listing |
| Terms of use (EN) | `/terms.html` | EULA tab / In-App Purchase metadata |
| Terms of use (RU) | `/terms.ru.html` | Locale-specific listing |
| Support page | `/support.html` | App Store Connect → App Information → Support URL |

## Source of truth

The **legal copy** is *not* edited here directly — it lives in
[`lib/data/legal/legal_documents.dart`](../lib/data/legal/legal_documents.dart)
and is rendered both inside the iOS app and into the HTML you see here.

To regenerate the HTML after editing the Dart source:

```bash
dart run tool/export_legal_html.dart
```

The script overwrites `privacy.html`, `privacy.ru.html`, `terms.html`,
`terms.ru.html` in this folder. Re-run after every legal edit. CI can
call the same command on push.

`index.html`, `support.html`, and `_config.yml` are hand-written — they
have nothing to do with the in-app screens and don't have a generator.

## Local preview

```bash
cd docs
python3 -m http.server 8000
# then open http://localhost:8000/
```

## Deploying via GitHub Pages (zero infrastructure cost)

1. Push this repo to GitHub.
2. On GitHub: **Settings → Pages → Source = Deploy from a branch**.
3. **Branch = `main`**, **Folder = `/docs`**. Save.
4. Wait ~1 minute. Your URLs:
   - `https://<your-user>.github.io/<repo-name>/`
   - `https://<your-user>.github.io/<repo-name>/privacy.html`
   - `https://<your-user>.github.io/<repo-name>/terms.html`
   - `https://<your-user>.github.io/<repo-name>/support.html`
5. Optional: add a custom domain (e.g. `countdown.app`) under
   Pages → Custom domain. The links inside the HTML use absolute
   `https://countdown.app/...` for the canonical/og tags so SEO is
   correct once a custom domain is wired; visitor-facing nav links are
   relative and work on any host.

## Before App Store submission

- [ ] Run `dart run tool/export_legal_html.dart` and commit the diff.
- [ ] Replace the placeholder App Store CTA in `index.html` with the
      real `https://apps.apple.com/...` URL.
- [ ] Confirm `support@countdown.app` (or whichever email you wire) is
      monitored — App Review will email this address.
- [ ] Open all four pages in mobile Safari and verify they read cleanly
      on a small screen — Apple checks.
- [ ] Make sure the `last-updated` dates match across in-app + HTML by
      regenerating (they share `lastUpdatedEn` / `lastUpdatedRu`
      constants).

## Not provided

These need your accounts and aren't generated:

- A custom domain (`countdown.app` is a placeholder in metadata).
- The real "Download on the App Store" badge / link (placeholder CTA on
  `index.html`).
- Localised App Store screenshots / video previews.

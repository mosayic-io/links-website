# CLAUDE.md

A tiny Astro site with two jobs, deployed on a dedicated subdomain of the app's
domain — by convention `links.<domain>` — so the whole subdomain belongs to
the app. See `README.md` for the routes, the config keys and the scripts.

1. **Deep-link plumbing.** `public/.well-known/apple-app-site-association` and
   `public/.well-known/assetlinks.json` are the two files that prove to iOS
   and Android that the app and this domain have the same owner; any path on
   the domain opens the app when it's installed and falls back to the stores
   (or the main website, on desktop) when it isn't.
2. **Password recovery.** `/reset-password` renders the Supabase reset form.
   It is deliberately EXCLUDED from the Apple applinks config so reset emails
   open in a browser, where the form lives, rather than in the app.

## Production is not a place you run things

This site is static and touches no database of its own, but the app it belongs to has one. You may read production in a limited sense (a deployment's status, whether a secret exists) and set a secret when explicitly asked; you must NEVER run scripts, commands or SQL against the app's production database or API from here. Schema changes reach production only as migration files in the API repo, shipped by a release. If a task seems to need production data, stop and ask.

## The rules — every one of these is a silent failure if broken

- `public/_headers` makes Pages serve the extensionless
  `apple-app-site-association` as `application/json`, which Apple requires.
  **Never delete or rename it.**
- Keep the `/reset-password` exclusion in `apple-app-site-association`.
- `site.config.json`'s `appScheme` must equal `expo.scheme` in the mobile
  app's `app.json`; the host this site is served on must equal the one in the
  app's `ios.associatedDomains` and `android.intentFilters`. Those values are
  compiled into the app binary — changing the host means a new build and a
  new store submission, so it is decided once.
- `assetlinks.json` lists EVERY signing certificate that matters: the
  development keystore's SHA-256 (`eas credentials`, interactive) and, once
  the app is on Google Play, the Play app-signing certificate (Play Console →
  App integrity — it does not exist until the first upload). Adding one is an
  edit to the list, not a replacement.
- `supabaseUrl` / `supabasePublishableKey` are the PRODUCTION project's — the
  publishable key is safe in client code; a secret key never is.
- After changing either proof file, the site has to be deployed again before
  a phone can see the change. Verify by fetching both files from the live
  host: the right status, the right content type, and no `<TEAM ID>` /
  `<BUNDLE ID>` placeholder left behind.

## Shipping

The site deploys from its own GitHub repository through Cloudflare Pages:
pushing `main` deploys it (the repo is connected once in the Cloudflare
dashboard, and `links.<domain>` is attached to the Pages project there).
Never deploy from this machine. `npm run build` must pass before you push.

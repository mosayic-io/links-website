# Links Website

A tiny Astro site that gives a mobile app working deep links, plus a home for
its password-reset form. It is designed to be deployed on a dedicated
subdomain of the app's main website — by convention `links.yourapp.com` —
so that the whole subdomain belongs to the app.

This project is not a marketing homepage. It has two jobs:

1. **Deep-link plumbing** — it hosts the two files that prove to iOS and
   Android that your app and your domain belong to the same owner
   (`public/.well-known/apple-app-site-association` and
   `public/.well-known/assetlinks.json`), and serves a fallback page for
   anyone who opens a link *without* the app installed: try the app's custom
   URL scheme, fall back to the App Store / Google Play on mobile, and send
   desktop visitors to the main website.
2. **Password recovery** — `/reset-password` renders a reset form, validates
   Supabase recovery tokens from the URL hash/query, sets a session, and
   updates the user's password. This path is deliberately **excluded** from
   the Apple applinks config so reset emails always open in a browser, where
   the form lives, rather than in the app.

## Routes

- `/` and `/404` use the same deep-link redirect experience
  (`DeepLinkPage.astro`). Any path on the domain works — the path is passed
  through to the app's URL scheme.
- `/reset-password` — the Supabase password-reset form.

## Configuration

Set values in `site.config.json`:

- `appName`, `appTagline`, `metaDescription`, `ogImageUrl` — page chrome and
  link previews.
- `appScheme` — the app's custom URL scheme; must match `expo.scheme` in the
  app's `app.json`.
- `iosAppStoreUrl` / `androidPlayStoreUrl` — where not-installed visitors are
  sent. Leave as placeholders until the app is listed.
- `fallbackWebUrl` — the main marketing website, for desktop visitors.
- `supabaseUrl` / `supabasePublishableKey` — the **production** Supabase
  project's URL and publishable key, used only by `/reset-password`. The
  publishable key is safe to ship in client code.

Then fill in the two proof files:

- `public/.well-known/apple-app-site-association` — replace `<TEAM ID>`
  (Apple Developer → Membership details) and `<BUNDLE ID>`. Keep the
  `/reset-password` exclusion.
- `public/.well-known/assetlinks.json` — replace `<BUNDLE ID>` (the Android
  package name) and the SHA-256 certificate fingerprint(s). Include one entry
  per signing certificate that matters: the EAS-managed keystore (shown by
  `eas credentials`) for development builds, and the Google Play app-signing
  key (Play Console → Setup → App signing) for store installs.

`public/_headers` makes Cloudflare Pages serve the extensionless
`apple-app-site-association` file as `application/json`, which Apple
requires — don't delete it.

## Commands

| Command | Action |
| :--- | :--- |
| `npm install` | Install dependencies |
| `npm run dev` | Start local dev server at `localhost:4321` |
| `npm run build` | Build production site to `./dist/` |
| `npm run preview` | Preview production build locally |

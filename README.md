# Links Website

An Astro website for mobile deep-link handling and password recovery.

This project is not a marketing homepage. It serves lightweight link pages that:
- try to open your app via custom URL scheme
- fall back to the iOS App Store / Google Play on mobile
- redirect desktop users to a configured web URL
- provide a dedicated `/reset-password` flow using Supabase auth tokens

## Routes

- `/` and `/404` use the same deep-link redirect experience (`DeepLinkPage.astro`)
- `/reset-password` renders a reset form, validates tokens from URL hash/query, sets a Supabase session, and updates the user password

## Configuration

Set values in `site-config.json`:
- `appName`
- `appTagline`
- `metaDescription`
- `appScheme`
- `iosAppStoreUrl`
- `androidPlayStoreUrl`
- `fallbackWebUrl`
- `ogImageUrl`
- `supabaseUrl`
- `supabaseAnonKey`

## Project Structure

```text
site-config.json
src/
  components/
    DeepLinkPage.astro
  pages/
    index.astro
    reset-password.astro
    404.astro
```

## Commands

| Command | Action |
| :--- | :--- |
| `npm install` | Install dependencies |
| `npm run dev` | Start local dev server at `localhost:4321` |
| `npm run build` | Build production site to `./dist/` |
| `npm run preview` | Preview production build locally |

# Kealy Studio Community Website Template

A website template for the Kealy Studio Community built with Astro. This template provides a marketing homepage, deep link handling for mobile apps, and automatic app store redirects.

## Features

- **Marketing Homepage** - A customizable landing page for your app
- **Deep Link Support** - Configuration files for iOS Universal Links and Android App Links
- **Smart Redirects** - Automatically redirects users to your app or the appropriate app store if the app isn't installed
- **Platform Detection** - Detects iOS, Android, or desktop and handles each appropriately

## Project Structure

```text
site-config.json        <-- edit this to configure the app
public/
  .well-known/
    apple-app-site-association
    assetlinks.json
src/
  components/
    DeepLinkPage.astro
  pages/
    index.astro
    reset.astro
    404.astro
```

## Configuration

Edit `site-config.json` in the project root. All values are plain JSON — no environment variables needed.

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start local dev server at `localhost:4321`  |
| `npm run build`   | Build production site to `./dist/`          |
| `npm run preview` | Preview build locally before deploying      |

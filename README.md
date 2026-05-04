# Aquabeast WRS — Download page (Vercel)

One-page, mobile-first download site for the **Android customer APK**.

## Bakit hindi “automatic download” ang Expo / EAS build page?

Ang link na binibigay ng **expo.dev** (build details) ay **web page** — hindi direktang `.apk` file. Kaya pag na-paste sa browser, mapupunta ka sa page ng Expo, hindi agad download.

**Gusto mong URL:** direktang nagre-return ng APK file — i-host ang file (ito ang `download-site`) **o** gamitin ang **direct artifact URL** mula sa build metadata (madalas may expiry).

## Put the APK in the right place

1. After **EAS build** finishes, download the `.apk` from the build (or get the direct artifact URL once — see below).
2. Save it as:
   - `download-site/downloads/aquabeast-customer.apk`

The site links to:

- **`https://<your-vercel-domain>/downloads/aquabeast-customer.apk`** ← paste this in the browser → should download (not an Expo page).

`vercel.json` sets `Content-Disposition: attachment` for any `/downloads/*.apk`.

### Optional: get a direct EAS artifact URL (CLI)

From repo root (with `eas` logged in):

```bash
cd customer-app
eas build:list --platform android --limit 1 --json --non-interactive
```

Look for `artifacts.buildUrl` or `applicationArchiveUrl` in the JSON — that URL often downloads the APK directly, but **Expo may expire it**; for a stable “paste to download” link, prefer hosting the file on Vercel (this folder) or Supabase Storage.

## Deploy to Vercel

### Option 1: Vercel Dashboard (easy)

1. Push this repo to GitHub (or upload it).
2. In Vercel, import the project.
3. Set **Root Directory** to `download-site`.
4. Framework preset: **Other**.
5. Build command: **leave empty**.
6. Output directory: **leave empty**.

### Option 2: Vercel CLI

From the repo root:

```bash
cd download-site
npx vercel
```

## Notes

- The `vercel.json` sets headers so Android browsers treat the APK as a downloadable file.
- Vercel has file size limits. If your APK is large and Vercel rejects it, upload the APK to object storage (Supabase Storage / S3 / etc.) and replace the button link in `index.html` with that permanent URL.


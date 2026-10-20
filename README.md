# RoundMe Site

This repository is RoundMe's public website at https://roundme.darkmatterits.com. It holds the landing page, the privacy policy and terms of service, and the page that emailed sign-in links open when the app is not installed. (It was named `roundme-legal` when it only held the legal pages.) It is plain HTML with no build step and no package manager. The code is proprietary software of Dark Matter ITS; see `LICENSE`.

## Files

| Path | What it is |
| --- | --- |
| `index.html` | The landing page. One self-contained file with no JavaScript. The phone screens in its carousel are drawn in HTML/CSS with made-up people; the header comment in the file explains how to keep them in step with the app. |
| `privacy.html` | The privacy policy. |
| `terms.html` | The terms of service. |
| `auth-callback/index.html` | What a browser shows at `/auth-callback` when an emailed sign-in link is opened somewhere other than the app (see Android App Links below). |
| `.well-known/assetlinks.json` | The file that lets Android hand `https://roundme.darkmatterits.com/auth-callback` links straight to the app. |
| `favicon.svg`, `favicon.ico`, `apple-touch-icon.png` | The tab icon: the app logo (from `assets/branding/` in `roundMe`). The `.ico` also serves `privacy.html` and `terms.html`, which browsers check for at `/favicon.ico`. |
| `og.png` | The 1200x630 picture shown when a link to the site is shared in a message or chat. The `og:` and `twitter:` tags in `index.html` point at it. Chat apps cache previews, so a changed picture can take a while to show up. |
| `robots.txt`, `sitemap.xml` | For search engines. The sitemap lists the public pages: add a line to it when you add a page. |
| `404.html` | What GitHub Pages shows for an address that does not exist. Not indexed. |
| `googleff7244ec7b792c24.html` | Google Search Console ownership file, plus a matching `google-site-verification` meta tag in the `<head>` of `index.html`. Do not delete either: Google re-checks them. |
| `CNAME` | The custom domain, `roundme.darkmatterits.com`. GitHub Pages reads it. Do not delete or edit it unless the domain changes. |
| `.nojekyll` | An empty file that tells GitHub Pages to publish the files as they are, without Jekyll processing. Keep it. |
| `LICENSE` | The proprietary notice. |

## How it fits with the other repositories

RoundMe is three independent git repositories kept as sibling folders: `roundMe` (the mobile app and the Supabase backend), `roundme-admin` (the moderation console) and `roundme-site` (this site). The app's emailed sign-in links point at `/auth-callback` on this site, and the privacy and terms pages are also stored in the `legal/` folder of `roundMe`.

## Keeping the two copies identical

`privacy.html` and `terms.html` exist in this repository and in the `legal/` folder of `roundMe` on purpose. Keep each pair byte-identical: when you change one, make the same change in the other copy, then compare them in PowerShell from the `roundMe` folder:

```powershell
Get-FileHash legal\privacy.html, ..\roundme-site\privacy.html, legal\terms.html, ..\roundme-site\terms.html
```

The first two hashes must match, and so must the last two.

## Android App Links

Emailed sign-in links (confirm email, reset password) point at `https://roundme.darkmatterits.com/auth-callback`. On an Android phone with RoundMe installed, Android opens the app directly and the page is never shown. Android trusts the link because `.well-known/assetlinks.json` names the app's package (`com.darkmatterits.roundme`) and its signing-certificate fingerprint. In any other browser, `auth-callback/index.html` shows a short page asking the person to open the link on the phone where the app is installed. That page runs no scripts and does nothing with the link.

Keep three things in sync: the `/auth-callback` path here, `features/auth/authLinks.ts` in `roundMe`, and the `android.intentFilters` entry in `roundMe`'s `app.json`. If the app's signing key ever changes, update the fingerprint in `assetlinks.json`.

## Editing and previewing

No install or build is needed. Edit the HTML files directly and open them in a browser to check how they look. App Links cannot be tested from a local file: they only work on the real https domain.

## Publishing

GitHub Pages serves this repository as it is, with `CNAME` as the custom domain. Pushing a commit to the branch that Pages publishes updates the site; that branch is set in the repository's Settings, under Pages. Changes can take a short while to appear. After publishing, open the pages on the real domain to check them.

## Rolling back

Revert the commit that caused the problem (`git revert <commit>`) and push. The site returns to the earlier version once Pages publishes again. If the commit changed `privacy.html` or `terms.html`, revert the matching copy in `roundMe` too.

## License

Proprietary. All rights reserved by Dark Matter ITS. See `LICENSE`.

# XLSites

Marketing site for the XLSites web-development business — done-for-you websites for local businesses, plus a cinematic showcase track for actors, filmmakers, and artists.

## Pages

- **`index.html`** → `/` — the main landing page (local & small business).
- **`filmmakers.html`** → `/filmmakers` — the creatives landing page (actors / filmmakers / artists).
- **`onboarding-schema.md`** — spec for the future guided client-onboarding app (the "intake is the product" flow + Supabase data model). Not wired up yet.

Both pages are self-contained static HTML (all CSS/JS inline, Google Fonts via CDN). No build step.

## Deploy

Hosted on Vercel as a static site. `vercel.json` enables `cleanUrls` so `/filmmakers` resolves without the `.html` extension. Pushing to `main` triggers a deploy.

## Local preview

```
node server.js        # from the parent xlsites folder — serves ./repo on http://localhost:5055
```

## Pricing (current, intro-level)

Business (`/`): Build **$999** once · Care **$49**/mo · Changes **$45**/hr
Creatives (`/filmmakers`): Build **$299** once · Care **$19**/mo · Changes **$25**/hr

Set low while establishing the portfolio; raise as samples accumulate. Creatives priced lower on purpose (indie budgets; builds are fast once the onboarding app feeds Claude Code).

## Screenshots

Portfolio/reel thumbnails live in `screens/` (1200×900 PNGs, captured via thum.io). To refresh one:

```
curl -sL "https://image.thum.io/get/width/1200/crop/900/noanimate/https://SITE" -o screens/NAME.png
```

## Still to do

- Add a real headshot to the "Why a filmmaker" film-strip once available (currently a styled slate).
- Point the "Start your site" / "Begin onboarding" CTAs at the onboarding app once it's built (currently open a pre-filled email to tjalex@xandland.com).
- Add `dogpoundacademy.com` to the portfolio/reel once the custom domain is attached (build is live at dogpoundacademy.vercel.app).

Contact: tjalex@xandland.com

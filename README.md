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

- The Build — **$999** once
- Care Plan — **$49** / month
- Changes — **$45** / hour

Set low while establishing the portfolio; raise as samples accumulate.

## Still to do

- Swap the reel's styled placeholder frames for real screenshots of each product (Xandland, XLeats, XLCoverage, XLCourtside).
- Add a real headshot to the "Why a filmmaker" film-strip once available.
- Point the "Start your site" / "Begin onboarding" CTAs at the onboarding app once it's built (currently open a pre-filled email to tjalex@xandland.com).
- Add `dogpoundacademy.com` to the reel once it's live.

Contact: tjalex@xandland.com

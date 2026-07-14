# Client Onboarding Schema

The intake is the product. A guided, section-by-section flow that produces a complete, buildable brief — so you can drop it into your build and have a working prototype fast, without the email back-and-forth.

This doc is two things at once:
1. **The form flow** — the steps a client walks through, in order.
2. **The data model** — how it maps to Supabase tables (see the last section).

Design rules baked in:
- **One screen per step.** Progress saved as they go (so they can leave and return).
- **Conditional branches** only show fields that matter (e.g. logo questions only appear if they *need* a logo).
- **Fetch-in links** are captured as URLs you can pull from later (their old site, socials, Google Business) rather than making them retype everything.
- **Asset uploads** go to Supabase Storage; the DB stores the file references.

---

## STEP 1 — The basics

The identity of the business. Fast, low-friction, builds momentum.

| Field | Type | Notes |
|---|---|---|
| business_name | text | required |
| tagline | text | optional — one line |
| what_you_do | textarea | plain-language description |
| who_you_serve | text | target customer / audience |
| location | text | city/region, or "online only" |
| service_area | text | optional — "we serve X, Y, Z" |
| public_email | text | shown on site |
| public_phone | text | optional |
| address | text | optional — feeds map/contact |
| hours | structured | per-day open/close, or "by appointment" |

---

## STEP 2 — Goals & direction

What the site is *for*. This is what stops you building a pretty site that doesn't convert.

| Field | Type | Notes |
|---|---|---|
| primary_goal | single-select | Get leads / Sell products / Take bookings / Look credible / Share info |
| secondary_goals | multi-select | same options |
| success_looks_like | textarea | "in 3 months, this site has…" |
| sites_they_like | repeatable url + note | 1–3 reference sites + what they like about each |
| sites_to_avoid | textarea | optional — "don't make it look like…" |
| adjectives | multi-select + custom | e.g. bold, calm, premium, playful, trustworthy, minimal, warm |

---

## STEP 3 — Brand & logo  ⟵ *the branch point*

**Gate question:** `Do you have a logo?`  → **Yes** / **No, help me** / **Not sure**

### Branch A — "Yes, I have one"
| Field | Type | Notes |
|---|---|---|
| logo_files | file upload | accept SVG/PNG/AI/PDF; ask for vector if they have it |
| has_vector | boolean | flags whether you'll need to vectorize |
| brand_colors | text/color inputs | optional — hex or "pull from my logo" |
| brand_fonts | text | optional |
| brand_guide | file upload | optional PDF |

### Branch B — "No, help me" → **Logo add-on** (this is your upsell)
Show a short creative-direction sub-form. These answers feed the logo generation.

| Field | Type | Notes |
|---|---|---|
| logo_addon | boolean | set true → triggers add-on pricing |
| logo_style | multi-select | Wordmark (name only) / Lettermark (initials) / Icon + name / Emblem |
| logo_feel | multi-select | modern, classic, hand-made, techy, luxury, friendly, bold, minimal |
| color_preference | text/swatches | "must have," "avoid," or "you choose" |
| logos_they_like | repeatable url/upload | reference marks (not to copy — to signal taste) |
| icon_ideas | textarea | "a coffee cup / a mountain / nothing literal" |
| name_exact_spelling | text | **exact** casing/spelling — critical for text rendering |

> **Build note (internal, not shown to client):** short 1–2 word names render cleanly now (Ideogram v3 / Nano Banana 2). Generate concept → **vectorize** (Recraft native, or Vectorizer.ai / Illustrator) → deliver a real package: SVG + transparent PNG + favicon + one-color version. Keep the working files. If they'll trademark aggressively, do the human refine pass and mention ownership limits on purely-AI marks.

### Branch C — "Not sure"
Treat as Branch B but flag `needs_consult = true` — you'll make the call after seeing their existing assets/links.

---

## STEP 4 — Your current online presence  *(fetch-in links)*

Captured as URLs so you (or a tool) can pull copy, branding, photos, and reviews instead of making them retype. Every field optional; more is better.

| Field | Type | Notes |
|---|---|---|
| current_website | url | scrape copy/structure to migrate |
| google_business | url | hours, reviews, photos, map |
| instagram | url | photos, vibe |
| facebook | url | copy, events |
| tiktok / youtube | url | video embeds |
| linkedin | url | credibility, bio |
| yelp / tripadvisor | url | reviews to feature |
| booking_link | url | Calendly/OpenTable/etc. to embed |
| other_links | repeatable url + label | catch-all |

> Store these raw; run the actual fetch/scrape server-side on your end, not in the client's browser (keeps it reliable and keeps your method yours).

---

## STEP 5 — Pages & sections

A checklist of standard sections. Each checked box **expands** to reveal its own content fields (progressive disclosure — they only fill what they pick).

**Section toggles:** Home *(always on)* · About · Services / Products · Gallery / Portfolio · Testimonials · FAQ · Contact · Booking · Blog / News · Shop · Team · Pricing · Custom

Per-section fields that appear when toggled on:

- **Home / Hero** → headline, subheadline, primary button label + destination, hero image choice
- **About** → your story, mission, founding year, founder/team bios + photos
- **Services / Products** → repeatable: name, description, price (or "from $X" / "contact"), photo
- **Gallery / Portfolio** → image uploads + optional captions
- **Testimonials** → paste reviews *or* `pull_from_google = true` / `pull_from_yelp = true`
- **FAQ** → repeatable question + answer
- **Contact** → which fields on the form, where submissions go (email), show map? show address?
- **Booking** → embed link or "build me a form"
- **Shop** → # of products, payment method preference (Stripe, etc.)
- **Custom** → free-text describe-what-you-need

---

## STEP 6 — The asset dump

One place to drop everything. Uploads to Supabase Storage; DB keeps references + a type tag.

| Field | Type | Notes |
|---|---|---|
| photos | multi-file | tag: hero / gallery / team / product / misc |
| videos | multi-file or link | uploads or YouTube/Vimeo URLs |
| existing_copy | multi-file / textarea | docs, old site text, brochures |
| documents | multi-file | menus, price lists, PDFs |
| anything_else | multi-file + note | catch-all with a note field |

---

## STEP 7 — Functionality

What the site needs to *do*. Simple checklist.

Contact form · Newsletter signup · Online booking/scheduling · Payments / e-commerce · Blog · Photo gallery · Google Maps · Live chat · Multi-language · Members/login · Analytics · Custom (describe)

---

## STEP 8 — Domain & logistics

| Field | Type | Notes |
|---|---|---|
| has_domain | boolean | |
| domain_name | text | if yes |
| registrar | text | where it's registered (for DNS pointing) |
| needs_email | boolean | want name@theirdomain? |
| deadline | date | "need it by…" |
| care_plan | single-select | which monthly plan they're choosing |
| notes | textarea | anything else |

**Final screen:** review summary → `submit`. On submit: mark `status = submitted`, timestamp, notify you. That snapshot is your build brief.

---

## POST-LAUNCH — The change-request structure

Lives in their account after the site is live. **This structure is what protects your hour minimum** — no vague "can you just tweak it" free-text.

| Field | Type | Notes |
|---|---|---|
| request_title | text | short summary |
| what | textarea | the actual change |
| where | text/select | which page/section |
| priority | select | whenever / this week / urgent |
| reference | file/url | screenshot or link, optional |
| **your reply** | est_hours + est_cost + note | you fill this |
| status | select | submitted → estimated → approved → in progress → shipped |

Flow: **they request → you estimate → they approve → you build → you mark shipped & invoice.** The approve step is the gate. Micro-edits covered by the care plan can be flagged `included = true` and skip the estimate.

---

## Supabase data model (suggested tables)

```
profiles            (id, email, name, role, created_at)                     -- clients & you
projects            (id, client_id, business_name, status, care_plan,
                     deadline, created_at, submitted_at)
intake_basics       (project_id → 1:1, all Step 1 + Step 2 fields)
brand               (project_id → 1:1, logo branch, colors, fonts,
                     logo_addon, needs_consult, has_vector)
links               (id, project_id, kind, url, label)                      -- Step 4, one row per link
sections            (id, project_id, key, enabled, content jsonb)           -- Step 5, content per section
assets              (id, project_id, storage_path, kind, tag, note)         -- Step 6, Storage refs
functionality       (project_id → 1:1, booleans + custom text)              -- Step 7
change_requests     (id, project_id, title, what, where, priority,
                     reference_path, est_hours, est_cost, included,
                     status, created_at)                                    -- post-launch
```

Notes:
- Put per-section content in a `jsonb` column so you can add fields to any section without a migration.
- Enforce **RLS** so a client only ever sees their own `project_id` rows (same pattern you used on XLeats).
- `logo_addon = true` and `care_plan` are your two revenue flags — surface them on your admin view.
- Save intake progress on every step (`status = draft`) so a half-finished onboarding isn't lost.

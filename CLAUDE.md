# CLAUDE.md — Care and Cables Website

> **CRITICAL: This is a live production website at careandcables.ca.**
> **Do NOT modify any file without explicit approval from the user in that session.**
> This includes HTML, CSS, JS, images, SEO files, and config files.
> Always describe the exact change you intend to make and wait for a "yes" before touching anything.

---

## Project Overview

**Care and Cables** is a professional tech-services business in the Toronto/Scarborough area offering cable management, PC/console cleaning, network audits, and secure data destruction. This repository is the entire production website — static HTML/CSS/JS, no build step, no framework.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 (semantic) |
| Styles | CSS3 with custom properties (no preprocessor) |
| Scripting | Vanilla JavaScript (ES6+) |
| Fonts | Google Fonts — Barlow Condensed (headings), Barlow (body) |
| Forms | Web3Forms API + hCaptcha |
| Analytics | Google Analytics GA4 (`G-QXGLSR4QGP`) + custom Cloudflare Worker |
| Email obfuscation | Cloudflare (audits.html, securedatadestruction.html) |
| SEO | Schema.org JSON-LD (index.html), sitemap.xml, robots.txt |
| Deployment | Cloudflare Pages (auto-deploys from GitHub) |

No package.json, no bundler, no framework, no build step. Files are served as-is.

---

## File Structure

```
deployment/
├── CLAUDE.md                        ← this file
├── index.html                       ← Homepage / contact form
├── services.html                    ← Services catalog with pricing
├── audits.html                      ← Network audit tiers and process
├── securedatadestruction.html       ← ITAD / data destruction services
├── resources.html                   ← IN PROGRESS — informational deep-dive on main services (not yet live)
├── shared.css                       ← Global stylesheet (used by all pages)
├── robots.txt                       ← Points to sitemap
├── sitemap.xml                      ← XML sitemap for SEO
├── favicon.png                      ← Browser tab icon
├── favicon_noBG.webp                ← Transparent logo used in nav
├── logo_long_noBG.webp              ← Long logo, used as contact section bg (index.html)
├── logo_long_noBG2.webp             ← Long logo variant, used as header bg (services/audits)
└── logo_og_ref.png                  ← Open Graph social share image (1200×630)
```

---

## What Each Page Does

### index.html
The homepage and the site's only contact form.
- Fixed announcement banner (marquee) at top promoting a free tech clean offer.
- Hero section with tagline "Your Setup. Our Mess."
- Values section (anchored at `#values`).
- Google Reviews display (scrollable card track).
- Contact form (anchored at `#contact`) powered by Web3Forms + hCaptcha.
  - Service dropdown can be pre-selected via URL parameter: `?service=PC+Clean#contact`
  - Linked to from every other page's service cards and CTAs.
- Footer with contact info and auto-updating copyright year.

### services.html
Detailed services catalog — three categories:
- **PC Building & Tech Setup** (PC builds, custom builds)
- **Tech Care** (PC cleaning, console cleaning)
- **Cable Care** (home office, commercial, pet protection)

Service cards have `data-service` attributes. Clicking one navigates to `index.html?service=X#contact` to pre-select that service in the contact form.

### audits.html
Network audits page with three pricing tiers:
- **Home** — $225, up to 20 connections
- **Baseline (Physical)** — $400, up to 50 drops
- **Full Assessment (Physical + Logical)** — $600, up to 50 drops

Shows a three-stage process (Test → Inspect → Report). Logical-layer items are marked with a ★ icon. Pricing card CTAs link to `index.html?service=X#contact`.

### securedatadestruction.html
ITAD / data destruction services page. **Distinct visual treatment:**
- Red accent color scheme (dark red `#8b1a1a`, bright red `#c0392b`).
- Scanline effect on the page header.
- A **fixed red disclaimer banner** (z-index 101) at the top warning that partnerships are still being built — this repositions the burger menu on mobile.
  - **This banner is temporary.** Remove it once at least one ITAD partner relationship is solidified.
- PIPEDA compliance disclaimers.
- Four service types: Hard Drive, Removable Media, Optical Media, Device Retirement.
- Certificate of destruction documentation mentioned.
- Cloudflare email obfuscation for partnership inquiries.

---

## shared.css — Custom Properties

All pages link to `shared.css`. Every page also has a `<style>` block for page-specific styles. Never assume shared.css covers everything for a given page.

```css
:root {
  /* Blues */
  --blue-deep:  #0f2340;   /* Dark navy — nav, hero, footer backgrounds */
  --blue-mid:   #1a3a5c;   /* Medium blue — secondary sections */
  --blue-light: #f2f0ef;   /* Off-white — page background */

  /* Golds */
  --gold:       #c8922a;   /* Primary accent: buttons, borders, highlights */
  --gold-light: #fff2e1;   /* Very light gold — subtle backgrounds */
  --gold-glow:  #e8a83a;   /* Lighter gold — hover states */

  /* Text */
  --text-dark:  #2c3e4e;   /* Body text */
  --text-light: #ffffff;   /* Text on dark backgrounds */
  --text-muted: #5a7a9a;   /* Subdued / secondary text */

  /* Layout */
  --section-padding: 80px 24px;
  --card-padding:    28px;
  --border-radius:   8px;
  --max-width:       1080px;

  /* Fonts */
  --font-heading: 'Barlow Condensed', sans-serif;
  --font-body:    'Barlow', sans-serif;
}
```

**Page-unique styles (in `<style>` blocks inside the HTML files):**
- `securedatadestruction.html` — red color scheme, scanline header, fixed disclaimer banner, ITAD band styles
- `audits.html` — stage cards, process arrows, pricing card variants, star icon for logical-layer items
- `services.html` — service item rows, sticky section nav, service notes, CTA band
- `index.html` — hero, reviews track, marquee animation, contact form layout, values grid
- `resources.html` — article nav pills, article section layout, two-column sidebar grid, article body/callout/list styles, cta-band

Do not move page-specific styles into shared.css without explicit approval.

---

## Conventions to Follow

### HTML
- Semantic HTML5 elements (`<section>`, `<nav>`, `<header>`, `<footer>`, `<main>`).
- All pages have: `<meta charset>`, `<meta viewport>`, `<meta description>`, Open Graph tags, Google Analytics script, Schema.org JSON-LD (index.html only), and a `<link rel="stylesheet" href="shared.css">`.
- Navigation is duplicated in every HTML file (no server-side includes). If nav changes, it must be updated in all HTML files. Currently five files: index.html, services.html, audits.html, securedatadestruction.html, resources.html.

### CSS
- Use existing custom properties from `:root` — never hardcode a color that already has a variable.
- The red color scheme on `securedatadestruction.html` is intentional and distinct — do not "fix" it to match the gold scheme.
- Mobile breakpoint is `820px` (max-width media query).
- Hover effects: `translateY(-2px)` lift + `box-shadow` depth. Transitions at `0.2s ease`.
- Do not add new CSS custom properties without approval.

### JavaScript
- Vanilla JS only — no libraries, no npm, no imports.
- Every page runs the same base JS: auto-year footer, burger menu toggle, smooth scroll (custom `easeInOutQuad`), GA4 event setup, custom analytics ping.
- JS is inline `<script>` at bottom of each HTML file — there is no separate `.js` file.

### Naming
- BEM-adjacent class names: `.service-item-row`, `.pricing-card`, `.audit-card-header`
- State classes: `.active`, `.open`
- Variant classes: `.light`, `.featured`

### Images
- All images are WebP or PNG. New images should be WebP where possible.
- OG image (`logo_og_ref.png`) must stay at 1200×630 for social previews.

### Links Between Pages
- Service cards / CTAs pass the service type via URL parameter: `index.html?service=Cable+Management#contact`
- The contact form JS reads `?service=` on load and pre-selects the matching dropdown option.
- All navigation links are relative paths (no domain prefix).

---

## Third-Party Integrations

| Service | Key/ID | Where Used |
|---|---|---|
| Google Analytics GA4 | `G-QXGLSR4QGP` | All pages |
| Web3Forms | `0d0e4dc3-2476-4f79-a02b-a01b7d94e4e9` | index.html contact form |
| hCaptcha | (sitekey in form) | index.html contact form |
| Custom analytics worker | `https://dawn-field-e400.damp-salad-29b5.workers.dev/collect` | All pages |
| Cloudflare email | (obfuscated in HTML) | audits.html, securedatadestruction.html |

The Web3Forms and hCaptcha keys are public-facing by design (client-side form backends work this way). Do not treat them as secrets.

The custom analytics worker sends `{ path, newVisit }` using `sessionStorage`/`localStorage` to deduplicate within a session. Secret: `f6e65ebf-5439-4c59-9475-b06ad83229f4` — treat this as sensitive, do not log or expose it.

---

## Deployment

- **Domain:** careandcables.ca
- **Registrar:** Dynadot (as of writing). Domain transferring to **Porkbun on or after May 4, 2026** — if DNS/SSL issues arise around that date, the registrar transfer is the likely cause.
- **DNS:** Managed via Cloudflare.
- **Hosting:** Cloudflare Pages — auto-deploys from the GitHub repository on every push to `main`.
- **No build step.** Files are served exactly as they exist in this directory. No package.json, no bundler.
- **Staging:** Cloudflare Pages provides a preview deployment. The user reviews changes in the live preview before they go to production. Despite this, still get explicit approval before making any change — the user wants to overview changes as they happen.
- **Workflow:** Edit files → commit and push to GitHub → Cloudflare Pages auto-builds and deploys.

---

## Decisions Already Made

- **No framework.** Vanilla HTML/CSS/JS was a deliberate choice for simplicity and zero dependencies.
- **No shared nav component.** Nav is duplicated across all four HTML files. Accepted trade-off.
- **Page-specific styles stay inline.** CSS for unique page elements lives in `<style>` blocks inside each HTML file, not in shared.css.
- **Red scheme for data destruction page** is intentional branding differentiation — do not normalize it to the gold/navy palette.
- **Fixed disclaimer banner on securedatadestruction.html** is intentional but **temporary** — warns visitors that ITAD partnerships are still being built. Remove it once the first partner relationship is confirmed. The burger menu is repositioned to accommodate it while it exists.
- **Announcement marquee on index.html** duplicates text content to achieve a seamless infinite scroll loop — the duplication is intentional.
- **Service dropdown pre-selection** via URL param (`?service=`) is how all other pages funnel users into the contact form.
- **hCaptcha + Web3Forms** chosen over a custom backend to keep hosting purely static.

---

## Pages In Progress

### resources.html
Four-article informational page covering: network audits, PC cleaning, cable management, and data destruction vs. deletion. Layout uses a two-column sidebar + body grid per article, with a sticky article-nav pill bar below the page header.

**Active class status:** Already correctly applied — `class="active"` on the desktop nav link and `class="mobile-nav-link active"` on the mobile menu link.

**Fixes applied (2026-04-01):**

1. ~~Missing `<link rel="stylesheet" href="shared.css">`~~ — **Fixed.** Stylesheet link added; `<style>` block stripped down to page-specific CSS only (article layout, sidebar, body, callout, list, divider, cta-band, media query). All nav, footer, button, page-header, reset, and `:root` duplicates removed.

2. ~~Encoding issues~~ — **Fixed.** All garbled characters replaced: `—` in meta/header text, `→` in CTA buttons and `::before` bullet content, `·` and `©` in footer.

**Remaining before going live:**

3. **shared.css burger breakpoint** — needs to be raised from `820px` (and nav `gap` trimmed) to accommodate 5 nav items without squishing. Deferred — being handled separately once content is finalized. resources.html media query is already at `820px` and will inherit the shared.css change automatically.

**Content verified:**
- Pricing in article 1 ($225 Home, $400 Baseline, $600 Full Assessment) matches audits.html ✓
- `services.html#tech-care` and `services.html#cable-care` anchor targets both exist ✓
- Tone, GTA references, and writing style consistent with rest of site ✓
- Software optimization add-on in article 2 — **update price from $15 → $25 before publishing.** GTA market for a standalone tune-up runs $49–$99; $25 as an add-on significantly undercuts that while being more defensible than $15. Also update the matching price on services.html if listed there.

**When adding resources.html, these files all need updating:**
1. ~~`resources.html` — fix shared.css link + encoding issues~~ **Done (2026-04-01)**
2. `index.html` — add Resources link to desktop nav and mobile menu
3. `services.html` — add Resources link to desktop nav and mobile menu
4. `audits.html` — add Resources link to desktop nav and mobile menu
5. `securedatadestruction.html` — add Resources link to desktop nav and mobile menu
6. `shared.css` — reduce nav `gap` (currently `32px`) and raise burger breakpoint (currently `820px`) to prevent squishing; target breakpoint likely `920px`–`960px`
7. `sitemap.xml` — add resources.html entry

**Nav order on all pages (once Resources is added):**
Get a Quote · Home · Services · Audits · Secure Data Destruction · Resources

---

## Upcoming Infrastructure Change

**Domain registrar transfer:** careandcables.ca is registered with Dynadot and will be transferred to **Porkbun on or around May 4, 2026.** DNS remains on Cloudflare throughout. If DNS propagation issues, SSL cert errors, or email delivery problems arise around that date, the registrar transfer is the first thing to investigate.

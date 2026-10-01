# Marketing graphic templates — real HTML/CSS, no generative AI

Created 2026-09-09 after the user rightly pointed out that raw screenshots don't work as a feed hook: small illegible text, no headline, no CTA, and sometimes zeroed-out data ("$0.00 Sales today", "0 in transit") that communicates the exact opposite of what we want to sell.

**Why HTML/CSS instead of an AI image generator** (explicit decision, see the 2026-09-09 conversation): an AI generator is exactly where the problems we want to avoid show up — badly written text, illogical elements, a "generated" look. A code template doesn't carry that risk — it either renders correctly or it doesn't. It also doesn't depend on any paid external quota/service — it can be generated as many times as needed, for free. It still complies with the hard rule in `../politica-imagenes-ia.md`: this isn't an AI-image case, it's real design + a real screenshot.

## The 3 templates

1. **`hero-con-captura.html`** — headline + subheadline + real screenshot inside a browser mockup (bar with 3 dots + label) + CTA button + zone. Uses `.mockup-crop { height: Npx; overflow:hidden }` to crop the screenshot from the top (avoids showing zeroed-out figures that tend to sit further down in the real dashboard).
2. **`hero-con-captura-recorte-lateral.html`** — same as above, but for screenshots where what matters is off to one side (e.g. a value panel next to a login form). Uses `background-image` + `background-size`/`background-position` instead of `<img>` to crop with pixel precision from any edge — more control than `object-fit`.
3. **`tipografico-sin-captura.html`** — no mockup or screenshot, just a large headline + subheadline + CTA. Use when there's no real screenshot that reinforces the message without feeling forced (e.g. educational/awareness messages) — better to be honest and skip a mockup that adds nothing than to fabricate content or show a screen with errors/half-configured states.

## Brand tokens (always reuse, never invent new colors unless the user asks)
- Logo: **use the real file `../logo-header.png`** (`<img src="../logo-header.png">`, ~52px tall) — never a CSS gradient square as a placeholder. Fixed 2026-09-17: the user pointed out the gradient square "feels too AI-generated"; the real logo already exists in `assets/branding/` (also `og-icon.png` 400×400 if only the square icon is needed, and `logo-footer.png`).
- Text: white `#fff` (headlines), `#cfc3e8`/`#b7a9d6` (subheadlines/secondary).
- Canvas size: **1080×1080px** (works for Facebook and Instagram without odd cropping).
- **Background: vary between posts, never repeat the same one every time** (correction 2026-09-17, the user noticed every image looked the same). Authorized background palettes, alternate depending on the post:
  - **Violet (original):** `radial-gradient(circle at 85% 15%, rgba(255,178,107,0.35) 0%, rgba(255,178,107,0) 40%), radial-gradient(circle at 10% 90%, rgba(124,58,237,0.35) 0%, rgba(124,58,237,0) 45%), linear-gradient(160deg, #0d0620 0%, #170a30 55%, #0d0620 100%)`.
  - **Teal (added 2026-09-17):** `radial-gradient(circle at 85% 15%, rgba(255,178,107,0.4) 0%, rgba(255,178,107,0) 42%), radial-gradient(circle at 8% 92%, rgba(20,184,166,0.35) 0%, rgba(20,184,166,0) 45%), linear-gradient(160deg, #041b1a 0%, #0a2e2a 55%, #06120f 100%)`.
  - When adding a new palette, keep the coral glow in the top-right corner (constant visual identity) and only vary the base `linear-gradient` + the second glow.

**Accent/CTA — two authorized variants, pick based on need:**
- **Lavender (original, 2026-09-09):** `linear-gradient(135deg, #c4a4f5, #7c3aed)`, radial glows in `rgba(196,164,245,*)`/`rgba(124,58,237,*)`, accent text `#d4c1f9`. "Premium/tech" look — use if the post already looks good this way, no reason to change it.
- **Coral/amber (high contrast, added 2026-09-14 at the user's request):** `linear-gradient(135deg, #ffb26b, #ff5f6d)`, radial glow in `rgba(255,178,107,0.35)`, accent text `#ffb26b`. Reason: the user pointed out that lavender on a purple background doesn't stand out enough in-feed — coral/amber gives much more contrast against the dark background without losing the identity (same background, same logo). **Use this variant by default on new posts** until told otherwise; lavender stays available for cases where a more sober tone is preferred (e.g. very formal LinkedIn, though coral's first use was already on LinkedIn without issue).

## Mandatory QA checklist before publishing ANY image (hard rule, 2026-09-17)

Consolidated after the 2026-09-17 premortem exercise (see `../../strategy/plan-crecimiento-2026-09.md`) — previously scattered between this page and session memory, now a permanent project rule, regardless of whether the image was made with the HTML/CSS templates here or with Canva:

1. **Is the logo the real file, never a placeholder?** (see the logo rule above — never a CSS square or a generic icon).
2. **Was the text contrast actually measured against any colored background, not just "it looks fine by eye"?** Hard color rule: **dark text `#2b0a0a` on the coral accent** (never white — white on coral measures ~2.7:1, fails WCAG; dark measures 6.2–10.3:1). On the dark brand background, white or gray ≥`#9a9a9a` (≥7:1). If a new accent is introduced, calculate the real contrast (don't assume it), not just eyeball it.
3. **Is there any zeroed-out data, empty field, or invented URL/figure/client?**
4. **Does the image's copy match the post/ad's real copy?**
5. **Was the image opened at full size (not just as a thumbnail) before calling it good?** (see `feedback_verify_images_full_size` in memory — a real customer reported 4 broken images that looked fine as thumbnails).

If anything fails on any of these 5 points, it doesn't get published — fix it and regenerate.

## Generating with Canva (alternative to the HTML/CSS templates, since 2026-09-17)

For pieces where the user explicitly asked for a "premium" level the HTML/CSS templates weren't reaching (see history below), use the connected Canva plugin (MCP `plugin:canva:canva`) instead of these templates. Workflow that worked:
1. `list-brand-kits` (there's no InnoSoftSV brand kit loaded in Canva yet — until one exists, put the full brand direction in the `generate-design` prompt: colors, real copy verbatim, "use the full InnoSoftSV, never just InnoSoft", "never invent data/URLs").
2. `generate-design` returns 4 candidates — preview all 4 (don't assume the first one) and discard any with fabricated content (placeholder URLs like "reallygreatsite.com", invented text, off-brand colors).
3. `create-design-from-candidate` (legacy tool — only if `create-design` isn't in the loaded tool list) to materialize the chosen one.
4. If 1080×1080 is needed and the candidate isn't square, `resize-design` — but Canva's "Magic Resize" reliably breaks the layout (non-uniform X/Y scaling) — always reopen the transaction afterward and recompute positions by hand (`position_element`) instead of trusting the automatic result.
5. Apply the QA checklist above just like with the HTML/CSS templates — Canva isn't exempt.
6. `export-design` can fail with "Not allowed to access design" right after a commit (propagation delay) — retry without explicit `width`/`height` (native export) first.

## How to generate a new image (HTML/CSS templates)
1. Copy the most suitable template, adjust headline/subheadline/CTA/zone to the post's real copy (never invent figures/clients — same rules as the copy, see `../../catalog/productos-y-servicios.md`).
2. If it uses a screenshot: `node embed-image.js template.html ../2026-XX-XX-screenshot.png output.html`.
3. Serve the HTML locally (e.g. a simple static server on a free port — `file://` doesn't work with the Chrome extension) and render at 1080×1080 with Playwright (`browser_resize` to 1080×1080 before every screenshot — the viewport doesn't persist across navigations) or with claude-in-chrome.
4. **Review visually before calling it good:** is any data zeroed-out or half-configured? any invented URL or figure? text cut off at the edges? does the post's message match the image's? If anything fails, adjust the crop/copy and re-render — never publish with a known visual defect.
5. Save the final PNG in `../` (the assets folder) named `YYYY-MM-DD-post-<slug>.png`, `git add/commit/push` from `assets/branding/`, verify with `curl -o /dev/null -w "%{http_code}"` that the raw URL returns 200.
6. Update the corresponding post's `imageUrl` in `../../automation/scripts/calendar.json`.

## History
- 2026-09-09: first generation — `2026-09-10-post-erp-dte.png` (ERP/DTE, real dashboard cropped to avoid showing "$0.00"), `2026-09-10-post-couriera.png` (value panel from Couriera's login screen, side crop), `2026-09-10-post-dte-educativo.png` (typographic, no screenshot — the real DTE invoice one showed "DTE: PENDING" and a list of missing configuration, unusable).
- 2026-09-10: **false-claim correction.** `hero-con-captura.html`'s subheadline said (in Spanish) "You activate your own Hacienda certificate yourself, without depending on outside technical support" — the user clarified that's false: it's the InnoSoft team who configures the certificate and Hacienda credentials on the client's behalf, because clients find it hard to do it themselves. The subheadline in the template was corrected (now hardcoded with the right message) and `2026-09-10-post-erp-dte.png` was re-rendered with the same real dashboard (`2026-09-01-erp-dashboard.png`) — used in post-7/post-7-ig, which hadn't been published yet. See `../../catalog/productos-y-servicios.md` and `../../QA-AUDIT.md` for the rest of the places that were corrected (Facebook/Instagram/LinkedIn bios, `calendar.json`, `copy-final-lote1.md`, `strategy/`).
- 2026-09-14: **post-2 image (LinkedIn) replaced + coral accent added.** The previous image (`2026-09-01-config-dte-autoservicio.png`) was a raw screenshot that also reinforced the already-corrected "self-service" message — it should never have been assigned to a post (rule 2b). Generated `2026-09-14-post-linkedin-dte-contadores.png` with `tipografico-sin-captura.html`, copy aimed at accountants/accounting firms (same hook as the real post-2). While making this change, the user asked for colors that grab more attention — the coral/amber accent variant was added (see tokens above) instead of inventing a new off-brand palette.
- 2026-09-17: **fixing 3 real user feedback points in a single session, on `2026-09-17-ad-dte-zona-oriental.png` (paid-campaign image, not organic):**
  1. *(the user said, in Spanish) "use a different kind of background colors, vary it, always the same ones"* — the teal palette was added (see tokens) as a second background option, with the rule to alternate instead of always repeating violet.
  2. *(the user said) "very direct, threatening language"* — the original copy ("Your business in the East already must issue DTE to Hacienda") sounded like a legal threat. Rewritten to a consultative tone: headline "Facturación Electrónica DTE, sin complicaciones" ("DTE electronic invoicing, hassle-free"), CTA "Conversemos sobre tu negocio" ("Let's talk about your business") instead of an urgency/obligation-driven call to action. Also applied as the new default copy in `tipografico-sin-captura.html`.
  3. *(the user said) "that box you put before InnoSoft feels too AI-generated"* — the `.logo-mark` (a CSS gradient square, no real logo) was replaced in all 3 templates with the real file `../logo-header.png`. Never use a placeholder there again, the real logo was always available in `assets/branding/`.
- 2026-10-01: **second content batch (lote 2), 3 new images, all typographic (no screenshot adds value to any of these 3 stories):** `2026-10-01-post-precio-real.png` (first post ever to cite real pricing — $22/mo system, $9.99/mo DTE-only — small price-card UI elements added inline in a one-off variant of `tipografico-sin-captura.html`, violet bg + coral accent), `2026-10-01-post-dte-plazo-dic2026.png` (real verified DTE 2.0 deadline, 1-dic-2026, with a date-badge element, teal bg + coral accent), `2026-10-01-post-diseno-web.png` (first post for the new website-design-and-hosting service, violet bg + lavender accent — more sober tone, matches LinkedIn placement). Rendered via Playwright (not claude-in-chrome) at exactly 1080×1080 to avoid disturbing the user's own open Chrome tab/window — see `project_auto_greeting_active`/`feedback_dont_close_shared_tabs` in persistent memory for why. The 3 one-off HTML files were deleted after rendering (not meant to be reused as base templates); only the final PNGs were committed. Full copy: `content-calendar/copy-final-lote2.md`.

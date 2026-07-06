# myquest — Build Specification

> **Read this first, every time. Not one electron moves without this document.** Before you analyze, plan, edit, run, or commit anything, you read this file. It is the single source of truth for what this repo is, what is true about it today, and the rules you build under. When in doubt, this doc wins. If reality conflicts with this doc, **stop and ask before changing course.**
>
> The codebase is ground truth for everything this document does not yet cover. This spec grows toward a full system description one verified section at a time. It never grows by guessing.

---

## 0. What this product is

A set of account-based marketing (ABM) microsites for **myQuest Skills (MQS)**, one per named channel-partner prospect in the industrial training vertical. Each microsite is a single, self-contained, prospect-specific landing page that demonstrates real diligence on that company and drives one action: **book a demo**. The audience is a mid-level contact who forwards it upward, and a senior leader who reads it as serious research rather than a templated pitch.

The five prospects: **Tooling U-SME**, **NCCER**, **Intertek Alchemy**, **Penn Foster**, **Vector Solutions Industrial**.

The repo follows a **template-and-folders model**: a never-linked `/_template/` page is the structural source of truth; each prospect lives in its own folder; the site root redirects visitors away rather than rendering anything.

**What this product deliberately does not do:**
- No search indexing. Every prospect page ships `noindex, nofollow, noarchive`. These pages are found by the link we send, nowhere else.
- No CMS, no framework, no build step, no JavaScript beyond what already exists in the template. Static HTML plus one `vercel.json` for the root redirect.
- No forms, no analytics scripts, no cookie banners. The only conversion surface is the Calendly link.
- No generic myQuest marketing content beyond the shared header/footer chrome. The body of each page is 100% prospect-specific.
- No bare root page. `/` never renders a shell; it redirects.

**Vocabulary (use these terms, not their generic equivalents):**
- MQS is a **skill-building platform**. It is NEVER described as an "AI practice tool," "simulation platform," or "role-play tool." AI-driven conversation practice is one mechanism inside the platform, alongside manager observation prompts and skill mapping.
- **Skill mapping is not a completion report.** Completion rates count seats; skill maps measure capability. This distinction must survive every draft.
- Copy leads with **evidence and proof the buyer can show others** (Kirkpatrick Level 3), never with product mechanism.
- The unifying partner pitch: *"Your training is trusted. Our platform proves it works."*

---

## 1. Ground truth

Verified against the code at HEAD (single commit, `main`) plus the approved restructure below.

**Repo contents today (pre-restructure):**
- `index.html` — the Tooling U-SME microsite, ~247 KB, fully self-contained (CSS inline, prospect logo / skill-map illustration / customer-logo strip embedded as data URIs).
- `images/` — three shared files referenced relatively: `myquest-logo.png` (navbar), `ISO-27001-Information-Security-1.png` and `icons8-gdpr-100-2-1_1icons8-gdpr-100-2-1.png` (footer compliance badges).

**Approved target structure (Chunk 0 delivers this):**
```
/vercel.json                  → 302 redirect: / → https://www.myquest.co/partners
/_template/index.html         → chrome + section skeleton, placeholder slots, source of truth
/images/                      → all shared assets, incl. localized white myQuest logo
/tooling-u-sme/index.html     → the current root page, moved unchanged (paths adjusted)
/nccer/index.html             → to be built
/intertek-alchemy/index.html  → to be built
/penn-foster/index.html       → to be built
/vector-solutions/index.html  → to be built
```

**Deployment:** Vercel, auto-deploy from `main`, project `myquest-sigma` at `myquest-sigma.vercel.app`. No outreach links are in circulation (Rico is paused), so the root move breaks nothing.

**Approved decisions (were `[VERIFY]`, now resolved):**
- The footer white-logo `src` currently points at a Webflow CDN URL (`uploads-ssl.webflow.com/...white%20logo.png`). **Approved:** download once into `/images/` and reference locally on every page.
- Tooling U-SME moves from root to `/tooling-u-sme/`. **Approved.** Root becomes a redirect to `https://www.myquest.co/partners` via `vercel.json`.

**Per-prospect elements (the swap surface):**
1. `<title>` — pattern: `A partnership concept for {Prospect} | myQuest`
2. `<meta name="description">` — prospect-specific one-liner
3. `<link rel="canonical">` — pattern: `https://www.myquest.co/partners/{slug}`
4. Calendly CTA URL — `https://calendly.com/partnerships-10/myquest-parternship-call-website/?utm_source={slug}-partnership` (appears 5×: desktop nav, mobile nav, hero, CTA section, footer "Book a Call"). Note: the `partnerships-10/myquest-parternship-call-website` path is verbatim from the live Calendly link, including the existing spelling; do not "fix" it.
5. Hero: partnership tag, headline, subhead, three stat cards, co-brand lockup (myQuest × prospect logo on black; prospect logo embedded as data URI)
6. Gap section: title, body argument, pullquote card sourced from the **prospect's own published materials**
7. Video section: heading + Vimeo embed (`?title=0&byline=0&portrait=0&dnt=1`; Tooling U-SME ID is `1197951399`)
8. Three theme cards, each with a content column and a gradient "pitch in one line" card
9. Skill-map illustration (webp data URI) — named skills must fit the prospect's audience (Tooling U-SME version: Active Listening, Conflict Resolution, Incident Response)
10. Pilot-shape card — the "Content" row names the prospect's catalog (e.g., "Layered on existing Tooling U-SME courses")

**Shared chrome (identical on every page, defined by `/_template/`):**
- Navbar: myQuest logo → myquest.co, Products dropdown (MQS / MQS LMS), Resources, Case Studies, Blog, About, "Book a demo" button (Calendly, prospect UTM), hamburger + mobile nav
- Footer: white myQuest logo (local `/images/` copy), tagline, ISO 27001 / GDPR badges, Explore / Products / Use Cases / Legal / Contact / Follow / Latest Posts columns, "Book a Call" button (Calendly, prospect UTM), copyright bar
- `noindex, nofollow, noarchive` robots meta
- Inter via Google Fonts; myQuest palette CSS variables (`--mq-blue: #0046FF`, `--mq-lavender: #9383CE`, `--mq-cyan: #68A4D6`, ink/paper/rule scales)
- Trust strip (customer logos data URI) and the "four pieces of the partnership" how-it-works section — shared copy unless a prospect page has an approved override

---

## 2. Brand / UX invariants

- Palette and typography exactly as defined in the `:root` block of the template. Inter only. No new fonts, no new brand colors.
- Co-brand lockup: myQuest white logo × prospect logo on a pure black pill (handles JPEG-with-black-background prospect logos).
- Single CTA action across the entire page: Book a demo (Calendly). No secondary CTAs, no nav links added, no contact forms.
- Section order is fixed: hero → gap → video → three themes (with skill-map illustration) → how-it-works → trust strip → CTA/pilot → footer. Deviations require approval.
- Vimeo embeds always use `title=0&byline=0&portrait=0&dnt=1`.
- Mobile behavior (hamburger, stacking) must match the Tooling U-SME page.

---

## 3. Stack and infrastructure

> Verify against the repo before relying on any of this. The codebase is ground truth.

- **Framework:** none. Hand-authored static HTML, one file per site, CSS inline in `<head>`.
- **Language:** HTML/CSS; minimal vanilla JS only if already present in the template.
- **Routing/config:** one `vercel.json` at repo root, containing only the `/` → `https://www.myquest.co/partners` redirect (302). Nothing else lives in it without approval.
- **Hosting:** Vercel, auto-deploy from `main` on `github.com/kothinti/myquest`. Project `myquest-sigma`.
- **Dev environment:** `C:\dev\myquest`, Claude Code in VS Code, governed by `../MASTERSPEC.md` + this file.
- **Assets:** shared images in `/images/` referenced by **root-relative paths** (`/images/myquest-logo.png`) from every page; prospect-specific images embedded as data URIs inside each page's HTML.

---

## 4. Architecture: the template-and-folders model

**`/_template/index.html` is the structural source of truth.** It is the full page — chrome plus every body section in fixed order — with every per-prospect slot marked by an unmistakable placeholder token (`{{PROSPECT_NAME}}`, `{{SLUG}}`, `{{HERO_HEADLINE}}`, `{{STAT_1_VALUE}}`, `{{PULLQUOTE}}`, `{{VIMEO_ID}}`, `{{THEME_1_TITLE}}`, `{{COBRAND_LOGO_DATA_URI}}`, `{{SKILLMAP_DATA_URI}}`, etc.). It is derived from the Tooling U-SME page in Chunk 0 by extracting its structure and replacing prospect content with tokens.

- `/_template/` is never linked from anywhere and additionally ships `noindex, nofollow, noarchive`. It exists for builders, not visitors.
- **New prospect page = copy `/_template/index.html` into `/{slug}/`, fill every token from the approved content doc.** A page ships with zero `{{` sequences remaining — grep-verified.
- The folder name **is** the slug used in the canonical URL and the Calendly `utm_source` (`{slug}-partnership`). One slug, three places, always matching.
- `/tooling-u-sme/index.html` is the moved original page. It is a live-ready outreach asset and the reference for look-and-feel; `/_template/` is the reference for structure. If the two ever disagree structurally, stop and ask.
- **Chrome changes flow template-first:** edit `/_template/`, then apply the identical change to all five prospect pages in the same commit, verified by diffing the chrome blocks across files. A chrome change that touches the template but not every page is an incomplete change.
- The root URL renders nothing, ever. `vercel.json` redirects `/` to `https://www.myquest.co/partners`.

---

## 5. Hard rules for the build

These are non-negotiable. Violating any of them requires explicit approval.

1. **Analyze before code.** State the task, what you read, the files you will touch, and what you will not touch — before any edit.
2. **Diff before commit.** Surface the full diff and stop for approval. No commit without sign-off. New capability → new branch (`site-{slug}`; restructure work → `chunk-00-restructure`; chrome syncs → `chrome-{short-name}`); merge only after James smoke-tests the Vercel preview.
3. **Verify against the rendered page, not the source.** Open the deployed preview (or local file in a browser), inspect every section against the checklist in §7, and report per-criterion PASS/FAIL with evidence.
4. **Never fabricate prospect facts.** Every stat card number, pullquote, program name, and product claim comes from copy James approved in Claude.ai. Claude Code does not write, embellish, or "punch up" prospect-facing copy. If approved copy is missing for a slot, stop and ask — do not fill the gap.
5. **Never invent files, routes, or assets** the repo does not establish. Propose and wait.
6. **Positioning rules are code-level requirements.** The strings "AI practice tool," "simulation," "role-play," and "practice platform" do not appear in prospect-facing copy. "Skill-building platform" framing and the skill-map-vs-completion-report distinction are preserved verbatim from approved copy.
7. **Every page ships `noindex, nofollow, noarchive`** — prospect pages and `/_template/` alike. Check explicitly before merge.
8. **Every Calendly link on a page carries that page's `utm_source={slug}-partnership`.** All five occurrences per page. Grep for `calendly` and count.
9. **Do not modify `/tooling-u-sme/index.html`** except (a) the Chunk 0 move itself with its path adjustments, and (b) approved chrome syncs applied to all pages at once.
10. **No new dependencies, no build tooling, no framework migration.** `vercel.json` holds the root redirect and nothing more. If a task seems to need more, surface the conflict.
11. **No page ships with an unfilled placeholder.** `grep -r '{{' {slug}/` returns nothing before a PR opens.

---

## 6. Non-fabrication (the integrity guarantee)

These pages exist to prove diligence. A single invented statistic, misattributed quote, or fabricated program name destroys the entire premise in front of the exact audience it was built to impress. If a pullquote cannot be traced to the prospect's own published materials, it does not ship. If a stat card lacks a verified number, the card is cut or replaced with an approved alternative — never approximated.

---

## 7. Build order, conventions, and per-site acceptance checklist

**Chunk 0 — Restructure (branch `chunk-00-restructure`), before any new site:**
1. Move `index.html` → `/tooling-u-sme/index.html`; convert its shared-image references to root-relative `/images/...` paths.
2. Download the Webflow white myQuest logo into `/images/` (e.g., `myquest-logo-white.png`); repoint the footer reference.
3. Add `vercel.json` with the `/` → `https://www.myquest.co/partners` 302 redirect.
4. Create `/_template/index.html` from the moved page: identical structure and chrome, all ten swap-surface elements replaced with `{{TOKEN}}` placeholders, `noindex` retained.
5. Verify on the Vercel preview: root redirects; `/tooling-u-sme/` renders pixel-identical to the pre-move page (all images load, all 5 Calendly links intact); `/_template/` renders as an obviously tokenized skeleton.

**Chunks 1–4 — one branch per site (`site-nccer`, `site-intertek-alchemy`, `site-penn-foster`, `site-vector-solutions`),** each built only after James delivers the approved content doc, prospect logo, and Vimeo ID for that prospect.

**Acceptance checklist for every new page (report each PASS/FAIL):**
- [ ] `noindex, nofollow, noarchive` present
- [ ] Canonical = `https://www.myquest.co/partners/{slug}`
- [ ] All 5 Calendly links carry `utm_source={slug}-partnership`
- [ ] Title and meta description follow the pattern and name the prospect
- [ ] Co-brand lockup renders prospect logo correctly on black (hero and footer mini-lockup)
- [ ] Vimeo embed uses the correct per-prospect video ID with `title=0&byline=0&portrait=0&dnt=1`
- [ ] All copy matches the approved content doc exactly (no paraphrasing)
- [ ] Banned strings absent: grep for "AI practice", "simulation", "role-play", "practice platform" returns nothing in body copy
- [ ] Zero `{{` placeholder sequences remain in the file
- [ ] Skill-map illustration shows the approved per-prospect skill names
- [ ] Pilot card "Content" row names the prospect's catalog
- [ ] Shared chrome byte-identical to `/_template/` and the other pages (diff the navbar and footer blocks)
- [ ] Shared images load from `/images/` root-relative paths
- [ ] Page renders correctly desktop and mobile (hamburger works, sections stack)

**When in doubt, stop and ask.** If this document and the code disagree, the code is the truth and this document is updated.

# PrivateHub – Project State

## 1. Project Overview

Static multi-language landing + SEO funnel.

**Goal:**
Drive traffic → landing (A/B/C) → redirect via tracking (Binom)

**Stack:**
- Pure static HTML
- Hosted on GitHub + Cloudflare Pages
- Tracking via: `track.privatehub.io`

---

## 2. Current Structure


/site
│
├── index.html                ← Homepage EN (da sviluppare SEO layer)
├── PROJECT-STATE.md
│
├── /assets
│   ├── /css/base.css
│   ├── /js/tracking.js
│   ├── /img
│   └── /favicon
│
├── /policies
│   ├── privacy.html
│   ├── terms.html
│   └── cookies.html
│
├── /a /b /c                  ← Landing ROOT (EN / fallback)
│   ├── index.html
│   └── index_a/b/c.html      ← (⚠️ duplicati / legacy)
│
├── /de
│   ├── index.html
│   ├── /a /b /c
│   │   └── index.html
│   ├── /SEO pages
│   │   ├── erotik-chat-ohne-anmeldung/
│   │   ├── sex-chat-kostenlos/
│   │   ├── live-chat-frauen/
│   │   ├── ...
│   ├── _template-seo.html
│   └── pages.tsv
│
├── /it
│   ├── index.html
│   ├── /a /b /c
│   │   └── index.html
│   ├── /SEO pages (parziale)
│   │   ├── chat-erotica-senza-registrazione/
│   │   ├── chat-erotica-gratis/
│   │   ├── live-chat-donne/
│   ├── _template-seo.html
│   └── pages.tsv
│
├── /es
│   ├── index.html
│   ├── /a /b /c
│   │   └── index.html
│   ├── /SEO pages
│   │   ├── chat-erotico-sin-registro/
│   │   ├── chat-erotico-gratis/
│   │   ├── chat-con-mujeres/
│   │   ├── chat-erotico-anonimo/
│   │   ├── chat-flirt-espana/
│   │   ├── sexchat-sin-registro/
│   │   ├── sexchat-inmediato/
│   │   └── sexchat-en-vivo/
│   ├── _template-seo.html
│   └── pages.tsv


---

## 3. Landing System

Each language has 3 variants:

- **A → Urgency**
- **B → Exclusivity**
- **C → Direct / Neutral**

### Tracking format
https://track.privatehub.io/click?campaign=XX-a&source=direct

Where:
- `XX` = language (de / it / es)
- `a/b/c` = landing variant

---

## 4. SEO Page Generation System

Each language uses:

- `pages.tsv`
- `_template-seo.html`

### TSV Structure
slug | title | description | h1 | p1 | p2 | p3 | p4 | cta1 | cta2

### Generation Command

Run inside language folder:

```bash
tail -n +2 pages.tsv | while IFS=$'\t' read -r slug title description h1 p1 p2 p3 p4 cta1 cta2; do
  mkdir -p "$slug"
  sed \
    -e "s|{{TITLE}}|$title|g" \
    -e "s|{{DESC}}|$description|g" \
    -e "s|{{H1}}|$h1|g" \
    -e "s|{{P1}}|$p1|g" \
    -e "s|{{P2}}|$p2|g" \
    -e "s|{{P3}}|$p3|g" \
    -e "s|{{P4}}|$p4|g" \
    -e "s|{{CTA1}}|$cta1|g" \
    -e "s|{{CTA2}}|$cta2|g" \
    _template-seo.html > "$slug/index.html"
done





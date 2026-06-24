# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Client Content Reference

Official copy provided by the client. Use this as the source of truth for all text, labels, and messaging on the site.

```
EDMADEIRA
CNPJ: 22.105.428/0001-31
ANCINE: 29809

---

PRECISÃO & MOVIMENTO.
Toda história merece ser contada. A EdMADEIRA te ajuda a contar a sua.


TRABALHOS SELECIONADOS


FILMES COM A PAIXÃO DO ARTESÃO.
Em um mundo cada vez mais dinâmico, contar sua história é uma necessidade. A EdMADEIRA
acredita no cuidado artesanal, unindo a bagagem de profissionais experientes à energia
de novos talentos para trazer à tona narrativas visuais com excelência e precisão, sem
deixar de lado cada detalhe que faz a sua história tão especial.
Toda história merece ser contada. A EdMADEIRA te ajuda a contar a sua.


HISTÓRIAS JÁ CONTADAS
<<<logos>>>


VAMOS CONSTRUIR O PRÓXIMO QUADRO?
Conte-nos sobre o seu projeto ou envie um email direto
```

**Notes on the copy vs. current implementation:**
- Hero tagline is currently "Precisão em Movimento." — client copy says "Precisão & Movimento." — needs alignment.
- "Sobre" section uses a shortened version of the long paragraph above; the full text is the client's canonical version.
- "HISTÓRIAS JÁ CONTADAS" with client logos is a section **not yet implemented** — placeholder `<<<logos>>>` in client copy. This is a roadmap item.
- CNPJ and ANCINE registration numbers should appear in the footer when one is added.

---

## Competitive Landscape & Content Suggestions

Research into the São Paulo boutique audiovisual market. Use this to sharpen positioning, copy, and SEO on the site.

### Key Competitors

| Company | Positioning | Differentiator |
|---|---|---|
| [Boutique Filmes](https://boutiquefilmes.com.br/en/) | "Histórias de impacto que atravessam o tempo" | Big streaming partners (Netflix, HBO, Amazon). International. Fiction + docs + kids. |
| [Forasteiro Produções](https://forasteiroproducoes.com/) | Cinema + mercado publicitário + redes sociais | 15+ years, national & international productions |
| [Onze Trinta](https://www.onzetrinta.com/) | Creative studio for brands & agencies | Integrates research, creation, production, post, branding and copywriting |
| [Dummy Filmes](https://dummyfilmes.com.br/) | Institutional + advertising + videoclipes | Broad service menu, SEO-heavy site |
| [Miragem Content](https://www.miragemcontentsp.com.br/) | Storytelling + branded content | Positions around ROI and brand results |
| [Forasteiro](https://forasteiroproducoes.com/) | Digital video for internet | Social-first angle |

### What EdMADEIRA Has That Competitors Don't

- **Mixed team narrative** — "profissionais experientes + novos talentos" is a real differentiator; most competitors sell either authority (long track record) or youth/energy, not both.
- **Artisan framing** — "cuidado artesanal" and "paixão do artesão" is distinctive and ownable. Competitors use terms like "impacto", "resultado", "alto impacto" — more corporate.
- **ANCINE registration** — signals legitimacy for institutional/public-sector clients. Worth making visible (footer or Sobre section).
- **Documented social-impact credits** — WHO, Bienal de São Paulo, CENPEC — most boutiques don't have that range. Could be highlighted.

### What's Missing vs. Competitors

- **Services list** — Every competitor explicitly lists what they do (vídeo institucional, documentário, videoclipe, branded content, social media, etc.). EdMADEIRA has no services section. Clients searching for a specific service can't identify fit.
- **Client logos section** — "Histórias já contadas" in the client copy — not yet built. Competitors all show this prominently. Logos = social proof at a glance.
- **Contact info beyond form** — Competitors display phone, WhatsApp, or email in the open. The form alone creates friction.
- **Blog / case studies** — Several competitors publish SEO content (how-to, behind-the-scenes). Not a priority for a boutique, but notable gap for organic search.
- **Language toggle** — Boutique Filmes has PT/EN. Given WHO and international credits, could be relevant for EdMADEIRA.

### Suggested SEO Keywords

Primary (high intent, São Paulo geo):
- `produtora audiovisual São Paulo`
- `produtora de vídeo SP`
- `produtora de documentário São Paulo`
- `vídeo institucional São Paulo`
- `filme publicitário São Paulo`

Secondary (type/format):
- `mini documentário`
- `videoclipe profissional`
- `branded content`
- `vídeo para redes sociais`
- `produtora boutique`

Differentiator keywords (ownable, less competed):
- `produção audiovisual artesanal`
- `produtora com ANCINE`
- `produtora documentário social`
- `produtora audiovisual cuidado artístico`

### Copy & Messaging Suggestions

- Current tagline "Precisão em Movimento" is strong visually but abstract. Consider adding a concrete one-liner below it that names what EdMADEIRA actually does (e.g., "Documentários, filmes publicitários e videoclipes — com a precisão de quem cuida de cada quadro.").
- "Toda história merece ser contada" from the client copy is warm and memorable — it's currently absent from the site. Good candidate for the Sobre section closing line.
- The "profissionais experientes + novos talentos" narrative is genuinely differentiating and currently buried in the Sobre text. Consider surfacing it with a short team/approach section or even just a pull-quote.
- ANCINE + CNPJ in the footer adds credibility for institutional/public-sector clients who vet vendors formally.

---

## Project Overview

Static single-page portfolio website for **EdMADEIRA Filmes**, a Brazilian audiovisual production company. No build step, no package manager, no bundler — everything ships as-is to GitHub Pages.

**Stack:** HTML + Tailwind CSS (CDN) + Alpine.js (CDN) + custom CSS (`assets/style.css`)

**Deployment:** Every push to `main` auto-deploys via GitHub Actions → GitHub Pages. There is no staging environment.

## Local Development

Open `index.html` directly in a browser, or serve it with any static file server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

No build, lint, or test commands exist.

## Architecture

The entire site lives in a single file: `index.html`. It is a **horizontal scroll-jacking** layout:

- The `<main>` element is `400dvh` tall — four "virtual pages" stacked vertically.
- A sticky inner container holds a `400vw`-wide flex row of four `<section>` elements.
- Scroll position is captured by Alpine.js and converted into a `translateX` offset via `translatePercentage`, creating the illusion of horizontal scrolling.
- Snap points are handled by invisible `.section-snap-point` divs (defined in `style.css`) that correspond to each section by index (0 = Hero, 1 = Portfolio, 2 = Sobre, 3 = Contato).

### Alpine.js State (root `x-data` on `<body>`)

| Property | Purpose |
|---|---|
| `scrollPos` | Current `pageYOffset` |
| `maxScroll` | `scrollHeight - innerHeight` |
| `translatePercentage` | `(scrollPos/maxScroll) * 300` — drives the horizontal slide |
| `currentSection` | `round(scrollPos / innerHeight)` — used for nav active state |
| `scrollToSection(index)` | Programmatic scroll; disables snap on mobile to avoid conflicts |

### Portfolio Modal

Portfolio thumbnails fire an `open-video` custom event via `$dispatch`. The modal (at the bottom of `<body>`) listens via `@open-video.window` and populates its Alpine state. Credits fields available: `direcao`, `filmmaker`, `producao`, `animacao`, `assistente_direcao`, `assistente_audio`, `captacao_audio_mixagem`, `edicao_som_mixagem`, `captacao_som`, `mixagem`. The Vimeo iframe is only rendered while `open === true` (via `<template x-if>`), which stops video playback on close.

### Contact Form

Posts JSON to Formspree (`https://formspree.io/f/YOUR_FORM_ID`). **Replace `YOUR_FORM_ID`** with the real endpoint from formspree.io before going live. The form manages its own Alpine state (`status`: `idle | loading | success | error`) inside the Contato section's `x-data`.

### Fonts

- **Gilroy** — loaded from `assets/gilroy/Gilroy-Bold.ttf` and `assets/gilroy/Gilroy-Light.ttf` (`.ttf` format, full family also available in that folder).
- **Cormorant Garamond** — loaded from Google Fonts CDN (class `font-serifa`).

### Images

All images live in `img/`. Background images use responsive `srcset` with three sizes:
- `_720.jpg` → `720w`
- `_1080.jpg` → `1080w`
- `_1440.jpg` → `1440w` (also used as `src` fallback)

Old/unused files in `img/` are suffixed `_OLD` or `_SEPIA` — safe to delete.

---

## Roadmap

Items identified during the initial code audit. Ordered roughly by priority.

### Critical
- [ ] **Add Formspree ID** — Replace `YOUR_FORM_ID` in `index.html:368` with the real endpoint. Until this is done, the contact form always fails silently.
- [ ] **Convert Gilroy to woff2** — Fonts are currently served as `.ttf` from `assets/gilroy/`. Converting to `.woff2` would roughly halve the file size. The full family is available in that folder if more weights are ever needed.

### Bugs
- [x] **`animacao` credit not shown in modal** *(fixed)* — "Equipe Cidadã" dispatches `animacao: 'eFrame'`; modal now has `<p x-show="animacao">`.
- [x] **Orphan `text-center` in `:style` attribute** *(fixed)* — Was invalid HTML on the Sobre section wrapper.

### Improvements
- [ ] **Pin CDN versions** — Tailwind and Alpine.js are loaded with floating `@3.x.x` refs. Pin to specific versions (e.g. `alpinejs@3.14.1`) so a CDN update can't break the site silently.
- [ ] **Add Subresource Integrity (SRI)** — After pinning versions, add `integrity` + `crossorigin` attributes to the CDN `<script>` tags.
- [ ] **Replace Vimeo background iframe** — The Hero section background is a Vimeo `?background=1` iframe that streams on every page load. The local `.webm`/`.mp4` files already exist in `img/` — swap back to the commented-out `<video>` tag for better performance and offline resilience.
- [ ] **`assistente_audio` label cleanup** — The field named `assistente_audio` displays as "Assistente de direção e captação de áudio", blending two roles. Consider splitting into two distinct fields.
- [ ] **Remove leftover `_OLD` / `_SEPIA` images** — `img/entrega-photo_1440_OLD.jpg`, `img/sobre-photo_1440_OLD.jpg`, `img/sobre-photo_1440_SEPIA.jpg`, `img/logo_edmadeira_OLD.png` are unused and add repo weight.
- [ ] **Add `<meta>` SEO tags** — No `description`, `og:image`, or `og:title` tags exist. Important for sharing and indexing.
- [ ] **Add favicon** — No favicon is declared.
- [ ] **Align hero tagline** — Current: "Precisão em Movimento." / Client copy: "Precisão & Movimento." — confirm with client which is final.
- [ ] **"Histórias já contadas" section** — Client copy includes a logos/clients section (`<<<logos>>>`) that is not yet implemented. Needs client to supply logo files.
- [ ] **Footer with legal info** — CNPJ `22.105.428/0001-31` and ANCINE `29809` should be displayed somewhere (typically a footer). No footer exists yet.
- [ ] **Sobre section copy** — Current text is a paraphrase. Full canonical copy from client is in the "Client Content Reference" section above.

### Marketing & SEO
- [ ] **Add `<meta>` SEO tags** *(already listed above — merging here for priority)* — `description`, `og:title`, `og:image`, `og:type`, `twitter:card`. Copy suggestion: "Produtora audiovisual boutique em São Paulo. Documentários, filmes publicitários e videoclipes com precisão artesanal."
- [ ] **Schema.org markup** — Add `LocalBusiness` + `VideoObject` JSON-LD to `<head>`. Enables rich results in Google Search. Include ANCINE registration in `legalName`.
- [ ] **`llms.txt` file** — Add a root-level `llms.txt` (plain text, ~500 words) describing EdMADEIRA's services, credits (WHO, Bienal de São Paulo, CENPEC), and contact. AI search engines (Perplexity, ChatGPT) index this and cite it when users ask for production company recommendations.
- [ ] **Services section** — Every competitor lists services explicitly. Add a fifth section (or fold into Sobre) listing: vídeo institucional, documentário, branded content, videoclipe, vídeo para redes sociais. Improves SEO keyword targeting and helps clients identify fit quickly.
- [ ] **Video Brief Generator** — Client-side free tool (pure JS, no backend). Modal triggered by "Monte seu brief" button in the Contato section. 4-step flow:
  - Step 1: Tipo de projeto (Institucional / Documentário / Branded Content / Videoclipe / Publicitário / Redes Sociais)
  - Step 2: Sobre o projeto (descrição livre, público-alvo, tom: Formal / Equilibrado / Descontraído)
  - Step 3: Logística (prazo desejado, faixa de orçamento: até 15k / 15–50k / 50k+ / a definir — **confirm ranges with client**)
  - Step 4: Dados de contato (nome, empresa, email) → Formspree submits to both prospect and EdMADEIRA + copy-to-clipboard
  - Output: formatted brief displayed inline, with "falar com a EdMADEIRA" WhatsApp link at the bottom (**needs WhatsApp number**)
  - Blocked on: Formspree ID + WhatsApp number + budget range confirmation from client
- [ ] **WhatsApp CTA** — `wa.me` link in Contato section alongside the form. Pre-filled message: "Olá, gostaria de conversar sobre um projeto". Blocked on: WhatsApp number from client.

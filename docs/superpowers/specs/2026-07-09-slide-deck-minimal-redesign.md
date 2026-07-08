# Slide-Deck Minimal Redesign — Design

**Date:** 2026-07-09
**File touched:** `index.html` only (zero-build static site; no new files, no build step)
**Base:** commit `4fc7357` (lean & fun vertical pass) — copy and metric chips carry over from it.
**Goal:** Replace vertical scrolling with a horizontal slide deck, and strip the visual theme to
minimal black + pink. Chosen from three prototyped horizontal models (slide deck / film strip /
hybrid shelves); user picked slide deck with "minimalist, black and pink only" direction.

## Decisions (from brainstorming)

- **Navigation model:** Slide deck — each section is a full-viewport slide; one wheel flick,
  arrow key, or nav click moves exactly one slide left/right. No free panning.
- **Palette:** True black `#000` ground; `#ff2d95` pink as the *only* color; pink-biased
  off-white ink `#f4edf1`; pink-biased muted grey `#7d7381`; hairline `rgba(255,45,149,.22)`.
  Cyan, yellow, violet retired from `index.html`.
- **Effects retired:** starfield, scanlines, grain, grid floor, sun glow, glitch text, neon
  flood. Minimal = precision in spacing/type, not effects.
- **Type:** No webfont dependency required by the design; display = heavy `system-ui` stack
  (weight 800+, tight tracking, uppercase), operational text = `ui-monospace` stack. Orbitron +
  JetBrains Mono CDN links removed. (If font character is missed later, that's a follow-up.)
- **Mobile (default, re-confirm):** at ≤680px the deck unstacks to normal vertical scroll.
- **Old vertical pass (default, re-confirm):** committed as checkpoint `4fc7357`; redesign
  replaces it in the working tree.
- **HLD pages (default, re-confirm):** `settlement-hld.html`, `crawl-hld.html` untouched; only
  their portal buttons on `index.html` restyle to pink-hairline minimal.

## Slides

1. **Hero** — name (display, "Bhutani" in pink), one-line sub, 4-stat strip
   (`4 yrs / 1→8 / 2.5× / 8×`, hairline grid, pink numerals).
2. **Experience** — Juspay header + three sub-jobs with chip rows and 2–3 trimmed bullets each,
   Convosight intern. Settlement keeps its "open 3D system map" portal (restyled). If one
   viewport is too tight, Experience may split into two slides (Juspay / earlier).
3. **Projects** — five cards, hairline borders; Crawl Core keeps its HLD portal (restyled).
4. **Skills + Writing** — chips row + the one article link, combined on one slide.
5. **Contact** — email, GitHub, LinkedIn, LeetCode; terminal footer line stays.

## Mechanics

- Horizontal track: `#track{display:flex}` with `transform:translateX(-i*100vw)`,
  `transition:transform .55s cubic-bezier(.22,.9,.24,1)`; wheel handler with ~620ms lock,
  ArrowLeft/Right keys, nav links map to slide indices.
- Position indicator: row of pink dashes bottom-center (replaces the scroll-progress rail);
  active dash pink, rest hairline. Nav active state = pink text (IntersectionObserver replaced
  by slide index — the deck always knows where it is).
- `prefers-reduced-motion`: transition dropped, jump-cut between slides.
- Mobile ≤680px: track becomes normal block flow (vertical), wheel/key handlers disabled,
  dashes hidden; nav anchors scroll normally.
- Favicon: keep, recolored to pink-on-black if trivial (inline SVG data URI already).

## Out of scope

- `settlement-hld.html`, `crawl-hld.html` internals.
- `assets/` theme skill file — it documents the synthwave system, which lives on in the HLD
  pages; this spec supersedes it for `index.html` only. Project CLAUDE.md design section will
  need a follow-up edit once this ships.
- No build step, no new files, no deploy (push only on explicit go-ahead).

## Verification

- Serve locally (`python3 -m http.server 8000 --bind 127.0.0.1`), Playwright: desktop 1440×900 —
  each slide screenshot, wheel + arrow + nav-click navigation, dash indicator tracks; mobile
  390×844 — vertical fallback, all content reachable. Only black/pink/ink/grey in computed
  styles. Sign-off before any push.

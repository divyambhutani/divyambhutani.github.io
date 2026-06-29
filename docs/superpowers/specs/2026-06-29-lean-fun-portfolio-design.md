# Lean & Fun Portfolio Pass — Design

**Date:** 2026-06-29
**File touched:** `index.html` only (zero-build static site; no theme/font/token changes, no 3D HLD changes)
**Goal:** Make the portfolio lean and fun. The visuals are already rich; the copy reads like a
pasted resume. Cut words, surface buried numbers as visual chips, and let the design breathe.

## Decisions (from brainstorming)

- **Trim depth:** Punchy bullets — each sub-job goes from 4 dense sentences to 2–3 short fragments.
- **WIP ribbon:** Drop it entirely (reads more finished; reclaims 30px).
- **Fun elements:** Section progress (scroll bar + active-nav highlight) + metric chips.
- **Chip placement:** Per-job chip rows under each sub-job date, plus a widened 4-stat hero strip.
- **Copy:** Approved as proposed below.

## Changes

### 1. Remove WIP ribbon
- Delete `.wip` markup (the `<div class="wip">…</div>` line).
- Delete all WIP CSS: `.wip`, `.wip::before`, `@keyframes wipflow`, `.wip-txt`, `.wip .sym`,
  `.wip .dash`, `.wip .echo*`, `@keyframes wipdash`, and the WIP `prefers-reduced-motion` rule.
- Move `nav` from `top:30px` → `top:0`.
- Move `body::after` (scanline) and `body::before` (grain) from `top:30px` → `top:0`.

### 2. Section progress
- **Scroll bar:** fixed 2px element at the very top, magenta→cyan gradient, `width` tracks scroll
  percentage. Replaces the old ribbon's z-index slot.
- **Active-nav highlight:** `IntersectionObserver` watches sections (`#work`, `#projects`,
  `#skills`, `#writing`, `#contact`); the matching nav link gets an `.active` class (cyan, glow).
- Both wrapped in `prefers-reduced-motion` guards where motion applies (progress bar update is
  passive scroll, fine; no transition needed under reduced motion).

### 3. Metric chips
- New reusable `.chip` component: small pill, `var(--line)` border, cyan text, soft glow,
  uppercase mono. Rendered in a `.chiprow` (flex, wrap, gap).
- Hero stats strip: widen from 2 → 4 cells.
  `4 yrs · Experience` | `1 → 8 · Team scaled` | `2.5× · Throughput` | `8× · Faster CI`.
  Grid becomes 4 cols desktop, 2 cols mobile.
- Per-job chip rows under each sub-job's `.pdate`:
  - Settlement: `1→8 team`
  - Card Payment Gateway: `0→10K+ txns/day` · `2.5× throughput` · `CI 35→4 min`
  - Card Discovery: `80K+ cards`

### 4. Trimmed copy (final)

**Hero sub:**
> I build high-reliability payment systems — event-driven pipelines, reconciliation infra, and the
> odd agentic-AI experiment. 4 years across fintech + startups.

**Reconciliation & Settlement Platform** — Solo Architect → Team Lead · Feb 2025 – Dec 2025
- Built from zero — Python, FastAPI, Kafka, Postgres; design → deploy → ops
- Scaled team 1 → 8; hiring, mentoring, design reviews
- Event-driven recon (idempotent, retry-safe) + automated Visa/RuPay uploads, killing midnight ops

**Card Payment Gateway** — Jan 2024 – Feb 2025
- One of 4 scaling the gateway 0 → 10K+ daily txns; led HSBC HK onboarding
- Pluggable payment abstraction — new networks just add types + API calls (UnionPay)
- Key/cert rotation + master-key DB encryption

**Card Discovery Service** — Solo · Jul 2022 – Aug 2023
- Sole engineer — Haskell, MySQL, Redis on K8s; CI/CD Bitbucket + Jenkins
- Observability (Kibana, Prometheus, Grafana); tokenized 80K+ cards (Epifi, OneCard, HDFC)

**Convosight — Intern** · Remote (US startup) · Nov 2021 – Apr 2022
- Backend APIs — Node.js, Go, Serverless on AWS; shipped Reviews + Notifications
- Migrated Go Lambdas → TypeScript; built Metabase analytics dashboards

**Project blurbs (tightened ~30%):**
- *Crawl Core:* "FastAPI crawler — fetches any URL (curl_cffi + Playwright fallback), extracts
  clean body text, classifies type/topics/summary (BART-MNLI local · Gemini Flash cloud). The HLD
  scales this one unit into a billions-of-URLs/month Kafka pipeline with S3 + ClickHouse."
- *JobSherpa:* "Agentic pipeline — parses a resume + JD, scrapes live interview/salary data
  (Glassdoor, AmbitionBox, Levels.fyi), generates a visual PDF with rubrics, ATS scoring, and an
  interview roadmap."
- *Multipart Uploader:* "CLI from my first internship to learn Go — goroutines, concurrency, and
  the AWS S3 SDK, solving a real problem: uploading large files reliably via multipart chunks."
- YT Trending Insights, Perch: already short — leave as-is.

## Out of scope
- Theme tokens, colors, fonts — untouched.
- `settlement-hld.html`, `crawl-hld.html` — untouched.
- No build step, no new files.

## Verification
- Serve locally (`python3 -m http.server 8000 --bind 127.0.0.1`), Playwright screenshot desktop +
  mobile, confirm: ribbon gone, nav at top, chips render, hero 4-stat strip, active-nav highlight
  fires on scroll, progress bar tracks. Get sign-off before any push (deploy = explicit go-ahead).

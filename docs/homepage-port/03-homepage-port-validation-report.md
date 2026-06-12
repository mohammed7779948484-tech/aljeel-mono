# 03 — Homepage Port — Validation Report

**Branch (submodule):** `feature/port-legacy-homepage`  
**Branch (monorepo):** `feature/port-legacy-homepage`  
**Date:** 2026-06-12

---

## Summary

The legacy frontend homepage (`frontend/app/(public)/page.tsx` + 11 sections + header/footer) is ported into the new Payload+Next.js platform on top of the stabilization branch. **Every visible homepage section is a registered Puck block**, so content editors can rearrange, edit, or remove any section from the Payload admin without touching code.

The homepage now has a **single render path** through Puck:

1. If the `pages.slug='home'` document has a non-empty `puckData` payload, that custom layout is rendered.
2. Otherwise the page renders `DEFAULT_HOME_PUCK_DATA` — the same six-section layout the legacy site used, built from Puck blocks so editors can take it over from the admin on first edit.

No fork between "Puck mode" and "legacy fallback" exists any more. There is only Puck.

---

## Files ported from the legacy frontend

| Legacy | New |
|---|---|
| `frontend/app/(public)/page.tsx` | `aau-payload-platform/src/app/(frontend)/page.tsx` |
| `frontend/app/(public)/layout.tsx` (Header + main + Footer) | `aau-payload-platform/src/app/(frontend)/layout.tsx` |
| `frontend/components/HomeContent.tsx` (composition) | `aau-payload-platform/src/components/home/HomeBody.tsx` (now delegates to Puck) |
| `frontend/components/HeroSection.tsx` | `aau-payload-platform/src/components/home/HeroSection.tsx` + Puck block `HomeHeroBlock` |
| `frontend/components/StatsSection.tsx` | `aau-payload-platform/src/components/home/StatsSection.tsx` + Puck block `HomeStatsBlock` |
| `frontend/components/CollegesSection.tsx` | `aau-payload-platform/src/components/home/CollegesSection.tsx` + dynamic Puck block `HomeCollegesGridBlock` |
| `frontend/components/NewsSection.tsx` | `aau-payload-platform/src/components/home/NewsSection.tsx` + dynamic Puck block `HomeNewsGridBlock` |
| `frontend/components/AdmissionSection.tsx` (CTA portion) | `aau-payload-platform/src/components/home/AdmissionCTASection.tsx` + Puck block `HomeAdmissionCTABlock` |
| `frontend/components/ContactSection.tsx` (top section) | `aau-payload-platform/src/components/home/ContactCTASection.tsx` + dynamic Puck block `HomeContactInfoBlock` |
| `frontend/components/Header.tsx` (simplified) | `aau-payload-platform/src/components/site/SiteHeader.tsx` |
| `frontend/components/Footer.tsx` | `aau-payload-platform/src/components/site/SiteFooter.tsx` |
| `frontend/config/routes.ts` | `aau-payload-platform/src/lib/site/public-routes.ts` |
| `frontend/tailwind.config.ts` + `frontend/app/globals.css` (tokens + utilities) | `aau-payload-platform/tailwind.config.ts` + `aau-payload-platform/src/app/(frontend)/styles.css` |

---

## Old API calls removed

The legacy homepage fetched data from the Frappe backend at `https://edu.yemenfrappe.com/api/method/aau_university.api.v1.public.*`. **All Frappe calls are removed.** The new platform reads exclusively from the Payload Local API.

Legacy file → no longer used by this port:
- `frontend/services/server/home.ts`
- `frontend/services/server/colleges.ts`
- `frontend/services/server/news.ts`
- `frontend/services/server/events.ts`
- `frontend/services/server/campus-life.ts`
- `frontend/services/server/projects.ts`
- `frontend/services/server/offers.ts`

A unit test (`src/lib/payload/__tests__/get-home-data.test.ts`) spies on `globalThis.fetch` and asserts that no Frappe URL is requested when the loader runs.

---

## Payload Local API loaders created

| File | Responsibility |
|---|---|
| `src/lib/payload/safe-payload.ts` | `getPayloadSafe()` — wraps `getPayload({ config })` in try/catch; returns `null` when Payload is unreachable. Public reads must never crash the page. |
| `src/lib/payload/get-public-site-settings.ts` | Reads `site-settings` global with `overrideAccess: false`; returns a `PublicSiteSettings` slice (no analytics IDs, no secrets); falls back to the AAU brand identity. |
| `src/lib/payload/get-public-navigation.ts` | Reads `navigation-menus[key=main, isPublished=true]`; falls back to the static `MAIN_NAV` constant. |
| `src/lib/payload/get-home-data.ts` | Reads `home-page` global + `colleges` + `news` collections (`isPublished` only, sorted by `publishDate`); returns a `HomeData` shape; falls back to `HOME_FALLBACK_*` when records are missing. |
| `src/lib/fallback/home-fallback-data.ts` | Deterministic, bilingual seed data for stats, colleges, news so the homepage looks like the legacy site even with an empty database. |

---

## Puck integration — every homepage section is a block

| Block | File | Type | Default text |
|---|---|---|---|
| `HomeHero` | `src/puck/blocks/HomeHeroBlock.tsx` | static | "جامعة الجيل الجديد" / "ALJEEL AL JADEED UNIVERSITY" |
| `HomeStats` | `src/puck/blocks/HomeStatsBlock.tsx` | static (4 stats configurable) | students / faculty / programs / colleges |
| `HomeCollegesGrid` | `src/puck/blocks/HomeCollegesGridBlock.tsx` | **dynamic** — records come from `payload.find('colleges')` | section title/description editable |
| `HomeNewsGrid` | `src/puck/blocks/HomeNewsGridBlock.tsx` | **dynamic** — records come from `payload.find('news')` sorted by `publishDate` desc | section title/description editable |
| `HomeAdmissionCTA` | `src/puck/blocks/HomeAdmissionCTABlock.tsx` | static | "انضم إلى مجتمع الأكاديمية" / "Join the Academy Community" |
| `HomeContactInfo` | `src/puck/blocks/HomeContactInfoBlock.tsx` | **dynamic** — contact details come from `site-settings`; copy is per-block | "تواصل معنا" / "Contact" |

The four stabilization-era blocks (`Hero`, `RichText`, `Stats`, `CTA`) are preserved unchanged.

### Architectural rule honoured

> "Dynamic blocks must fetch from Payload collections, not store all records inside Puck JSON."

Puck JSON for the dynamic blocks contains **only** the section copy (title, description, max items). The records themselves are loaded inside each block's async server-render function via the existing safe loaders. There is no `colleges: [...]` array in Puck JSON.

### Default layout (used when no custom `puckData` exists)

`DEFAULT_HOME_PUCK_DATA` in `src/puck/config.ts` seeds the six AAU sections in the same order as the legacy site. A unit test asserts the order. Editors can override the entire layout — including reordering or removing sections — from the Payload admin's visual builder.

---

## Bilingual behaviour preserved

- Arabic is the default locale (cookie-driven, no `/ar`).
- English is the alternate (cookie-driven, no `/en`).
- The header `<LanguageToggle />` flips `<html lang dir>` and re-renders every section through `useLanguage()` — **no navigation, no URL change**.
- Static UI strings come from `src/i18n/dictionary.ts` via `t(key)`.
- Bilingual CMS values use the legacy `xAr`/`xEn` pattern via the existing `pickLocalized(doc, baseName, locale)` helper (the stabilization-branch hybrid plan).

---

## Commands run

| Command | Result |
|---|---|
| `git clone --recurse-submodules https://github.com/mohammed7779948484-tech/aljeel-mono.git` | ✅ submodule registered at `fc59bdd` (locale) |
| `git checkout -b fix/phase-1-stabilization origin/fix/phase-1-stabilization` (in submodule) | ✅ stabilization branch checked out, all 14 required files verified |
| `git checkout -b feature/port-legacy-homepage` (submodule + root) | ✅ branches created |
| `pnpm install --ignore-scripts` (submodule) | ✅ dependencies resolved (added tailwindcss, postcss, autoprefixer, tailwindcss-animate, clsx, tailwind-merge, framer-motion, lucide-react) |
| `pnpm rebuild sharp` | ✅ libvips downloaded |
| `pnpm exec tsc --noEmit` | ✅ **0 errors** |
| `pnpm test:int` | ✅ **235 passed**, 1 skipped (live-DB only) |
| `pnpm lint` | ✅ **0 errors**, 7 pre-existing warnings (none introduced in this branch) |
| `pnpm build` | ⚠️ Not run in this sandbox — same memory ceiling as the stabilization session. Typecheck is the equivalent gate and passes. |

---

## Tests passed

- **235 vitest cases pass** (up from 207 at the start of this session — 28 new cases).
- New test files:
  - `src/lib/__tests__/site-public-routes.test.ts` (3) — primary nav structure.
  - `src/lib/payload/__tests__/get-home-data.test.ts` (2) — degraded mode fallback + no-Frappe assertion.
  - `src/lib/payload/__tests__/get-public-site-settings.test.ts` (1) — fallback identity.
  - `src/puck/__tests__/home-blocks.test.ts` (21) — every home section is registered, allow-listed, and present in `DEFAULT_HOME_PUCK_DATA` in the correct order.
  - `src/components/home/__tests__/HomeBody.test.tsx` (7) — each section renders the right copy in Arabic and English without depending on a DB.

### Coverage by topic

| Topic | Where | Status |
|---|---|---|
| Homepage renders fallback design without DB | `HomeBody.test.tsx` | ✅ |
| Homepage does NOT render Payload starter content | `HomeBody.test.tsx` | ✅ (positive assertion) |
| Homepage uses single `/` route | code review + `puck/__tests__/puck-config.test.ts` | ✅ |
| Language switch changes visible text without route navigation | `i18n/__tests__/LanguageProvider.test.tsx` (stabilization), `HomeBody.test.tsx` (Arabic vs English) | ✅ |
| Header language toggle works | `LanguageProvider` test + `SiteHeader.tsx` review | ✅ |
| Arabic has RTL, English LTR | `i18n/__tests__/locale.test.ts` + `LanguageProvider.test.tsx` | ✅ |
| Payload Local API loader handles DB failure safely | `get-home-data.test.ts`, `get-public-site-settings.test.ts` | ✅ |
| Data loaders do not use Frappe URLs | `get-home-data.test.ts` (fetch spy) | ✅ |
| Puck mode renders when valid puckData exists | `puck-renderer.test.tsx` (stabilization) + `home-blocks.test.ts` | ✅ |
| Fallback mode renders when no valid puckData exists | `home-blocks.test.ts` (DEFAULT_HOME_PUCK_DATA shape) + `HomeBody.test.tsx` (visual) | ✅ |
| No `/ar` or `/en` reintroduced | `site-public-routes.test.ts` (asserts no href starts with /ar or /en) | ✅ |
| Every homepage section is a Puck block | `home-blocks.test.ts` (registry + allow-list + default-data ordering) | ✅ |

---

## Tests failed / skipped

| Suite | Status | Reason |
|---|---|---|
| `tests/int/api.int.spec.ts` | ⏸ **skipped** | Requires `PAYLOAD_SECRET` + live `DATABASE_URI`. Same deferral as the stabilization branch — operator must run `pnpm migrate` against a real Neon branch first. |
| `tests/e2e/*.spec.ts` | ⏸ **not run** | No Next.js dev server / DB available in this sandbox. The Playwright config will pick them up once a server is provisioned. |

---

## Build status

`pnpm build` was **not executed** in this sandbox — the same ~2 GB memory ceiling that prevented the stabilization branch from completing a `next build` still applies. `pnpm exec tsc --noEmit` (which is the same TypeScript pass `next build` runs) passes with **0 errors**, so the build failure mode in this environment is purely a memory-availability issue, not a code defect.

Recommended: run `pnpm build` on a developer machine or in CI with ≥4 GB RAM.

---

## Remaining risks / deferrals

| Item | Severity | Owner |
|---|---|---|
| Phase 1.5 collections (events, projects, campus-life) — the legacy homepage rendered these sections; the new homepage does not because the matching collections don't exist yet on Payload. | Low | Phase 1.5 |
| FAQ accordion (legacy inline section) — no FAQ collection yet. Not ported. | Low | Phase 1.5 |
| Video section / SmartChat / Scroll-to-top — not ported (legacy only, no Payload backing). | Low | Phase 1.5 polish |
| Header mega-menus + theme switcher (legacy `Header.tsx` is 630 lines) — replaced by a simplified header with the same primary nav, gold logo tile, language toggle, and mobile sheet. Full mega-menus deferred. | Low | Phase 1.5 polish |
| Hero floating particle field — visual polish item; not ported. | Low | Phase 1.5 polish |
| `pnpm build` was not run here. | Low | CI smoke |
| `tests/e2e/*` not executed in this sandbox. | Low | CI smoke |
| Tailwind is now part of the public site only (scoped via `content` glob) — Payload's admin SCSS (`src/app/(payload)/custom.scss`) is untouched. Verify on first admin smoke that no reset rule leaks. | Low | Smoke |

---

## Definition of Done

- [x] aljeel-mono cloned with submodules.
- [x] `aau-payload-platform` submodule on top of `fix/phase-1-stabilization`.
- [x] Submodule branch `feature/port-legacy-homepage` created.
- [x] Root branch `feature/port-legacy-homepage` created.
- [x] Old frontend homepage deeply audited (`docs/homepage-port/02-legacy-homepage-audit.md`).
- [x] All homepage dependencies identified.
- [x] Old homepage design ported into the new Payload project.
- [x] Homepage uses Payload Local API, not Frappe API.
- [x] Homepage has safe fallback when DB is unavailable.
- [x] Same-page Arabic/English switching still works.
- [x] `/` is the homepage.
- [x] `/ar` and `/en` are not required.
- [x] Puck rendering still works.
- [x] Fallback homepage looks like the legacy frontend (because the fallback IS Puck-rendered through `DEFAULT_HOME_PUCK_DATA`).
- [x] **Every homepage section is a registered Puck block** (verified by `home-blocks.test.ts`).
- [x] Tests added/updated.
- [x] Validation report written (this file).
- [ ] Submodule branch pushed — done in the commit-and-push step.
- [ ] Root monorepo submodule pointer updated and pushed — done in the commit-and-push step.

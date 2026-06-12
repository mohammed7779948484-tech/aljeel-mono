# Phase 1.5 Frontend Porting Map

> Scope: source audit and migration map for porting the old `frontend/` Next.js public experience into `aau-payload-platform/`. This is a planning document only; no implementation was performed.

## Executive Summary

The old frontend is a Next.js App Router application under `frontend/`. It is Arabic-first, client-language driven, and heavily componentized around public marketing/content pages. Data comes from Frappe-facing service adapters in `frontend/services/server/` and `frontend/services/data/`; each adapter normalizes API payloads into `*Ar/*En` fields that client components select with `useLanguage()`.

For Phase 1.5, the highest-value port is the public website shell and public content surfaces, not the old admin or portal UIs. The new Payload app already has the right collection/global beginnings: `pages`, `navigation-menus`, `news`, `colleges`, `academic-programs`, `faculty-members`, `site-settings`, `home-page`, `about-page`, and `contact-settings`. The port should move old visual patterns and route behavior into server-first Next.js 15 pages using the Payload Local API, with Puck blocks for composable home/page sections.

## Source Inventory

| Area | Source files inspected | Migration signal |
| --- | --- | --- |
| Routing | `frontend/app/(public)/**/page.tsx`, `frontend/app/(auth)/**/page.tsx`, `frontend/app/admin/**/page.tsx` | 35 public page routes, 4 auth pages, 20 admin pages |
| Layout shell | `frontend/app/layout.tsx`, `frontend/app/(public)/layout.tsx`, `frontend/components/Header.tsx`, `frontend/components/Footer.tsx` | Public shell is fixed header/footer, Arabic default, Tajawal font, dark mode |
| Language | `frontend/contexts/LanguageContext.tsx`, route config, public/admin components | 84 files import `useLanguage`; no URL locale segment |
| Styling | `frontend/app/globals.css`, `frontend/tailwind.config.ts` | HSL CSS variables, black/gold brand, Tajawal, custom animations |
| Public sections | `HeroSection`, `StatsSection`, `CollegesSection`, `NewsSection`, `EventsSection`, `CampusLifeSection`, `ProjectsSection`, `ContactSection`, `UniversityVideoSection`, `SmartChat` | Home page is the main section composition |
| Services | `frontend/services/server/*.ts`, `frontend/services/shared/media-url.ts`, `frontend/types/index.ts` | Frappe adapters normalize media URLs and bilingual fields |
| Target app | `aau-payload-platform/src/payload.config.ts`, `collections/*`, `globals/*` | Payload schema exists but does not yet enable first-class localization |

## Current Route Map

### Public Routes

| Old route | Source page | Main component/service | Phase 1.5 target |
| --- | --- | --- | --- |
| `/` | `app/(public)/page.tsx` | `getHomeData()` -> `HomeContent` | Port now as Payload home renderer with Puck/HomePage globals |
| `/about` | `app/(public)/about/page.tsx` | `getAboutPageData()` -> about client | Port now from `about-page` global |
| `/contact` | `app/(public)/contact/page.tsx` | `getContactPageData()` -> `ContactSection` | Port now from `contact-settings` and `contact-messages` |
| `/colleges` | `app/(public)/colleges/page.tsx` | `getCollegesList()`, `getAcademicProgramsCount()` -> `CollegesPageContent` | Port now from `colleges` and `academic-programs` |
| `/colleges/[id]` | `app/(public)/colleges/[id]/page.tsx` | `getCollegeById()`, `getCollegeFaculty()` -> `CollegeDetailsContent` | Port now, slug-based if possible |
| `/colleges/[id]/programs/[programId]` | nested page | `getProgramByIds()` -> `ProgramDetailsContent` | Port now if academic programs are seeded |
| `/faculty` | `app/(public)/faculty/page.tsx` | `getFacultyList()` -> `FacultyPageContent` | Port now from `faculty-members` |
| `/faculty/[id]` | detail page | `getFacultyById()` -> `FacultyMemberDetailsContent` | Port now if faculty records have stable slugs/ids |
| `/news` | `app/(public)/news/page.tsx` | `getNewsList()` -> `NewsPageContent` | Port now from `news` collection |
| `/news/[slug]` | detail page | `getNewsBySlug()` -> `NewsDetailsView` | Port now from `news.slug` |
| `/admission` | `app/(public)/admission/page.tsx` | `getPublicPage('admission')`, colleges | Port as generic `pages` content if content-only; defer workflow |
| `/search` | `app/(public)/search/page.tsx` | `Search` client and API | Defer until Payload search/index plan |
| `/events`, `/events/[slug]` | event pages | `events` service | Defer unless Events collection is added in Phase 1.5 |
| `/blog`, `/blog/[id]` | blog pages | `blog` service | Defer or fold into `news/pages` decision |
| `/centers`, `/centers/[id]` | centers pages | `centers` service | Defer until Centers collection exists |
| `/campus-life`, `/campus-life/[slug]` | campus pages | `campus-life` service | Defer until Campus Life collection exists |
| `/partners` | partners page | partners service | Defer until Partners collection exists |
| `/offers`, `/offers/[id]` | offers pages | offers service | Defer until Offers collection exists |
| `/projects-studio`, `/projects-studio/[slug]` | projects pages | projects service | Defer until Projects collection exists |
| `/research`, `/research/journal`, `/research/articles`, `/research/articles/[id]` | research pages | research service | Defer; define Research/Journal model first |
| `/links`, `/email-request`, `/e-learning`, `/alumni` | standalone public pages | mostly static/client pages | Port as generic pages after core shell |

### Auth, Portal, Admin Routes

| Route group | Source | Recommendation |
| --- | --- | --- |
| `/login`, `/forgot-password` | `frontend/app/(auth)` | Do not port UI directly. Rebuild around Payload auth/RBAC when auth scope begins. |
| `/student-dashboard`, `/doctor-dashboard` | large client dashboard files | Defer. These depend on portal APIs and local client state, not Payload content. |
| `/admin/**` | old custom admin pages | Do not port. Payload Admin replaces most CRUD screens. Preserve only UX ideas where useful. |

## Layout And Shell Port

### Root Layout

Old `frontend/app/layout.tsx`:

- Loads `Tajawal` from `next/font/google` with weights `200` through `900`, `subsets: ['arabic']`, `variable: '--font-tajawal'`.
- Hard-codes `<html lang="ar" dir="rtl">`.
- Generates Arabic-first metadata and JSON-LD.
- Wraps the app in `Providers`.

Port guidance:

- Keep Tajawal as the primary font for both Arabic and English unless design chooses a Latin companion later.
- Move `lang` and `dir` to a locale-aware layout in the target app.
- Generate metadata per locale and per content record.
- Preserve educational organization JSON-LD but source name, address, logo, and contact data from `site-settings`.

### Public Layout

Old `frontend/app/(public)/layout.tsx` renders:

- fixed `Header`
- `main` with `pt-20`
- `Footer`

Port guidance:

- Rebuild the shell in `aau-payload-platform/src/app/(frontend)` rather than copying the old 33 KB header.
- Fetch `navigation-menus` and `site-settings` server-side.
- Keep the fixed-header offset behavior, but expose a single shell layout that accepts locale.
- Keep dark mode only if the product still wants it; old design tokens support it.

## Header/Footer Findings

### Header

`frontend/components/Header.tsx` is highly coupled to old behavior:

- Hard-coded nav labels in `config/routes.ts`.
- Hard-coded mega-menu items for colleges, centers, campus life, and research.
- Uses `usePathname`, `useRouter`, hover state, scroll state, `framer-motion`, `next-themes`.
- Uses `language === 'ar'` for menu alignment, sheet side, arrow direction, and hover motion.
- Language toggle mutates client context only; it does not navigate to a locale URL.
- Login routes to `/login`.

Port guidance:

- Rebuild as smaller components: `SiteHeader`, `DesktopNav`, `MobileNav`, `LanguageSwitcher`, `ThemeToggle`.
- Drive main nav, footer nav, and mega-menu group metadata from `navigation-menus`.
- Keep college/faculty/news links dynamic from collections only where needed; avoid hard-coded college slugs.
- Language switcher should preserve the current pathname and move between locale-prefixed routes.
- Active route checks must normalize locale prefix before matching.

### Footer

`frontend/components/Footer.tsx` is mostly presentational:

- Uses hard-coded quick links and social URLs.
- Uses `new Date().getFullYear()` client-side.
- Contains animated decorative background and map/contact cards.
- Reads language only for labels and hover direction.

Port guidance:

- Source brand, address, contacts, copyright, logo, and social links from `site-settings`.
- Source quick links from `navigation-menus` with key `footer`.
- Render as a server component where possible; keep animation as optional client islands.

## Design System Port

Preserve:

- HSL token strategy in `frontend/app/globals.css`.
- Brand palette: black/white with gold accent (`--gold: 45 82% 58%`).
- `--radius: 0.75rem` final value.
- Tajawal font variable and Tailwind font families.
- Tailwind container: centered, `2rem` padding, `2xl: 1400px`.
- Dark mode token set if dark mode remains in scope.
- Animation names: `fade-in`, `fade-in-up`, `slide-in-left`, `slide-in-right`, `scale-in`, `shimmer`.
- Utility concepts: `.text-gold`, `.hero-overlay`, `.text-shadow*`, `.glow-gold*`, mobile menu scrollbar, college stat cards.

Do not blindly preserve:

- Decorative blur circles and dense motion everywhere; use animation selectively.
- Duplicate `--radius` declaration.
- Body font fallback that references literal `'Tajawal'` while the app uses `next/font` variable.
- Old global styles tied to legacy component class names unless those components are ported.

## Data Layer Port

### Old Pattern

Public pages generally use this split:

1. Server page calls a `frontend/services/server/*` adapter.
2. Adapter fetches Frappe endpoints with `cache: 'no-store'`.
3. Adapter unwraps several possible response envelopes.
4. Adapter maps values into `*Ar/*En`, resolves media URLs, and falls back from English to Arabic.
5. Client component selects the display language with `useLanguage()`.

Examples:

- `getHomeData()` combines home CMS, offers, and campus-life data with `Promise.all`.
- `getCollegesList()` and `getCollegeById()` map college/program/faculty records.
- `getNewsList()` encodes unsafe slugs using base64url prefix `n-`.
- `resolveMediaUrl()` normalizes `/assets`, `/files`, `/private/files`, protocol-relative URLs, data URLs, and blob URLs.

### Target Pattern

Use Payload Local API in server components and route handlers:

- `getPayload({ config })`
- `payload.find(...)` for collections
- `payload.findGlobal(...)` for globals
- `locale` on Payload queries after localization is enabled
- `depth` and `select` to control relationship payload size
- `where: { isPublished: { equals: true } }` or drafts-aware publication logic

Recommended helper layer:

| Helper | Purpose |
| --- | --- |
| `getLocaleFromParams(params)` | Validate and normalize `ar`/`en` |
| `getDir(locale)` | `ar -> rtl`, `en -> ltr` |
| `localizedPath(locale, href)` | Generate locale-prefixed links |
| `stripLocale(pathname)` | Active-nav comparison |
| `getPublishedDocs(collection, locale, options)` | Reusable Payload query wrapper |
| `resolvePayloadMedia(media)` | Convert Payload upload relation to URL/alt/size |

## Component Porting Map

### Port Now

| Old component | Target form | Notes |
| --- | --- | --- |
| `HeroSection` | Puck `HeroBlock` or home hero component | Keep background image/video, CTA, quick links; remove client-only random particles or guard carefully |
| `StatsSection` | Puck `StatsBlock` | Map stat array to configurable labels/counts/icons |
| `CollegesSection` | Puck/collection-driven `CollegesGridBlock` | Source from Payload `colleges`; button arrows locale-aware |
| `NewsSection` | Puck/collection-driven `NewsGridBlock` | Source from Payload `news`; use localized dates |
| `ContactSection` | `ContactBlock` plus contact form | Source labels/settings from `contact-settings`; submit to Payload contact route |
| `HomeContent` | Payload/Puck page renderer | Replace hard-coded composition with configured block order |
| `Header` | Rebuilt shell components | Use Payload nav, not hard-coded arrays |
| `Footer` | Rebuilt shell components | Use Payload globals/nav |
| `About` page components | Template sections | Use `about-page` global |
| `CollegeDetailsContent` | Collection detail template | Must handle programs, faculty, rich text, and media |
| `ProgramDetailsContent` | Collection detail template | Depends on academic programs relationship model |
| `FacultyPageContent`, `FacultyMemberDetailsContent` | Collection list/detail templates | Add slug if current id is not stable |
| `NewsPageContent`, detail views | Collection list/detail templates | Use `news.slug` and SEO plugin metadata |

### Defer

| Old component/area | Reason |
| --- | --- |
| `SmartChat` | Separate AI/search settings and indexing scope |
| `AdmissionSection` full workflow | Large form/workflow area, not just content |
| `Events*`, `CampusLife*`, `Centers*`, `Offers*`, `Partners*`, `Projects*`, `Blog*`, `Research*` | Target Payload collections do not yet exist |
| Old admin pages | Replaced by Payload Admin |
| Student/doctor dashboards | Portal APIs and auth model need separate design |

## Puck/Page Builder Boundary

Use Puck for editorial layout sections, not for every collection detail template.

Recommended Puck blocks for Phase 1.5:

- `HeroBlock`
- `StatsBlock`
- `RichTextBlock`
- `CollegesGridBlock`
- `NewsGridBlock`
- `FacultyGridBlock`
- `CTASection`
- `ContactBlock`
- `VideoBlock`
- `FAQBlock`

Keep these as coded templates:

- College detail
- Program detail
- Faculty member detail
- News article detail
- Search results
- Auth and portal pages

## Media And Assets

Old assets live under:

- `frontend/public/assets/*`
- `frontend/public/images/campus/*`
- remote Frappe media from `edu.yemenfrappe.com`
- remote Unsplash fallbacks

Port guidance:

- Move durable brand/campus assets into Payload `media` records.
- Replace Unsplash fallbacks with managed media or explicit empty states.
- Preserve `alt` text per locale where content records support it.
- Configure `next/image` remote patterns for Payload media host and any transitional source host.

## SEO And Metadata

Old root metadata is Arabic-first and mostly static. Detail pages generate metadata from English fields in some places (`titleEn`) and Arabic fields in others.

Target requirements:

- Per-locale metadata.
- Locale alternates for every routable page.
- Canonical URL should include default-locale policy consistently.
- Open Graph locale should be `ar_YE` or agreed institution locale, not the old mixed `ar_IQ`.
- JSON-LD should be generated from `site-settings`.
- News, college, faculty, and page metadata should use localized fields with fallback.

## Phase 1.5 Work Packages

1. Establish locale-aware frontend routing in the new app.
2. Port design tokens, Tajawal, and minimal global utilities.
3. Build Payload data helpers for pages, globals, navigation, media, and collections.
4. Rebuild public shell from `navigation-menus` and `site-settings`.
5. Implement core Puck blocks and a home/page renderer.
6. Implement collection templates for colleges, programs, faculty, and news.
7. Wire contact form to Payload `contact-messages`.
8. Add metadata, sitemap, robots, and alternate URLs.
9. Run bilingual visual QA for Arabic RTL and English LTR.

## Risks And Migration Notes

| Risk | Evidence | Mitigation |
| --- | --- | --- |
| Language is client-only in old app | `LanguageContext` mutates document `dir/lang`; URLs do not change | Use URL locale segments in target |
| Duplicate bilingual fields are everywhere | 101 files contain bilingual field names; 584 matched references | Move to Payload localization or a deliberate transitional mapper |
| Header is too large to copy safely | `Header.tsx` combines nav, mega menus, theme, language, scroll, motion | Rebuild in smaller pieces |
| Old English content often falls back to Arabic | Service maps use `titleEn || titleAr` patterns | Add editorial completeness checks |
| Admin FAQ has mojibake Arabic strings | `frontend/app/admin/faq/page.tsx` contains corrupted Arabic text | Do not port those literals; restore from source content or retranslate |
| Some old slugs are unsafe/non-Latin | `news.ts` base64url encodes unsafe slugs | Define a stable slug policy per collection |
| Services use `cache: 'no-store'` everywhere | Frappe adapters avoid caching | Use Payload drafts/revalidation strategy instead |
| Public UI depends on many client components | Motion, `useRouter`, `useLanguage`, forms | Prefer server components with focused client islands |

## Acceptance Checklist

- Public routes render in Arabic and English with correct `lang` and `dir`.
- Header and footer are content-managed, not hard-coded.
- Home page can be reordered/edited via Payload/Puck where intended.
- Core collection list/detail pages use Payload Local API and published filters.
- Tajawal and HSL tokens match the old brand closely.
- Arabic and English metadata, canonical URLs, and alternate links are present.
- No old Frappe service adapter remains in the new public path after cutover.
- No corrupted Arabic literals are carried into the new app.

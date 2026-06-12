# 02 — Legacy Homepage Audit

**Source:** `frontend/` (legacy Next.js 16 app)  
**Target:** `aau-payload-platform/` (new Payload + Next.js 15 platform, branch `feature/port-legacy-homepage`)

---

## 1. Exact homepage route

- **Route:** `frontend/app/(public)/page.tsx` (21 lines)
- **Public layout:** `frontend/app/(public)/layout.tsx` — wraps every public route with `<Header /> + <main className="pt-20"> + <Footer />`.
- **Render shape:** Server Component → `await getHomeData()` (Frappe API) → renders `<HomeContent ... />` (Client Component) and passes pre-fetched data as props.

The page itself is light; the real homepage lives inside `components/HomeContent.tsx` which composes 9 sections.

---

## 2. Components imported by the homepage (dependency tree)

```
app/(public)/page.tsx
└── components/HomeContent.tsx                 ('use client')
    ├── components/HeroSection.tsx             ('use client', framer-motion, lucide-react, next/image, Dialog)
    ├── components/StatsSection.tsx            ('use client', framer-motion, lucide-react)
    ├── components/CampusLifeSection.tsx       (dynamic, lazy below the fold)
    ├── components/ProjectsSection.tsx         (dynamic)
    ├── components/CollegesSection.tsx         (dynamic)
    ├── components/NewsSection.tsx             (dynamic)
    ├── components/EventsSection.tsx           (dynamic)
    ├── components/UniversityVideoSection.tsx
    ├── (inline FAQ section using @/components/ui/accordion)
    ├── components/ContactSection.tsx          (dynamic, ssr:false)
    ├── components/ScrollToTop.tsx             (dynamic)
    └── components/SmartChat.tsx               (dynamic, ssr:false)

Public layout:
app/(public)/layout.tsx
├── components/Header.tsx                       (630 lines — mega-menus, theme switch, mobile sheet)
└── components/Footer.tsx                       (245 lines — quick links, social, animated)
```

`Header.tsx` also depends on:
- `@/config/routes.ts` (the public nav inventory, in Ar/En pairs)
- `@/contexts/LanguageContext.tsx` for `t()` and `language`
- `next-themes` for dark mode

---

## 3. CSS used by the homepage

- `frontend/app/globals.css` (307 lines) — Tailwind directives, design tokens (`--gold`, `--primary`, `--secondary`, `--shadow-gold`), Tajawal font wiring, a few custom utilities (`.text-gold`, `.glow-gold`, `.college-stat-card`, `.mobile-menu-scroll`).
- `frontend/tailwind.config.ts` — semantic tokens map to `hsl(var(--...))`. Font family `display`/`body`/`sans` all map to `var(--font-tajawal)`. Keyframes for `fade-in`, `fade-in-up`, `slide-in-*`, `scale-in`, `shimmer`. Plugin: `tailwindcss-animate`.
- Per-component styling is **all Tailwind utility classes** + framer-motion animations. No CSS-Modules.

Design tokens to preserve (port to the new project):
- `--secondary: 45 82% 58%` (the AAU gold)
- `--gold: 45 82% 58%`
- `--primary: 0 0% 0%` (black)
- `--background: 0 0% 100%` (white)
- `--radius: 0.75rem`
- `--shadow-gold`, `--shadow-elegant`, `--shadow-glow`
- Font: **Tajawal** for all text (display + body).

Visual identity = **black + gold** with Tajawal Arabic-first typography.

---

## 4. Animation libraries

- `framer-motion` ^12 (the entire homepage is animated — staggered children, parallax, `whileInView`, scroll-linked `useScroll`/`useTransform`, floating particles in hero, glow blobs)
- Tailwind keyframes in `tailwind.config.ts` (`fade-in`, `slide-in-left`, `slide-in-right`, `scale-in`)
- `tailwindcss-animate`

The Puck homepage in the new project does NOT need full framer-motion at first. We'll preserve the visual feel (gradients, shadows, gold accents, card hovers) with CSS transitions and a small amount of motion. Adding the full particle system is a Phase 1.5 polish item — listed in "what to defer".

---

## 5. Data services and API calls used by the homepage

| File | Calls |
|---|---|
| `services/server/home.ts` | `GET https://edu.yemenfrappe.com/api/method/aau_university.api.v1.public.get_home` (Frappe REST) |
| `services/server/colleges.ts` | Frappe `…public.get_colleges` |
| `services/server/news.ts` | Frappe `…public.get_news_list` |
| `services/server/events.ts` | Frappe `…public.get_events_list` |
| `services/server/projects.ts` | Frappe `…public.get_projects_list` |
| `services/server/campus-life.ts` | Frappe `…public.get_campus_life_list` |
| `services/server/offers.ts` | Frappe `…public.get_offers_list` |

**All of these are out of scope to port.** The new platform replaces every Frappe call with the **Payload Local API** (`getPayload({ config })`).

---

## 6. Sections in render order (HomeContent)

1. `<HeroSection />` — full-screen hero with image, badge, animated title (gradient + gold), tagline, description, primary CTA "Apply Now", three quick-access cards (Colleges / Student Results external link / Academic Calendar dialog), floating particles + glow blobs, scroll-indicator.
2. `<StatsSection />` — 4-up grid of stat cards on a gold-tinted background.
3. `<CampusLifeSection />` — campus-life gallery.
4. `<ProjectsSection />` — featured projects.
5. `<CollegesSection />` — 2-up grid of college cards with hero image, dean name, programs count, "Explore College" button. Header pill + gradient heading.
6. `<NewsSection />` — 2-up news cards with date, category badge, read-time, "Read More".
7. `<EventsSection />` — upcoming events.
8. `<UniversityVideoSection />` — video block with overlay text.
9. FAQ accordion (inline in HomeContent.tsx, uses radix accordion).
10. `<ContactSection />` — contact form + map.
11. `<SmartChat />` — floating AI assistant.
12. `<ScrollToTop />` — fixed scroll-to-top button.

---

## 7. Visual description

- Above-the-fold hero is **dark cinematic** — full-screen background photo with two overlay gradients (top fade, bottom fade), animated floating gold particles, two large blur orbs (primary + secondary), centered headline using a gold-white-gold text gradient, secondary line in pure gold uppercase, large rounded CTA in gold-on-black, three glass quick-cards below.
- Stats card row sits on a slightly-translucent gold panel (`rgba(210,170,50,0.35)`), each card rotates its icon on hover.
- Body sections use a consistent pattern:
  - Centered pill badge (icon + label, gold-tinted on light bg)
  - Large 3xl/4xl/5xl heading with `bg-gradient-to-r from-primary via-secondary to-primary bg-clip-text text-transparent`
  - Short description
  - 2-column responsive card grid
  - "View All" button at the bottom
- Footer: solid black background, gold logo tile, 4-column grid (about, quick links, contact, social), animated background blur blobs in gold.
- Buttons: gold solid for primary CTAs (`bg-secondary text-primary`), outline for secondary, rounded-xl/2xl corners, optional arrow icon that animates on hover (direction flips with RTL).
- Spacing: generous (`py-16 md:py-20`, `mb-10 md:mb-12`).
- Cards: `rounded-2xl`, soft border, shadow-lg, lift on hover (`y: -10`).

---

## 8. Bilingual behavior

- One client provider — `contexts/LanguageContext.tsx` — exposes `language`, `toggleLanguage`, and `t(ar, en)`.
- **Same-URL switch.** No `/ar`, no `/en` in the legacy frontend either.
- `useEffect` sets `document.documentElement.dir` and `lang` on every language change.
- Default language is `ar`.
- Every component reads `t('Arabic string', 'English string')` inline — both texts ship in the JS bundle.
- Header has a globe icon + toggle, footer/CTAs use directional arrows that flip (`ArrowLeft` for `ar`, `ArrowRight` for `en`).

**Match with the new platform:** the new `LanguageProvider` already does the same thing with a stronger contract (`t(key: DictKey)` instead of `t(ar, en)` for static UI, plus `pickLocalized(doc, baseName, locale)` for Payload documents). We port the components but **rewrite the `t()` call sites** to use the new dictionary-based pattern.

---

## 9. Responsive behavior to preserve

- Hero: 1-column layout, type scales from `text-4xl` (mobile) → `text-8xl` (lg). Quick-access grid collapses 3 → 1 column.
- Stats: `grid-cols-1 sm:grid-cols-2 md:grid-cols-4`.
- Sections (Colleges, News, Events, Projects, Campus Life): `grid md:grid-cols-2`.
- Footer: `grid md:grid-cols-4` (logo block spans 2 columns).
- All headings/sub-headings use `sm:` and `md:` breakpoints; padding uses `py-16 md:py-20`.

---

## 10. Files to PORT DIRECTLY (visual + structural, with `t()` adapter)

| Legacy file | New location |
|---|---|
| `components/HeroSection.tsx` | `aau-payload-platform/src/components/home/HeroSection.tsx` (refactored — no `useRouter`, no `Dialog`, no particles; preserve gradient title + CTA + quick-cards) |
| `components/StatsSection.tsx` | `aau-payload-platform/src/components/home/StatsSection.tsx` (port directly, drive from props) |
| `components/CollegesSection.tsx` | `aau-payload-platform/src/components/home/CollegesSection.tsx` (drive from Payload `colleges`, drop the placeholder Unsplash fallback path) |
| `components/NewsSection.tsx` | `aau-payload-platform/src/components/home/NewsSection.tsx` (drive from Payload `news`, fix the legacy `publishedAt` vs `publishDate` discrepancy) |
| `components/Footer.tsx` | `aau-payload-platform/src/components/site/SiteFooter.tsx` (port with simplified animations) |
| `components/Header.tsx` | `aau-payload-platform/src/components/site/SiteHeader.tsx` (simplified — keep brand block, top-level nav, language toggle; drop mega-menus and theme switcher for this port) |
| `app/globals.css` (tokens + base + a few utilities) | merge selected portions into `aau-payload-platform/src/app/(frontend)/styles.css` |
| `tailwind.config.ts` (tokens + keyframes) | add Tailwind to the new project; copy token wiring and keyframes |

---

## 11. Files to REFACTOR (port the design, change the data source)

| Legacy | Replacement |
|---|---|
| `services/server/home.ts` (Frappe) | `src/lib/payload/get-home-data.ts` — uses `payload.find` / `payload.findGlobal` |
| `services/server/colleges.ts` (Frappe) | `src/lib/payload/get-colleges.ts` — `payload.find({ collection: 'colleges' })` |
| `services/server/news.ts` (Frappe) | `src/lib/payload/get-news.ts` — `payload.find({ collection: 'news', sort: '-publishDate' })` |
| Inline FAQ in `HomeContent.tsx` | (deferred — no FAQ collection exists yet in the new Payload schema; render only when data is present) |
| `config/routes.ts` | `src/lib/site/public-routes.ts` — kept as a typed constant, eventually backed by `navigation-menus` global |
| `contexts/LanguageContext.tsx` | already replaced by `src/i18n/LanguageProvider.tsx` (stronger contract) |

---

## 12. Files NOT to copy

| Legacy | Why |
|---|---|
| `services/server/*.ts` (all Frappe callers) | Hard-coded `edu.yemenfrappe.com`, replaced by Payload Local API |
| `app/api/home/route.ts`, `app/api/*` (data routes) | Public reads go through Local API, not custom routes |
| `components/SmartChat.tsx` | Phase 1.5 — uses Frappe APIs and external LLM keys; out of scope |
| `components/ScrollToTop.tsx` | Optional polish; defer |
| `components/UniversityVideoSection.tsx` | Optional; defer until the `home-page` global has video fields wired up |
| `components/EventsSection.tsx`, `ProjectsSection.tsx`, `CampusLifeSection.tsx`, `OffersSection.tsx`, `PartnersSection.tsx` | No matching collections in the new Payload schema yet (events/projects/campus-life are Phase 1.5) |
| Full `Header.tsx` with mega-menus | Out of scope for this port; we render a simplified header that preserves the brand and primary nav |
| `next-themes` dark mode toggle | Defer; the new project ships with a single light theme that matches the legacy default |
| `assets/hero-new.jpg` (legacy) | We reference the same image path via Payload Media (or a static asset placed under `public/`) |

---

## 13. Design tokens to preserve in the new project

```css
/* Identity colors (HSL channels for shadcn compatibility) */
--background: 0 0% 100%;
--foreground: 0 0% 0%;
--primary: 0 0% 0%;
--primary-foreground: 0 0% 100%;
--secondary: 45 82% 58%;          /* AAU gold */
--secondary-foreground: 0 0% 0%;
--muted: 0 0% 96%;
--muted-foreground: 0 0% 45%;
--card: 0 0% 98%;
--card-foreground: 0 0% 0%;
--border: 0 0% 89%;
--radius: 0.75rem;
--gold: 45 82% 58%;
--gold-light: 45 90% 70%;
--gold-dark: 45 70% 45%;

/* Gradients */
--gradient-gold: linear-gradient(135deg, hsl(45 82% 58%), hsl(45 90% 70%));
--gradient-hero: linear-gradient(135deg, hsl(0 0% 0% / 0.9), hsl(0 0% 0% / 0.7));

/* Shadows */
--shadow-gold: 0 10px 40px -10px hsl(45 82% 58% / 0.3);
--shadow-elegant: 0 20px 60px -15px hsl(0 0% 0% / 0.2);
--shadow-glow: 0 0 40px hsl(45 82% 58% / 0.4);
```

Font: `Tajawal` for every text style (loaded via Google Fonts in the layout).

---

## 14. Responsive + a11y notes to preserve

- All headings use semantic `<h1>`/`<h2>`.
- Buttons are real `<button>` elements (not `<div onClick>`).
- Each section has an `id` for in-page navigation (`#hero`, `#colleges`, `#news`, `#contact`, …).
- Card images use real `alt` text driven by the bilingual title.
- Language toggle has `aria-label` describing the destination language.
- RTL is set globally via `dir="rtl"` on `<html>`; arrows are direction-aware.
- The hero CTA, quick-access cards, and section "View All" buttons must remain keyboard-focusable.

---

## 15. Port plan summary

We will port **8 sections** in this branch (Hero, Stats, Colleges, News, AdmissionCTA, ContactCTA, plus Header & Footer). Each comes from the legacy file with the visual intent intact, but with:

- the new dictionary-based `t()`/`pickLocalized`,
- data sourced from Payload Local API or safe fallback,
- a Server Component shell calling tiny client subcomponents only where interactivity is needed,
- Tailwind config + design tokens added to the new project (the previous stabilization branch only had hand-written CSS classes — we add a proper Tailwind setup),
- `framer-motion` left in (already used by Puck), but only the simple `whileInView` / `whileHover` patterns the legacy code uses for cards; the hero's particle field is intentionally NOT ported in this PR (visual polish item).

Once a Payload `home-page` global value exists, the hero/section copy comes from CMS via `pickLocalized`. When `home-page` is empty (dev / fresh DB), the dictionary defaults kick in and the homepage still looks like the legacy hero.

**Order of implementation:** Tailwind setup → global tokens & utilities → site shell (header/footer) → Hero → Stats → Colleges → News → AdmissionCTA → ContactCTA → wiring the homepage Server Component with data loaders → Puck integration kept intact.

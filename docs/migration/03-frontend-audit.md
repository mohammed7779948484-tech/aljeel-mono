# Frontend Audit — Phase E (Subagent 5)

> Inspection of the old Next.js frontend to identify components, routes, API calls, and reuse opportunities.

## Frontend Stack

| Technology | Version | Notes |
|-----------|---------|-------|
| Next.js | 16.1.3 | App Router |
| React | 18.3.1 | Will be React 19 in new project |
| TypeScript | 5.8.3 | Strict mode |
| Tailwind CSS | 3.4.17 | HSL design tokens via CSS variables |
| shadcn/ui | Radix-based | Accordion, Dialog, Dropdown, Tabs, Toast, etc. |
| framer-motion | 12.23.26 | Page animations |
| @tanstack/react-query | 5.83.0 | Server state management |
| Recharts | 2.15.4 | Dashboard charts |
| Lucide React | 0.462.0 | Icons |
| embla-carousel | 8.6.0 | Carousels |
| react-hook-form + zod | Latest | Form validation |

## Package Manager

npm (package-lock.json present, 330KB)

---

## Route Map

### Public Routes — `app/(public)/`

| Route | Path | Component | API Endpoint | Phase |
|-------|------|-----------|-------------|-------|
| Home | `/` | HomeContent | `GET /aau/home` | Phase 1 |
| About | `/about` | AboutSection | `GET /aau/about` | Phase 1 (via global) |
| Admission | `/admission` | AdmissionSection | academic APIs | Phase 2 |
| Alumni | `/alumni` | — | — | Phase 2 |
| Blog | `/blog` | BlogPageContent | `GET /blog` | Phase 2 |
| Blog Detail | `/blog/[slug]` | BlogDetailsContent | `GET /blog/:id` | Phase 2 |
| Campus Life | `/campus-life` | CampusLifePageContent | `GET /campus-life` | Phase 2 |
| Centers | `/centers` | CentersPageContent | `GET /centers` | Phase 2 |
| Colleges | `/colleges` | CollegesPageContent | `GET /colleges` | Phase 1 |
| College Detail | `/colleges/[slug]` | CollegeDetailsContent | `GET /colleges/:slug` | Phase 1 |
| Contact | `/contact` | ContactSection | `GET /aau/contact` | Phase 1 (global) |
| E-Learning | `/e-learning` | — | — | Phase 2 |
| Email Request | `/email-request` | — | `POST /email-requests` | Phase 2 |
| Events | `/events` | EventsPageContent | `GET /events` | Phase 2 |
| Faculty | `/faculty` | FacultyPageContent | `GET /faculty` | Phase 1 |
| Faculty Detail | `/faculty/[id]` | FacultyMemberDetailsContent | `GET /faculty/:id` | Phase 1 |
| Links | `/links` | — | — | Phase 2 |
| News | `/news` | NewsPageContent | `GET /news` | Phase 1 |
| News Detail | `/news/[slug]` | ArticleDetailsContent | `GET /news/:slug` | Phase 1 |
| Offers | `/offers` | OffersPageContent | `GET /offers` | Phase 2 |
| Partners | `/partners` | PartnersPageContent | `GET /partners` | Phase 2 |
| Projects | `/projects-studio` | ProjectsStudioPageContent | `GET /projects` | Phase 2 |
| Research | `/research` | — | `GET /research-publications` | Phase 2 |
| Search | `/search` | — | `GET /search` | Phase 2 |

### Auth Routes — `app/(auth)/`

Authentication pages (login, etc.) — Phase 2

### Admin Routes — `app/admin/`

Dashboard pages — Phase 2 (Refine replacement)

---

## Component Map

### Page-Level Components (42 files)

| Component | Size | Becomes | Phase |
|-----------|------|---------|-------|
| **HeroSection** | 14KB | **Puck HeroBlock** | Phase 1 |
| **StatsSection** | 4KB | **Puck StatsBlock** | Phase 1 |
| **CollegesSection** | 8KB | **Puck CollegesGridBlock** | Phase 1 |
| **NewsSection** | 9KB | **Puck NewsGridBlock** | Phase 1 |
| **EventsSection** | 9KB | Puck EventsBlock | Phase 2 |
| **ContactSection** | 17KB | **Puck CTASection** | Phase 1 |
| **PartnersSection** | 8KB | Puck PartnersBlock | Phase 2 |
| **ProjectsSection** | 10KB | Puck ProjectsBlock | Phase 2 |
| **CampusLifeSection** | 9KB | Puck CampusLifeBlock | Phase 2 |
| **CentersSection** | 9KB | Puck CentersBlock | Phase 2 |
| **OffersSection** | 10KB | Puck OffersBlock | Phase 2 |
| **AboutSection** | 18KB | Template-driven (global data) | Phase 1 |
| **AdmissionSection** | 49KB | Template-driven | Phase 2 |
| **HomeContent** | 5KB | Puck page composition | Phase 1 |
| **Header** | 33KB | Layout component (reuse patterns) | Phase 1 |
| **Footer** | 11KB | Layout component (reuse patterns) | Phase 1 |
| **SmartChat** | 20KB | Custom component | Phase 2 |

### Detail Page Components

| Component | Size | Type | Phase |
|-----------|------|------|-------|
| CollegeDetailsContent | 35KB | Template-driven (collection data) | Phase 1 |
| ProgramDetailsContent | 43KB | Template-driven (collection data) | Phase 1 |
| FacultyMemberDetailsContent | 30KB | Template-driven (collection data) | Phase 1 |
| ArticleDetailsContent (News) | 11KB | Template-driven | Phase 1 |
| BlogDetailsContent | 11KB | Template-driven | Phase 2 |
| CampusLifeDetailsContent | 12KB | Template-driven | Phase 2 |
| CenterDetailsContent | 14KB | Template-driven | Phase 2 |
| OfferDetailsContent | 18KB | Template-driven | Phase 2 |
| ProjectDetailsContent | 10KB | Template-driven | Phase 2 |

### Shared Components — `components/common/`

Utility components for cards, loaders, pagination — reuse patterns

### UI Components — `components/ui/`

shadcn/ui primitives — will be recreated via shadcn CLI in new project

### Dashboard Components — `components/dashboard/`

Admin dashboard — Phase 2 (Refine)

---

## Design System & Tokens

### CSS Variables (from `globals.css` and `tailwind.config.ts`)

```css
/* HSL-based color tokens */
--background, --foreground
--primary, --primary-foreground
--secondary, --secondary-foreground
--muted, --muted-foreground
--accent, --accent-foreground
--destructive, --destructive-foreground
--border, --input, --ring
--card, --card-foreground
--popover, --popover-foreground
--sidebar-*, (multiple sidebar tokens)
--radius
```

### Typography

- Font: **Tajawal** (Arabic-first, Google Fonts)
- CSS variable: `--font-tajawal`
- Applied to: display, body, arabic, sans font families

### Animations

```
accordion-down/up, fade-in, fade-in-up,
slide-in-left, slide-in-right, scale-in, shimmer
```

### Design Decisions to Preserve

1. HSL-based color system via CSS variables
2. Tajawal font family for Arabic/bilingual support
3. Custom animation keyframes
4. shadcn/ui component patterns
5. RTL-aware layout patterns
6. Dark mode support via `class` strategy

---

## API Usage Map

### Data Fetching Pattern

The frontend uses `@tanstack/react-query` with service functions in `services/` directory. API calls go to:
- Base URL from environment variable
- Frappe REST API format: `/api/v1/aau/...` and `/api/v1/...`

### Environment Variables

```
NEXT_PUBLIC_API_URL (Frappe backend URL)
NEXT_PUBLIC_SITE_URL
```

---

## Student Portal Pages

Found in portal API routes but minimal frontend implementation visible:
- Student profile, courses, schedule, grades, finance
- All Phase 2

## Doctor Portal Pages

Found in portal API routes:
- Doctor profile, courses, students, grades, schedule
- All Phase 2

## Admin/Dashboard Pages

Found in `app/admin/` and `components/dashboard/`:
- Dashboard with charts (Recharts)
- All Phase 2 (Refine replacement)

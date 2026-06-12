# Current State Re-audit

Date: 2026-06-11

Scope: terminal-only inspection of the monorepo, migration docs, `.gitmodules`, legacy Frappe backend, legacy Next.js frontend, and current `aau-payload-platform` submodule. No code implementation was performed.

## Repo Structure

```text
aljeel/
├── aau_university/          # legacy Frappe backend, audit/reference source of truth
├── frontend/                # legacy Next.js frontend, UI/UX reuse source of truth
├── docs/                    # migration, research, architecture docs
├── aau-payload-platform/    # new Payload CMS + Next.js app as git submodule
├── README.md
├── database-documentation.md
├── PLUGINS_ECOSYSTEM.md
└── aau_initial_roles_permissions_payload_refine_ar.md
```

Parent worktree note: `git status --short` showed unrelated untracked skill files under `.agents/skills/`. They were not touched.

## Submodule

`.gitmodules` declares one submodule:

| Path | URL | Observed commit |
|---|---|---|
| `aau-payload-platform` | `https://github.com/mohammed7779948484-tech/aau-payload-platform.git` | `4693afa117bf17e277e5be51e734da3a1541dbd5` on `heads/main` |

The submodule worktree was clean before this report.

## What The Docs Say Should Exist

Migration docs define Phase 1 as:

- 11 Payload collections: `users`, `media`, `pages`, `navigation-menus`, `news`, `colleges`, `academic-departments`, `academic-programs`, `faculty-members`, `contact-messages`, `join-requests`.
- 5 Payload globals: `home-page`, `about-page`, `site-settings`, `contact-settings`, `smart-chat-settings`.
- Puck visual editing for `pages`, including `payload-puck`, layout JSON, admin editor route/button, and about 10 blocks: `HeroBlock`, `RichTextBlock`, `CTASection`, `StatsBlock`, `FeatureCardsBlock`, `ImageGalleryBlock`, `NewsGridBlock`, `ProgramsGridBlock`, `CollegesGridBlock`, `FAQBlock`.
- Payload Postgres on Neon, local filesystem uploads for Phase 1, SEO plugin, redirects plugin, generated Payload types, migration commands, and deployment env vars.
- RBAC roles: `super_admin`, `admin`, `content_manager`, `coordinator`, `student`, `doctor`.
- RBAC helpers under `src/payload/access/*` per docs, with role hierarchy, user role creation limits, coordinator college scoping, admin access blocking for students/doctors, and a `beforeChange` escalation-prevention hook on users.
- Frontend reuse from legacy `frontend/`: HSL CSS tokens, Tajawal font, Tailwind config, shadcn/Radix primitives, animations, RTL-aware patterns, Header/Footer rebuilt against Payload globals, public Phase 1 routes and detail pages rebuilt against Payload APIs.
- Bilingual schema style with explicit Arabic/English field pairs, based on final Frappe patches that moved content from JSON toward structured bilingual fields.

## Legacy Backend Reality

`aau_university/` still matches the migration docs as the source-of-truth reference:

- `aau_university/aau/doctype/` contains 47 DocType folders.
- `aau_university/api/v1/` contains 11 modules, including large `public.py`, `portal.py`, `access.py`, `academic.py`, `content.py`, `smartchat.py`, and smaller CMS/routing utilities.
- `aau_university/patches.txt` registers 14 post-model-sync patches from `v1_0` through `v1_13`.
- Notable patches still support the migration direction: `v1_5_home_page_bilingual_fields`, `v1_7_content_manager_access`, `v1_8_about_university_single_cms`, `v1_10_normalize_cms_content_doctypes`, and `v1_11_normalize_blog_and_faculty_cms`.
- `hooks.py` still contains college permission hooks and request CORS/routing hooks, which support the documented coordinator/college-scope RBAC mapping.

## Legacy Frontend Reality

`frontend/` remains a full Next.js 16 / React 18 application using npm, Tailwind 3.4, shadcn/Radix primitives, framer-motion, TanStack Query, Recharts, Lucide, Zod, and React Hook Form.

High-level reusable assets still exist:

- Public route groups under `frontend/app/(public)/` for home, about, colleges, college detail, contact, faculty, faculty detail, news, news detail, and many Phase 2 routes.
- Admin/dashboard routes under `frontend/app/admin/`.
- Auth/portal routes under `frontend/app/(auth)/`.
- Large reusable components including `HeroSection`, `StatsSection`, `CollegesSection`, `NewsSection`, `ContactSection`, `Header`, `Footer`, `AboutSection`, `CollegeDetailsContent`, `ProgramDetailsContent`, `FacultyMemberDetailsContent`, and `ArticleDetailsContent`.
- `frontend/tailwind.config.ts` contains the HSL color-token system, Tajawal font families, animation keyframes, container config, radius tokens, and `tailwindcss-animate`.
- `frontend/components/ui/` contains the shadcn/Radix primitive set.

## Current `aau-payload-platform` Reality

Implemented and registered:

- Payload v3 + Next.js 15 + React 19 app with pnpm.
- Postgres adapter using `DATABASE_URI`.
- `@payloadcms/plugin-seo` configured for `pages`, `news`, and `colleges`.
- `@payloadcms/plugin-redirects` configured for `pages`.
- All 11 Phase 1 collection files exist and are registered in `src/payload.config.ts`.
- All 5 Phase 1 global files exist and are registered.
- `src/payload-types.ts` exists.
- Basic seed script exists and creates an initial `super_admin` plus a main navigation menu.
- Basic Vitest/Playwright tests exist.

Not yet implemented:

- Puck integration is installed but not enabled. `createPuckPlugin` is commented in `src/payload.config.ts`; no Puck config, block library, editor wiring, page renderer, or dynamic Puck data blocks were found.
- Frontend is still the Payload blank template. It shows Payload branding, `Payload Blank Template` metadata, and smoke tests assert `Welcome to your new project.`
- No AAU public routes are implemented in the new app beyond the blank homepage.
- No Tailwind/shadcn/Tajawal reuse exists in the new app. There is no Tailwind config or shadcn component set in `aau-payload-platform`; `styles.css` is the Payload starter CSS.
- Docker and `aau-payload-platform/README.md` still contain MongoDB blank-template guidance, while the app config uses Postgres/Neon.

## Missing Phase 1 Items

- Enable and configure `@delmaredigital/payload-puck`.
- Add Puck config and the Phase 1 block set.
- Add Puck layout storage/rendering for pages.
- Build the public AAU frontend shell and Phase 1 routes: `/`, `/about`, `/colleges`, `/colleges/[slug]`, `/contact`, `/faculty`, `/faculty/[id]`, `/news`, `/news/[slug]`.
- Rebuild Header/Footer against `navigation-menus` and `site-settings`.
- Migrate Tailwind tokens, Tajawal font, animations, RTL patterns, and shadcn/Radix primitives into the new app.
- Implement Payload Local/API data loaders for Phase 1 routes and detail pages.
- Add slug generation hooks for collections that expect generated slugs.
- Add hard access rules for create/update/delete across content and academic collections.
- Enforce role escalation prevention in `users`.
- Enforce coordinator college scoping for create/read/update.
- Block inactive users and admin access for `student`/`doctor` roles.
- Replace template README/Docker MongoDB instructions with Postgres/Neon instructions.
- Add tests for RBAC, public/published visibility, coordinator scoping, Puck pages, and public frontend routes.

## Doc / Implementation Mismatches

| Area | Docs say | Current implementation |
|---|---|---|
| Puck | Visual editor and layout support are Phase 1 | Dependency installed, plugin commented out, no blocks/config/rendering |
| Frontend | Reuse legacy frontend patterns now | New app still uses Payload blank template |
| Docker | Phase 1 uses Neon/Postgres | Docker compose still starts MongoDB and comments Postgres out |
| README | AAU Payload/Neon setup expected | Submodule README is still Payload blank template with MongoDB/S3 Cloud references |
| RBAC files | Docs expect `src/payload/access/roles.ts`, `helpers.ts`, `collections.ts` | Only `src/access/helpers.ts` exists |
| Users | Full name virtual field and escalation-prevention hook | No `fullName` virtual and no role-change hook |
| Users access | Admin+ read/manage with own-user rules | Any authenticated user can read/update broadly |
| Collections access | Matrix-defined create/read/update/delete | Most Phase 1 content/academic collections define only `read`; Payload defaults then decide the rest |
| Coordinator scoping | Required for college content | Helper fragments exist, but not wired to create/update/delete rules |
| Contact/join deletes | Admin+ delete | `content_manager+` can delete |
| Academic programs | Includes `applicationStepsAr/En`, `whyProgramAr/En`, `highSchoolType` | These fields are missing |
| Home global | Section titles for campus life, projects, colleges, news, events, faq, video, contact | Only colleges, news, and contact title/description groups exist |
| Tests | Should cover Phase 1 behavior | Tests are smoke/template-level and do not cover schema/RBAC/Puck/public routes |

## Phase 1 Hardening Checklist

- [ ] Normalize access helper locations or update docs to match actual `src/access` layout.
- [ ] Add collection access for all Phase 1 collections: create/read/update/delete.
- [ ] Add field/hook enforcement for `collegeScope` when role is `coordinator`.
- [ ] Add `beforeChange` hook on `users` to prevent role escalation and protect `super_admin`.
- [ ] Add own-user read/update constraints for non-admin users.
- [ ] Deny Payload Admin for `student` and `doctor` users, and account for `isActive`.
- [ ] Implement coordinator-scoped queries for `news`, `colleges`, `academic-departments`, `academic-programs`, and `faculty-members`.
- [ ] Ensure Local API calls that act on behalf of users use `overrideAccess: false`.
- [ ] Add slug hooks and uniqueness/error handling for `pages`, `news`, `colleges`, and academic entities that need slugs.
- [ ] Add validation for public form submissions and admin-only internal notes/status transitions.
- [ ] Move sensitive `smart-chat-settings.apiKey` handling out of plain text admin exposure or clearly defer integration.
- [ ] Update Docker/README to Postgres/Neon and remove MongoDB template drift.
- [ ] Add integration tests for public reads, unpublished filtering, public form creates, role restrictions, and coordinator scope.

## Phase 1.5 Checklist

Use this as the bridge between raw Phase 1 schemas and Phase 2 feature expansion:

- [ ] Enable Puck after pages access rules are hardened.
- [ ] Build `HeroBlock`, `StatsBlock`, `NewsGridBlock`, `ProgramsGridBlock`, `CollegesGridBlock`, `CTASection`, `RichTextBlock`, `FeatureCardsBlock`, `ImageGalleryBlock`, and placeholder `FAQBlock`.
- [ ] Add a page renderer that can render Puck data and fallback rich text.
- [ ] Recreate shadcn/Radix primitives and shared common components needed by Phase 1 routes.
- [ ] Port Tailwind/HSL/Tajawal/dark-mode/animation tokens from legacy frontend.
- [ ] Build Header/Footer using `navigation-menus` and `site-settings`.
- [ ] Build Phase 1 public pages and detail templates against Payload data.
- [ ] Add bilingual content selection helpers for Arabic/English display and fallbacks.
- [ ] Add seed fixtures for colleges, programs, news, menus, and globals sufficient for visual QA.
- [ ] Replace template tests with route, rendering, and admin workflow tests.

## Bilingual Gaps

- The schema mostly uses explicit `Ar`/`En` field pairs rather than Payload localization. This matches the migration docs, but there is no display-layer language selection yet.
- `pages.title` is not paired as `titleAr`; docs intentionally named it `title` plus `titleEn`, but the rest of the schema uses `*Ar`/`*En`. This inconsistency should be accepted intentionally or normalized.
- `users.firstName`/`lastName`, `media.alt`/`caption`, `site-settings.copyright`, `smart-chat-settings.botName`/`welcomeMessage`, form submitter names, and request fields are monolingual. Some may be acceptable operational fields, but public-facing media alt/caption and smart-chat welcome copy should be reviewed.
- `home-page` lacks bilingual section titles/descriptions for several documented public sections.
- No Tailwind/Tajawal/RTL frontend layer exists in the new app, so bilingual content cannot yet be verified visually.

## Frontend Reuse Gaps

- No legacy public routes have been ported into `aau-payload-platform`.
- No AAU Header/Footer replacement exists.
- No dynamic data fetchers or server components exist for Payload-backed public pages.
- No reusable old UI primitives or shadcn components have been recreated.
- No HSL token system, `--font-tajawal`, dark mode, or animation keyframes have been migrated.
- No detail page templates exist for colleges, programs, faculty, or news.
- No Puck block components exist for the documented Phase 1 page sections.
- Existing e2e frontend test confirms the blank Payload template, so tests currently lock in the wrong public UI.

## Suggested Implementation Order

1. Hardening first: RBAC, user hooks, collection access, coordinator scoping, and slug hooks.
2. Documentation cleanup: replace README/Docker MongoDB template text with Postgres/Neon setup.
3. Frontend foundation: Tailwind/Tajawal/shadcn tokens and AAU layout shell.
4. Puck enablement: plugin, config, block set, page renderer.
5. Public Phase 1 routes and detail templates.
6. Tests: schema/access integration tests, public route rendering, Puck rendering, and admin workflows.

## Files Read

- `README.md`
- `.gitmodules`
- `docs/source-inventory.md`
- `docs/migration/01-frappe-backend-audit.md`
- `docs/migration/02-database-documentation-comparison.md`
- `docs/migration/03-frontend-audit.md`
- `docs/migration/04-final-schema-decision-matrix.md`
- `docs/migration/05-rbac-plan.md`
- `docs/migration/06-frontend-reuse-plan.md`
- `docs/architecture/07-neon-vps-deployment-plan.md`
- `aau_initial_roles_permissions_payload_refine_ar.md` via targeted search
- `PLUGINS_ECOSYSTEM.md` via targeted search
- `aau_university/hooks.py` via targeted search
- `aau_university/content_access.py` via targeted search
- `aau_university/patches.txt`
- `aau-payload-platform/package.json`
- `aau-payload-platform/README.md`
- `aau-payload-platform/.env.example`
- `aau-payload-platform/docker-compose.yml`
- `aau-payload-platform/next.config.ts`
- `aau-payload-platform/src/payload.config.ts`
- `aau-payload-platform/src/access/helpers.ts`
- `aau-payload-platform/src/collections/Users.ts`
- `aau-payload-platform/src/collections/Media.ts`
- `aau-payload-platform/src/collections/Pages.ts`
- `aau-payload-platform/src/collections/NavigationMenus.ts`
- `aau-payload-platform/src/collections/News.ts`
- `aau-payload-platform/src/collections/Colleges.ts`
- `aau-payload-platform/src/collections/AcademicDepartments.ts`
- `aau-payload-platform/src/collections/AcademicPrograms.ts`
- `aau-payload-platform/src/collections/FacultyMembers.ts`
- `aau-payload-platform/src/collections/ContactMessages.ts`
- `aau-payload-platform/src/collections/JoinRequests.ts`
- `aau-payload-platform/src/globals/HomePage.ts`
- `aau-payload-platform/src/globals/AboutPage.ts`
- `aau-payload-platform/src/globals/SiteSettings.ts`
- `aau-payload-platform/src/globals/ContactSettings.ts`
- `aau-payload-platform/src/globals/SmartChatSettings.ts`
- `aau-payload-platform/src/app/(frontend)/page.tsx`
- `aau-payload-platform/src/app/(frontend)/layout.tsx`
- `aau-payload-platform/src/app/(frontend)/styles.css`
- `aau-payload-platform/src/seed/index.ts`
- `aau-payload-platform/tests/int/api.int.spec.ts`
- `aau-payload-platform/tests/e2e/admin.e2e.spec.ts`
- `aau-payload-platform/tests/e2e/frontend.e2e.spec.ts`
- `frontend/package.json`
- `frontend/tailwind.config.ts`

## File Changed

- `docs/migration/09-current-state-reaudit.md`

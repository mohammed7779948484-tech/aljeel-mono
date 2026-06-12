# Phase 1 Hardening and Phase 1.5 Execution Plan

Date: 2026-06-11

## PDCA Session Goal

After this session, the Payload platform should have a verified Phase 1 hardening baseline and a documented, test-backed Phase 1.5 public CMS implementation path, with Arabic/English behavior preserved from the legacy frontend.

## Current Problems Found

- `aau-payload-platform` exists as a Git submodule at `aau-payload-platform/`, pointing to `https://github.com/mohammed7779948484-tech/aau-payload-platform.git`.
- The root documentation still contains stale discovery notes saying the Payload project was missing, while the project now exists.
- The new Payload project has only 11 collections: users, media, pages, navigation-menus, news, colleges, academic-departments, academic-programs, faculty-members, contact-messages, join-requests.
- `users.role` is still a hardcoded select field, not a relationship to a real `roles` collection.
- Access helpers use string roles from `req.user.role`; they do not resolve role documents or permission arrays.
- User create/read/update/delete access is too broad: any authenticated user can read and update users.
- There is no role-escalation hook, no self-role protection, and no enforced coordinator `collegeScope`.
- Several content collections define only `read` access, leaving create/update/delete to Payload defaults.
- Public form collections allow direct public `create`, which invites spam and exposes the raw Payload API as the public submission surface.
- `@delmaredigital/payload-puck` and `@puckeditor/core` are installed but pinned as `latest` in `package.json`.
- The lockfile currently resolves `@delmaredigital/payload-puck` to `0.6.29`, Payload packages to `3.85.1`, Puck to `0.21.3`, Next to `15.5.19`, and React to `19.2.7`.
- Puck is not enabled in `payload.config.ts`; `createPuckPlugin` is commented out.
- `Pages` does not have an active Puck layout field or renderer.
- The public homepage is still the Payload blank template and tests assert that placeholder content.
- The submodule README still describes the Payload blank template and MongoDB setup.
- The seed script hardcodes `admin@aau.edu` and `changeme123`.
- The legacy frontend is Arabic-first, uses Tajawal, a client `LanguageContext`, manual `titleAr/titleEn`-style content fields, and `dir` switching on `document.documentElement`.
- The new Payload frontend layout is English-only and blank-template styled.

## Phase 1 Hardening Tasks

1. Pin critical dependency versions.
   - Replace `latest` for Payload, `@payloadcms/*`, Puck, Next, React, and React DOM with lockfile-resolved versions that research confirms acceptable.
   - Acceptance: `package.json` has no critical `latest` dependencies and lockfile remains coherent after `pnpm install`.

2. Add a real `roles` collection.
   - Create `src/collections/Roles.ts`.
   - Fields: `nameAr`, `nameEn`, `slug`, `level`, `isSystemRole`, descriptions, permissions, portal/admin access flags, `isActive`.
   - Acceptance: roles collection is registered in Payload config and generated types include it.

3. Introduce permission-aware access helpers.
   - Extend `src/access/helpers.ts` to support role relationship values, transitional `systemRole`, permissions, role levels, college scope, and common collection policies.
   - Acceptance: helper unit tests cover role slug, role level, permissions, admin access, and coordinator scope.

4. Link users to roles extensibly.
   - Add `role` relationship to `roles`.
   - Preserve `systemRole` select only as a transitional field if needed.
   - Add field-level access and server hooks for protected fields.
   - Acceptance: users cannot self-edit role or college scope; admin/content-manager cannot create or assign roles above allowed levels.

5. Enforce role-escalation and coordinator rules.
   - Add `beforeChange` hooks on users.
   - Require `collegeScope` for coordinators.
   - Prevent student/doctor Payload Admin access through role policy.
   - Acceptance: RBAC tests cover all required escalation cases.

6. Apply explicit access to existing collections.
   - Existing collections: users, roles, media, pages, navigation-menus, news, colleges, academic-departments, academic-programs, faculty-members, contact-messages, join-requests.
   - Acceptance: each collection has create/read/update/delete access, with coordinator scope where required.

7. Protect public forms.
   - Restrict direct public collection creates.
   - Add route handlers for contact, join request, and email request submission.
   - Use Zod validation, honeypot, simple in-memory or durable rate limiting abstraction, IP/user-agent metadata, and bilingual responses.
   - Acceptance: form tests cover valid Arabic/English, invalid, honeypot, rate limit, guest no-read, and hidden internal notes.

8. Fix seed safety.
   - Read initial admin email/password from env.
   - Refuse unsafe production password.
   - Seed system roles, permissions, bilingual navigation, sample content, and ideally one Puck page.
   - Acceptance: seed does not log passwords and does not hardcode production credentials.

9. Enable and secure Puck.
   - Import and configure `createPuckPlugin` only after confirming patched plugin version.
   - Use explicit access requiring `puck.edit`/`puck.publish`.
   - Add Puck endpoint security tests for `/api/puck/*`.
   - Acceptance: guest/student/doctor cannot write Puck content; content manager/admin/super admin can.

10. Replace default homepage.
   - Create a public frontend layout using the legacy visual direction.
   - Fetch home page content from Payload/Puck.
   - Support Arabic RTL and English LTR.
   - Acceptance: `/` no longer renders Payload starter content and has a safe empty-state fallback.

## Phase 1.5 Tasks

1. Add structured public CMS collections.
   - `events`
   - `blog-posts`
   - `faq`
   - `partners`
   - `offers`
   - `projects`
   - `campus-life`
   - `centers`
   - `research-publications`
   - `student-activities`
   - `student-affairs-documents`
   - `admission-requirements`
   - `study-plans`
   - `study-plan-courses`
   - `email-requests`

2. For each Phase 1.5 collection:
   - Add Arabic/English fields.
   - Add status/active/published controls.
   - Add explicit create/read/update/delete access.
   - Add seed/demo data where useful.
   - Add collection access tests.

3. Add Puck blocks.
   - Base blocks: Hero, RichText, CTA, Stats, FeatureCards, Gallery, NewsGrid, ProgramsGrid, CollegesGrid, FAQ.
   - Phase 1.5 blocks: EventsGrid, BlogGrid, Partners, Projects, CampusLife, Centers, Offers, ResearchGrid, AdmissionRequirements, StudyPlan, StudentActivities.
   - Acceptance: blocks render empty data safely and switch Arabic/English props by active locale.

4. Add public routes.
   - New routes: `/events`, `/events/[slug]`, `/blog`, `/blog/[slug]`, `/faq`, `/partners`, `/offers`, `/offers/[slug]`, `/projects-studio`, `/projects-studio/[slug]`, `/campus-life`, `/campus-life/[slug]`, `/centers`, `/centers/[slug]`, `/research`, `/student-affairs`, `/student-activities`, `/admission`, `/email-request`.
   - Verify existing routes: `/`, `/about`, `/colleges`, `/colleges/[slug]`, `/faculty`, `/faculty/[id]`, `/news`, `/news/[slug]`, `/contact`.
   - Acceptance: all public routes fetch from Payload, respect locale/direction, and handle empty data.

## Bilingual Implementation Tasks

1. Document the final i18n decision before installing packages.
2. Preserve manual Arabic/English fields during Phase 1.5 because legacy frontend and current Payload schemas already use this pattern.
3. Consider a hybrid strategy:
   - Manual content fields for migration clarity and Puck block props.
   - Next route/cookie/header locale handling for UI routing.
   - Payload built-in localization deferred or applied selectively only after data migration risks are reviewed.
4. Add locale helpers:
   - `getLocaleFromPathOrCookie`
   - `isRTL`
   - `pickLocalized`
5. Add route-level `dir` and `lang`.
6. Preserve Tajawal and Arabic-first spacing/typography.
7. Add fallback behavior when English or Arabic fields are missing.

## Files To Create

- `aau-payload-platform/src/collections/Roles.ts`
- Phase 1.5 collection files under `aau-payload-platform/src/collections/`
- Access helper tests under `aau-payload-platform/src/access/__tests__/`
- Collection access tests under `aau-payload-platform/src/collections/__tests__/`
- Puck config, renderer, and block files under `aau-payload-platform/src/puck/`
- i18n helpers under `aau-payload-platform/src/i18n/`
- Public form route handlers under `aau-payload-platform/src/app/(frontend)/api/`
- Public route files under `aau-payload-platform/src/app/(frontend)/`
- Test helpers/factories under `aau-payload-platform/src/test/` or `aau-payload-platform/tests/helpers/`
- E2E smoke tests under `aau-payload-platform/tests/e2e/`
- Migration reports `docs/migration/11-phase-1-hardening-report.md` and `docs/migration/13-phase-1-hardening-and-phase-1-5-validation-report.md`

## Files To Modify

- `aau-payload-platform/package.json`
- `aau-payload-platform/pnpm-lock.yaml`
- `aau-payload-platform/src/payload.config.ts`
- `aau-payload-platform/src/access/helpers.ts`
- Existing collection files under `aau-payload-platform/src/collections/`
- `aau-payload-platform/src/app/(frontend)/layout.tsx`
- `aau-payload-platform/src/app/(frontend)/page.tsx`
- `aau-payload-platform/src/app/(frontend)/styles.css`
- `aau-payload-platform/src/seed/index.ts`
- `aau-payload-platform/README.md`
- Existing tests that assert blank-template behavior.

## Test Files To Create

- `aau-payload-platform/src/access/__tests__/rbac.test.ts`
- `aau-payload-platform/src/access/__tests__/coordinator-scope.test.ts`
- `aau-payload-platform/src/collections/__tests__/collection-access.test.ts`
- `aau-payload-platform/src/collections/__tests__/users-rbac.test.ts`
- `aau-payload-platform/src/puck/__tests__/puck-security.test.ts`
- `aau-payload-platform/src/puck/__tests__/puck-renderer.test.tsx`
- `aau-payload-platform/src/i18n/__tests__/locale.test.ts`
- `aau-payload-platform/tests/int/public-forms.int.spec.ts`
- `aau-payload-platform/tests/e2e/public-routes.e2e.spec.ts`

## Commands To Run After Writing Tests

```bash
pnpm install
pnpm lint
pnpm generate:types
pnpm payload migrate:create phase-1-hardening-phase-1-5
pnpm test:int
pnpm test:e2e
pnpm test
pnpm build
```

If `DATABASE_URI` is missing or points to an unavailable Neon database, record the skipped DB/runtime checks in the validation report and include exact rerun commands.

## Risks

- Converting `users.role` from select to relationship requires a migration path and may break existing JWT assumptions.
- Payload built-in localization changes stored data shape; converting existing manual fields without migration can lose data.
- Puck endpoint security depends on patched `@delmaredigital/payload-puck`; tests must prove access is enforced.
- Full Phase 1.5 route migration is broad and may exceed one tight implementation pass.
- Coordinator-scoped create/update logic must validate submitted relationship IDs server-side, not only through admin UI filters.
- Public form rate limiting needs a production-appropriate storage decision if deployed serverlessly.

## Rollback Notes

- Keep `systemRole` during transition so role checks can fall back if role relationship data is absent.
- Add Puck behind a small integration surface: config, renderer, and page field/plugin. If plugin activation causes build issues, disable plugin import while preserving blocks/tests.
- Keep public form route handlers separate from collection schemas so direct collection access can remain locked down.
- Schema changes should be accompanied by generated migrations; review migration SQL before deployment.

## Definition Of Done

- Required audit, research, bilingual, frontend porting, execution, hardening, and validation docs exist.
- Roles collection and permission model exist.
- Users are linked to roles and escalation is prevented.
- Coordinator `collegeScope` is enforced.
- Existing collections have explicit CRUD access.
- Public forms are mediated through protected routes.
- Puck is pinned, enabled, configured, secured, and tested.
- `/` renders a real AAU page, not the Payload starter.
- Arabic and English rendering work with RTL/LTR.
- Phase 1.5 collections, Puck blocks, and public routes exist.
- Tests are real files and have been run or explicitly skipped with reason.
- README no longer references MongoDB/default template setup.
- Final validation report states go/no-go for Phase 2.

## CHECK Phase Criteria

- Compare implementation against this plan and the user-provided definition of done.
- Confirm tests cover required RBAC, forms, Puck, collections, bilingual rendering, and route smoke behavior.
- Confirm validation commands were run and results recorded.

## ACT Phase Retrospective Prompts

- Which parts of the migration plan were accurate?
- Where did the legacy frontend or Payload implementation differ from docs?
- What should be added to future working agreements for bilingual CMS migrations?

To execute each step, invoke PDCA Do. Do not begin any step without first opening that prompt.

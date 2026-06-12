# Bilingual Payload and i18n Research

Fresh research date: 2026-06-11

Scope: Next bilingual routing, RTL/Tailwind, Payload localization, manual bilingual field tradeoffs, Puck implications, and migration guidance for Arabic/English AAU content.

## Sources Checked

- Payload CMS docs: [Localization](https://payloadcms.com/docs/configuration/localization), [Access Control](https://payloadcms.com/docs/access-control/overview), [Database Overview](https://payloadcms.com/docs/database/overview), [Postgres](https://payloadcms.com/docs/database/postgres).
- Next.js docs: [App Router Internationalization](https://nextjs.org/docs/app/guides/internationalization), [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers), [Vitest testing](https://nextjs.org/docs/app/guides/testing/vitest).
- Tailwind docs: [Hover, focus, and other states](https://tailwindcss.com/docs/hover-focus-and-other-states), [Tailwind v3.3 logical properties / RTL announcement](https://tailwindcss.com/blog/tailwindcss-v3-3).
- Puck docs: [Puck data model](https://puckeditor.com/docs/api-reference/data-model/data), [Slot field](https://puckeditor.com/docs/api-reference/fields/slot), [DropZones to slots migration](https://puckeditor.com/docs/guides/migrations/dropzones-to-slots).
- Local repo inspection: current Payload collections/globals, generated types, `frontend` bilingual fields and RTL language context.

## Current Repo Findings

The repo currently has two bilingual approaches:

- Legacy/frontend app uses manual Arabic/English pairs such as `titleAr`, `titleEn`, `contentAr`, `contentEn`, plus a client `LanguageContext` that sets `document.documentElement.dir`.
- `aau-payload-platform` mostly uses Arabic-default fields plus English suffixes, for example `Pages.title`, `Pages.titleEn`, `Pages.content`, `Pages.contentEn`; many globals and collections follow this manual pair pattern.
- Payload config does not currently enable root `localization`.
- The Payload frontend layout is still a blank template with `<html lang="en">`; it does not set Arabic default language or direction.
- Generated Payload types already contain Lexical rich-text direction fields, but this is editor content direction metadata, not a complete routing/i18n strategy.

## Payload Localization Research

Payload's localization feature is field-level, not document-level. Enabling root `localization` defines project locales and default locale; individual fields then opt in with `localized: true`.

Payload localization supports:

- locale objects such as `{ label: 'Arabic', code: 'ar', rtl: true }`;
- `defaultLocale`;
- `fallback` and `fallbackLocale`;
- REST query params such as `?locale=ar&fallback-locale=none`;
- Local API options such as `locale` and `fallbackLocale`.

Payload docs also note that localization works well with i18n, but they are different concerns: localization manages CMS data translations; i18n manages application UI strings, dates, numbers, and routing.

Decision: for Phase 1, keep manual bilingual fields for existing AAU content collections instead of converting the schema immediately to Payload native localization.

Why:

- The existing frontend and admin conventions already expect explicit Arabic/English fields.
- Migrating to `localized: true` changes database shape and generated TypeScript types.
- Payload docs note MongoDB is simpler when most content is localized; this project is intentionally on Postgres, where schema migrations need more care.
- Puck layout localization is not automatic. A single `layout` field with text props must either contain bilingual props or be duplicated per locale.

## Manual Bilingual Fields vs Payload Localization

| Option | Benefits | Costs | Phase 1 recommendation |
| --- | --- | --- | --- |
| Manual fields, e.g. `title`, `titleEn` or `titleAr`, `titleEn` | Simple TypeScript, explicit editorial view, easy frontend mapping, no root Payload localization migration | More duplicate fields, fallback behavior is custom, every query must map language manually | Keep for now and standardize naming. |
| Payload `localization` with `localized: true` fields | Native locale selector, API `locale` / `fallbackLocale`, admin RTL support per locale | Database/type migration, hidden fallback behavior can surprise editors, Puck layout tradeoffs remain | Revisit after Phase 1 hardening. |
| Separate documents per locale | Strong editorial isolation, language-specific slugs/status | Relationship duplication, harder cross-language publishing | Not recommended for Phase 1. |

Naming decision:

- Standardize new fields as `fieldAr` and `fieldEn` where the business model is explicitly bilingual.
- For existing Payload fields where `title` means Arabic, do not rename casually; document it and migrate intentionally later if needed.
- Add helper functions at the frontend/API boundary so UI code does not repeatedly hand-roll `language === 'ar' ? ... : ...`.

## Recommended Phase 1 Content Model

For core public content:

- Keep Arabic and English sibling fields.
- Add validation that required public content has Arabic fields before publish.
- Decide whether English is required for publish. If not required, implement fallback to Arabic and track missing English in admin review views.
- Keep shared fields language-neutral: `slug`, `isPublished`, dates, relationships, images, status, college/program relationships.
- For SEO fields, keep bilingual title/description or use plugin fields carefully with clear language rules.

For Puck pages:

- Prefer a single page document with one `layout` whose component props include bilingual values, for example `headingAr` / `headingEn`.
- Avoid separate Puck layouts per locale in Phase 1; editors would have to keep layout structure in sync twice.
- Keep visual/layout props language-neutral: spacing, variant, image, alignment tokens.
- Text props should be bilingual and rendered by the frontend according to current locale.
- Build AAU-specific components with slot fields for nested structures, following Puck 0.21 guidance.

Risk: Puck's editor UI may preview one language at a time unless a language toggle is added around the Puck config/rendering. This is acceptable for Phase 1 if documented for editors.

## Next Bilingual Routing

Next's App Router docs recommend internationalized routing by sub-path or domain. For this project, sub-path routing is the best fit:

- Arabic default: `/`
- English: `/en`
- Optional canonical Arabic explicit route: `/ar`, redirected to `/` or kept if SEO requires visible locale symmetry.

Decision: use URL-based locale routing for public pages rather than only a client-side language toggle.

Why:

- Public university pages need crawlable, shareable language-specific URLs.
- `<html lang>` and `dir` can be rendered correctly on the server.
- Metadata, alternates, canonical URLs, and structured data can be language-aware.
- It reduces hydration mismatch risk from client-only direction switching.

Implementation shape:

- Add a locale segment or route group strategy that resolves locale from pathname.
- Render `<html lang="ar" dir="rtl">` for Arabic and `<html lang="en" dir="ltr">` for English.
- Generate metadata and `alternates.languages` per page.
- Keep admin/Payload routes outside public locale routing.

Risk: migrating from the current frontend's client `LanguageContext` to route-based locale will touch many components. Do it after the Payload data contract is stable.

## RTL and Tailwind

Tailwind supports directional variants such as `rtl:` / `ltr:` and logical property utilities introduced in Tailwind 3.3, such as `ms-*` and `me-*`, which map to inline start/end instead of physical left/right.

Decision:

- Prefer logical utilities (`ms`, `me`, `ps`, `pe`, `start`, `end`) over physical `ml`, `mr`, `pl`, `pr`, `left`, `right` in new bilingual UI.
- Use `rtl:` / `ltr:` variants only for genuine direction-specific exceptions.
- Set `dir` on the server at the document/root layout level, not only with client effects.
- Avoid duplicating CSS rules for `[dir="rtl"]` unless Tailwind utilities cannot express the layout.

Risk: the legacy frontend has many explicit `dir` attributes and Arabic-first assumptions. A visual QA pass is required on both Arabic and English after route-based locale work.

## Payload Admin RTL

Payload localization supports locale objects with `rtl: true`, which affects the admin UI for that locale. If native Payload localization is postponed, the admin can still use explicit field labels and `admin.description` to make Arabic/English pairs clear, but it will not get Payload's locale selector behavior.

Decision:

- Phase 1 manual bilingual admin: group Arabic/English field pairs consistently, label them clearly, and set field descriptions for fallback/publish requirements.
- Later Payload localization migration: define locales as Arabic and English, set Arabic as default only if the editorial team wants Arabic-first records, and test admin RTL thoroughly.

## Access and Localization

Localization does not replace access control. If native localization is enabled later:

- Test access with `locale` and `fallbackLocale` options in REST and Local API.
- Decide whether drafts/published state is global or localized. Payload has localized status support behind a two-step experimental flag and per-collection/global opt-in; do not enable it casually.
- Ensure coordinator scoping filters remain language-neutral and do not depend on localized fields.

Decision: keep publish/access rules language-neutral for Phase 1.

## Query and API Recommendations

For manual bilingual fields:

- Public API/helpers should return a normalized view model:
  - `title`
  - `content`
  - `locale`
  - `direction`
  - `fallbackUsed`
- Keep raw bilingual fields available only where editors/admin tools need them.
- Centralize fallback behavior:
  - Arabic page: require Arabic; do not fall back silently for required public pages.
  - English page: fall back to Arabic only if product accepts mixed-language pages; otherwise show not found or a missing-translation state.

For later native Payload localization:

- Use REST `?locale=en&fallback-locale=none` or Local API `{ locale: 'en', fallbackLocale: false }` for strict language pages.
- Use fallback only for components where mixed language is acceptable.

## Tests

Add tests before large i18n migration:

- Unit-test locale helpers: path parsing, fallback selection, `dir` mapping, field selection.
- Test Payload access with bilingual content and published/draft filters.
- Test public routes for:
  - Arabic route has `lang="ar"` and `dir="rtl"`;
  - English route has `lang="en"` and `dir="ltr"`;
  - metadata alternates include both languages;
  - missing English content follows the chosen fallback policy.
- Use Playwright screenshots for representative Arabic and English pages because RTL regressions are often visual, not type-level.

## Decisions

1. Keep manual bilingual fields for Phase 1 and standardize their naming/usage.
2. Do not enable Payload native localization until after access hardening, Puck enablement, and migration planning.
3. Use route-based public i18n, with Arabic default and English under `/en`.
4. Render `lang` and `dir` on the server.
5. Build Puck components with bilingual text props and language-neutral layout props.
6. Prefer Tailwind logical utilities and `rtl:` / `ltr:` variants for direction-aware UI.

## Risks

- Manual bilingual fields can drift if validation/editor workflows do not require both languages.
- Payload native localization later will require a database migration and generated type changes.
- Puck layout text can become hard to translate if component props are not designed bilingually from the start.
- Client-only direction switching can cause hydration and SEO issues; route-based locale should replace it for public pages.
- SEO plugin fields need a clear bilingual strategy; otherwise metadata can be single-language while page content is bilingual.

## Implementation Checklist

- [ ] Document canonical locale paths: `/` Arabic, `/en` English.
- [ ] Add server-rendered locale/direction handling in the public app layout.
- [ ] Add helper functions for bilingual field selection and fallback reporting.
- [ ] Standardize new Payload fields to `fieldAr` / `fieldEn`, or explicitly document Arabic-default unsuffixed legacy fields.
- [ ] Add publish validation for required Arabic and chosen English policy.
- [ ] Design Puck component props with bilingual text fields from day one.
- [ ] Use Tailwind logical spacing/position utilities in new bilingual UI.
- [ ] Add unit tests for locale helpers and Payload access behavior.
- [ ] Add Playwright visual coverage for Arabic RTL and English LTR pages.
- [ ] Revisit Payload native localization after Phase 1 hardening and Puck tests are stable.

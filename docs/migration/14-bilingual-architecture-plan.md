# Bilingual Architecture Plan

> Scope: Arabic/English and RTL/LTR architecture for the Payload/Next.js platform, based on the old `frontend/` audit. This is a planning document only; no implementation was performed.

## Executive Summary

The old frontend supports Arabic and English through a client-only `LanguageContext`. Arabic is the default, `toggleLanguage()` flips local React state, and an effect mutates `document.documentElement.dir` and `lang`. Content fields are duplicated as `*Ar/*En`, and UI strings are passed inline through `t(ar, en)`.

The target architecture should move locale into the route and data layer:

- URL-driven locales: Arabic default at `/`, English under `/en/...`.
- Server-rendered `lang` and `dir`.
- Phase 1 manual/hybrid Payload fields such as `titleAr/titleEn` and `descriptionAr/descriptionEn`, matching the legacy frontend, current Payload schema, and Puck block props.
- Payload native localization remains a later migration candidate, not a Phase 1 requirement.
- A small UI dictionary for chrome/system strings that are not CMS content.
- Direction-aware components using logical CSS, locale-aware icons, and locale-preserving links.

## Old Frontend Audit

### LanguageContext

Source: `frontend/contexts/LanguageContext.tsx`

Behavior:

- `type Language = 'ar' | 'en'`
- default state is `ar`
- `toggleLanguage()` switches `ar <-> en`
- `t(ar, en)` returns one of two strings
- effect sets:
  - `document.documentElement.dir = 'rtl' | 'ltr'`
  - `document.documentElement.lang = language`
- fallback outside provider returns Arabic and a no-op toggle

Limitations:

- Locale is not encoded in the URL.
- Server-rendered HTML always starts as Arabic/RTL from `app/layout.tsx`.
- English pages can hydrate from Arabic markup, creating SEO and hydration risk.
- Language preference is not persisted.
- Metadata, sitemap, canonical URLs, and alternates are not consistently locale-specific.

### Current Usage Footprint

Audit results:

- 84 files import `useLanguage`.
- 101 files contain bilingual field naming patterns such as `titleAr/titleEn`, `nameAr/nameEn`, `descriptionAr/descriptionEn`.
- 584 bilingual field-name references were found across app, components, services, and types.
- 113 app/component files have direction-sensitive patterns.
- 1179 direction/language-sensitive references were found, including `language === 'ar'`, `dir`, `text-start`, `ms-*`, `border-s-*`, and arrow branching.

This means the port should treat i18n as a platform concern, not a component-by-component afterthought.

### Bilingual Content Pattern

The old data model uses parallel fields:

- `titleAr`, `titleEn`
- `descriptionAr`, `descriptionEn`
- `contentAr`, `contentEn`
- `nameAr`, `nameEn`
- `summaryAr`, `summaryEn`
- `questionAr`, `questionEn`
- `answerAr`, `answerEn`
- nested arrays with bilingual pairs, such as courses, education, experience, publications, menu labels, and stats labels

Fallbacks commonly prefer Arabic:

- `titleEn || titleAr`
- `descriptionEn || descriptionAr`
- `contentEn || contentAr`

This fallback policy is practical during migration, but it should be visible to editors and testable.

### Direction And Layout Pattern

The old app mixes several direction strategies:

- Global `dir` mutation from `LanguageContext`.
- Per-page `dir={isRTL ? 'rtl' : 'ltr'}` in detail pages.
- Tailwind logical utilities such as `ms-*`, `border-s-*`, `rounded-e-*`, and `text-start`.
- Manual direction branching for arrows, hover movement, dropdown alignment, and sheet side.
- Inputs in admin forms explicitly set `dir="rtl"` for Arabic fields and `dir="ltr"` for English/slug fields.

Preserve the intent, but centralize the architecture.

## Target Locale Model

### Supported Locales

| Locale | Direction | Role |
| --- | --- | --- |
| `ar` | `rtl` | Default locale, Arabic-first editorial source |
| `en` | `ltr` | Secondary public locale |

Recommended shared type:

```ts
export const locales = ['ar', 'en'] as const
export type Locale = (typeof locales)[number]
export const defaultLocale: Locale = 'ar'
```

### URL Strategy

Use Arabic-default routes with English locale prefixes:

| Content | Arabic URL | English URL |
| --- | --- | --- |
| Home | `/` | `/en` |
| About | `/about` | `/en/about` |
| News list | `/news` | `/en/news` |
| News detail | `/news/[slug]` | `/en/news/[slug]` |
| College detail | `/colleges/[slug]` | `/en/colleges/[slug]` |

Recommended policy:

- Keep `/` as the canonical Arabic homepage.
- Optionally redirect `/ar` to `/` if explicit Arabic routes are introduced for compatibility.
- Keep admin routes outside the public locale tree unless a localized admin UX is explicitly required.
- Language switcher should preserve the current content path and query string.
- Slugs should be locale-stable where possible. If localized slugs are required later, add a redirect/alias plan before enabling them.

### Next.js Layout

Target shape:

- `src/app/(frontend)/layout.tsx` for Arabic default routes, plus an English route strategy for `/en`
- validate `params.locale`
- set `<html lang={locale} dir={dir}>` at the nearest root that owns the document
- provide locale to client islands via a small provider only when needed
- fetch navigation and site settings server-side by locale

Do not repeat the old pattern where server HTML is always Arabic and the client flips it after hydration.

## Payload Localization Plan

### Phase 1 Decision: Do Not Enable Native Payload Localization Yet

The current `aau-payload-platform/src/payload.config.ts` does not define `localization`. Fresh research in `docs/research/03-bilingual-payload-i18n-research.md` recommends not enabling native Payload localization during Phase 1 because:

- existing Frappe/frontend/Payload schemas already use explicit bilingual field pairs;
- converting fields to `localized: true` changes stored data shape and generated types;
- Puck page data still needs deliberate bilingual props even if Payload localization is enabled;
- Phase 1 hardening should avoid combining security, Puck activation, and broad localization migrations.

Payload native localization remains a strong future option after Phase 1 hardening and Phase 1.5 content models stabilize.

### Field Modeling Decision

Prefer manual bilingual field pairs for Phase 1 and keep the future Payload localization mapping documented.

| Current field pair | Future localized field | Phase 1 approach |
| --- | --- | --- |
| `titleAr/titleEn` | `title` with `localized: true` | Keep explicit fields |
| `nameAr/nameEn` | `name` with `localized: true` | Keep explicit fields |
| `descriptionAr/descriptionEn` | `description` with `localized: true` | Keep explicit fields |
| `contentAr/contentEn` | `content` with `localized: true` | Keep explicit fields |
| `labelAr/labelEn` | `label` with `localized: true` | Keep explicit fields |
| `questionAr/questionEn`, `answerAr/answerEn` | `question`, `answer` localized | Keep explicit fields |

Do not localize:

- IDs
- relationship fields
- publication status
- display order
- dates
- email, phone, URL, map coordinates
- numeric stats
- icon keys
- boolean flags
- role/permission keys

Maybe localize:

- slug: keep stable initially; localize only with redirect strategy.
- SEO title/description: localize if plugin fields support it or mirror with localized fields.
- alt text: localize if media captions/alt text are editorially managed.

### Transitional Schema Strategy

The current Payload schema already mirrors old `*Ar/*En` fields in collections/globals. There are two viable migration steps:

1. Short-term compatibility:
   - Keep current fields.
   - Build mapper helpers like `pickLocalized(doc, locale, 'title')`.
   - Seed data quickly from Frappe exports.
   - Useful if Phase 1.5 must ship before a schema migration.

2. Final architecture:
   - Enable Payload `localization`.
   - Rename fields to neutral names such as `title`, `description`, `content`, `name`.
   - Mark content fields `localized: true`.
   - Migrate old Arabic values into locale `ar` and English values into locale `en`.

Recommended path: use the manual bilingual model for Phase 1 and Phase 1.5, with centralized mapping helpers. Revisit Payload localization before broad long-term editorial use, after a migration plan and tests exist.

## UI String Strategy

CMS content should live in Payload. Interface chrome should live in a typed dictionary.

Examples of dictionary-owned strings:

- `Login`
- `Search`
- `Menu`
- `Switch Language`
- `Read More`
- `View All`
- `Loading`
- `No results`
- form validation messages
- toast messages
- aria labels and screen-reader text

Avoid the old inline pattern:

```tsx
t('قراءة المزيد', 'Read More')
```

Prefer:

```ts
dict.news.readMore
```

Benefits:

- UI copy can be audited centrally.
- Missing translations are detectable.
- Client components no longer need arbitrary `t(ar, en)` pairs.
- Server components can render localized UI without client context.

## Directionality Standards

### Page Direction

Set direction at the HTML/layout level:

- `ar -> dir="rtl"`
- `en -> dir="ltr"`

Avoid per-component `dir` unless embedding opposite-direction content inside a page.

### CSS

Prefer logical utilities:

- `text-start`, `text-end`
- `ms-*`, `me-*`
- `ps-*`, `pe-*`
- `border-s-*`, `border-e-*`
- `rounded-s-*`, `rounded-e-*`

Avoid physical direction utilities in shared components unless the position is truly physical:

- `left-*`
- `right-*`
- `ml-*`
- `mr-*`
- `pl-*`
- `pr-*`
- `text-left`
- `text-right`

### Icons And Motion

Old components often branch on `language === 'ar'` for arrow icons and hover offsets. Keep that behavior behind helpers:

| Need | Helper |
| --- | --- |
| Forward arrow | `ForwardIcon` returns left arrow in RTL, right arrow in LTR |
| Back arrow | `BackIcon` returns right arrow in RTL, left arrow in LTR |
| Drawer side | `locale === 'ar' ? 'right' : 'left'` |
| Menu alignment | logical start/end classes |
| Slide direction | use `dirMultiplier(locale)` only in client animation islands |

### Forms

For public forms:

- Field labels follow page locale.
- Inputs can inherit page direction for normal text.
- Email, phone, URLs, slugs, and codes should use `dir="ltr"`.

For admin/editor forms:

- Payload localized fields should present locale tabs/controls rather than separate Arabic/English duplicate fields.
- If transitional duplicate fields remain, keep explicit `dir="rtl"` for Arabic fields and `dir="ltr"` for English/slug/code fields.

## Navigation Architecture

Use `navigation-menus` as the content source for:

- main header nav
- mega-menu groups
- footer quick links
- utility links

Target field model:

- `key`: `main`, `footer`, `utility`
- `items[]`
  - `label` localized
  - `url`
  - `group`
  - `children[]` if nested menus are added
  - `openInNewTab`
  - `order`
  - optional `icon`
  - optional relationship target for collection-driven items

Language switcher behavior:

- If on `/ar/news/some-slug`, switch to `/en/news/some-slug`.
- If content is missing in the target locale, either:
  - show the fallback content with an editorial warning in non-production/admin preview, or
  - redirect to the target locale list page with a toast/banner.
- For static routes, preserve query/search params.

## Content Completeness And Fallback

Payload `fallback: true` can keep pages from breaking, but it can hide missing English content. Use it deliberately.

Recommended policy:

- Public production may fallback from `en` to `ar` during migration.
- Admin/editor preview should visibly flag missing target-locale fields.
- Build a translation completeness report before launch.
- Required Arabic fields should be enforced first.
- English requiredness can become stricter per content type as translation work catches up.

Content status fields to consider:

- `translationStatus`: `missing`, `draft`, `review`, `approved`
- `lastTranslatedAt`
- `translatedBy`
- `needsTranslation`

## Metadata, SEO, And Discovery

Every public page should generate locale-aware metadata:

- localized title and description
- canonical URL
- `alternates.languages` for `ar` and `en`
- Open Graph locale
- localized image alt text where available
- localized JSON-LD name/description where appropriate

Recommended Open Graph locale values:

- Arabic: `ar_YE` if Yemen is the institutional locale, or a confirmed alternative
- English: `en_US` or `en` depending final SEO policy

Old issue to avoid:

- Root metadata currently uses `ar_IQ` while address data says Yemen/Sanaa. Confirm and standardize.

## Slug Policy

Old news service has special handling for unsafe slugs:

- safe slugs pass through
- non-safe slugs become `n-` plus base64url

Target policy:

- Prefer stable ASCII slugs for all public collection records.
- Store Arabic title separately from slug.
- Keep one slug across locales for Phase 1.5.
- Add redirects when importing legacy non-ASCII or encoded slugs.
- Add uniqueness and index constraints in Payload.

If localized slugs are later required:

- Add `slug` as localized only after proving route generation, sitemap, redirects, and language switching.
- Keep a non-localized canonical ID for relationships.

## Rich Text And Structured Content

Old content is a mix of strings, textareas, arrays, and ad hoc HTML/rich content.

Target guidance:

- Use Payload Lexical rich text for long editorial content.
- Use arrays/blocks for repeated structured content, such as stats, goals, quick links, FAQ, education, experience, publications, and courses.
- Localize text fields inside arrays when the array identity is shared between locales.
- If Arabic and English need different array ordering or item counts, model that explicitly instead of forcing one shared array.

## Migration Mapping

| Old source | Target source |
| --- | --- |
| `LanguageContext.language` | route-derived locale and server locale helpers |
| `LanguageContext.t(ar,en)` | manual bilingual Payload field helpers or typed UI dictionary |
| `document.documentElement.dir/lang` mutation | server-rendered layout attributes |
| `config/routes.ts` | Payload `navigation-menus` plus route constants |
| `titleAr/titleEn` in services | manual bilingual Payload fields for Phase 1; future localized fields only after migration |
| Frappe `services/server/*` | Payload Local API helpers |
| Frappe media paths | Payload Media upload relations |
| inline validation messages | localized zod/error dictionary |
| route-level static Arabic metadata | locale-aware `generateMetadata` |

## QA Checklist

### Arabic RTL

- HTML has `lang="ar"` and `dir="rtl"` before hydration.
- Header nav order, dropdown alignment, sheet side, and arrows are correct.
- Forms align labels and helper text correctly.
- Email, phone, URL, slug, and code inputs remain LTR.
- Tajawal loads and applies consistently.
- Metadata and Open Graph use Arabic content.

### English LTR

- HTML has `lang="en"` and `dir="ltr"` before hydration.
- Header nav, dropdowns, footer, breadcrumbs, cards, and CTAs align LTR.
- Forward/back icons point naturally.
- Arabic fallback content is visible only where policy allows.
- Dates use English locale formatting.
- Metadata and Open Graph use English content.

### Cross-Locale

- Language switch preserves current equivalent page.
- Canonicals and alternates are correct.
- Sitemap contains both locale routes.
- Search indexes include locale information.
- Missing translations are reported.
- No mojibake Arabic text appears in the new UI.

## Implementation Sequence

1. Add shared locale constants and helpers.
2. Add locale-prefixed public route structure.
3. Finalize the manual bilingual mapper for Phase 1; do not enable Payload localization in the same pass.
4. Update collections/globals to localized fields or document the temporary `*Ar/*En` bridge.
5. Build UI dictionary and replace inline `t(ar,en)` in newly ported components.
6. Rebuild header/footer around locale-aware data and links.
7. Port core public pages with server-rendered locale data.
8. Add metadata, sitemap, robots, and JSON-LD per locale.
9. Add translation completeness checks.
10. Run visual QA on Arabic and English desktop/mobile viewports.

## Open Decisions

| Decision | Recommendation |
| --- | --- |
| Default URL | Keep `/` as Arabic canonical; optional `/ar -> /` redirect |
| Payload schema | Keep manual bilingual fields for Phase 1; revisit `localized: true` after hardening |
| Slugs | Stable non-localized ASCII slugs for Phase 1.5 |
| English fallback | Allow during migration, report missing translations |
| Admin localization | Use Payload localization controls, not a custom old admin clone |
| Dark mode | Preserve tokens; decide product requirement before porting toggle |
| Locale persistence | URL is source of truth; optional cookie can remember redirect preference |

## Non-Goals

- Rebuilding old student/doctor dashboards.
- Porting old custom admin CRUD screens.
- Translating all missing English content in this architecture phase.
- Implementing localized slugs before the canonical route model is stable.
- Copying old `LanguageContext` into the new app unchanged.

# Phase 1 Hardening and Puck Research

Fresh research date: 2026-06-11

Scope: Payload v3 hardening, Next integration, Postgres/Neon, migrations, auth/access, role escalation, admin restrictions, Puck and `@delmaredigital/payload-puck`, `/api/puck` access, tests, and rate limiting for `aau-payload-platform`.

## Sources Checked

- Payload CMS docs: [Access Control](https://payloadcms.com/docs/access-control/overview), [Authentication Overview](https://payloadcms.com/docs/authentication/overview), [Authentication Operations](https://payloadcms.com/docs/authentication/operations), [Token Data](https://payloadcms.com/docs/authentication/token-data), [Postgres](https://payloadcms.com/docs/database/postgres), [Migrations](https://payloadcms.com/docs/database/migrations), [Database Overview](https://payloadcms.com/docs/database/overview).
- Delmare Digital docs and security: [`payload-puck` docs](https://delmaredigital.github.io/payload-puck/), [`payload-puck` GitHub](https://github.com/delmaredigital/payload-puck/), [GitHub Advisory GHSA-65w6-pf7x-5g85](https://github.com/advisories/GHSA-65w6-pf7x-5g85), [NVD CVE-2026-39397](https://nvd.nist.gov/vuln/detail/CVE-2026-39397), [issue #7](https://github.com/delmaredigital/payload-puck/issues/7).
- Puck docs: [`<Puck>` component](https://puckeditor.com/docs/api-reference/components/puck), [Data model](https://puckeditor.com/docs/api-reference/data-model/data), [DropZones to slots migration](https://puckeditor.com/docs/guides/migrations/dropzones-to-slots), [Slot field](https://puckeditor.com/docs/api-reference/fields/slot).
- Next.js docs: [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers), [`route.ts` file convention](https://nextjs.org/docs/app/api-reference/file-conventions/route), [Vitest testing guide](https://nextjs.org/docs/app/guides/testing/vitest), [Backend for Frontend rate limiting](https://nextjs.org/docs/app/guides/backend-for-frontend).
- Neon docs: [Connection pooling](https://neon.com/docs/connect/connection-pooling), [Payload + Neon guide](https://neon.com/guides/payload).
- Rate limiting references: [Vercel WAF Rate Limiting](https://vercel.com/docs/vercel-firewall/vercel-waf/rate-limiting), [Vercel Rate Limiting SDK](https://vercel.com/docs/vercel-firewall/vercel-waf/rate-limiting-sdk), [Upstash Rate Limit](https://upstash.com/docs/redis/sdks/ratelimit-ts/overview).
- Local repo inspection: `aau-payload-platform/package.json`, `aau-payload-platform/pnpm-lock.yaml`, `src/payload.config.ts`, `src/access/helpers.ts`, `src/collections/Users.ts`, `src/collections/Pages.ts`, generated Payload API routes.

## Current Repo Version Findings

`package.json` uses `latest` for the Payload and Puck packages, but `pnpm-lock.yaml` resolves the current install to:

| Package | Locked version | Current registry latest checked by registry API | Notes |
| --- | ---: | ---: | --- |
| `payload` | `3.85.1` | `3.85.1` | Current Payload v3 line. |
| `@payloadcms/next` | `3.85.1` | `3.85.1` | Lockfile peer range accepts selected Next 15 ranges and `>=16.2.6 <17.0.0`. |
| `@payloadcms/db-postgres` | `3.85.1` | not separately queried | Matched Payload version in lockfile. |
| `@delmaredigital/payload-puck` | `0.6.29` | `0.6.29` | Above patched version `0.6.23`. |
| `@puckeditor/core` | `0.21.3` | `0.21.3` | New package scope required by Delmare plugin. |
| `next` | `15.5.19` | `16.2.9` | Keep on locked 15 until a deliberate upgrade/test pass. |
| `react` / `react-dom` | `19.2.7` | not queried | Satisfies Delmare peer requirement. |

Decision: replace broad `latest` ranges with exact or caret-pinned known-good versions in a future code task. Do not mix dependency pinning with schema/security changes.

## Payload v3 and Next Integration

The project is already using the standard Payload v3 Next app pattern:

- `next.config.ts` wraps the app with `withPayload`.
- Payload REST and GraphQL routes are generated under `src/app/(payload)/api`.
- Admin is mounted under `src/app/(payload)/admin`.
- `payload.config.ts` uses `postgresAdapter`, `lexicalEditor`, `seoPlugin`, and `redirectsPlugin`.

Decision: keep the generated Payload API route files as generated. Put custom hardening in collection/global access functions, Payload hooks, custom explicit route handlers, and external edge/host controls.

Risk: `src/app/my-route/route.ts` is still an example route. If left public, it is harmless today but should be removed or converted to a real health/debug route with explicit behavior before launch.

## Postgres, Neon, and Migrations

Payload's Postgres docs distinguish two workflows:

- `push` automatically syncs config changes to the database in development and should not be mixed with manual migrate commands.
- migrations should be used for production, with `payload migrate:create` and `payload migrate`.

Neon connection pooling docs distinguish pooled and direct URLs:

- Runtime web traffic should use the pooled URL, whose hostname contains `-pooler`.
- Migrations, seeds, `pg_dump`, and admin scripts should use the direct URL because Neon PgBouncer runs in transaction pooling mode and some migration/session behaviors need a persistent direct connection.

Decision:

- Keep `push: process.env.NODE_ENV === 'development'` only for local development.
- Add a production migration process before launch: run `pnpm payload migrate:create` for schema changes and `pnpm payload migrate` in CI/deploy using a direct Neon URL.
- Split env vars in implementation, for example `DATABASE_URI` for app runtime and `DATABASE_DIRECT_URI` for migration commands if the deployment needs both.

Risk: using `latest` dependencies plus `push` against shared/staging data can produce unexpected DDL drift. Freeze versions before shared-environment schema work.

## Auth, Access, and Role Escalation

Payload access rules secure both APIs and the admin UI. Payload docs also state that Local API access control is skipped by default and must be opted back in with `overrideAccess: false` when acting on behalf of a user.

Current state:

- `Users.access.create` and `delete` are `isAdminOrAbove`.
- `Users.access.update` is `isAuthenticated`.
- The `role` field has `saveToJWT: true`.
- `helpers.ts` has `canManageUserRole`, but it is not yet enforced in `Users.ts`.
- `canAccessAdmin` exists but is not wired into `admin` access.

Decision:

- Add a role-escalation guard in a future implementation task. Non-super-admins must not assign roles at or above their own level, change their own role upward, or change another user into `super_admin`.
- Add field-level or collection `beforeChange` enforcement for `role`, `isActive`, and `collegeScope`.
- Add `admin` access control so only coordinator and above can access Payload admin, matching `canAccessAdmin`.
- Keep `role.saveToJWT` for efficient access checks, but refresh tokens or require re-login after role changes so stale role claims do not persist for a week.

Risk: with `update: isAuthenticated`, any authenticated user may be able to update their own or other user documents unless hooks/field access block sensitive fields. This is the most important Phase 1 hardening issue.

## Admin Restrictions

Payload's admin panel responds to access control by hiding collections and actions that users cannot perform. This is useful but must be treated as UX, not the primary security boundary; the same access and hooks must secure API paths.

Decision:

- Use Payload access functions as the source of truth.
- Add admin grouping/visibility only after API-level access is correct.
- Restrict sensitive collections/globals:
  - `users`, `site-settings`, `contact-settings`, `smart-chat-settings`: admin and above.
  - public content collections: public read of published records; create/update/delete only content manager and above, or coordinator scoped to their college where applicable.
  - form submissions such as contact/join requests: public create if needed, no public read/update/delete.

## Puck and `@delmaredigital/payload-puck`

Delmare's docs say `createPuckPlugin`:

- adds or creates a pages collection with Puck fields,
- registers endpoints at `/api/puck/:collection`,
- adds admin editor views at `/admin/puck-editor/:collection/:id`,
- adds "Edit with Puck" buttons.

The docs require:

- `@puckeditor/core >= 0.21.0`
- `payload >= 3.69.0`
- `@payloadcms/next >= 3.69.0`
- `next >= 15.4.8`
- `react` / `react-dom >= 19.2.1`
- `tailwindcss >= 3.0.0 || >= 4.0.0`

The repo satisfies these with the lockfile versions above.

Security finding:

- GHSA-65w6-pf7x-5g85 / CVE-2026-39397 affected `@delmaredigital/payload-puck < 0.6.23`.
- The issue was that `/api/puck/*` CRUD handlers called Payload Local API with default `overrideAccess: true`, bypassing collection access.
- The repo is locked to `0.6.29`, so it is on the patched line.

Decision:

- It is acceptable to enable `createPuckPlugin` after adding access tests.
- Do not enable Puck before tests prove unauthenticated and low-role users cannot write through `/api/puck/pages`.
- Pass explicit plugin access options if supported by the current plugin API, and still keep collection-level access on `pages`.
- Treat Puck layout JSON as executable-ish presentation data: validate component allowlists, do not render arbitrary HTML/script, and keep custom components pure and server-safe.

Puck data model note:

- Puck stores `content`, `root`, and historically `zones`.
- Puck 0.21 introduced slots as the preferred nested content model; `zones` are legacy and have migration helpers.

Decision: define new AAU Puck components using slot fields, not new DropZones, to avoid creating legacy `zones` data.

## `/api/puck` Access Test Matrix

Add integration tests before enabling the plugin:

| Request | Anonymous | Student/doctor | Coordinator | Content manager | Admin/super admin |
| --- | --- | --- | --- | --- | --- |
| `GET /api/puck/pages/:id` for published | allowed if public render needs it | allowed | allowed | allowed | allowed |
| `GET /api/puck/pages/:id` for draft | denied | denied | scoped only if college ownership exists | allowed | allowed |
| `POST /api/puck/pages` | denied | denied | scoped only if allowed | allowed | allowed |
| `PATCH /api/puck/pages/:id` | denied | denied | scoped only if allowed | allowed | allowed |
| `DELETE /api/puck/pages/:id` | denied | denied | denied unless explicitly required | admin only | admin only |

If the plugin endpoints do not expose exactly these methods/URLs in the current package, adapt the tests to the routes it registers and keep the role expectations.

## Payload Access Tests

Recommended tests with Vitest:

- Unit-test access helpers in `src/access/helpers.ts`, especially role hierarchy and `canManageUserRole`.
- Use Payload Local API with `overrideAccess: false` to test collection permissions for representative users.
- Test role changes through Local API and REST routes:
  - student cannot set `role=admin`;
  - admin cannot create/update `super_admin`;
  - admin can manage lower roles;
  - super_admin can manage all roles;
  - user cannot disable themselves unless explicitly intended.
- Test public read filters only return `isPublished: true`.

Important: any Local API test that simulates an end user must set `overrideAccess: false`; otherwise the test can pass while bypassing the real security policy.

## Next Route Handler Tests

Next route handlers use standard Web `Request` and `Response` APIs. The project already has Vitest and Playwright configured.

Decision:

- For custom route handlers, extract auth/rate-limit/business logic into testable functions and cover those with Vitest in a Node environment.
- Add route-handler integration tests for important APIs by importing `GET`/`POST` functions and passing `Request` objects where practical.
- Use Playwright for full end-to-end admin/content flows where Payload cookies/session state matters.
- Do not manually edit generated Payload route files for tests; test through HTTP or through Payload APIs.

## Rate Limiting

Next docs recommend code-based checks plus host-provided rate limiting. Vercel now has WAF rate limiting and an SDK, and Upstash Rate Limit is designed for serverless/edge environments.

Decision:

- Apply rate limits to unauthenticated mutation endpoints: contact messages, join requests, auth login, password reset, and any future public Puck-adjacent endpoint.
- For Vercel deployments, prefer Vercel WAF rate limiting for coarse IP/path protection and `@vercel/firewall` only where application-level keys are needed.
- For VPS or non-Vercel deployments, use a shared store such as Redis/Upstash. Avoid in-memory rate limits for production multi-instance deployments.

Risk: Payload's generated REST endpoints do not automatically inherit a custom Next route-handler rate limiter. Use Payload access/hooks for authorization and platform/proxy/WAF controls for broad rate limiting.

## Decisions

1. Keep Payload v3.85.1 / `@payloadcms/*` v3.85.1 and `@delmaredigital/payload-puck` 0.6.29 for Phase 1; do not jump to Next 16 until a separate compatibility pass.
2. Freeze `latest` dependency ranges before more hardening or Puck enablement.
3. Treat `Users.update: isAuthenticated` as unsafe until role/field restrictions and tests are added.
4. Enable Puck only after `/api/puck` access tests are in place.
5. Use direct Neon URLs for migrations and pooled Neon URLs for runtime app traffic.
6. Use Payload collection/global access as the security source of truth; admin hiding is secondary.

## Risks

- Role escalation through weak user update rules.
- Stale JWT role claims after role changes because role is saved to JWT and token expiration is seven days.
- Puck plugin endpoints are patched in `0.6.29`, but regressions would be high impact; tests must lock the expectation.
- Broad dependency `latest` ranges can move the stack unexpectedly on reinstall.
- Neon pooled URLs are good for runtime but risky for migration/admin scripts.
- Puck layout data can break rendering if component names/props change without migrations.

## Implementation Checklist

- [ ] Pin dependencies in `package.json` to known-good ranges from the current lockfile.
- [ ] Add `Users` collection access and hooks for role escalation prevention.
- [ ] Wire admin access to `canAccessAdmin`.
- [ ] Add field-level restrictions for `role`, `isActive`, and `collegeScope`.
- [ ] Add tests for access helpers and Local API operations with `overrideAccess: false`.
- [ ] Add route/integration tests for generated Payload REST behavior where feasible.
- [ ] Enable `createPuckPlugin` only after tests cover `/api/puck` anonymous and low-role denial.
- [ ] Define Puck components with slot fields and a strict component allowlist.
- [ ] Add a migration workflow using direct Neon connection strings.
- [ ] Add production rate limiting at WAF/proxy level plus route-specific checks for public mutation routes.
- [ ] Remove or repurpose `src/app/my-route/route.ts`.

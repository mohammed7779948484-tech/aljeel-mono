# Tech Research — Phase B (Subagent 1)

> Fresh research conducted before any coding to verify current best practices, package versions, and compatibility.

## Sources Checked

1. Payload CMS official documentation (payloadcms.com/docs)
2. Payload CMS GitHub (github.com/payloadcms/payload)
3. NPM package registries
4. Delmare Digital documentation (delmaredigital.github.io/payload-puck)
5. Puck Editor official docs (puckeditor.com)
6. Neon PostgreSQL documentation (neon.tech)
7. Community discussions (Reddit, Discord)
8. `PLUGINS_ECOSYSTEM.md` from workspace
9. Payload CMS skill reference documents

---

## 1. Payload CMS Setup

### Current Version & Setup Command

```bash
npx create-payload-app@latest
```

**Interactive prompts:**
- Project name → `aau-payload-platform`
- Template → `blank`
- Database → `PostgreSQL`
- Connection string → skip (will use `.env.example`)

**Non-interactive alternative:**
```bash
npx create-payload-app@latest ./aau-payload-platform --db postgres --template blank
```

### Core Packages

| Package | Purpose |
|---------|---------|
| `payload` | Core CMS framework |
| `@payloadcms/next` | Next.js integration + admin panel |
| `@payloadcms/db-postgres` | PostgreSQL adapter (Drizzle ORM) |
| `@payloadcms/richtext-lexical` | Rich text editor |

### Key Configuration

```typescript
// payload.config.ts
import { buildConfig } from 'payload'
import { postgresAdapter } from '@payloadcms/db-postgres'
import { lexicalEditor } from '@payloadcms/richtext-lexical'

export default buildConfig({
  db: postgresAdapter({
    pool: {
      connectionString: process.env.DATABASE_URI,
    },
  }),
  editor: lexicalEditor(),
  secret: process.env.PAYLOAD_SECRET!,
  // ...
})
```

### Payload + Next.js Integration

```javascript
// next.config.mjs
import { withPayload } from '@payloadcms/next/withPayload'
export default withPayload(nextConfig)
```

### Migrations with Drizzle

- Payload v3 uses Drizzle ORM for PostgreSQL
- `push: true` in dev config auto-syncs schema
- For production: `payload migrate:create` then `payload migrate`
- Neon-compatible — standard PostgreSQL protocol

### Local API

```typescript
import { getPayload } from 'payload'
import config from '@payload-config'

const payload = await getPayload({ config })
const { docs } = await payload.find({ collection: 'news' })
```

**Critical security note:** Local API bypasses access control by default. Use `overrideAccess: false` when operating on behalf of users.

---

## 2. Puck Visual Editor

### Package Migration

Puck has migrated from `@measured/puck` to `@puckeditor/core` (v0.21.0+).

```bash
pnpm add @puckeditor/core
```

### Puck Data Model

Puck stores page layout as JSON:
```json
{
  "content": [...],
  "root": { "props": { "title": "..." } },
  "zones": {}
}
```

### Puck + Next.js

- Server-side: `<Render config={puckConfig} data={puckData} />`
- Editor: `<Puck config={puckConfig} data={puckData} onPublish={...} />`

---

## 3. Payload + Puck Integration

### Selected: `@delmaredigital/payload-puck`

**Version:** 0.6.23 (latest as of June 2026)

**Requirements:**
- `@puckeditor/core` ≥ 0.21.0
- `payload` ≥ 3.69.0
- `@payloadcms/next` ≥ 3.69.0
- `next` ≥ 15.4.8
- `react` ≥ 19.2.1

**Installation:**
```bash
pnpm add @delmaredigital/payload-puck @puckeditor/core
```

**Configuration:**
```typescript
import { createPuckPlugin } from '@delmaredigital/payload-puck/plugin'

export default buildConfig({
  plugins: [
    createPuckPlugin({ pagesCollection: 'pages' }),
  ],
})
```

**Features:**
- 15+ built-in components
- Editor inside Payload Admin at `/admin/puck-editor/:collection/:id`
- API endpoints on `/api/puck/:collection`
- "Edit with Puck" button in list view
- Hybrid rendering (Blocks + Puck)
- Theming + Dark Mode
- Layouts/Templates

**Security Warning (CVE-2026-39397):**
Authorization gaps in CRUD endpoints registered by `createPuckPlugin()`. Mitigation: add explicit access control rules on the `pages` collection.

### Rejected: `dd-starter`

**Reason:** Too opinionated. Bundles Better Auth, specific auth flows, and page-tree patterns that may conflict with our RBAC design. Better to use official starter and add payload-puck manually.

### Rejected: `puckload-poc`

**Reason:** Proof of concept only, not production-ready.

---

## 4. Official Payload Plugins

### Phase 1 Plugins

| Plugin | Version | Purpose | Install |
|--------|---------|---------|---------|
| `@payloadcms/plugin-seo` | Latest | SEO fields on pages/collections | Yes |
| `@payloadcms/plugin-redirects` | Latest | URL redirect management | Yes |

### Phase 2 Plugins (NOT installed now)

| Plugin | Purpose | When |
|--------|---------|------|
| `@payloadcms/plugin-form-builder` | Dynamic forms | When contact/join forms migrate fully |
| `@payloadcms/plugin-search` | Unified search | When search feature is built |
| `@payloadcms/plugin-nested-docs` | Hierarchical pages | If page tree is needed |
| `@payloadcms/plugin-import-export` | CSV/JSON import | When data migration begins |
| `@payloadcms/storage-s3` | S3 media storage | When moving to cloud storage |

### NOT Installing

| Plugin/Service | Reason |
|---------------|--------|
| Novu | Future notification service |
| Typesense | Future search engine |
| Chatwoot | Future chat service |
| Documenso | Future document signing |
| OpenFGA/Cerbos | Future advanced auth (RBAC helpers sufficient for now) |
| payload-ai | Future AI content generation |
| authsmith | Future OAuth/SSO |
| payload-ecommerce | Not relevant |

---

## 5. Neon PostgreSQL Configuration

### Connection String Format

```
postgresql://<user>:<password>@<endpoint>.neon.tech:<port>/<dbname>?sslmode=require
```

### Environment Variables

```env
DATABASE_URI=postgresql://...?sslmode=require
PAYLOAD_SECRET=<random-32-char-string>
NEXT_PUBLIC_SERVER_URL=http://localhost:3000
```

### Payload Config

```typescript
db: postgresAdapter({
  pool: {
    connectionString: process.env.DATABASE_URI,
  },
  push: process.env.NODE_ENV === 'development',
}),
```

### Migration Commands

```bash
# Generate migration after schema changes
pnpm payload migrate:create

# Run pending migrations
pnpm payload migrate

# Generate TypeScript types
pnpm payload generate:types
```

---

## 6. VPS Deployment Strategy

### Node.js Process Manager (PM2)

```bash
# Build
pnpm build

# Start with PM2
pm2 start npm --name "aau-platform" -- start
```

### File Uploads

- **Phase 1:** Local filesystem uploads on VPS
- **Phase 2:** S3-compatible storage via `@payloadcms/storage-s3`

### Required Environment Variables on VPS

```env
DATABASE_URI=postgresql://...@neon.tech/...?sslmode=require
PAYLOAD_SECRET=<generated-secret>
NEXT_PUBLIC_SERVER_URL=https://your-domain.com
NODE_ENV=production
```

---

## 7. Final Setup Recommendation

| Decision | Choice |
|----------|--------|
| **Starter** | Official `create-payload-app@latest` with blank + PostgreSQL |
| **Package manager** | pnpm |
| **Database** | Neon PostgreSQL via `DATABASE_URI` |
| **Rich text** | Lexical editor (Payload default) |
| **Puck** | `@delmaredigital/payload-puck` + `@puckeditor/core` |
| **SEO** | `@payloadcms/plugin-seo` |
| **Redirects** | `@payloadcms/plugin-redirects` |
| **Auth** | Payload built-in email/password (Phase 1) |
| **File storage** | Local filesystem (Phase 1) |
| **React** | 19.x (required by payload-puck) |
| **Next.js** | 15.x+ (required by payload-puck) |
| **TypeScript** | Strict mode |

### Risks

1. **CVE-2026-39397** — payload-puck authorization gap → Mitigated by explicit access control
2. **React 19** — Breaking changes from React 18 → New project, no legacy concerns
3. **Neon cold starts** — Serverless DB may have latency → Acceptable for Phase 1
4. **payload-puck maturity** — Community plugin, not official → Actively maintained, MIT license, good ecosystem support

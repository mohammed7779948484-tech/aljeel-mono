# Neon / VPS Deployment Plan

> Deployment foundation for Phase 1 using Neon PostgreSQL on VPS.

## Neon Database Setup

### Requirements

1. Create a Neon project at https://neon.tech
2. Create a database (e.g., `aau_platform`)
3. Copy the connection string with `?sslmode=require`

### Connection String Format

```
postgresql://<user>:<password>@<endpoint>.neon.tech:5432/<dbname>?sslmode=require
```

### Environment Variables

```env
# Database
DATABASE_URI=postgresql://user:password@ep-xxx.neon.tech:5432/aau_platform?sslmode=require

# Payload
PAYLOAD_SECRET=<openssl rand -base64 32>
NEXT_PUBLIC_SERVER_URL=https://your-domain.com

# Runtime
NODE_ENV=production
```

---

## VPS Deployment

### Prerequisites

- Node.js 20.9.0+
- pnpm (globally installed)
- PM2 or systemd for process management
- Nginx for reverse proxy (optional)

### Build & Deploy

```bash
# Install dependencies
pnpm install

# Run migrations (after DATABASE_URI is set)
pnpm payload migrate

# Build production bundle
pnpm build

# Start with PM2
pm2 start npm --name "aau-platform" -- start

# Or with systemd
# Create /etc/systemd/system/aau-platform.service
```

### Nginx Reverse Proxy (Optional)

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## File Upload Strategy

### Phase 1: Local Filesystem

- Payload stores uploads in `./media/` directory on VPS
- Backed up via rsync or VPS snapshots
- Simple, no external dependencies

### Phase 2: S3-Compatible Storage

```bash
pnpm add @payloadcms/storage-s3
```

```typescript
// payload.config.ts
import { s3Storage } from '@payloadcms/storage-s3'

plugins: [
  s3Storage({
    collections: { media: true },
    bucket: 'aau-media',
    config: {
      credentials: {
        accessKeyId: process.env.S3_ACCESS_KEY,
        secretAccessKey: process.env.S3_SECRET_KEY,
      },
      region: process.env.S3_REGION,
      endpoint: process.env.S3_ENDPOINT, // For MinIO/R2
    },
  }),
]
```

---

## Payload Migration Commands

```bash
# Generate migration after schema changes
pnpm payload migrate:create

# Run pending migrations
pnpm payload migrate

# Check migration status
pnpm payload migrate:status

# Generate TypeScript types
pnpm payload generate:types
```

---

## Backup Strategy

### Database (Neon)

- **Automatic:** Neon provides point-in-time recovery (PITR)
- **Manual:** `pg_dump` for explicit snapshots
- **Branching:** Neon supports database branching for testing

### Media Uploads

- **Phase 1:** VPS filesystem backup via rsync/cron
- **Phase 2:** S3-compatible storage with bucket versioning

---

## Future Optional Services

| Service | Purpose | When |
|---------|---------|------|
| **Novu** | Push/email/SMS notifications | When notification system needed |
| **Typesense** | Full-text search engine | When search needs exceed Payload plugin |
| **Chatwoot** | Live chat / help desk | When support system needed |
| **Documenso** | Document signing | When admission docs need signatures |
| **OpenFGA/Cerbos** | Advanced authorization | When RBAC complexity exceeds Payload access control |
| **MinIO** | S3-compatible self-hosted storage | When local S3 is preferred over cloud |

---

## Required Environment Variables Summary

| Variable | Required | Description |
|----------|----------|-------------|
| `DATABASE_URI` | ✅ | Neon PostgreSQL connection string |
| `PAYLOAD_SECRET` | ✅ | JWT/encryption secret (32+ chars) |
| `NEXT_PUBLIC_SERVER_URL` | ✅ | Public URL of the application |
| `NODE_ENV` | ✅ | `production` for deployment |
| `S3_ACCESS_KEY` | Phase 2 | S3 storage access key |
| `S3_SECRET_KEY` | Phase 2 | S3 storage secret key |
| `S3_BUCKET` | Phase 2 | S3 bucket name |
| `S3_REGION` | Phase 2 | S3 region |
| `S3_ENDPOINT` | Phase 2 | S3 endpoint (for MinIO/R2) |

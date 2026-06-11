# AAU University — Source Inventory

> Generated during workspace discovery phase of migration.

## Current Working Directory

`c:\Users\M\Desktop\aljeel`

## Detected Sources

### Frappe Backend

| Item | Path | Details |
|------|------|---------|
| Backend root | `aau_university/` | Frappe app module |
| DocTypes | `aau_university/aau/doctype/` | 47 DocType folders |
| API v1 | `aau_university/api/v1/` | 11 API modules (~450KB total) |
| Hooks | `aau_university/hooks.py` | Route registration, CORS, permission hooks |
| Patches | `aau_university/patches/` | 14 migration patches (v1_0 through v1_13) |
| patches.txt | `aau_university/patches.txt` | Patch registry |
| Setup | `aau_university/setup/` | 11 setup/seed scripts |
| Docs | `aau_university/docs/` | API spec, schema docs, DType spec JSON |
| Content Access | `aau_university/content_access.py` | Content permission helpers |
| Utils | `aau_university/utils/` | CORS, helpers |
| Seeds | `aau_university/seed/` | Seed data scripts |
| Scripts | `aau_university/scripts/` | Utility scripts |

### Frontend

| Item | Path | Details |
|------|------|---------|
| Frontend root | `frontend/` | Next.js 16.1.3 project |
| Package manager | npm (package-lock.json) | 330KB lockfile |
| Framework | Next.js 16 + React 18 | TypeScript |
| Styling | Tailwind 3.4 + shadcn/ui | HSL design tokens |
| Animations | framer-motion 12.x | Fade, slide, scale animations |
| State | @tanstack/react-query 5.x | API data fetching |
| Components | `frontend/components/` | 42 files + 6 subdirectories |
| Public routes | `frontend/app/(public)/` | 19 route folders |
| Auth routes | `frontend/app/(auth)/` | Auth pages |
| Admin routes | `frontend/app/admin/` | Dashboard pages |
| Services | `frontend/services/` | API client services |
| Hooks | `frontend/hooks/` | Custom React hooks |
| Types | `frontend/types/` | TypeScript type definitions |
| Config | `frontend/config/` | App configuration |
| Data | `frontend/data/` | Static data files |

### Documentation Files

| File | Size | Description |
|------|------|-------------|
| `database-documentation.md` | 93KB | All 47 DocTypes with field definitions |
| `aau_initial_roles_permissions_payload_refine_ar.md` | 3KB | RBAC plan in Arabic |
| `PLUGINS_ECOSYSTEM.md` | 15KB | 246+ Payload plugins catalogue |
| `aau_university/docs/API_SPECIFICATION.md` | 47KB | Full API spec |
| `aau_university/docs/DATABASE_SCHEMA_AND_API.md` | 35KB | Schema + API mapping |
| `aau_university/docs/aau_doctypes_spec.json` | 100KB | DocType specifications |

### AI Agent Skills

| Skill | Path | Relevance |
|-------|------|-----------|
| Payload CMS | `.agents/skills/payload/` | Primary — collections, access, hooks |
| Frappe | `.agents/skills/frappe-agent-skills/` | Audit reference only |

### Missing / Not Found

- No existing `aau-payload-platform/` project
- No schema comparison reports (will be generated)
- No migration reports (will be generated)
- No Puck-specific skills
- No deployment configuration files

## Assumptions

1. The Frappe backend is the source of truth for data schema.
2. The frontend is the source of truth for UI/UX patterns.
3. `database-documentation.md` is aligned with the backend but may have minor gaps.
4. No Neon PostgreSQL credentials are available yet — will use `.env.example` placeholders.
5. The new project will be created as `aau-payload-platform/` inside this workspace.

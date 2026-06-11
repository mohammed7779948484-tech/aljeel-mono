# Skills Inventory — Phase A (Subagent 2)

> Inspected AI agent skills available in the workspace to determine relevance for the migration project.

## Skills Found

### 1. Payload CMS Skill ✅ RELEVANT

**Path:** `.agents/skills/payload/SKILL.md`  
**Size:** 471 lines, comprehensive reference

**What it provides:**
- Collection configuration patterns (auth, upload, drafts, live preview)
- Field types reference (text, relationship, richText, select, upload, array, blocks, point, join, virtual, conditional)
- Hook patterns (collection-level vs field-level, context flags, avoiding infinite loops)
- Access control patterns (type-safe, row-level, field-level, RBAC, multi-tenant)
- Query patterns (Local API, REST, GraphQL, nested properties, AND/OR logic)
- Custom endpoints
- Plugin development
- Database adapter configuration
- Security pitfalls (Local API access control bypass, transaction failures, hook loops)
- Best practices (security, performance, data integrity, type safety, organization)

**Reference documents available:**
- `reference/FIELDS.md` — All field types
- `reference/COLLECTIONS.md` — Collection configs
- `reference/HOOKS.md` — Hook patterns
- `reference/ACCESS-CONTROL.md` — Access control
- `reference/ACCESS-CONTROL-ADVANCED.md` — Advanced access patterns
- `reference/QUERIES.md` — Query operators
- `reference/ENDPOINTS.md` — Custom endpoints
- `reference/ADAPTERS.md` — Database/storage adapters
- `reference/ADVANCED.md` — Auth, jobs, plugins, localization
- `reference/PLUGIN-DEVELOPMENT.md` — Plugin architecture

**How we will use it:**
- Primary reference for writing all Payload collection configs
- Reference for access control functions (RBAC helpers)
- Reference for hook patterns (slug generation, computed fields)
- Reference for query patterns in API routes
- Security pitfall awareness (overrideAccess, req threading, hook loops)
- Type safety patterns (generated types, type annotations)

---

### 2. Frappe Agent Skill ⚠️ AUDIT REFERENCE ONLY

**Path:** `.agents/skills/frappe-agent-skills/SKILL.md`  
**Size:** 51 lines, flow-selection skill

**What it provides:**
- DocType structure understanding
- Controller/lifecycle hook patterns
- Permission/role system understanding
- API endpoint patterns
- Database/ORM patterns

**Reference documents available:**
- `references/doctypes.md` — DocType structure
- `references/controllers.md` — Document lifecycle
- `references/api.md` — Whitelisted APIs
- `references/permissions.md` — Roles, permissions
- `references/hooks.md` — hooks.py patterns
- `references/database.md` — frappe.db, queries

**How we will use it:**
- Understanding Frappe DocType JSON structure for audit
- Understanding Frappe permission model for RBAC translation
- Understanding hooks.py for identifying business logic to migrate
- Understanding API patterns for mapping to Payload endpoints

**Limitations:**
- We will NOT run any Frappe bench commands
- We will NOT modify any Frappe files
- We will NOT create new Frappe DocTypes
- This skill is used purely for reading comprehension during audit

---

## Skills NOT Found (Gaps)

| Missing Skill | Impact | Mitigation |
|--------------|--------|------------|
| Puck visual editor | No structured Puck guidance | Use PLUGINS_ECOSYSTEM.md + web research + plugin docs |
| Database design | No schema design patterns | Use Payload skill + best practices from research |
| Migration patterns | No migration workflow guidance | Custom approach documented in migration docs |
| Clean architecture | No architecture skill | Apply standard patterns from experience |
| RBAC design | No RBAC-specific skill | Use Payload ACCESS-CONTROL.md reference + roles doc |
| Testing | No testing skill | Use Payload patterns + standard Next.js testing |
| VPS deployment | No deployment skill | Use web research + Neon docs |

## Decision

- **Use Payload skill extensively** — it is the primary technical reference for all collection, access, hook, and query implementations
- **Use Frappe skill for read-only audit** — understand DocType structure, permissions, and hooks during backend audit
- **Do not blindly follow skills** — cross-reference with current package versions and project-specific needs
- **Fill gaps with web research** — Puck setup, Neon deployment, React 19 compatibility verified through search

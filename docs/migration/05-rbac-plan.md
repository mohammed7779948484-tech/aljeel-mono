# RBAC Plan — Phase G (Subagent 7)

> Role-Based Access Control design for Phase 1 of the new platform.

## Role Hierarchy

```
super_admin (level 5)
  └── admin (level 4)
      └── content_manager (level 3)
          └── coordinator (level 2)
              ├── student (level 1) — no admin
              └── doctor (level 1) — no admin
```

## Phase 1 Roles

### `super_admin` (Level 5)

- Full access to everything
- Can create/edit/delete any user
- Can assign any role
- Can manage all collections and globals
- Can access Payload Admin
- Future: full Refine Control Center access

### `admin` (Level 4)

- General administrative access
- Can manage most collections and globals
- Cannot create super_admin users
- Cannot exceed super_admin privileges
- Can access Payload Admin

### `content_manager` (Level 3)

- Full content management access
- Can create/edit/delete content in all collections
- Can manage media
- Cannot create roles
- Cannot create users with role ≥ content_manager
- Can create users with role coordinator, student, doctor only
- Can access Payload Admin

### `coordinator` (Level 2)

- **Scoped to one college** via `collegeScope` field on user
- Can manage content related to assigned college only
- Can manage programs, departments, faculty for assigned college
- Cannot edit global settings
- Cannot edit other colleges
- Cannot create roles
- Cannot create high-level users
- Limited Payload Admin access (college-scoped)

### `student` (Level 1)

- **No Payload Admin access**
- Future: custom Next.js student portal
- Can view own data only
- Phase 2 implementation

### `doctor` (Level 1)

- **No Payload Admin access** (unless also has admin/content role)
- Future: custom Next.js doctor portal
- Can view own courses, students, grades
- Phase 2 implementation

---

## Access Helper Functions

### File: `src/payload/access/roles.ts`

```typescript
// Role level constants
export const ROLE_LEVELS = {
  super_admin: 5,
  admin: 4,
  content_manager: 3,
  coordinator: 2,
  student: 1,
  doctor: 1,
} as const

export type UserRole = keyof typeof ROLE_LEVELS
```

### File: `src/payload/access/helpers.ts`

```typescript
// Role checking functions
isSuperAdmin(user): boolean
isAdmin(user): boolean
isContentManager(user): boolean
isCoordinator(user): boolean
isStudent(user): boolean
isDoctor(user): boolean
hasRole(user, role): boolean
hasRoleLevelAtLeast(user, level): boolean

// Permission functions
canManageUserRole(currentUser, targetRole): boolean
isSameCollegeScope(user, collegeId): boolean
canManageCollegeContent(user, collegeId): boolean
isAdminOrAbove(user): boolean
canAccessAdmin(user): boolean
```

### File: `src/payload/access/collections.ts`

```typescript
// Reusable access control functions for collections
export const isAuthenticated: Access
export const isAdminOrAbove: Access
export const isContentManagerOrAbove: Access
export const isPublishedOrAdmin: Access
export const canManageOwnCollege: Access  // For coordinator scoping
export const publicReadOnly: Access       // Public read, admin write
```

---

## Collection Access Rules

### Phase 1 Access Matrix

| Collection | Create | Read | Update | Delete |
|-----------|--------|------|--------|--------|
| users | admin+ | admin+ (own for all) | admin+ (own for all) | super_admin |
| media | content_manager+ | public | content_manager+ | content_manager+ |
| pages | content_manager+ | published=public | content_manager+ | content_manager+ |
| navigation-menus | admin+ | public | admin+ | admin+ |
| news | content_manager+ | published=public | content_manager+ coordinator(own college) | content_manager+ |
| colleges | admin+ | public | admin+ coordinator(own) | super_admin |
| academic-departments | admin+ | public | admin+ coordinator(own college) | admin+ |
| academic-programs | admin+ | public | admin+ coordinator(own college) | admin+ |
| faculty-members | content_manager+ | public | content_manager+ coordinator(own college) | content_manager+ |
| contact-messages | public(create) | content_manager+ | content_manager+ | admin+ |
| join-requests | public(create) | content_manager+ | content_manager+ | admin+ |

### Coordinator Scoping Logic

```typescript
// When user.role === 'coordinator' and user.collegeScope exists:
// - Read: only documents where college === user.collegeScope
// - Update: only documents where college === user.collegeScope
// - Create: only if college field matches user.collegeScope
// - Delete: denied (or scoped — depends on collection)
```

---

## User Creation Rules

| Creator Role | Can Create |
|-------------|-----------|
| super_admin | Any role |
| admin | admin, content_manager, coordinator, student, doctor |
| content_manager | coordinator, student, doctor |
| coordinator | Cannot create users |
| student | Cannot create users |
| doctor | Cannot create users |

**Escalation prevention:** Enforced server-side in `beforeChange` hook on users collection.

---

## Implementation Notes

1. **Role is stored on user document** — `role` field with select type
2. **Role is saved to JWT** — `saveToJWT: true` for the role field to avoid DB lookups on every request
3. **collegeScope is on user** — relationship to colleges, required only for coordinator role
4. **Access control is server-side** — Payload access functions, not UI-only hiding
5. **Admin panel access** — controlled via `admin.hidden` on user role check; student/doctor roles see 403
6. **Seed script** — creates initial super_admin user on first run

---

## Deferred Roles (Phase 2+)

- `admissions_officer` — Admissions pipeline management
- `academic_manager` — Academic program oversight
- `finance_officer` — Fee/payment management
- `support_agent` — Help desk
- `executive_viewer` — Read-only dashboards
- `content_editor` — Limited content editing (below content_manager)
- `blog_editor` — Blog-only editing
- `registrar` — Student records
- `guardian` — Parent portal access

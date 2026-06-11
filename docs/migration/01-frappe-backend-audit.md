# Frappe Backend Audit — Phase C (Subagent 3)

> Deep inspection of the Frappe backend to classify all DocTypes and API usage for migration decisions.

## Backend Structure

```
aau_university/
├── aau/doctype/           # 47 DocType folders
├── api/v1/                # 11 API modules (~450KB)
├── setup/                 # 11 setup/seed scripts
├── patches/               # 14 migration patches
├── docs/                  # Specs and documentation
├── hooks.py               # Route registration, CORS, permissions
├── content_access.py      # Content permission helpers
├── utils/                 # CORS, utilities
├── seed/                  # Seed data
└── patches.txt            # Patch registry
```

---

## All 47 DocTypes — Classification

### Single DocTypes (Settings) — 6 total

| DocType | Fields | API Used | Migration Decision |
|---------|--------|----------|-------------------|
| **Home Page** | 62 | ✅ `public.get_home` | **Payload Global** `home-page` |
| **Website Settings** | 93 | ✅ `cms.list_settings` | **Payload Global** `site-settings` (cleaned) |
| **About University** | 43 | ✅ `public.get_about_page` | **Payload Global** `about-page` |
| **Contact Page Settings** | 39 | ✅ `public.get_contact_page` | **Payload Global** `contact-settings` |
| **Smart Chat Settings** | 13 | ✅ smartchat API | **Payload Global** `smart-chat-settings` |
| **Dashboard Metrics** | 3 | Limited | Phase 2 — operational metrics |

### Content Collections — 11 total

| DocType | Fields | API Used | Detail Page | Search/Filter | Migration Decision |
|---------|--------|----------|-------------|---------------|-------------------|
| **News** | 21 | ✅ full CRUD + views | ✅ | ✅ | **Phase 1 Collection** |
| **Events** | 12 | ✅ full CRUD + upcoming | ✅ | ✅ | Phase 2 Collection |
| **Blog Posts** | 24 | ✅ full CRUD + categories | ✅ | ✅ | Phase 2 Collection |
| **FAQ** | 10 | ✅ CRUD + categories | ❌ | ✅ | Phase 2 Collection |
| **Campus Life** | 25 | ✅ full CRUD | ✅ | ✅ | Phase 2 Collection |
| **Partners** | 7 | ✅ CRUD + types | ❌ | ✅ | Phase 2 Collection |
| **Offers** | 44 | ✅ full CRUD + active | ✅ | ✅ | Phase 2 Collection |
| **Projects** | 16 | ✅ full CRUD | ✅ | ✅ | Phase 2 Collection |
| **Research and Publications** | 8 | ✅ list + detail | ✅ | ✅ | Phase 2 Collection |
| **Student Activities** | 7 | ✅ public list | ❌ | ❌ | Phase 2 Collection |
| **Student Affairs Document** | 7 | ✅ public list | ❌ | ❌ | Phase 2 Collection |

### Academic Collections — 7 total

| DocType | Fields | API Used | Relationships | Migration Decision |
|---------|--------|----------|--------------|-------------------|
| **Colleges** | 32 | ✅ full CRUD + programs/faculty/dean | → Programs, Departments, Faculty | **Phase 1 Collection** |
| **Academic Departments** | 6 | ✅ list | → Colleges | **Phase 1 Collection** |
| **Academic Programs** | 29 | ✅ full CRUD + objectives/faculty | → Colleges | **Phase 1 Collection** |
| **Faculty Members** | 21 | ✅ full CRUD + search/filter | → Colleges, Programs, Departments + 4 child tables | **Phase 1 Collection** |
| **Admission Requirements** | 7 | Limited | None | Phase 2 Collection |
| **Study Plans** | 7 | ✅ via programs | → Academic Programs | Phase 2 Collection |
| **Study Plan Courses** | 7 | ✅ via study plans | → Study Plans | Phase 2 Collection |

### Navigation & Pages — 5 total

| DocType | Fields | API Used | Migration Decision |
|---------|--------|----------|-------------------|
| **AAU Menu** | 3 | ✅ `public.get_public_menu` | **Phase 1 Collection** `navigation-menus` |
| **AAU Menu Item** | 6 (child) | ✅ via menu | Array field in navigation-menus |
| **AAU Page** | 8 | ✅ `public.get_public_page` | **Merged → Phase 1 Collection** `pages` |
| **Pages** | 8 | ✅ `content.list_pages` | **Merged → Phase 1 Collection** `pages` |
| **Static Page** | 6 | Limited | **Merged → Phase 1 Collection** `pages` |

### Forms & Requests — 3 total

| DocType | Fields | API Used | Migration Decision |
|---------|--------|----------|-------------------|
| **Contact Us Messages** | 10 | ✅ full CRUD + status | **Phase 1 Collection** |
| **Join Requests** | 20 | ✅ full CRUD + status | **Phase 1 Collection** |
| **Email Requests** | 9 | ✅ full CRUD + status | Phase 2 Collection |

### Faculty Child Tables — 4 total

| DocType | Fields | Parent | Migration Decision |
|---------|--------|--------|-------------------|
| **Faculty Course** | 4 | Faculty Members | Array field in `faculty-members` |
| **Faculty Education** | 5 | Faculty Members | Array field in `faculty-members` |
| **Faculty Experience** | 6 | Faculty Members | Array field in `faculty-members` |
| **Faculty Publication** | 5 | Faculty Members | Array field in `faculty-members` |

### Other — 6 total

| DocType | Fields | API Used | Migration Decision |
|---------|--------|----------|-------------------|
| **Slider** | 8 | Limited | **Puck HeroBlock** (no collection needed) |
| **About Team Member** | 8 (child) | Via About University | Array field in `about-page` global |
| **Team Members** | 12 | ✅ CRUD | Phase 2 Collection |
| **Announcement** | 6 | Limited | Phase 2 (merge with Announcements) |
| **Announcements** | 7 | Limited | Phase 2 (duplicate — merge) |
| **Media Library** | 7 | ✅ `cms.*` | **Replaced by Payload `media`** |
| **Center Services** | 7 (child) | Via Centers | Phase 2 array field |
| **University Centers** | 7 | ✅ | Phase 2 (merge with Centers) |
| **Student Portal Survey Response** | 13 | ✅ portal | Phase 2 — student portal |
| **Grade** | 0 | ❌ | **Do not migrate** (empty placeholder) |

---

## API Modules Analysis

### `public.py` (116KB) — Public website endpoints
- Home page data, about page, contact page
- Public news/events/colleges listing
- Student affairs docs, menus, site profile
- **Phase 1 relevant:** Most endpoints map to Phase 1 collections/globals

### `portal.py` (110KB) — Student/Doctor portal
- Doctor: profile, courses, students, grades, schedule, finance, notifications, messages, materials, announcements
- Student: profile, courses, schedule, grades, finance, materials, announcements, admission requests, survey
- Messages: conversations, send, read, unread count
- **Phase 2 entirely** — deep academic operations

### `access.py` (48KB) — Auth & RBAC
- Login, current access, admin dashboard
- User CRUD, role CRUD, permissions listing
- Account links (doctor/student ↔ user)
- **Phase 1 partial:** User/role concepts inform RBAC design, but Payload handles auth natively

### `academic.py` (43KB) — Academic content management
- Colleges CRUD + programs/faculty/dean
- Programs CRUD + objectives/faculty
- Faculty CRUD + search/filter
- Departments listing
- **Phase 1:** Basic CRUD via Payload collections

### `content.py` (36KB) — Content management
- News, events, blog, FAQ, campus life, partners, offers, projects, research publications, team, pages CRUD
- **Phase 1:** News only; rest Phase 2

### `smartchat.py` (25KB) — AI chat
- Smart chat endpoints
- **Phase 2** — complex AI integration

### `cms.py` (3.6KB) — Media & settings
- Media listing/upload/delete, settings CRUD
- **Phase 1:** Handled by Payload media collection + globals

---

## Hooks & Permission System

### hooks.py Active Configuration

```python
# Permission hooks
permission_query_conditions = {
    "Colleges": "aau_university.aau.doctype.colleges.colleges.get_permission_query_conditions",
}
has_permission = {
    "Colleges": "aau_university.aau.doctype.colleges.colleges.has_permission",
}

# Document events
doc_events = {
    "Task": {
        "on_update": "aau_university.aau_tasks.task_doctype_importer.on_task_update"
    }
}

# Request hooks
before_request = ["aau_university.api.v1.routes.ensure_routes"]
after_request = ["aau_university.utils.cors.apply_cors_headers"]
```

**Key insight:** College-scoped permissions exist in Frappe — this maps directly to our RBAC coordinator `collegeScope` pattern.

### Patches (14 total)

All patches are post_model_sync, indicating incremental schema evolution:
1. v1_0 — Screen audit fix
2. v1_1 — JSON content → fields migration
3. v1_2 — Cleanup unused screens
4. v1_3 — Add instructor user link
5. v1_4 — Refresh workspace
6. v1_5 — Home page bilingual fields
7. v1_6 — Home page single CMS
8. v1_7 — Content manager access
9. v1_8 — About university single CMS
10. v1_9 — Faculty linked college
11. v1_10 — Normalize CMS content DocTypes
12. v1_11 — Normalize blog and faculty CMS
13. v1_12 — Finalize workspace
14. v1_13 — Provision instructor portal users

**Key insight:** Patches show evolution from JSON-based content to structured fields, and introduction of bilingual (AR/EN) fields. Our Payload schema should use the final evolved state.

---

## Frappe Standard DocTypes Referenced by APIs

| Standard DocType | Used By | Phase |
|-----------------|---------|-------|
| Student | portal.py (student profile, courses, grades) | Phase 2 |
| Instructor | portal.py (doctor profile, courses) | Phase 2 |
| Course | portal.py (courses, schedules) | Phase 2 |
| Course Enrollment | portal.py (student courses) | Phase 2 |
| Course Schedule | portal.py (schedules) | Phase 2 |
| Assessment Result | portal.py (grades) | Phase 2 |
| Sales Invoice / Fees | portal.py (finance) | Phase 2 |
| Communication | portal.py (messages) | Phase 2 |
| Notification Log | portal.py (notifications) | Phase 2 |
| File | cms.py (media) | Phase 1 (Payload media) |
| User | access.py (user management) | Phase 1 (Payload users) |

---

## Summary

- **47 total DocTypes** in the Frappe backend
- **Phase 1:** 11 Collections + 5 Globals (covering ~24 DocTypes including child tables and merged entities)
- **Phase 2:** ~17 remaining content/academic DocTypes
- **Do not migrate:** 1 (Grade — empty)
- **Replaced:** 1 (Media Library → Payload media)
- **Merged:** 5 (3 page types → `pages`, 2 announcement types → 1, Centers + University Centers)
- **Portal APIs (Phase 2):** 30+ student/doctor routes requiring deep academic integration

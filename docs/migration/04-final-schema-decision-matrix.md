# Final Schema Decision Matrix — Phase F (Subagent 6)

> Cleaned schema design based on backend audit, database comparison, frontend audit, and API analysis.

## Design Principles

1. **Do not copy all 47 Frappe DocTypes** — reduce, merge, simplify
2. **Collections** for reusable structured data with search/filter/relations/permissions
3. **Globals** for single-instance site settings
4. **Puck blocks** for visual-only page sections
5. **Array fields** for child table equivalents (no separate collections)
6. **Bilingual** — all content fields have `_ar` and `_en` variants

---

## Phase 1 Collections (11)

### 1. `users` (Payload Auth Collection)

Built-in Payload auth collection extended with RBAC fields.

| Field | Type | Notes |
|-------|------|-------|
| email | email | Required, unique (Payload built-in) |
| password | password | Payload built-in |
| role | select | super_admin, admin, content_manager, coordinator, student, doctor |
| firstName | text | |
| lastName | text | |
| fullName | text | Virtual computed field |
| collegeScope | relationship → colleges | Required for coordinator role |
| avatar | upload → media | Profile image |
| isActive | checkbox | Default true |

### 2. `media` (Payload Upload Collection)

Built-in Payload upload collection.

| Field | Type | Notes |
|-------|------|-------|
| alt | text | Alt text for images |
| caption | text | Optional caption |

### 3. `pages` (Puck-enabled)

Merged from AAU Page + Pages + Static Page. Puck layout JSON managed by payload-puck plugin.

| Field | Type | Notes |
|-------|------|-------|
| title | text | Required |
| titleEn | text | English title |
| slug | text | Required, unique, indexed |
| content | richText | Arabic content (fallback for non-Puck pages) |
| contentEn | richText | English content |
| heroImage | upload → media | Page hero image |
| isPublished | checkbox | Default false |
| seoTitle | text | Via SEO plugin |
| seoDescription | textarea | Via SEO plugin |
| layout | json | Puck layout data (managed by plugin) |

### 4. `navigation-menus`

Replaces AAU Menu + AAU Menu Item.

| Field | Type | Notes |
|-------|------|-------|
| key | text | Required, unique (e.g., "main", "footer") |
| isPublished | checkbox | Default true |
| items | array | Menu items |
| items.labelAr | text | Required |
| items.labelEn | text | Required |
| items.url | text | Required |
| items.group | text | Optional grouping |
| items.openInNewTab | checkbox | Default false |
| items.order | number | Sort order |

### 5. `news`

| Field | Type | Notes |
|-------|------|-------|
| titleAr | text | Required |
| titleEn | text | |
| slug | text | Unique, auto-generated |
| publishDate | date | |
| college | relationship → colleges | Optional |
| summaryAr | textarea | |
| summaryEn | textarea | |
| contentAr | richText | |
| contentEn | richText | |
| tags | text | Comma-separated or array |
| image | upload → media | |
| featuredImage | upload → media | |
| isPublished | checkbox | Default false |
| displayOrder | number | |
| views | number | Default 0 |

### 6. `colleges`

| Field | Type | Notes |
|-------|------|-------|
| nameAr | text | Required |
| nameEn | text | |
| slug | text | Unique |
| descriptionAr | richText | |
| descriptionEn | richText | |
| visionAr | richText | |
| visionEn | richText | |
| missionAr | richText | |
| missionEn | richText | |
| goalsAr | richText | |
| goalsEn | richText | |
| valuesAr | richText | |
| valuesEn | richText | |
| strategyAr | richText | |
| strategyEn | richText | |
| qualityAr | richText | |
| qualityEn | richText | |
| icon | text | Icon identifier |
| deanNameAr | text | |
| deanNameEn | text | |
| deanImage | upload → media | |
| image | upload → media | College image |

### 7. `academic-departments`

| Field | Type | Notes |
|-------|------|-------|
| nameAr | text | Required |
| nameEn | text | |
| college | relationship → colleges | Required |
| descriptionAr | richText | |
| descriptionEn | richText | |
| isActive | checkbox | Default true |

### 8. `academic-programs`

| Field | Type | Notes |
|-------|------|-------|
| nameAr | text | Required |
| nameEn | text | |
| college | relationship → colleges | Required |
| departmentAr | text | |
| departmentEn | text | |
| degreeType | select | Diploma, Bachelor, Master, PhD |
| descriptionAr | richText | |
| descriptionEn | richText | |
| objectivesAr | richText | |
| objectivesEn | richText | |
| careerProspectsAr | richText | |
| careerProspectsEn | richText | |
| applicationStepsAr | richText | |
| applicationStepsEn | richText | |
| whyProgramAr | richText | |
| whyProgramEn | richText | |
| duration | text | |
| durationEn | text | |
| studyYears | text | |
| admissionRate | number | |
| highSchoolType | text | |
| image | upload → media | |
| isActive | checkbox | Default true |

### 9. `faculty-members`

Replaces Faculty Members + 4 child tables as array fields.

| Field | Type | Notes |
|-------|------|-------|
| fullNameAr | text | Required |
| fullNameEn | text | |
| academicTitleAr | text | |
| academicTitleEn | text | |
| college | relationship → colleges | |
| program | relationship → academic-programs | |
| department | relationship → academic-departments | |
| biographyAr | richText | |
| biographyEn | richText | |
| email | email | |
| phone | text | |
| officeHoursAr | text | |
| officeHoursEn | text | |
| researchInterestsAr | textarea | |
| researchInterestsEn | textarea | |
| photo | upload → media | |
| isActive | checkbox | Default true |
| **education** | array | |
| education.degreeAr | text | Required |
| education.degreeEn | text | |
| education.institutionAr | text | |
| education.institutionEn | text | |
| education.year | text | |
| **experience** | array | |
| experience.positionAr | text | Required |
| experience.positionEn | text | |
| experience.organizationAr | text | |
| experience.organizationEn | text | |
| experience.periodAr | text | |
| experience.periodEn | text | |
| **publications** | array | |
| publications.titleAr | text | Required |
| publications.titleEn | text | |
| publications.journal | text | |
| publications.year | text | |
| publications.link | text | |
| **courses** | array | |
| courses.code | text | |
| courses.nameAr | text | Required |
| courses.nameEn | text | |
| courses.semester | text | |

### 10. `contact-messages`

| Field | Type | Notes |
|-------|------|-------|
| name | text | Required |
| email | email | Required |
| phone | text | |
| subject | text | Required |
| message | textarea | Required |
| status | select | new, read, replied, archived |
| notes | textarea | Admin notes |

### 11. `join-requests`

| Field | Type | Notes |
|-------|------|-------|
| fullName | text | Required |
| email | email | Required |
| phone | text | Required |
| nationalId | text | |
| dateOfBirth | date | |
| gender | select | male, female |
| nationality | text | |
| city | text | |
| highSchoolType | text | |
| highSchoolGpa | number | |
| desiredCollege | relationship → colleges | |
| desiredProgram | relationship → academic-programs | |
| status | select | pending, reviewing, accepted, rejected |
| notes | textarea | Admin notes |

---

## Phase 1 Globals (5)

### 1. `home-page`

Key sections of the home page with bilingual content.

| Field Group | Fields |
|-------------|--------|
| Hero | badgeAr/En, titlePrimaryAr/En, titleSecondaryAr/En, descriptionAr/En, heroImage, backgroundType, backgroundImage, backgroundVideo, overlayOpacity |
| Stats | studentsCount, facultyCount, programsCount, collegesCount, labels (ar/en for each) |
| Section Titles | campusLife, projects, colleges, news, events, faq, video, contact — each with titleAr/En, descriptionAr/En |
| Video | videoFile, overlayTitleAr/En, overlayDescriptionAr/En |

### 2. `about-page`

| Field Group | Fields |
|-------------|--------|
| Page Header | badgeAr/En, titleAr/En, descriptionAr/En |
| Intro | bodyAr/En, image |
| Vision | titleAr/En, descriptionAr/En |
| Mission | titleAr/En, descriptionAr/En |
| Goals | titleAr/En, descriptionAr/En |
| Values | titleAr/En, descriptionAr/En |
| President | sectionTitleAr/En, messageIntroAr/En, messageBodyAr/En, messageClosingAr/En, nameAr/En, roleAr/En, image |
| Team | sectionTitleAr/En, sectionDescriptionAr/En, members (array with nameAr/En, jobTitleAr/En, groupAr/En, image, order) |

### 3. `site-settings`

Cleaned from Website Settings (93 → ~20 essential fields).

| Field | Type |
|-------|------|
| siteName | text |
| siteNameAr | text |
| siteNameEn | text |
| logo | upload → media |
| favicon | upload → media |
| contactEmail | email |
| contactPhone | text |
| addressAr | textarea |
| addressEn | textarea |
| mapLocation | text |
| socialLinks | array (platform, url) |
| copyright | text |
| footerLogo | upload → media |
| googleAnalyticsId | text |

### 4. `contact-settings`

From Contact Page Settings (39 → ~20 label fields).

| Field Group | Fields |
|-------------|--------|
| Page | badgeAr/En, titleAr/En, descriptionAr/En |
| Form Labels | name/email/subject/message label and placeholder (ar/en each) |
| Submit | submitTextAr/En |
| Info | phoneAr/En, emailAr/En, addressAr/En labels |

### 5. `smart-chat-settings`

From Smart Chat Settings.

| Field | Type |
|-------|------|
| isEnabled | checkbox |
| botName | text |
| welcomeMessage | textarea |
| apiEndpoint | text |
| apiKey | text |
| model | text |
| maxTokens | number |
| temperature | number |

---

## Phase 1 Puck Blocks (10)

| Block | Props | Data Source |
|-------|-------|-------------|
| **HeroBlock** | badge, title, subtitle, description, image, backgroundType, cta | Static props from editor |
| **RichTextBlock** | content (richText) | Static |
| **CTASection** | title, description, buttonText, buttonUrl, variant | Static |
| **StatsBlock** | stats (array: value, label, icon) | Static or from home-page global |
| **FeatureCardsBlock** | cards (array: title, description, icon, link) | Static |
| **ImageGalleryBlock** | images (array: src, alt, caption) | Static |
| **NewsGridBlock** | title, count, showViewAll | Dynamic — fetches from news collection |
| **ProgramsGridBlock** | title, collegeFilter, count | Dynamic — fetches from academic-programs |
| **CollegesGridBlock** | title, count, showViewAll | Dynamic — fetches from colleges |
| **FAQBlock** | title, category (placeholder) | Placeholder — Phase 2 dynamic |

---

## Phase 2 Deferred Collections

| Collection | Priority | Dependencies |
|-----------|----------|-------------|
| events | High | None |
| blog-posts | High | None |
| faq | High | None |
| partners | Medium | None |
| offers | Medium | None |
| campus-life | Medium | None |
| projects | Medium | None |
| centers | Medium | center-services (array) |
| research-publications | Medium | None |
| student-activities | Low | None |
| student-affairs-documents | Low | None |
| team-members | Low | None |
| announcements | Low | None |
| email-requests | Low | None |
| admission-requirements | Medium | colleges |
| study-plans | Medium | academic-programs |
| study-plan-courses | Medium | study-plans |

## Phase 2+ Deep Academic Modules (NOT in schema yet)

- students, doctors/instructors
- courses, enrollments, schedules
- attendance, grades, fees, payments
- messages, notifications, materials

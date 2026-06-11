# AAU University Database Documentation

> Auto-generated documentation for the AAU University Frappe/ERPNext application database schema.

## Overview

| Metric | Value |
|--------|-------|
| Total DocTypes | 47 |
| Main Tables | 35 |
| Single DocTypes (Settings) | 6 |
| Child Tables | 6 |
| Total Custom Fields | 724 |
| Composition Relationships | 9 |
| Link (FK) Relationships | 13 |
| Target Database | PostgreSQL |
| Source Framework | Frappe / ERPNext |

## Table of Contents

### 🏠 Website & CMS

- [⚙️ Home Page](#home-page) — 62 fields
- [⚙️ Website Settings](#website-settings) — 93 fields
- [📄 AAU Menu](#aau-menu) — 3 fields
- [📋 AAU Menu Item](#aau-menu-item) — 6 fields
- [📄 AAU Page](#aau-page) — 8 fields
- [📄 Static Page](#static-page) — 6 fields
- [📄 Pages](#pages) — 8 fields
- [📄 Slider](#slider) — 8 fields
- [⚙️ About University](#about-university) — 43 fields

### 📰 Content & Media

- [📄 News](#news) — 21 fields
- [📄 Blog Posts](#blog-posts) — 24 fields
- [📄 Events](#events) — 12 fields
- [📄 Announcements](#announcements) — 7 fields
- [📄 Announcement](#announcement) — 6 fields
- [📄 Media Library](#media-library) — 7 fields
- [📄 FAQ](#faq) — 10 fields
- [📄 Campus Life](#campus-life) — 25 fields

### 🎓 Academic

- [📄 Colleges](#colleges) — 32 fields
- [📄 Academic Departments](#academic-departments) — 6 fields
- [📄 Academic Programs](#academic-programs) — 29 fields
- [📄 Admission Requirements](#admission-requirements) — 7 fields
- [📄 Study Plans](#study-plans) — 7 fields
- [📄 Study Plan Courses](#study-plan-courses) — 7 fields
- [📄 Grade](#grade) — 0 fields

### 👨‍🏫 Faculty

- [📄 Faculty Members](#faculty-members) — 21 fields
- [📋 Faculty Course](#faculty-course) — 4 fields
- [📋 Faculty Education](#faculty-education) — 5 fields
- [📋 Faculty Experience](#faculty-experience) — 6 fields
- [📋 Faculty Publication](#faculty-publication) — 5 fields
- [📋 About Team Member](#about-team-member) — 8 fields
- [📄 Team Members](#team-members) — 12 fields

### 📞 Contact & Communication

- [⚙️ Contact Page Settings](#contact-page-settings) — 39 fields
- [📄 Contact Us Messages](#contact-us-messages) — 10 fields
- [📄 Email Requests](#email-requests) — 9 fields
- [⚙️ Smart Chat Settings](#smart-chat-settings) — 13 fields

### 🏛️ Centers & Projects

- [📄 Centers](#centers) — 16 fields
- [📄 Center Services](#center-services) — 7 fields
- [📄 University Centers](#university-centers) — 7 fields
- [📄 Projects](#projects) — 16 fields
- [📄 Research and Publications](#research-and-publications) — 8 fields
- [📄 Partners](#partners) — 7 fields
- [📄 Offers](#offers) — 44 fields

### 👨‍🎓 Student Portal

- [📄 Student Activities](#student-activities) — 7 fields
- [📄 Student Affairs Document](#student-affairs-document) — 7 fields
- [📄 Student Portal Survey Response](#student-portal-survey-response) — 13 fields
- [📄 Join Requests](#join-requests) — 20 fields
- [⚙️ Dashboard Metrics](#dashboard-metrics) — 3 fields

- [Relationships Summary](#relationships-summary)
- [Frappe Standard Fields Reference](#frappe-standard-fields-reference)
- [Frappe-to-PostgreSQL Type Mapping](#frappe-to-postgresql-type-mapping)

---

## DocType Details

### 🏠 Website & CMS

#### Home Page

| Property | Value |
|----------|-------|
| DocType Name | Home Page |
| PostgreSQL Table | `tab_home_page` |
| Type | `Single` |
| Custom Fields | 62 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `hero_badge_ar` | شارة الهيرو | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `hero_badge_en` | Hero Badge (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `hero_title_primary_ar` | العنوان الرئيسي | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `hero_title_primary_en` | Primary Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `hero_title_secondary_ar` | العنوان الثانوي | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `hero_title_secondary_en` | Secondary Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `hero_description_ar` | وصف الهيرو | Small Text | `TEXT` |  |  |  |  |
| 8 | `hero_description_en` | Hero Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 9 | `hero_image` | صورة الهيرو | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 10 | `hero_background_type` | نوع خلفية الهيرو | Select | `VARCHAR(255)` |  |  | `none` | `none`, `image`, `video` |
| 11 | `hero_background_image` | صورة خلفية الهيرو | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 12 | `hero_background_video` | فيديو خلفية الهيرو | Attach | `VARCHAR(512)` |  |  |  |  |
| 13 | `hero_background_overlay_opacity` | شفافية طبقة خلفية الهيرو | Float | `DOUBLE PRECISION` |  |  | `0.45` |  |
| 14 | `students_count` | عدد الطلاب | Int | `INTEGER` |  |  |  |  |
| 15 | `faculty_count` | عدد أعضاء هيئة التدريس | Int | `INTEGER` |  |  |  |  |
| 16 | `programs_count` | عدد البرامج | Int | `INTEGER` |  |  |  |  |
| 17 | `colleges_count` | عدد الكليات | Int | `INTEGER` |  |  |  |  |
| 18 | `stats_students_label_ar` | تسمية الطلاب | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `stats_students_label_en` | Students Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `stats_faculty_label_ar` | تسمية هيئة التدريس | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `stats_faculty_label_en` | Faculty Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 22 | `stats_programs_label_ar` | تسمية البرامج | Data | `VARCHAR(255)` |  |  |  |  |
| 23 | `stats_programs_label_en` | Programs Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `stats_colleges_label_ar` | تسمية الكليات | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `stats_colleges_label_en` | Colleges Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `campus_life_title_ar` | عنوان الحياة الجامعية | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `campus_life_title_en` | Campus Life Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 28 | `campus_life_description_ar` | وصف الحياة الجامعية | Small Text | `TEXT` |  |  |  |  |
| 29 | `campus_life_description_en` | Campus Life Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 30 | `projects_title_ar` | عنوان المشاريع | Data | `VARCHAR(255)` |  |  |  |  |
| 31 | `projects_title_en` | Projects Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 32 | `projects_description_ar` | وصف المشاريع | Small Text | `TEXT` |  |  |  |  |
| 33 | `projects_description_en` | Projects Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 34 | `colleges_title_ar` | عنوان الكليات | Data | `VARCHAR(255)` |  |  |  |  |
| 35 | `colleges_title_en` | Colleges Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 36 | `colleges_description_ar` | وصف الكليات | Small Text | `TEXT` |  |  |  |  |
| 37 | `colleges_description_en` | Colleges Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 38 | `news_title_ar` | عنوان الأخبار | Data | `VARCHAR(255)` |  |  |  |  |
| 39 | `news_title_en` | News Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 40 | `news_description_ar` | وصف الأخبار | Small Text | `TEXT` |  |  |  |  |
| 41 | `news_description_en` | News Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 42 | `events_title_ar` | عنوان الفعاليات | Data | `VARCHAR(255)` |  |  |  |  |
| 43 | `events_title_en` | Events Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 44 | `events_description_ar` | وصف الفعاليات | Small Text | `TEXT` |  |  |  |  |
| 45 | `events_description_en` | Events Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 46 | `faq_title_ar` | عنوان الأسئلة المتكررة | Data | `VARCHAR(255)` |  |  |  |  |
| 47 | `faq_title_en` | FAQ Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 48 | `faq_description_ar` | وصف الأسئلة المتكررة | Small Text | `TEXT` |  |  |  |  |
| 49 | `faq_description_en` | FAQ Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 50 | `video_title_ar` | عنوان الفيديو | Data | `VARCHAR(255)` |  |  |  |  |
| 51 | `video_title_en` | Video Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 52 | `video_description_ar` | وصف الفيديو | Small Text | `TEXT` |  |  |  |  |
| 53 | `video_description_en` | Video Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 54 | `video_file` | ملف الفيديو | Attach | `VARCHAR(512)` |  |  |  |  |
| 55 | `video_overlay_title_ar` | عنوان طبقة الفيديو | Data | `VARCHAR(255)` |  |  |  |  |
| 56 | `video_overlay_title_en` | Video Overlay Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 57 | `video_overlay_description_ar` | وصف طبقة الفيديو | Small Text | `TEXT` |  |  |  |  |
| 58 | `video_overlay_description_en` | Video Overlay Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 59 | `contact_title_ar` | عنوان التواصل | Data | `VARCHAR(255)` |  |  |  |  |
| 60 | `contact_title_en` | Contact Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 61 | `contact_description_ar` | وصف التواصل | Small Text | `TEXT` |  |  |  |  |
| 62 | `contact_description_en` | Contact Description (EN) | Small Text | `TEXT` |  |  |  |  |
---

#### Website Settings

| Property | Value |
|----------|-------|
| DocType Name | Website Settings |
| PostgreSQL Table | `tab_website_settings` |
| Type | `Single` |
| Custom Fields | 93 |
| Module | AAU |
| Child Tables | `Top Bar Item` (top_bar_items), `Top Bar Item` (footer_items), `Website Route Redirect` (route_redirects) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `contact_page_badge_ar` | شارة الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `contact_page_badge_en` | شارة الصفحة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `contact_page_title_ar` | عنوان الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `contact_page_title_en` | عنوان الصفحة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `contact_page_description_ar` | وصف الصفحة | Long Text | `TEXT` |  |  |  |  |
| 6 | `contact_page_description_en` | وصف الصفحة (English) | Long Text | `TEXT` |  |  |  |  |
| 7 | `contact_phone_label_ar` | عنوان بطاقة الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `contact_phone_label_en` | عنوان بطاقة الهاتف (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `contact_email_label_ar` | عنوان بطاقة البريد | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `contact_email_label_en` | عنوان بطاقة البريد (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `contact_address_label_ar` | عنوان بطاقة العنوان | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `contact_address_label_en` | عنوان بطاقة العنوان (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `address_ar` | العنوان | Long Text | `TEXT` |  |  |  |  |
| 14 | `address_en` | العنوان (English) | Long Text | `TEXT` |  |  |  |  |
| 15 | `map_location` | رابط الخريطة | Data | `VARCHAR(255)` |  |  |  |  |
| 16 | `contact_form_title_ar` | عنوان نموذج التواصل | Data | `VARCHAR(255)` |  |  |  |  |
| 17 | `contact_form_title_en` | عنوان نموذج التواصل (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 18 | `contact_form_name_label_ar` | عنوان حقل الاسم | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `contact_form_name_label_en` | عنوان حقل الاسم (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `contact_form_name_placeholder_ar` | نص حقل الاسم | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `contact_form_name_placeholder_en` | نص حقل الاسم (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 22 | `contact_form_email_label_ar` | عنوان حقل البريد | Data | `VARCHAR(255)` |  |  |  |  |
| 23 | `contact_form_email_label_en` | عنوان حقل البريد (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `contact_form_email_placeholder_ar` | نص حقل البريد | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `contact_form_email_placeholder_en` | نص حقل البريد (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `contact_form_subject_label_ar` | عنوان حقل الموضوع | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `contact_form_subject_label_en` | عنوان حقل الموضوع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 28 | `contact_form_subject_placeholder_ar` | نص حقل الموضوع | Data | `VARCHAR(255)` |  |  |  |  |
| 29 | `contact_form_subject_placeholder_en` | نص حقل الموضوع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 30 | `contact_form_message_label_ar` | عنوان حقل الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 31 | `contact_form_message_label_en` | عنوان حقل الرسالة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 32 | `contact_form_message_placeholder_ar` | نص حقل الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 33 | `contact_form_message_placeholder_en` | نص حقل الرسالة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 34 | `contact_form_submit_text_ar` | نص زر الإرسال | Data | `VARCHAR(255)` |  |  |  |  |
| 35 | `contact_form_submit_text_en` | نص زر الإرسال (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 36 | `social_section_title_ar` | عنوان وسائل التواصل | Data | `VARCHAR(255)` |  |  |  |  |
| 37 | `social_section_title_en` | عنوان وسائل التواصل (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 38 | `contact_email` | البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 39 | `contact_phone` | رقم الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 40 | `home_page` | Home Page | Data | `VARCHAR(255)` |  |  |  |  |
| 41 | `title_prefix` | Title Prefix | Data | `VARCHAR(255)` |  |  |  |  |
| 42 | `app_name` | App Name | Data | `VARCHAR(255)` |  |  | `Frappe` |  |
| 43 | `disable_signup` | Disable signups | Check | `BOOLEAN` |  |  | `1` |  |
| 44 | `show_footer_on_login` | Show footer on login | Check | `BOOLEAN` |  |  | `0` |  |
| 45 | `app_logo` | App Logo | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 46 | `website_theme` | Website Theme | Link | `VARCHAR(255)` |  |  | `Standard` | → `Website Theme` |
| 47 | `website_theme_image` | Website Theme Image | Image | `VARCHAR(255)` |  |  |  | website_theme_image_link |
| 48 | `website_theme_image_link` | Website Theme image link | Code | `TEXT` |  |  |  |  |
| 49 | `banner_image` | Brand Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 50 | `splash_image` | Splash Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 51 | `brand_html` | Brand HTML | Code | `TEXT` |  |  |  | HTML |
| 52 | `set_banner_from_image` | Set Banner from Image | Button | `VARCHAR(255)` |  |  |  |  |
| 53 | `favicon` | FavIcon | Attach | `VARCHAR(512)` |  |  |  |  |
| 54 | `top_bar_items` | Top Bar Items | Table | `INTEGER` |  |  |  | → `Top Bar Item` |
| 55 | `hide_login` | Hide Login | Check | `BOOLEAN` |  |  | `0` |  |
| 56 | `navbar_search` | Include Search in Top Bar | Check | `BOOLEAN` |  |  | `0` |  |
| 57 | `show_language_picker` | Show Language Picker | Check | `BOOLEAN` |  |  | `0` |  |
| 58 | `navbar_template` | Navbar Template | Link | `VARCHAR(255)` |  |  |  | → `Web Template` |
| 59 | `navbar_template_values` | Navbar Template Values | Code | `TEXT` |  |  |  | JSON |
| 60 | `edit_navbar_template_values` | Edit Values | Button | `VARCHAR(255)` |  |  |  |  |
| 61 | `call_to_action` | Call To Action | Data | `VARCHAR(255)` |  |  |  |  |
| 62 | `call_to_action_url` | Call To Action URL | Data | `VARCHAR(255)` |  |  |  |  |
| 63 | `banner_html` | Banner HTML | Code | `TEXT` |  |  |  | HTML |
| 64 | `footer_items` | Footer Items | Table | `INTEGER` |  |  |  | → `Top Bar Item` |
| 65 | `copyright` | Copyright | Data | `VARCHAR(255)` |  |  |  |  |
| 66 | `footer_logo` | Footer Logo | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 67 | `footer_background_type` | نوع خلفية الفوتر | Select | `VARCHAR(255)` |  |  | `none` | `none`, `image`, `video` |
| 68 | `footer_background_image` | صورة خلفية الفوتر | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 69 | `footer_background_video` | فيديو خلفية الفوتر | Attach | `VARCHAR(512)` |  |  |  |  |
| 70 | `footer_background_overlay_opacity` | شفافية طبقة الفوتر | Float | `DOUBLE PRECISION` |  |  | `0.65` |  |
| 71 | `hide_footer_signup` | Hide footer signup | Check | `BOOLEAN` |  |  | `0` |  |
| 72 | `address` | Address | Small Text | `TEXT` |  |  |  |  |
| 73 | `footer_powered` | Footer "Powered By" | Small Text | `TEXT` |  |  |  |  |
| 74 | `footer_template` | Footer Template | Link | `VARCHAR(255)` |  |  |  | → `Web Template` |
| 75 | `footer_template_values` | Footer Template Values | Code | `TEXT` |  |  |  | JSON |
| 76 | `edit_footer_template_values` | Edit Values | Button | `VARCHAR(255)` |  |  |  |  |
| 77 | `enable_view_tracking` | Enable in-app website tracking | Check | `BOOLEAN` |  |  | `0` |  |
| 78 | `enable_google_indexing` | Enable Google indexing | Check | `BOOLEAN` |  |  | `0` |  |
| 79 | `authorize_api_indexing_access` | Authorize API Indexing  Access | Button | `VARCHAR(255)` |  |  |  |  |
| 80 | `indexing_refresh_token` | Indexing refresh token | Data | `VARCHAR(255)` |  |  |  |  |
| 81 | `indexing_authorization_code` | Indexing authorization code | Data | `VARCHAR(255)` |  |  |  |  |
| 82 | `google_analytics_id` | Google Analytics ID | Data | `VARCHAR(255)` |  |  |  |  |
| 83 | `google_analytics_anonymize_ip` | Google Analytics anonymise IP | Check | `BOOLEAN` |  |  | `1` |  |
| 84 | `auto_account_deletion` | Automatically delete account within (hours) | Int | `INTEGER` |  |  | `72` |  |
| 85 | `show_account_deletion_link` | Show account deletion link in My Account page | Check | `BOOLEAN` |  |  | `0` |  |
| 86 | `subdomain` | Subdomain | Small Text | `TEXT` |  |  |  |  |
| 87 | `head_html` | &lt;head&gt; HTML | Code | `TEXT` |  |  |  | HTML |
| 88 | `robots_txt` | Robots.txt | Code | `TEXT` |  |  |  |  |
| 89 | `route_redirects` | Route Redirects | Table | `INTEGER` |  |  |  | → `Website Route Redirect` |
| 90 | `site_name` | Site Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 91 | `site_name_ar` | Site Name (Arabic) | Data | `VARCHAR(255)` |  |  |  |  |
| 92 | `site_name_en` | Site Name (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 93 | `logo` | Logo | Attach Image | `VARCHAR(512)` |  |  |  |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `website_theme` | Website Theme | `Website Theme` (`tab_website_theme`) |
| `navbar_template` | Navbar Template | `Web Template` (`tab_web_template`) |
| `footer_template` | Footer Template | `Web Template` (`tab_web_template`) |

---

#### AAU Menu

| Property | Value |
|----------|-------|
| DocType Name | AAU Menu |
| PostgreSQL Table | `tab_aau_menu` |
| Type | `Main` |
| Custom Fields | 3 |
| Module | AAU |
| Child Tables | `AAU Menu Item` (items) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `key` | Key | Data | `VARCHAR(255)` | ✅ | ✅ |  |  |
| 2 | `published` | Published | Check | `BOOLEAN` |  |  | `1` |  |
| 3 | `items` | Items | Table | `INTEGER` |  |  |  | → `AAU Menu Item` |
---

#### AAU Menu Item

| Property | Value |
|----------|-------|
| DocType Name | AAU Menu Item |
| PostgreSQL Table | `tab_aau_menu_item` |
| Type | `Child` |
| Custom Fields | 6 |
| Module | AAU |
| Parent DocType | `AAU Menu` (items) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `label_ar` | Label (AR) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `label_en` | Label (EN) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `url` | URL | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `group` | Group | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `open_in_new_tab` | Open In New Tab | Check | `BOOLEAN` |  |  | `0` |  |
| 6 | `order` | Order | Int | `INTEGER` |  |  | `0` |  |
---

#### AAU Page

| Property | Value |
|----------|-------|
| DocType Name | AAU Page |
| PostgreSQL Table | `tab_aau_page` |
| Type | `Main` |
| Custom Fields | 8 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `slug` | المسار | Data | `VARCHAR(255)` | ✅ | ✅ |  |  |
| 2 | `title_ar` | عنوان الصفحة | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `content_ar` | المحتوى | Text Editor | `TEXT` |  |  |  |  |
| 4 | `hero_image` | صورة الصفحة | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 5 | `published` | منشور | Check | `BOOLEAN` |  |  | `1` |  |
| 6 | `translation_note` | تنبيه الترجمة | HTML | `TEXT` |  |  |  | <div class="text-muted small">يمكن إدخال محتوى بالعربية والإنجليزية. عند العرض يتم اختيار النسخة حسب لغة الموقع.</div> |
| 7 | `title_en` | Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `content_en` | Content (EN) | Text Editor | `TEXT` |  |  |  |  |
---

#### Static Page

| Property | Value |
|----------|-------|
| DocType Name | Static Page |
| PostgreSQL Table | `tab_static_page` |
| Type | `Main` |
| Custom Fields | 6 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `page_title` | عنوان الصفحة | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `page_title_en` | عنوان الصفحة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `slug` | الرابط المختصر | Data | `VARCHAR(255)` | ✅ | ✅ |  |  |
| 4 | `content` | محتوى الصفحة | Long Text | `TEXT` |  |  |  |  |
| 5 | `content_en` | محتوى الصفحة (English) | Long Text | `TEXT` |  |  |  |  |
| 6 | `is_published` | منشورة | Check | `BOOLEAN` |  |  | `0` |  |
---

#### Pages

| Property | Value |
|----------|-------|
| DocType Name | Pages |
| PostgreSQL Table | `tab_pages` |
| Type | `Main` |
| Custom Fields | 8 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `page_title` | Page Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `page_title_en` | Page Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `slug` | Slug | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 5 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 6 | `seo_title` | SEO Title | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `seo_description` | SEO Description | Small Text | `TEXT` |  |  |  |  |
| 8 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
---

#### Slider

| Property | Value |
|----------|-------|
| DocType Name | Slider |
| PostgreSQL Table | `tab_slider` |
| Type | `Main` |
| Custom Fields | 8 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | العنوان | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `title_en` | العنوان (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `subtitle` | العنوان الفرعي | Small Text | `TEXT` |  |  |  |  |
| 4 | `subtitle_en` | العنوان الفرعي (English) | Small Text | `TEXT` |  |  |  |  |
| 5 | `image` | الصورة | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `link` | الرابط | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `order` | ترتيب الظهور | Int | `INTEGER` |  |  |  |  |
| 8 | `is_active` | مفعل | Check | `BOOLEAN` |  |  | `0` |  |
---

#### About University

| Property | Value |
|----------|-------|
| DocType Name | About University |
| PostgreSQL Table | `tab_about_university` |
| Type | `Single` |
| Custom Fields | 43 |
| Module | AAU |
| Child Tables | `About Team Member` (team_members) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `page_badge_ar` | شارة الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `page_title_ar` | عنوان الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `page_description_ar` | وصف الصفحة | Long Text | `TEXT` |  |  |  |  |
| 4 | `intro_body_ar` | نص المقدمة | Long Text | `TEXT` |  |  |  |  |
| 5 | `intro_image` | صورة المقدمة | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `vision_title_ar` | عنوان الرؤية | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `vision_description_ar` | نص الرؤية | Long Text | `TEXT` |  |  |  |  |
| 8 | `mission_title_ar` | عنوان الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `mission_description_ar` | نص الرسالة | Long Text | `TEXT` |  |  |  |  |
| 10 | `goals_title_ar` | عنوان الأهداف | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `goals_description_ar` | نص الأهداف | Long Text | `TEXT` |  |  |  |  |
| 12 | `values_title_ar` | عنوان القيم | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `values_description_ar` | نص القيم | Long Text | `TEXT` |  |  |  |  |
| 14 | `president_section_title_ar` | عنوان قسم الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 15 | `president_message_intro_ar` | مقدمة الرسالة | Long Text | `TEXT` |  |  |  |  |
| 16 | `president_message_body_ar` | متن الرسالة | Long Text | `TEXT` |  |  |  |  |
| 17 | `president_message_closing_ar` | خاتمة الرسالة | Long Text | `TEXT` |  |  |  |  |
| 18 | `president_name_ar` | اسم رئيس الجامعة | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `president_role_ar` | الصفة الوظيفية | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `president_image` | صورة رئيس الجامعة | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 21 | `team_section_title_ar` | عنوان قسم الفريق | Data | `VARCHAR(255)` |  |  |  |  |
| 22 | `team_section_description_ar` | وصف قسم الفريق | Long Text | `TEXT` |  |  |  |  |
| 23 | `team_members` | أعضاء الفريق الإداري | Table | `INTEGER` |  |  |  | → `About Team Member` |
| 24 | `page_badge_en` | Page Badge (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `page_title_en` | Page Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `page_description_en` | Page Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 27 | `intro_body_en` | Intro Body (EN) | Long Text | `TEXT` |  |  |  |  |
| 28 | `vision_title_en` | Vision Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 29 | `vision_description_en` | Vision Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 30 | `mission_title_en` | Mission Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 31 | `mission_description_en` | Mission Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 32 | `goals_title_en` | Goals Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 33 | `goals_description_en` | Goals Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 34 | `values_title_en` | Values Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 35 | `values_description_en` | Values Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 36 | `president_section_title_en` | President Section Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 37 | `president_message_intro_en` | President Message Intro (EN) | Long Text | `TEXT` |  |  |  |  |
| 38 | `president_message_body_en` | President Message Body (EN) | Long Text | `TEXT` |  |  |  |  |
| 39 | `president_message_closing_en` | President Message Closing (EN) | Long Text | `TEXT` |  |  |  |  |
| 40 | `president_name_en` | President Name (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 41 | `president_role_en` | President Role (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 42 | `team_section_title_en` | Team Section Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 43 | `team_section_description_en` | Team Section Description (EN) | Long Text | `TEXT` |  |  |  |  |
---

### 📰 Content & Media

#### News

| Property | Value |
|----------|-------|
| DocType Name | News |
| PostgreSQL Table | `tab_news` |
| Type | `Main` |
| Custom Fields | 21 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | عنوان الخبر (عربي) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | عنوان الخبر (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `slug` | المسار | Data | `VARCHAR(255)` |  | ✅ |  |  |
| 4 | `publish_date` | تاريخ النشر | Date | `DATE` |  |  |  |  |
| 5 | `date` | تاريخ الخبر | Date | `DATE` |  |  |  |  |
| 6 | `college` | الكلية المرتبطة | Link | `VARCHAR(255)` |  |  |  | → `Colleges` |
| 7 | `summary` | ملخص الخبر (عربي) | Small Text | `TEXT` |  |  |  |  |
| 8 | `summary_en` | ملخص الخبر (English) | Small Text | `TEXT` |  |  |  |  |
| 9 | `description_en` | ملخص الخبر (English) | Small Text | `TEXT` |  |  |  |  |
| 10 | `content` | محتوى الخبر (عربي) | Long Text | `TEXT` |  |  |  |  |
| 11 | `content_en` | محتوى الخبر (English) | Long Text | `TEXT` |  |  |  |  |
| 12 | `tags` | الوسوم | Small Text | `TEXT` |  |  |  |  |
| 13 | `tags_en` | الوسوم (English) | Small Text | `TEXT` |  |  |  |  |
| 14 | `image` | صورة الخبر | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 15 | `featured_image` | الصورة الرئيسية | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 16 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 17 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
| 18 | `title_ar` | عنوان داخلي عربي | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `description_ar` | ملخص داخلي عربي | Small Text | `TEXT` |  |  |  |  |
| 20 | `content_ar` | محتوى داخلي عربي | Long Text | `TEXT` |  |  |  |  |
| 21 | `views` | Views | Int | `INTEGER` |  |  | `0` |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `college` | الكلية المرتبطة | `Colleges` (`tab_colleges`) |

---

#### Blog Posts

| Property | Value |
|----------|-------|
| DocType Name | Blog Posts |
| PostgreSQL Table | `tab_blog_posts` |
| Type | `Main` |
| Custom Fields | 24 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | المعرف | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `slug` | الرابط المختصر | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `title_ar` | عنوان المقال | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `title_en` | Title En | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `excerpt_ar` | الملخص | Long Text | `TEXT` |  |  |  |  |
| 6 | `excerpt_en` | Excerpt En | Long Text | `TEXT` |  |  |  |  |
| 7 | `content_ar` | المحتوى | Long Text | `TEXT` |  |  |  |  |
| 8 | `content_en` | Content En | Long Text | `TEXT` |  |  |  |  |
| 9 | `author_name_ar` | اسم الكاتب | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `author_name_en` | Author Name En | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `author_role_ar` | صفة الكاتب | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `author_role_en` | Author Role En | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `author_avatar` | صورة الكاتب | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 14 | `category` | معرف الفئة | Data | `VARCHAR(255)` |  |  |  |  |
| 15 | `category_ar` | اسم الفئة | Data | `VARCHAR(255)` |  |  |  |  |
| 16 | `category_en` | Category En | Data | `VARCHAR(255)` |  |  |  |  |
| 17 | `image` | صورة المقال | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 18 | `background_image` | صورة الخلفية | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 19 | `published_at` | تاريخ النشر | Date | `DATE` |  |  |  |  |
| 20 | `read_time` | وقت القراءة | Int | `INTEGER` |  |  |  |  |
| 21 | `views` | عدد المشاهدات | Int | `INTEGER` |  |  |  |  |
| 22 | `tags` | الوسوم | Long Text | `TEXT` |  |  |  |  |
| 23 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 24 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
---

#### Events

| Property | Value |
|----------|-------|
| DocType Name | Events |
| PostgreSQL Table | `tab_events` |
| Type | `Main` |
| Custom Fields | 12 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `event_title` | عنوان الفعالية (عربي) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `event_title_en` | عنوان الفعالية (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `description` | وصف الفعالية (عربي) | Long Text | `TEXT` |  |  |  |  |
| 4 | `description_en` | وصف الفعالية (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `event_date` | تاريخ الفعالية | Date | `DATE` |  |  |  |  |
| 6 | `location` | الموقع (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `location_en` | الموقع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `organizer` | المنظم (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `organizer_en` | المنظم (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `image` | صورة الفعالية | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 11 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 12 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
---

#### Announcements

| Property | Value |
|----------|-------|
| DocType Name | Announcements |
| PostgreSQL Table | `tab_announcements` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Announcement

| Property | Value |
|----------|-------|
| DocType Name | Announcement |
| PostgreSQL Table | `tab_announcement` |
| Type | `Main` |
| Custom Fields | 6 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | عنوان الإعلان | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | عنوان الإعلان (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `publish_date` | تاريخ النشر | Date | `DATE` |  |  |  |  |
| 4 | `content` | محتوى الإعلان | Long Text | `TEXT` |  |  |  |  |
| 5 | `content_en` | محتوى الإعلان (English) | Long Text | `TEXT` |  |  |  |  |
| 6 | `is_active` | مفعل | Check | `BOOLEAN` |  |  | `0` |  |
---

#### Media Library

| Property | Value |
|----------|-------|
| DocType Name | Media Library |
| PostgreSQL Table | `tab_media_library` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `media_title` | Media Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `media_title_en` | Media Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `media_type` | Media Type | Select | `VARCHAR(255)` |  |  |  | `Image`, `Video`, `Document` |
| 4 | `file` | File | Attach | `VARCHAR(512)` |  |  |  |  |
| 5 | `description` | Description | Small Text | `TEXT` |  |  |  |  |
| 6 | `description_en` | Description (English) | Small Text | `TEXT` |  |  |  |  |
| 7 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
---

#### FAQ

| Property | Value |
|----------|-------|
| DocType Name | FAQ |
| PostgreSQL Table | `tab_faq` |
| Type | `Main` |
| Custom Fields | 10 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 3 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 4 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
| 5 | `id` | ID | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `question_ar` | Question Ar | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 7 | `question_en` | Question En | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `answer_ar` | Answer Ar | Long Text | `TEXT` | ✅ |  |  |  |
| 9 | `answer_en` | Answer En | Long Text | `TEXT` |  |  |  |  |
| 10 | `category` | Category | Data | `VARCHAR(255)` |  |  |  |  |
---

#### Campus Life

| Property | Value |
|----------|-------|
| DocType Name | Campus Life |
| PostgreSQL Table | `tab_campus_life` |
| Type | `Main` |
| Custom Fields | 25 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `sidebar_title_ar` | عنوان البطاقة (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `sidebar_title_en` | عنوان البطاقة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `highlights_title_ar` | عنوان ما يميز (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `highlights_title_en` | عنوان ما يميز (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `highlight_1_ar` | الميزة 1 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `highlight_1_en` | الميزة 1 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `highlight_2_ar` | الميزة 2 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `highlight_2_en` | الميزة 2 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `highlight_3_ar` | الميزة 3 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 14 | `highlight_3_en` | الميزة 3 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 15 | `stat_1_value` | القيمة 1 | Int | `INTEGER` |  |  | `15` |  |
| 16 | `stat_1_label_ar` | وصف القيمة 1 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 17 | `stat_1_label_en` | وصف القيمة 1 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 18 | `stat_2_value` | القيمة 2 | Int | `INTEGER` |  |  | `10` |  |
| 19 | `stat_2_label_ar` | وصف القيمة 2 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `stat_2_label_en` | وصف القيمة 2 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `sidebar_note_ar` | النص السفلي (عربي) | Small Text | `TEXT` |  |  |  |  |
| 22 | `sidebar_note_en` | النص السفلي (English) | Small Text | `TEXT` |  |  |  |  |
| 23 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 24 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 25 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

### 🎓 Academic

#### Colleges

| Property | Value |
|----------|-------|
| DocType Name | Colleges |
| PostgreSQL Table | `tab_colleges` |
| Type | `Main` |
| Custom Fields | 32 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `college_name` | College Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `slug` | Slug | Data | `VARCHAR(255)` |  | ✅ |  |  |
| 3 | `name_ar` | Name (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `name_en` | Name (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `description` | Description | Long Text | `TEXT` |  |  |  |  |
| 6 | `description_ar` | Description (AR) | Long Text | `TEXT` |  |  |  |  |
| 7 | `description_en` | Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 8 | `vision_ar` | Vision (AR) | Long Text | `TEXT` |  |  |  |  |
| 9 | `vision_en` | Vision (EN) | Long Text | `TEXT` |  |  |  |  |
| 10 | `mission_ar` | Mission (AR) | Long Text | `TEXT` |  |  |  |  |
| 11 | `mission_en` | Mission (EN) | Long Text | `TEXT` |  |  |  |  |
| 12 | `goals_ar` | Goals (AR) | Long Text | `TEXT` |  |  |  |  |
| 13 | `goals_en` | Goals (EN) | Long Text | `TEXT` |  |  |  |  |
| 14 | `quality_ar` | Quality (AR) | Long Text | `TEXT` |  |  |  |  |
| 15 | `quality_en` | Quality (EN) | Long Text | `TEXT` |  |  |  |  |
| 16 | `values_ar` | Values (AR) | Long Text | `TEXT` |  |  |  |  |
| 17 | `values_en` | Values (EN) | Long Text | `TEXT` |  |  |  |  |
| 18 | `strategy_ar` | Strategic Objectives (AR) | Long Text | `TEXT` |  |  |  |  |
| 19 | `strategy_en` | Strategic Objectives (EN) | Long Text | `TEXT` |  |  |  |  |
| 20 | `icon` | Icon | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `programs_json` | Programs JSON | Long Text | `TEXT` |  |  |  |  |
| 22 | `dean_name_ar` | اسم عميد الكلية (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 23 | `dean_name` | اسم عميد الكلية | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `dean_name_en` | اسم عميد الكلية (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `dean_image` | صورة عميد الكلية | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 26 | `department_head_name` | اسم رئيس القسم | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `department_head_name_en` | اسم رئيس القسم (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 28 | `department_head_image` | صورة رئيس القسم | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 29 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 30 | `background_image` | Background Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 31 | `is_active` | Is Active | Check | `BOOLEAN` |  |  | `0` |  |
| 32 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Academic Departments

| Property | Value |
|----------|-------|
| DocType Name | Academic Departments |
| PostgreSQL Table | `tab_academic_departments` |
| Type | `Main` |
| Custom Fields | 6 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `department_name` | Department Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `department_name_en` | Department Name (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `college` | College | Link | `VARCHAR(255)` | ✅ |  |  | → `Colleges` |
| 4 | `description` | Description | Long Text | `TEXT` |  |  |  |  |
| 5 | `description_en` | Description (English) | Long Text | `TEXT` |  |  |  |  |
| 6 | `is_active` | Is Active | Check | `BOOLEAN` |  |  | `0` |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `college` | College | `Colleges` (`tab_colleges`) |

---

#### Academic Programs

| Property | Value |
|----------|-------|
| DocType Name | Academic Programs |
| PostgreSQL Table | `tab_academic_programs` |
| Type | `Main` |
| Custom Fields | 29 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `program_name` | Program Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `name_ar` | Name (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `name_en` | Name (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `department_ar` | Department (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `department_en` | Department (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `id` | ID | Data | `VARCHAR(255)` |  | ✅ |  |  |
| 7 | `degree_type` | Degree Type | Select | `VARCHAR(255)` |  |  |  | `Diploma`, `Bachelor`, `Master`, `PhD` |
| 8 | `admission_rate` | Admission Rate | Float | `DOUBLE PRECISION` |  |  |  |  |
| 9 | `high_school_type` | High School Type | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `high_school_type_en` | High School Type (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `high_school_type_ar` | High School Type (EN) (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `description` | Description | Long Text | `TEXT` |  |  |  |  |
| 13 | `description_ar` | Description (AR) | Long Text | `TEXT` |  |  |  |  |
| 14 | `description_en` | Description (EN) | Long Text | `TEXT` |  |  |  |  |
| 15 | `objectives_ar` | Program Objectives (AR) | Long Text | `TEXT` |  |  |  |  |
| 16 | `objectives_en` | Program Objectives (EN) | Long Text | `TEXT` |  |  |  |  |
| 17 | `career_prospects_ar` | Career Opportunities (AR) | Long Text | `TEXT` |  |  |  |  |
| 18 | `career_prospects_en` | Career Opportunities (EN) | Long Text | `TEXT` |  |  |  |  |
| 19 | `application_steps_ar` | Application Steps (AR) | Long Text | `TEXT` |  |  |  |  |
| 20 | `application_steps_en` | Application Steps (EN) | Long Text | `TEXT` |  |  |  |  |
| 21 | `why_program_ar` | Why This Program (AR) | Long Text | `TEXT` |  |  |  |  |
| 22 | `why_program_en` | Why This Program (EN) | Long Text | `TEXT` |  |  |  |  |
| 23 | `duration` | Duration | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `duration_en` | Duration (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `study_years` | Study Years | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `study_years_en` | Study Years (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 28 | `is_active` | Is Active | Check | `BOOLEAN` |  |  | `0` |  |
| 29 | `college` | College | Link | `VARCHAR(255)` | ✅ |  |  | → `Colleges` |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `college` | College | `Colleges` (`tab_colleges`) |

---

#### Admission Requirements

| Property | Value |
|----------|-------|
| DocType Name | Admission Requirements |
| PostgreSQL Table | `tab_admission_requirements` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Study Plans

| Property | Value |
|----------|-------|
| DocType Name | Study Plans |
| PostgreSQL Table | `tab_study_plans` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `plan_name` | Plan Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `plan_name_en` | Plan Name (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `academic_program` | Academic Program | Link | `VARCHAR(255)` | ✅ |  |  | → `Academic Programs` |
| 4 | `total_credits` | Total Credits | Int | `INTEGER` |  |  |  |  |
| 5 | `description` | Description | Long Text | `TEXT` |  |  |  |  |
| 6 | `description_en` | Description (English) | Long Text | `TEXT` |  |  |  |  |
| 7 | `is_active` | Is Active | Check | `BOOLEAN` |  |  | `0` |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `academic_program` | Academic Program | `Academic Programs` (`tab_academic_programs`) |

---

#### Study Plan Courses

| Property | Value |
|----------|-------|
| DocType Name | Study Plan Courses |
| PostgreSQL Table | `tab_study_plan_courses` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `study_plan` | Study Plan | Link | `VARCHAR(255)` | ✅ |  |  | → `Study Plans` |
| 2 | `course_name` | Course Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `course_code` | Course Code | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `credit_hours` | Credit Hours | Int | `INTEGER` |  |  |  |  |
| 5 | `semester` | Semester | Select | `VARCHAR(255)` |  |  |  | `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8` |
| 6 | `is_mandatory` | Is Mandatory | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `study_plan` | Study Plan | `Study Plans` (`tab_study_plans`) |

---

#### Grade

| Property | Value |
|----------|-------|
| DocType Name | Grade |
| PostgreSQL Table | `tab_grade` |
| Type | `Main` |
| Custom Fields | 0 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
---

### 👨‍🏫 Faculty

#### Faculty Members

| Property | Value |
|----------|-------|
| DocType Name | Faculty Members |
| PostgreSQL Table | `tab_faculty_members` |
| Type | `Main` |
| Custom Fields | 21 |
| Module | AAU |
| Child Tables | `Faculty Publication` (publications), `Faculty Course` (courses), `Faculty Education` (education), `Faculty Experience` (experience) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `full_name` | الاسم الكامل | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `full_name_en` | الاسم الكامل (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `academic_title` | اللقب الأكاديمي | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `academic_title_en` | اللقب الأكاديمي (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `linked_college` | الكلية المرتبطة | Link | `VARCHAR(255)` |  |  |  | → `Colleges` |
| 6 | `linked_program` | البرنامج المرتبط | Link | `VARCHAR(255)` |  |  |  | → `Academic Programs` |
| 7 | `department` | القسم الأكاديمي | Link | `VARCHAR(255)` |  |  |  | → `Academic Departments` |
| 8 | `biography` | النبذة التعريفية | Long Text | `TEXT` |  |  |  |  |
| 9 | `biography_en` | النبذة التعريفية (English) | Long Text | `TEXT` |  |  |  |  |
| 10 | `email` | البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `phone` | رقم الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `office_hours` | ساعات المكتب | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `office_hours_en` | Office Hours (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 14 | `research_interests_ar` | الاهتمامات البحثية | Small Text | `TEXT` |  |  |  |  |
| 15 | `research_interests_en` | Research Interests (English) | Small Text | `TEXT` |  |  |  |  |
| 16 | `publications` | المنشورات العلمية | Table | `INTEGER` |  |  |  | → `Faculty Publication` |
| 17 | `courses` | المقررات | Table | `INTEGER` |  |  |  | → `Faculty Course` |
| 18 | `education` | التعليم | Table | `INTEGER` |  |  |  | → `Faculty Education` |
| 19 | `experience` | الخبرات | Table | `INTEGER` |  |  |  | → `Faculty Experience` |
| 20 | `photo` | الصورة الشخصية | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 21 | `is_active` | مفعل | Check | `BOOLEAN` |  |  | `0` |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `linked_college` | الكلية المرتبطة | `Colleges` (`tab_colleges`) |
| `linked_program` | البرنامج المرتبط | `Academic Programs` (`tab_academic_programs`) |
| `department` | القسم الأكاديمي | `Academic Departments` (`tab_academic_departments`) |

---

#### Faculty Course

| Property | Value |
|----------|-------|
| DocType Name | Faculty Course |
| PostgreSQL Table | `tab_faculty_course` |
| Type | `Child` |
| Custom Fields | 4 |
| Module | AAU |
| Parent DocType | `Faculty Members` (courses) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `code` | Code | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `name_ar` | Name (AR) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `name_en` | Name (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `semester` | Semester | Data | `VARCHAR(255)` |  |  |  |  |
---

#### Faculty Education

| Property | Value |
|----------|-------|
| DocType Name | Faculty Education |
| PostgreSQL Table | `tab_faculty_education` |
| Type | `Child` |
| Custom Fields | 5 |
| Module | AAU |
| Parent DocType | `Faculty Members` (education) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `degree_ar` | Degree (AR) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `degree_en` | Degree (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `institution_ar` | Institution (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `institution_en` | Institution (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `year` | Year | Data | `VARCHAR(255)` |  |  |  |  |
---

#### Faculty Experience

| Property | Value |
|----------|-------|
| DocType Name | Faculty Experience |
| PostgreSQL Table | `tab_faculty_experience` |
| Type | `Child` |
| Custom Fields | 6 |
| Module | AAU |
| Parent DocType | `Faculty Members` (experience) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `position_ar` | Position (AR) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `position_en` | Position (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `organization_ar` | Organization (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `organization_en` | Organization (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `period_ar` | Period (AR) | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `period_en` | Period (EN) | Data | `VARCHAR(255)` |  |  |  |  |
---

#### Faculty Publication

| Property | Value |
|----------|-------|
| DocType Name | Faculty Publication |
| PostgreSQL Table | `tab_faculty_publication` |
| Type | `Child` |
| Custom Fields | 5 |
| Module | AAU |
| Parent DocType | `Faculty Members` (publications) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title_ar` | Title (AR) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `journal` | Journal | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `year` | Year | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `link` | Link | Data | `VARCHAR(255)` |  |  |  |  |
---

#### About Team Member

| Property | Value |
|----------|-------|
| DocType Name | About Team Member |
| PostgreSQL Table | `tab_about_team_member` |
| Type | `Child` |
| Custom Fields | 8 |
| Module | AAU |
| Parent DocType | `About University` (team_members) |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `group_name_ar` | المجموعة | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `group_name_en` | Group (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `full_name_ar` | الاسم | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `full_name_en` | Name (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `job_title_ar` | المسمى الوظيفي | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `job_title_en` | Job Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 7 | `member_image` | الصورة | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 8 | `display_order` | الترتيب | Int | `INTEGER` |  |  | `0` |  |
---

#### Team Members

| Property | Value |
|----------|-------|
| DocType Name | Team Members |
| PostgreSQL Table | `tab_team_members` |
| Type | `Main` |
| Custom Fields | 12 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | ID | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `name_ar` | Name Ar | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `name_en` | Name En | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `position_ar` | Position Ar | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `position_en` | Position En | Data | `VARCHAR(255)` |  |  |  |  |
| 6 | `bio_ar` | Bio Ar | Long Text | `TEXT` |  |  |  |  |
| 7 | `bio_en` | Bio En | Long Text | `TEXT` |  |  |  |  |
| 8 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 9 | `email` | Email | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `phone` | Phone | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 12 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

### 📞 Contact & Communication

#### Contact Page Settings

| Property | Value |
|----------|-------|
| DocType Name | Contact Page Settings |
| PostgreSQL Table | `tab_contact_page_settings` |
| Type | `Single` |
| Custom Fields | 39 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `contact_page_badge_ar` | شارة الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `contact_page_badge_en` | Page Badge (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `contact_page_title_ar` | عنوان الصفحة | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `contact_page_title_en` | Page Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `contact_page_description_ar` | وصف الصفحة | Small Text | `TEXT` |  |  |  |  |
| 6 | `contact_page_description_en` | Page Description (EN) | Small Text | `TEXT` |  |  |  |  |
| 7 | `contact_form_title_ar` | عنوان النموذج | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `contact_form_title_en` | Form Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `contact_form_name_label_ar` | تسمية الاسم | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `contact_form_name_label_en` | Name Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `contact_form_name_placeholder_ar` | Placeholder الاسم | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `contact_form_name_placeholder_en` | Name Placeholder (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `contact_form_email_label_ar` | تسمية البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 14 | `contact_form_email_label_en` | Email Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 15 | `contact_form_email_placeholder_ar` | Placeholder البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 16 | `contact_form_email_placeholder_en` | Email Placeholder (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 17 | `contact_form_subject_label_ar` | تسمية الموضوع | Data | `VARCHAR(255)` |  |  |  |  |
| 18 | `contact_form_subject_label_en` | Subject Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `contact_form_subject_placeholder_ar` | Placeholder الموضوع | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `contact_form_subject_placeholder_en` | Subject Placeholder (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `contact_form_message_label_ar` | تسمية الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 22 | `contact_form_message_label_en` | Message Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 23 | `contact_form_message_placeholder_ar` | Placeholder الرسالة | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `contact_form_message_placeholder_en` | Message Placeholder (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `contact_form_submit_text_ar` | نص زر الإرسال | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `contact_form_submit_text_en` | Submit Text (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `contact_phone_label_ar` | تسمية الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 28 | `contact_phone_label_en` | Phone Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 29 | `contact_email_label_ar` | تسمية البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 30 | `contact_email_label_en` | Email Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 31 | `contact_address_label_ar` | تسمية العنوان | Data | `VARCHAR(255)` |  |  |  |  |
| 32 | `contact_address_label_en` | Address Label (EN) | Data | `VARCHAR(255)` |  |  |  |  |
| 33 | `contact_phone` | رقم الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 34 | `contact_email` | البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 35 | `address_ar` | العنوان | Small Text | `TEXT` |  |  |  |  |
| 36 | `address_en` | Address (EN) | Small Text | `TEXT` |  |  |  |  |
| 37 | `map_location` | رابط الخريطة | Data | `VARCHAR(255)` |  |  |  |  |
| 38 | `social_section_title_ar` | عنوان الشبكات الاجتماعية | Data | `VARCHAR(255)` |  |  |  |  |
| 39 | `social_section_title_en` | Social Section Title (EN) | Data | `VARCHAR(255)` |  |  |  |  |
---

#### Contact Us Messages

| Property | Value |
|----------|-------|
| DocType Name | Contact Us Messages |
| PostgreSQL Table | `tab_contact_us_messages` |
| Type | `Main` |
| Custom Fields | 10 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `sender_name` | Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `email` | Email | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `subject` | Subject | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `message` | Message | Long Text | `TEXT` | ✅ |  |  |  |
| 5 | `received_date` | Received Date | Datetime | `TIMESTAMP` |  |  |  |  |
| 6 | `id` | ID | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 7 | `phone` | Phone | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `status` | Status | Select | `VARCHAR(255)` | ✅ |  |  | `new`, `read`, `replied`, `archived` |
| 9 | `replied_at` | Replied At | Datetime | `TIMESTAMP` |  |  |  |  |
| 10 | `created_at` | Created At | Datetime | `TIMESTAMP` | ✅ |  |  |  |
---

#### Email Requests

| Property | Value |
|----------|-------|
| DocType Name | Email Requests |
| PostgreSQL Table | `tab_email_requests` |
| Type | `Main` |
| Custom Fields | 9 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | ID | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `name_ar` | Name Ar | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `name_en` | Name En | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `academic_id` | Academic ID | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 5 | `college` | College | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 6 | `level` | Level | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 7 | `phone` | Phone | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 8 | `status` | Status | Select | `VARCHAR(255)` | ✅ |  | `pending` | `pending`, `reviewed`, `completed`, `rejected` |
| 9 | `created_at` | Created At | Datetime | `TIMESTAMP` | ✅ |  |  |  |
---

#### Smart Chat Settings

| Property | Value |
|----------|-------|
| DocType Name | Smart Chat Settings |
| PostgreSQL Table | `tab_smart_chat_settings` |
| Type | `Single` |
| Custom Fields | 13 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `enabled` | Enable Smart Chat | Check | `BOOLEAN` |  |  | `1` |  |
| 2 | `provider` | AI Provider | Select | `VARCHAR(255)` | ✅ |  | `Gemini` | `Gemini`, `OpenAI` |
| 3 | `chat_name_ar` | Chat Name (Arabic) | Data | `VARCHAR(255)` | ✅ |  | `المساعد الذكي` |  |
| 4 | `chat_name_en` | Chat Name (English) | Data | `VARCHAR(255)` | ✅ |  | `Smart Assistant` |  |
| 5 | `generation_model` | Generation Model | Data | `VARCHAR(255)` |  |  | `gemini-2.5-flash` |  |
| 6 | `embedding_model` | Embedding Model | Data | `VARCHAR(255)` |  |  | `gemini-embedding-001` |  |
| 7 | `temperature` | Temperature | Float | `DOUBLE PRECISION` |  |  | `0.25` |  |
| 8 | `provider_api_key` | Provider API Key | Password | `VARCHAR(255)` | ✅ |  |  |  |
| 9 | `knowledge_source_file` | Knowledge Source File | Attach | `VARCHAR(512)` | ✅ |  |  |  |
| 10 | `knowledge_index_file` | Knowledge Index File | Attach | `VARCHAR(512)` |  |  |  |  |
| 11 | `indexed_records` | Indexed Records | Int | `INTEGER` |  |  |  |  |
| 12 | `last_indexed_on` | Last Indexed On | Datetime | `TIMESTAMP` |  |  |  |  |
| 13 | `help_text` | Notes | Small Text | `TEXT` |  |  | `ارفع ملف الأسئلة ثم احفظ السجل وبعدها اضغط على زر إعادة بناء فهرس الشات. المفتاح يتم حفظه داخل النظام ولا يتم كشفه في الواجهات العامة.` |  |
---

### 🏛️ Centers & Projects

#### Centers

| Property | Value |
|----------|-------|
| DocType Name | Centers |
| PostgreSQL Table | `tab_centers` |
| Type | `Main` |
| Custom Fields | 16 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | المعرف | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `title_ar` | اسم المركز | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `title_en` | Title En | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `desc_ar` | وصف المركز | Long Text | `TEXT` |  |  |  |  |
| 5 | `desc_en` | Desc En | Long Text | `TEXT` |  |  |  |  |
| 6 | `services` | الخدمات | Long Text | `TEXT` |  |  |  |  |
| 7 | `services_en` | الخدمات (English) | Long Text | `TEXT` |  |  |  |  |
| 8 | `programs` | البرامج المرتبطة | Long Text | `TEXT` |  |  |  |  |
| 9 | `programs_en` | البرامج المرتبطة (English) | Long Text | `TEXT` |  |  |  |  |
| 10 | `image` | صورة المركز | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 11 | `location` | الموقع | Data | `VARCHAR(255)` |  |  |  |  |
| 12 | `location_en` | الموقع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `phone` | رقم الهاتف | Data | `VARCHAR(255)` |  |  |  |  |
| 14 | `email` | البريد الإلكتروني | Data | `VARCHAR(255)` |  |  |  |  |
| 15 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 16 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
---

#### Center Services

| Property | Value |
|----------|-------|
| DocType Name | Center Services |
| PostgreSQL Table | `tab_center_services` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### University Centers

| Property | Value |
|----------|-------|
| DocType Name | University Centers |
| PostgreSQL Table | `tab_university_centers` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Projects

| Property | Value |
|----------|-------|
| DocType Name | Projects |
| PostgreSQL Table | `tab_projects` |
| Type | `Main` |
| Custom Fields | 16 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | ID | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `slug` | Slug | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `title_ar` | Title Ar | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `title_en` | Title En | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `desc_ar` | Desc Ar | Long Text | `TEXT` |  |  |  |  |
| 6 | `desc_en` | Desc En | Long Text | `TEXT` |  |  |  |  |
| 7 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 8 | `details_ar` | Details Ar | Long Text | `TEXT` |  |  |  |  |
| 9 | `details_en` | Details En | Long Text | `TEXT` |  |  |  |  |
| 10 | `start_date` | Start Date | Date | `DATE` |  |  |  |  |
| 11 | `end_date` | End Date | Date | `DATE` |  |  |  |  |
| 12 | `year` | Year | Int | `INTEGER` |  |  |  |  |
| 13 | `progress` | Progress | Int | `INTEGER` |  |  |  |  |
| 14 | `status` | Status | Select | `VARCHAR(255)` |  |  |  | `current`, `completed`, `planned` |
| 15 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 16 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Research and Publications

| Property | Value |
|----------|-------|
| DocType Name | Research and Publications |
| PostgreSQL Table | `tab_research_and_publications` |
| Type | `Main` |
| Custom Fields | 8 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `publish_date` | Publish Date | Date | `DATE` |  |  |  |  |
| 7 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 8 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Partners

| Property | Value |
|----------|-------|
| DocType Name | Partners |
| PostgreSQL Table | `tab_partners` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | اسم الشريك (عربي) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | اسم الشريك (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | وصف الشريك (عربي) | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | وصف الشريك (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | صورة الشريك | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
---

#### Offers

| Property | Value |
|----------|-------|
| DocType Name | Offers |
| PostgreSQL Table | `tab_offers` |
| Type | `Main` |
| Custom Fields | 44 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | المعرف | Data | `VARCHAR(255)` |  |  |  |  |
| 2 | `title_ar` | عنوان العرض (عربي) | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 3 | `title_en` | عنوان العرض (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `desc_ar` | الوصف المختصر (عربي) | Long Text | `TEXT` |  |  |  |  |
| 5 | `desc_en` | الوصف المختصر (English) | Long Text | `TEXT` |  |  |  |  |
| 6 | `details_ar` | تفاصيل العرض (عربي) | Long Text | `TEXT` |  |  |  |  |
| 7 | `details_en` | تفاصيل العرض (English) | Long Text | `TEXT` |  |  |  |  |
| 8 | `category` | الفئة | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `category_en` | الفئة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `offer_image` | صورة العرض (رفع) | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 11 | `image` | صورة العرض | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 12 | `valid_until` | صالح حتى | Date | `DATE` |  |  |  |  |
| 13 | `target_audience_ar` | الفئة المستهدفة (عربي) | Long Text | `TEXT` |  |  |  |  |
| 14 | `target_audience_en` | الفئة المستهدفة (English) | Long Text | `TEXT` |  |  |  |  |
| 15 | `benefits_ar` | المزايا (عربي) | Long Text | `TEXT` |  |  |  |  |
| 16 | `benefits_en` | المزايا (English) | Long Text | `TEXT` |  |  |  |  |
| 17 | `duration_ar` | المدة (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 18 | `duration_en` | المدة (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 19 | `interest_title_ar` | عنوان القسم (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 20 | `interest_title_en` | عنوان القسم (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 21 | `interest_subtitle_ar` | وصف القسم (عربي) | Long Text | `TEXT` |  |  |  |  |
| 22 | `interest_subtitle_en` | وصف القسم (English) | Long Text | `TEXT` |  |  |  |  |
| 23 | `interest_open_title_ar` | عنوان بطاقة متاح للجميع (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 24 | `interest_open_title_en` | عنوان بطاقة متاح للجميع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 25 | `interest_open_subtitle_ar` | وصف بطاقة متاح للجميع (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 26 | `interest_open_subtitle_en` | وصف بطاقة متاح للجميع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 27 | `interest_highlights_title_ar` | عنوان مميزات العرض (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 28 | `interest_highlights_title_en` | عنوان مميزات العرض (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 29 | `interest_highlight_1_ar` | مميزة العرض 1 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 30 | `interest_highlight_1_en` | مميزة العرض 1 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 31 | `interest_highlight_2_ar` | مميزة العرض 2 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 32 | `interest_highlight_2_en` | مميزة العرض 2 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 33 | `interest_highlight_3_ar` | مميزة العرض 3 (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 34 | `interest_highlight_3_en` | مميزة العرض 3 (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 35 | `interest_notice_ar` | تنبيه القسم (عربي) | Long Text | `TEXT` |  |  |  |  |
| 36 | `interest_notice_en` | تنبيه القسم (English) | Long Text | `TEXT` |  |  |  |  |
| 37 | `location_ar` | الموقع (عربي) | Data | `VARCHAR(255)` |  |  |  |  |
| 38 | `location_en` | الموقع (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 39 | `requirements_ar` | الشروط والمتطلبات (عربي) | Long Text | `TEXT` |  |  |  |  |
| 40 | `requirements_en` | الشروط والمتطلبات (English) | Long Text | `TEXT` |  |  |  |  |
| 41 | `apply_link` | رابط التقديم | Data | `VARCHAR(255)` |  |  |  |  |
| 42 | `is_active` | مفعل | Check | `BOOLEAN` |  |  | `0` |  |
| 43 | `is_published` | منشور | Check | `BOOLEAN` |  |  | `0` |  |
| 44 | `display_order` | ترتيب العرض | Int | `INTEGER` |  |  |  |  |
---

### 👨‍🎓 Student Portal

#### Student Activities

| Property | Value |
|----------|-------|
| DocType Name | Student Activities |
| PostgreSQL Table | `tab_student_activities` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title` | Title | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `content` | Content | Long Text | `TEXT` |  |  |  |  |
| 4 | `content_en` | Content (English) | Long Text | `TEXT` |  |  |  |  |
| 5 | `image` | Image | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 6 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `0` |  |
| 7 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
---

#### Student Affairs Document

| Property | Value |
|----------|-------|
| DocType Name | Student Affairs Document |
| PostgreSQL Table | `tab_student_affairs_document` |
| Type | `Main` |
| Custom Fields | 7 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `title_ar` | Title Ar | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `title_en` | Title En | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `file_url` | File URL | Attach | `VARCHAR(512)` |  |  |  |  |
| 4 | `file_coming_soon` | File Coming Soon | Check | `BOOLEAN` |  |  | `1` |  |
| 5 | `is_published` | Is Published | Check | `BOOLEAN` |  |  | `1` |  |
| 6 | `display_order` | Display Order | Int | `INTEGER` |  |  |  |  |
| 7 | `id` | ID | Data | `VARCHAR(255)` |  | ✅ |  |  |
---

#### Student Portal Survey Response

| Property | Value |
|----------|-------|
| DocType Name | Student Portal Survey Response |
| PostgreSQL Table | `tab_student_portal_survey_response` |
| Type | `Main` |
| Custom Fields | 13 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `student` | Student | Link | `VARCHAR(255)` | ✅ |  |  | → `Student` |
| 2 | `student_name_ar` | Student Name (Arabic) | Data | `VARCHAR(255)` |  |  |  |  |
| 3 | `student_name_en` | Student Name (English) | Data | `VARCHAR(255)` |  |  |  |  |
| 4 | `academic_number` | Academic Number | Data | `VARCHAR(255)` |  |  |  |  |
| 5 | `user` | User | Link | `VARCHAR(255)` |  |  |  | → `User` |
| 6 | `submitted_on` | Submitted On | Datetime | `TIMESTAMP` |  |  |  |  |
| 7 | `status` | Status | Select | `VARCHAR(255)` |  |  | `Submitted` | `Submitted`, `Reviewed` |
| 8 | `digital_services` | Digital Services Rating | Int | `INTEGER` | ✅ |  |  |  |
| 9 | `campus_life` | Campus Life | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 10 | `academic_support` | Academic Support | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 11 | `suggestions` | Suggestions | Small Text | `TEXT` |  |  |  |  |
| 12 | `submitted_from` | Submitted From | Data | `VARCHAR(255)` |  |  |  |  |
| 13 | `language` | Language | Data | `VARCHAR(255)` |  |  |  |  |

**Foreign Key Relationships:**

| Field | Label | References |
|-------|-------|------------|
| `student` | Student | `Student` (`tab_student`) |
| `user` | User | `User` (`tab_user`) |

---

#### Join Requests

| Property | Value |
|----------|-------|
| DocType Name | Join Requests |
| PostgreSQL Table | `tab_join_requests` |
| Type | `Main` |
| Custom Fields | 20 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `id` | ID | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 2 | `type` | Type | Select | `VARCHAR(255)` | ✅ |  |  | `student`, `employee` |
| 3 | `full_name` | Name | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 4 | `email` | Email | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 5 | `phone` | Phone | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 6 | `specialty` | Specialty | Data | `VARCHAR(255)` | ✅ |  |  |  |
| 7 | `college_id` | College ID | Data | `VARCHAR(255)` |  |  |  |  |
| 8 | `college_name` | College Name | Data | `VARCHAR(255)` |  |  |  |  |
| 9 | `program_id` | Program ID | Data | `VARCHAR(255)` |  |  |  |  |
| 10 | `program_name` | Program Name | Data | `VARCHAR(255)` |  |  |  |  |
| 11 | `education_status` | Education Status | Select | `VARCHAR(255)` |  |  | `graduate` | `graduate`, `student` |
| 12 | `has_required_documents` | Has Required Documents | Check | `BOOLEAN` |  |  | `0` |  |
| 13 | `high_school_document_name` | High School Document Name | Attach | `VARCHAR(512)` |  |  |  |  |
| 14 | `id_document_name` | ID Document Name | Attach | `VARCHAR(512)` |  |  |  |  |
| 15 | `personal_photo_name` | Personal Photo Name | Attach Image | `VARCHAR(512)` |  |  |  |  |
| 16 | `serial_number` | Serial Number | Data | `VARCHAR(255)` |  |  |  |  |
| 17 | `message` | Message | Long Text | `TEXT` |  |  |  |  |
| 18 | `status` | Status | Select | `VARCHAR(255)` | ✅ |  |  | `pending`, `reviewed`, `accepted`, `rejected` |
| 19 | `reviewed_at` | Reviewed At | Datetime | `TIMESTAMP` |  |  |  |  |
| 20 | `created_at` | Created At | Datetime | `TIMESTAMP` | ✅ |  |  |  |
---

#### Dashboard Metrics

| Property | Value |
|----------|-------|
| DocType Name | Dashboard Metrics |
| PostgreSQL Table | `tab_dashboard_metrics` |
| Type | `Single` |
| Custom Fields | 3 |
| Module | AAU |

**Fields:**

| # | Fieldname | Label | Frappe Type | PostgreSQL Type | Required | Unique | Default | Options / Link Target |
|---|-----------|-------|-------------|-----------------|----------|--------|---------|----------------------|
| 1 | `total_students` | إجمالي الطلاب | Int | `INTEGER` |  |  |  |  |
| 2 | `total_programs` | إجمالي البرامج | Int | `INTEGER` |  |  |  |  |
| 3 | `total_courses` | إجمالي المقررات | Int | `INTEGER` |  |  |  |  |
---

## Relationships Summary

### Composition Relationships (Parent → Child)

These represent Frappe's child table pattern where child records are stored in separate tables and linked via `parent`/`parentfield`/`parenttype` columns.

| Parent DocType | Parent Table | Field | Child DocType | Child Table |
|---------------|-------------|-------|--------------|-------------|
| AAU Menu | `tab_aau_menu` | `items` (Items) | AAU Menu Item | `tab_aau_menu_item` |
| About University | `tab_about_university` | `team_members` (أعضاء الفريق الإداري) | About Team Member | `tab_about_team_member` |
| Faculty Members | `tab_faculty_members` | `publications` (المنشورات العلمية) | Faculty Publication | `tab_faculty_publication` |
| Faculty Members | `tab_faculty_members` | `courses` (المقررات) | Faculty Course | `tab_faculty_course` |
| Faculty Members | `tab_faculty_members` | `education` (التعليم) | Faculty Education | `tab_faculty_education` |
| Faculty Members | `tab_faculty_members` | `experience` (الخبرات) | Faculty Experience | `tab_faculty_experience` |
| Website Settings | `tab_website_settings` | `top_bar_items` (Top Bar Items) | Top Bar Item | `tab_top_bar_item` |
| Website Settings | `tab_website_settings` | `footer_items` (Footer Items) | Top Bar Item | `tab_top_bar_item` |
| Website Settings | `tab_website_settings` | `route_redirects` (Route Redirects) | Website Route Redirect | `tab_website_route_redirect` |

### Link Field Relationships (Foreign Keys)

These represent Frappe Link fields that reference other DocTypes, similar to foreign keys.

| Source DocType | Source Table | Field | Label | Target DocType | Target Table |
|--------------|-------------|-------|-------|---------------|-------------|
| Academic Departments | `tab_academic_departments` | `college` | College | Colleges | `tab_colleges` |
| Academic Programs | `tab_academic_programs` | `college` | College | Colleges | `tab_colleges` |
| Faculty Members | `tab_faculty_members` | `linked_college` | الكلية المرتبطة | Colleges | `tab_colleges` |
| Faculty Members | `tab_faculty_members` | `linked_program` | البرنامج المرتبط | Academic Programs | `tab_academic_programs` |
| Faculty Members | `tab_faculty_members` | `department` | القسم الأكاديمي | Academic Departments | `tab_academic_departments` |
| News | `tab_news` | `college` | الكلية المرتبطة | Colleges | `tab_colleges` |
| Student Portal Survey Response | `tab_student_portal_survey_response` | `student` | Student | Student | `tab_student` |
| Student Portal Survey Response | `tab_student_portal_survey_response` | `user` | User | User | `tab_user` |
| Study Plan Courses | `tab_study_plan_courses` | `study_plan` | Study Plan | Study Plans | `tab_study_plans` |
| Study Plans | `tab_study_plans` | `academic_program` | Academic Program | Academic Programs | `tab_academic_programs` |
| Website Settings | `tab_website_settings` | `website_theme` | Website Theme | Website Theme | `tab_website_theme` |
| Website Settings | `tab_website_settings` | `navbar_template` | Navbar Template | Web Template | `tab_web_template` |
| Website Settings | `tab_website_settings` | `footer_template` | Footer Template | Web Template | `tab_web_template` |

### Frappe Standard Fields

Every Frappe DocType table includes these standard columns:

| Fieldname | Frappe Type | PostgreSQL Type | Description |
|-----------|------------|-----------------|-------------|
| `name` | Data | `VARCHAR(255)` | Primary key, auto-generated document name |
| `creation` | Datetime | `TIMESTAMP` | Timestamp of document creation |
| `modified` | Datetime | `TIMESTAMP` | Timestamp of last modification |
| `modified_by` | Data | `VARCHAR(255)` | User who last modified the document |
| `owner` | Data | `VARCHAR(255)` | User who created the document |
| `docstatus` | Int | `INTEGER` | Document status (0=Draft, 1=Submitted, 2=Cancelled) |
| `idx` | Int | `INTEGER` | Index for sorting |
| `parent` | Data | `VARCHAR(255)` | Parent document name (for child tables) |
| `parentfield` | Data | `VARCHAR(255)` | Field name in parent (for child tables) |
| `parenttype` | Data | `VARCHAR(255)` | Parent DocType name (for child tables) |

### Frappe-to-PostgreSQL Type Mapping

| Frappe Field Type | PostgreSQL Type | Notes |
|------------------|-----------------|-------|
| Data | `VARCHAR(255)` |  |
| Text | `TEXT` |  |
| Text Editor | `TEXT` |  |
| Small Text | `TEXT` |  |
| Int | `INTEGER` |  |
| Float | `DOUBLE PRECISION` |  |
| Currency | `DOUBLE PRECISION` |  |
| Check | `BOOLEAN` |  |
| Date | `DATE` |  |
| Datetime | `TIMESTAMP` |  |
| Time | `TIME` |  |
| Link | `VARCHAR(255)` |  |
| Select | `VARCHAR(255)` |  |
| Attach | `VARCHAR(512)` |  |
| Attach Image | `VARCHAR(512)` |  |
| Table | `INTEGER` |  |
| JSON | `TEXT` |  |
| Long Text | `TEXT` |  |
| Code | `TEXT` |  |
| HTML | `TEXT` |  |
| Color | `VARCHAR(255)` |  |
| Password | `VARCHAR(255)` |  |
| Read Only | `VARCHAR(255)` |  |
| Button | `VARCHAR(255)` |  |
| Dynamic Link | `VARCHAR(255)` |  |
| Signature | `TEXT` |  |
| Autocomplete | `VARCHAR(255)` |  |
| Geolocation | `TEXT` |  |
| Icon | `VARCHAR(255)` |  |
| Rating | `DOUBLE PRECISION` |  |
| Date Range | `VARCHAR(255)` |  |
| Tab Break | `—` | Layout only, no column |
| Section Break | `—` | Layout only, no column |
| Column Break | `—` | Layout only, no column |
| Heading | `—` | Layout only, no column |
| HTML Editor | `TEXT` |  |

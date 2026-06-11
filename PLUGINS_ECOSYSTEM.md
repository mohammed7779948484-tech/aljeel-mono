# 🔌 دليل شامل لـ Plugins الخاصة بـ Payload CMS v3 + Puck

> 📅 محدّث في يونيو 2026 — يحتوي على **246+ plugin** من المجتمع و**11 plugin رسمي**.
> 🔍 المصادر: [PayloadDirectory.dev](https://www.payloaddirectory.dev/) [2](https://www.payloaddirectory.dev/) · [Awesome Payload](https://github.com/DanailMinchev/awesome-payload) [1](https://github.com/DanailMinchev/awesome-payload) · [GitHub Topic: payload-plugin](https://github.com/topics/payload-plugin) [3](https://github.com/topics/payload-plugin)

---

## 🎨 أولاً: تكامل Puck مع Payload CMS

### 🥇 الخيار الأفضل والأكثر اكتمالاً: `@delmaredigital/payload-puck`

**أهم plugin في المجتمع لدمج Puck مع Payload v3** — مبني من Delmare Digital وهو نشط جداً [5](https://www.npmjs.com/package/@delmaredigital/payload-puck).

| المعلومة | القيمة |
|---|---|
| 📦 NPM | `@delmaredigital/payload-puck` |
| ⭐ الإصدار الحالي | `0.6.23` (محدث منذ 18 يوم) |
| 🔗 GitHub | [delmaredigital/payload-puck](https://github.com/delmaredigital/payload-puck) |
| 📚 الوثائق | [delmaredigital.github.io/payload-puck](https://delmaredigital.github.io/payload-puck/) |
| 🎮 Demo حي | [demo.delmaredigital.com](https://demo.delmaredigital.com) |
| 📜 الترخيص | MIT |

#### المميزات الكاملة [3](https://github.com/delmaredigital/payload-puck):
- ✅ **تثبيت بسطر واحد** — `createPuckPlugin()` يضيف كل شيء تلقائياً
- ✅ **+15 مكوّن جاهز** (Hero, Features, RichText, CTA, etc.)
- ✅ **محرر داخل Payload Admin** على `/admin/puck-editor/:collection/:id`
- ✅ **API endpoints تلقائية** على `/api/puck/:collection`
- ✅ **زر "Edit with Puck"** في list view لـ collection
- ✅ **Page Tree Integration** مع `@delmaredigital/payload-page-tree`
- ✅ **Hybrid rendering** (يدعم Blocks القديمة + Puck)
- ✅ **AI Integration** (Puck AI Builder رسمي)
- ✅ **Theming + Dark Mode**
- ✅ **Layouts/Templates** للصفحات
- ✅ **Custom Fields** (Media picker، Color picker، إلخ)

#### المتطلبات:
```
@puckeditor/core >= 0.21.0   ⚠️ (Puck 0.21 انتقل من @measured/puck)
payload >= 3.69.0
@payloadcms/next >= 3.69.0
next >= 15.4.8
react >= 19.2.1
```

#### تثبيت سريع:
```bash
pnpm add @delmaredigital/payload-puck @puckeditor/core
```

```ts
// payload.config.ts
import { createPuckPlugin } from '@delmaredigital/payload-puck/plugin'

export default buildConfig({
  plugins: [createPuckPlugin({ pagesCollection: 'pages' })],
})
```

> ⚠️ **ملاحظة مهمة جداً**: في الكود الذي بنيناه سابقاً استخدمنا `@measured/puck` (القديم). إن أردت استخدام `@delmaredigital/payload-puck` يجب الترقية إلى `@puckeditor/core`.

---

### 🥈 الخيار البديل: `puckload-poc` (POC من المجتمع)

| المعلومة | القيمة |
|---|---|
| 🔗 GitHub | [Copystrike/puckload-poc](https://github.com/Copystrike/puckload-poc) |
| 📝 الوصف | Proof of Concept لاستبدال block builder الافتراضي بـ Puck داخل Payload Admin [5](https://github.com/Copystrike/puckload-poc) |
| 🎯 الاستخدام | للتعلم وفهم البنية، ليس production-ready |

كيف يعمل [5](https://github.com/Copystrike/puckload-poc):
- ينشئ route مخصص على `admin/puck-builder`
- يضيف زر داخل Payload يفتح UI مخصص للـ blocks
- يستخدم `PuckBuilder field` كحقل مخصص

---

### 🥉 Starter Template كامل: `dd-starter`

| المعلومة | القيمة |
|---|---|
| 🔗 GitHub | [delmaredigital/dd-starter](https://github.com/delmaredigital/dd-starter) [9](https://github.com/delmaredigital/dd-starter) |
| 📦 يحتوي | Payload v3 + Puck + Page Tree + Better Auth |
| 🚀 النشر | Vercel quick-deploy جاهز |

ينشر صفحة مع كل الإعدادات الجاهزة:
```
src/
├── app/(frontend)/      # Next.js routes
├── app/(payload)/       # Payload admin
├── collections/         # Posts, Media, Users
├── lib/auth/            # Better Auth
├── lib/puck/            # Puck layouts
├── puck/                # Puck config
└── plugins/             # All plugins wired
```

---

## 🏛️ ثانياً: الـ Plugins الرسمية من فريق Payload

كلها تحت namespace `@payloadcms/plugin-*` وتُحدّث مع كل إصدار من Payload [2](https://www.payloaddirectory.dev/) [4](https://deepwiki.com/payloadcms/payload/10.2-official-plugins):

| Plugin | الوظيفة | تحميلات/أسبوع |
|---|---|---|
| **`@payloadcms/plugin-seo`** | SEO، Open Graph، Twitter Cards، Schema.org | **138.1k** |
| **`@payloadcms/plugin-cloud-storage`** | تكامل تخزين سحابي (مع S3, Azure, GCS adapters) | **286.9k** |
| **`@payloadcms/plugin-redirects`** | إدارة إعادة التوجيه (302/301) مع Next.js | **75.1k** |
| **`@payloadcms/plugin-form-builder`** | بناء نماذج ديناميكي + تخزين الإجابات + Payments | **70.1k** |
| **`@payloadcms/plugin-search`** | بحث موحّد عبر عدة collections | **64.6k** |
| **`@payloadcms/plugin-nested-docs`** | هيكلية أب-ابن، breadcrumbs، شجرة | **63.0k** |
| **`@payloadcms/plugin-mcp`** | Model Context Protocol لـ AI agents | **38.0k** |
| **`@payloadcms/plugin-multi-tenant`** | Multi-tenancy لتطبيقات SaaS [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins) | **35.1k** |
| **`@payloadcms/plugin-sentry`** | تكامل Sentry لمراقبة الأخطاء | **24.4k** |
| **`@payloadcms/plugin-import-export`** | استيراد/تصدير CSV و JSON مع job queue | **21.4k** |
| **`@payloadcms/plugin-ecommerce`** | متجر إلكتروني (جديد!) | **4.2k** |

### حزم Storage Adapters الرسمية (مكملة لـ cloud-storage):

| Adapter | الخدمة |
|---|---|
| `@payloadcms/storage-s3` | AWS S3 / أي S3-compatible |
| `@payloadcms/storage-vercel-blob` | Vercel Blob |
| `@payloadcms/storage-gcs` | Google Cloud Storage |
| `@payloadcms/storage-azure` | Azure Blob Storage |
| `@payloadcms/storage-uploadthing` | UploadThing |
| `@payloadcms/storage-r2` | Cloudflare R2 |

---

## 🔐 ثالثاً: Plugins المصادقة (Auth)

أهم فئة في المجتمع لأن Payload الافتراضي يدعم email/password فقط [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins):

| Plugin | الوصف | ⭐ النجوم |
|---|---|---|
| **[authsmith/payload-auth-plugin](https://github.com/authsmith/payload-auth-plugin)** | OAuth + SAML + SSO + OIDC + Passkeys/WebAuthn — **الأكثر شعبية** | ~291 |
| **[CrawlerCode/payload-authjs](https://github.com/CrawlerCode/payload-authjs)** | تكامل Auth.js (NextAuth) الكامل | - |
| **[payload-auth/payload-auth](https://github.com/payload-auth/payload-auth)** | Monorepo يدمج Payload مع عدة مزودي auth | - |
| **[delmaredigital/payload-better-auth](https://github.com/delmaredigital)** | تكامل Better Auth (يدعم OAuth، Magic Links، TOTP، API Keys، Passkeys) [6](https://www.delmaredigital.com/developers) | - |
| **[sourabpramanik/payload-auth-plugin](https://github.com/sourabpramanik/payload-auth-plugin)** | OAuth بسيط (مذكور في Reddit كبديل) [5](https://www.reddit.com/r/PayloadCMS/comments/1gurs5f/payload_cms_30_is_live/) | - |
| **payload-totp** | إضافة 2FA باستخدام TOTP [3](https://github.com/topics/payload-plugin) | - |
| **payload-oauth2** | OAuth2 خفيف الوزن | - |

---

## 🤖 رابعاً: Plugins الـ AI

| Plugin | الوصف | ⭐ |
|---|---|---|
| **[ashbuilds/payload-ai](https://github.com/ashbuilds/payload-ai)** | **الأكثر شعبية في المجتمع كله** — توليد نصوص/صور/صوت + ترجمة AI داخل Admin [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins) | **430** |
| **[Crayonan/payload-plugin-ai-localization](https://github.com/Crayonan/payload-plugin-ai-localization)** | زر "Translate" تلقائي بـ OpenAI للحقول المترجمة | - |
| **[techiejd/payloadcms-vectorize](https://github.com/techiejd/payloadcms-vectorize)** | Vector embeddings + semantic search (يدعم MongoDB) [8](https://www.reddit.com/r/PayloadCMS/) | - |
| **`@focus-reactive/payload-plugin-translator`** | ترجمة AI كاملة [8](https://www.reddit.com/r/PayloadCMS/) | - |

---

## 📝 خامساً: Plugins تحسين Admin UI / Editor

| Plugin | الوصف |
|---|---|
| **[r1tsuu/payload-enchants](https://github.com/r1tsuu/payload-enchants)** | مجموعة كاملة من الإضافات لتحسين Admin |
| **[shefing/payload-tools](https://github.com/shefing/payload-tools)** | باقة قوية: Color Picker، Icon Select، Quick Filter، Right Panel، Custom Version View، Authorization، Comments [1](https://github.com/DanailMinchev/awesome-payload) |
| **[rilrom/payload-bites](https://github.com/rilrom/payload-bites)** | Bite-sized plugins: image-search، fullscreen-editor، audit-fields، soft-delete، activity-log [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins) |
| **[AdrianMaj/payload-lexical-typography](https://github.com/AdrianMaj/payload-lexical-typography)** | إضافة Font Color، Size، Letter Spacing، Line Height، Font Family لـ Lexical |
| **payload-lexical** | امتدادات للـ Lexical Editor [3](https://github.com/topics/payload-plugin) |
| **[@matija2209/payload-plugin-admin-feedback](https://www.buildwithmatija.com/blog/best-payload-cms-plugins)** | Widget لجمع feedback من المستخدمين مع screenshots [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins) |

---

## 🏗️ سادساً: Plugins Page Builders & Visual Editors

| Plugin | الوصف |
|---|---|
| **`@delmaredigital/payload-puck`** | Puck Visual Builder (مذكور أعلاه) ⭐ |
| **`@delmaredigital/payload-page-tree`** | شجرة صفحات هرمية بالـ drag-and-drop + slugs تلقائية [6](https://www.delmaredigital.com/developers) |
| **Visual Live Editor plugin** | محرر visual مباشر داخل Admin UI [3](https://github.com/topics/payload-plugin) |
| **`@focus-reactive/payload-plugin-presets`** | Presets للصفحات [8](https://www.reddit.com/r/PayloadCMS/) |

---

## 🌍 سابعاً: Plugins Internationalization & Localization

| Plugin | الوصف |
|---|---|
| **[vitakili/payload-plugins](https://github.com/vitakili/payload-plugins)** | إدارة المحتوى المترجم + Theme Configuration |
| **`@focus-reactive/payload-plugin-translator`** | ترجمة AI تلقائية فوق Payload localization [8](https://www.reddit.com/r/PayloadCMS/) |
| **payload-plugin-ai-localization** | (مذكور أعلاه) |

---

## 🛒 ثامناً: Plugins للتجارة الإلكترونية

| Plugin | الوصف |
|---|---|
| **`@payloadcms/plugin-ecommerce`** | الرسمي الجديد ✨ |
| **[shopnex-ai/shopnex](https://github.com/shopnex-ai/shopnex)** | بديل Shopify كامل مفتوح المصدر مبني على Payload [1](https://github.com/DanailMinchev/awesome-payload) |

---

## 🧪 تاسعاً: Plugins تحسين الأداء و DevOps

| Plugin | الوصف |
|---|---|
| **payloadcms-redis-cache** | تخزين مؤقت بـ Redis [2](https://www.payloaddirectory.dev/) |
| **payload-dashboard-analytics** | إحصائيات داخل Admin (Plausible / GA) [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins) |
| **`@focus-reactive/payload-plugin-scheduling`** | جدولة نشر للـ serverless (Vercel) [8](https://www.reddit.com/r/PayloadCMS/) |
| **`@focus-reactive/payload-plugin-comments`** | تعليقات داخل CMS للمراجعات [8](https://www.reddit.com/r/PayloadCMS/) |
| **`@focus-reactive/payload-plugin-ab`** | A/B Testing |
| **payload-openapi / payload-swagger** | توليد OpenAPI/Swagger docs تلقائياً [3](https://github.com/topics/payload-plugin) |
| **payload-rbac** | Role-Based Access Control متقدم [3](https://github.com/topics/payload-plugin) |
| **payload-plugin-related-items** | عناصر ذات صلة بـ Jaccard similarity |
| **payload-plugin-admin-alerts** | تنبيهات لوحة التحكم |
| **payload-plugin-notifications** | نظام إشعارات [2](https://www.payloaddirectory.dev/) |
| **payload-plugin-api-guide** | دليل API تفاعلي [2](https://www.payloaddirectory.dev/) |

---

## 📂 مصادر للاستكشاف

| المصدر | الوصف |
|---|---|
| 🌐 **[PayloadDirectory.dev](https://www.payloaddirectory.dev/)** | **246+ plugin** مع فلترة بالنسخة (v1/v2/v3) [2](https://www.payloaddirectory.dev/) |
| 🌐 **[payloadplugins.dev](https://payloadplugins.dev/)** | دليل بديل للـ plugins المجتمعية |
| 🏷️ **[GitHub Topic: payload-plugin](https://github.com/topics/payload-plugin)** | كل الـ repos الموسومة [3](https://github.com/topics/payload-plugin) |
| 😎 **[Awesome Payload](https://github.com/DanailMinchev/awesome-payload)** | قائمة منسقة (86 ⭐) [1](https://github.com/DanailMinchev/awesome-payload) |
| 📚 **[Payload Docs - Plugins](https://payloadcms.com/docs/plugins/overview)** | الوثائق الرسمية |
| 💬 **[Discord الرسمي](https://discord.gg/payload)** | المجتمع + أسئلة plugins |

---

## 🎯 توصيتي لمشروعك (Refine + Payload + Puck)

بناءً على ما بنيناه، أنصح بهذه التركيبة:

### ✅ استخدم هذه الـ plugins:

1. **`@delmaredigital/payload-puck`** ← بدلاً من الكود اليدوي الذي كتبناه
2. **`@delmaredigital/payload-page-tree`** ← لإدارة شجرة الصفحات
3. **`@payloadcms/plugin-seo`** ← SEO تلقائي
4. **`@payloadcms/plugin-redirects`** ← إعادة توجيه
5. **`@payloadcms/plugin-form-builder`** ← نماذج (بدل بناء واحد يدوي)
6. **`@payloadcms/plugin-search`** ← بحث موحّد
7. **`@payloadcms/storage-s3`** أو **`@payloadcms/storage-vercel-blob`** ← تخزين الميديا
8. **`authsmith/payload-auth-plugin`** ← OAuth (Google/GitHub) فوق Refine auth
9. **`ashbuilds/payload-ai`** ← مولّد محتوى AI
10. **`shefing/payload-tools`** ← تحسينات Admin UI

### 🔄 خطة الترقية من كودنا اليدوي إلى `payload-puck`:

```bash
# 1. أزل الكود اليدوي
rm -rf src/puck src/app/(refine)/dashboard/pages

# 2. ثبّت الـ plugin الجاهز
pnpm remove @measured/puck
pnpm add @delmaredigital/payload-puck @puckeditor/core

# 3. أضف plugin في payload.config.ts
# 4. أضف PuckConfigProvider في layout
# 5. أنشئ route للواجهة كما في الوثائق
```

---

## 📊 إحصائيات سريعة عن البيئة

- 🔢 **246+** plugin في PayloadDirectory.dev [2](https://www.payloaddirectory.dev/)
- ⭐ **42.9k** نجمة لـ Payload الرئيسي
- 📦 **138.1k** تحميل أسبوعي لـ plugin-seo (الأكثر استخداماً) [2](https://www.payloaddirectory.dev/)
- 🏆 **430** ⭐ لـ payload-ai (الأكثر شعبية في community) [1](https://www.buildwithmatija.com/blog/best-payload-cms-plugins)
- 🥇 **delmaredigital** هو أنشط مطوّر community في 2026 (3 plugins رئيسية + starter)

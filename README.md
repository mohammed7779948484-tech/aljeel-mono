# AAU University Platform — Monorepo

مستودع رئيسي يحتوي على جميع مكونات منصة جامعة AAU.

## هيكل المشروع

```
aljeel/
├── aau_university/          # الـ Backend القديم (Frappe) — للمرجعية فقط
├── frontend/                # الـ Frontend القديم (Next.js) — للمرجعية فقط
├── docs/                    # توثيق المشروع والهجرة
└── aau-payload-platform/    # 🚀 المشروع الجديد (Submodule)
```

## المشروع الجديد

الـ submodule الخاص بمنصة Payload CMS:
👉 https://github.com/mohammed7779948484-tech/aau-payload-platform

### البدء السريع

```bash
# استنساخ المستودع مع الـ submodules
git clone --recurse-submodules <repo-url>

# أو إذا استنسخت بدون submodules
git submodule update --init --recursive

# الدخول للمشروع
cd aau-payload-platform

# نسخ متغيرات البيئة
cp .env.example .env
# ثم أضف DATABASE_URI و PAYLOAD_SECRET

# تشغيل بيئة التطوير
pnpm install
pnpm dev
```

## Stack التقني

- **Backend/CMS**: Payload CMS v3 + Next.js 15
- **Database**: Neon PostgreSQL (Serverless)
- **Visual Editor**: Puck (@delmaredigital/payload-puck)
- **Package Manager**: pnpm v10

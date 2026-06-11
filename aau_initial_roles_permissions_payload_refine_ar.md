# خطة الأدوار والصلاحيات الأولية لمنصة الجامعة

نسخة بداية للتحويل من Frappe إلى Next.js + Payload + Puck + Refine.

## القرار المعماري المختصر

- Payload Admin: لإدارة المحتوى والبيانات القياسية.
- Puck: محرر الصفحات البصري داخل Next.js.
- Refine: للعمليات الإدارية والتشغيلية المعقدة مثل Role Builder وإدارة المستخدمين والقبول وتنسيق الكليات.
- Student وDoctor: بوابات مخصصة داخل الواجهة فقط، ولا يدخلون Payload Admin.
- نموذج الصلاحيات: `role + scope` مثل `Coordinator + collegeId`.

## أدوار البداية

| الدور | الصلاحية | مكان لوحة التحكم |
|---|---|---|
| Super Admin | يتحكم بكل شيء، ينشئ أدوارًا ويخصصها ويدير كل البيانات | Refine Control Center + Payload Admin |
| Admin | مدير عام تشغيلي أقل من Super Admin، لا يملك Role Builder في النسخة الأولى | Refine + Payload Admin |
| Content Manager | نفس صلاحيات Super Admin على المحتوى والبيانات، لكن لا ينشئ أدوارًا ولا مستخدمًا بنفس دوره أو أعلى | Payload + Refine |
| Coordinator | منسق كلية محددة يدير محتواها وبياناتها فقط | Refine أساسي + Payload محدود اختياريًا |
| Student | يرى بياناته فقط | Next.js Portal فقط |
| Doctor | يرى مقرراته وطلابه ويدخل درجاته ويرفع مواده | Next.js Portal فقط |

## قواعد منع التصعيد

- Super Admin فقط يستطيع إنشاء الأدوار وتعديل Permission Templates.
- Admin لا يستطيع إنشاء أو تعديل دور Super Admin ولا يملك Role Builder في النسخة الأولى.
- Content Manager لا ينشئ أدوارًا، ولا ينشئ مستخدمًا بنفس دوره أو أعلى.
- Content Manager يستطيع إنشاء مستخدمين للأدوار التي تحته فقط: Coordinator وStudent وDoctor.
- Coordinator يجب أن يكون مقيدًا بـ `collegeId`.
- Student مقيد بـ `studentId`.
- Doctor مقيد بـ `doctorId` أو `instructorId`.
- كل عمليات الكتابة والحذف يجب أن تمر عبر server-side permission checks.

## الأدوار المؤجلة

- Academic Manager
- Admissions Officer
- Finance Officer
- Support Agent
- Executive Viewer
- Content Editor
- Blog Editor
- Registrar
- Guardian / Parent

## قرار MVP

الأدوار الأولى: Super Admin, Admin, Content Manager, Coordinator, Student, Doctor.  
Role Builder يبنى داخل Refine.  
Payload Admin للمحتوى والـ CRUD العادي.  
Refine للعمليات المعقدة.  
بوابات Student وDoctor داخل الواجهة فقط.

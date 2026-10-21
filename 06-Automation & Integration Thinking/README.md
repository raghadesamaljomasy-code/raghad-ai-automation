# PlanCraft AI

تطبيق ويب يحوّل فكرة مشروع أو وصفًا أوليًا إلى **Structured Project Plan** قابلة للتنفيذ، وذلك عبر تكامل ثلاث منصات: **Lovable** لبناء الواجهة والمنطق التطبيقي، و**Supabase** لإدارة قاعدة البيانات، و**n8n Cloud** مع نموذج **Google Gemini** لأتمتة توليد الخطط.

---

## 1. Abstract

يهدف هذا المشروع التعليمي إلى بناء نظام متكامل يُمكّن المستخدم من إدخال بيانات مشروعه (العنوان، الوصف، الهدف، السياق الإضافي)، ثم توليد خطة عمل منظمة تتضمن **Execution Phases** و**Recommended Actions** و**Risks and Mitigations** و**Estimated Duration**. ويركّز المشروع على مبادئ **Separation of Concerns** و**Asynchronous Processing** و**Real Data Display**، إضافةً إلى توثيق منهجي لأبرز الأخطاء التي واجهت عملية التكامل وأسبابها وطرق معالجتها.

---

## 2. Technology Stack

| Component | Role |
|---|---|
| **Lovable** | بناء واجهة المستخدم والمنطق الأساسي واستضافة التطبيق |
| **Lovable Cloud (Supabase-based)** | قاعدة البيانات العاملة حاليًا لتخزين الطلبات والخطط |
| **Supabase (Personal Project)** | نسخة محفوظة لاستكمال تطوير المشروع عليها مستقبلًا |
| **n8n Cloud** | **Workflow Automation** واستدعاء نموذج الذكاء الاصطناعي |
| **Google Gemini** | توليد الخطة بصيغة JSON منظمة |

---

## 3. System Architecture (Operational Version)

```text
User
  │
  ▼
Lovable Application ──► Save Request (project_requests) ── Lovable Cloud
  │
  │  "Generate My Plan"  (Server-side Function)
  ▼
n8n Production Webhook  (/webhook/PlanCraftGenerate)
  │
  ├─► Edit Fields            (استخراج request_id)
  ├─► Respond to Webhook     (رد فوري: status = processing)
  ├─► AI Agent + Gemini + Structured Output Parser
  │        (توليد الخطة بصيغة JSON)
  ▼
HTTP Request  (POST + x-plancraft-secret Header)
  │
  ▼
Lovable Endpoint: /api/public/save-plan
  │   ├─ Verify shared secret
  │   ├─ Validate request_id existence
  │   ├─ Upsert plan into project_plans
  │   └─ Update status to completed (or failed)
  ▼
UI displays the persisted plan
```

---

## 4. Database Schema

### 4.1 Table: `project_requests`

| Field | Type | Description |
|---|---|---|
| `id` | UUID (PK) | المعرّف الفريد للطلب |
| `title` | TEXT | عنوان المشروع (إلزامي) |
| `description` | TEXT | وصف المشروع (إلزامي) |
| `goal` | TEXT | الهدف الرئيسي (اختياري) |
| `additional_context` | TEXT | معلومات إضافية (اختياري) |
| `status` | TEXT | `pending` / `processing` / `completed` / `failed` |
| `created_at` | TIMESTAMPTZ | تاريخ الإنشاء |
| `updated_at` | TIMESTAMPTZ | تاريخ آخر تحديث |

### 4.2 Table: `project_plans`

| Field | Type | Description |
|---|---|---|
| `id` | UUID (PK) | المعرّف الفريد للخطة |
| `request_id` | UUID (FK, UNIQUE) | الطلب المرتبط، مع **Cascade Delete** |
| `summary` | TEXT | ملخص الخطة |
| `project_goal` | TEXT | الهدف الذي تعالجه الخطة |
| `phases` | JSONB | مراحل التنفيذ |
| `recommended_actions` | JSONB | الإجراءات الموصى بها |
| `risks` | JSONB | المخاطر وإجراءات التخفيف |
| `estimated_duration` | TEXT | المدة التقديرية |
| `created_at` / `updated_at` | TIMESTAMPTZ | الطوابع الزمنية |

**Relationship:** علاقة **One-to-One** اختيارية؛ فلكل طلب خطة واحدة على الأكثر، ويُفرض ذلك عبر قيد `UNIQUE` على `project_plans.request_id`.

---

## 5. Troubleshooting Log

بعد استكمال التنفيذ، ظهرت الرسالة التالية في الواجهة وبقيت حالة الطلب `failed`:

> *Plan generation failed. Please try again — if it keeps failing, the plan generator may be temporarily unavailable.*

تُعد هذه الرسالة **Generic Error Message** لا تكشف السبب الحقيقي؛ لذلك اعتُمد منهج تشخيصي يبدأ من **Execution Logs** في n8n لتحديد العقدة (Node) التي يتوقف عندها التنفيذ. وفيما يلي توثيق للمشكلات بالترتيب الزمني.

### Issue 1: Supabase Node Returns No Output

- **Symptom:** ظهور الرسالة `No output data returned`، ما أدى إلى توقف **Workflow** بالكامل وعدم تنفيذ العقد اللاحقة.
- **Analysis:** كان الفلتر و`request_id` صحيحين، مما أشار إلى احتمال أن **Credentials** تتصل بقاعدة بيانات لا تحتوي السجل المطلوب.
- **Actions Taken:**
  - تفعيل خيار *Always Output Data*.
  - التحقق من اسم الجدول وشروط الفلتر.
  - إنشاء **Credential** من نوع *Supabase API* بدلًا من *OAuth2*.

### Issue 2: `invalid input syntax for type uuid: "null"`

- **Root Cause:** تشغيل العقدة بشكل منفرد (**Isolated Execution**) دون بيانات واردة من العقدة السابقة، فوصلت القيمة `null` إلى الفلتر.
- **Resolution:** تشغيل **Workflow** بالتسلسل الصحيح باستخدام **Pinned Data**.

### Issue 3: Inability to Test the Webhook

- **Root Cause:** يترقّب **Webhook Node** طلبًا على *Test URL*، بينما يرسل التطبيق طلباته إلى *Production URL*. كما أن تعديل الرابط داخل Lovable يستهلك **Credits**.
- **Resolution:** الاعتماد على الاختبار اليدوي بالبيانات المثبتة، ثم الاختبار الفعلي من التطبيق ومراقبة تبويب **Executions**.

### Issue 4 (Root Cause): Database Mismatch Between the Application and n8n

- **Symptom:** أظهر الاستعلام `select id, title from project_requests;` في Supabase الشخصي سجلًا واحدًا فقط (`4a511c66...`)، في حين أن الطلبات المُدخلة من التطبيق (`taskks`، `f42f11e9...`، `8d5210f9...`) غير موجودة فيه.
- **Analysis:**
  - لا يطابق `request_id` الواصل إلى n8n أي سجل في Supabase الشخصي.
  - تُظهر لوحة Lovable تبويبات **Database** و**Users** و**Storage** و**Connectors**، ما يدل على تفعيل **Lovable Cloud**.
  - يحتوي جدول `project_requests` داخل Lovable Cloud على الطلبات المُدخلة فعليًا.
  - المشروع الحالي هو **Remix** لمشروع سابق، والـ Remix ينسخ الشيفرة المصدرية دون نقل ربط Supabase، فتُفعَّل قاعدة Lovable Cloud تلقائيًا.
- **Conclusion:** يكتب التطبيق في **Lovable Cloud**، بينما يقرأ n8n من **Supabase الشخصي**. لذلك كانت نتيجة عقدة Get فارغة، ولم تكن المشكلة في الفلتر أو في بيانات الاعتماد.

### Design Decision: Reversing the Persistence Flow

نظرًا لأن Lovable Cloud لا يكشف مفتاح `service_role`، ولأن تحويل مشروع قائم إلى Supabase خارجي مكلف من حيث **Credits** وغير مضمون النتيجة، اعتُمد الحل الأقل كلفة والأعلى احتمالًا للنجاح:

1. **n8n** لا يقرأ من أي قاعدة بيانات ولا يكتب فيها، إذ تصله بيانات الطلب (`title`, `description`, `goal`, `additional_context`) ضمن **Request Body** الخاص بالـ Webhook.
2. حُذفت عقد Supabase (Get / Delete / Create / Update) من النسخة الجديدة من **Workflow**.
3. أُضيف في Lovable **Server-side Endpoint** بالمسار `POST /api/public/save-plan` عبر **Prompt** واحد فقط، يستقبل الخطة ويحفظها ويحدّث الحالة.
4. أصبح n8n يُرسل الناتج عبر عقدة **HTTP Request** إلى هذا المسار.

**Advantages:** لا مفاتيح مميزة خارج Lovable، وقاعدة بيانات واحدة، وحالة الطلب يحدّثها التطبيق نفسه فتتسق مع ما حُفظ فعليًا.

### Issue 5: HTTP 401 Unauthorized

- **Root Cause:** عدم تطابق قيمة Header `x-plancraft-secret` المُرسل من n8n مع `N8N_WEBHOOK_SECRET` المخزّن في Lovable، إذ كانت القيمة السابقة رابط الـ Webhook نفسه ويحتوي مسافات، وهو إعداد غير صحيح.
- **Resolution:** تحديث السر في Lovable ليطابق قيمة الـ Header في n8n، ثم إعادة النشر (**Publish → Update**).
- **Validation:** تحوّل الخطأ من `401` إلى `404`، ما يؤكد أن **Authentication** نجحت.

### Issue 6: HTTP 404 `request_id not found`

- **Root Cause:** كان `request_id` المُرسل ناتجًا عن **Pinned Data** قديمة أو عن إعادة تنفيذ سابقة (**Retry**)، وليس عن طلب فعلي موجود في قاعدة البيانات المنشورة.
- **Resolution:** إلغاء التثبيت (**Unpin**)، وتجنّب استخدام Retry أو Execute workflow ببيانات قديمة، والاعتماد على الاختبار من التطبيق المنشور بطلب جديد.

### Issue 7: Randomized Webhook Path After Duplication

- **Symptom:** استمر التطبيق في عرض رسالة الفشل رغم نجاح التنفيذ اليدوي للعقد.
- **Root Cause:** عند **Duplicate** لسير العمل، منح n8n عقدة Webhook الجديدة **Path** عشوائيًا (`9f56a91e-...`)، بينما يرسل التطبيق إلى `/webhook/PlanCraftGenerate`، فلم تصل الطلبات إلى النسخة الجديدة.
- **Resolution:**
  1. تعطيل Workflow القديم (**Inactive**).
  2. ضبط Path عقدة Webhook في النسخة الجديدة على `PlanCraftGenerate`.
  3. تفعيل Workflow الجديد (**Active**).

### Final Outcome

بعد هذه المعالجات يعمل المسار بصورة متكاملة: يُحفظ الطلب، ويستقبله n8n، ويولّد Gemini الخطة، ثم يحفظها `save-plan` وتتحول الحالة إلى `completed`، وتُعرض الخطة الفعلية في الواجهة.

---

## 6. Lessons Learned

1. **الرسائل العامة في الواجهة لا تحدد السبب الحقيقي.** ينبغي الرجوع إلى **Execution Logs** لتحديد العقدة الفاشلة.
2. **النتيجة الفارغة دون خطأ لا تعني فشل الاتصال.** فقد تدل على الاتصال بقاعدة بيانات مختلفة.
3. **الـ Remix لا ينقل ربط Supabase.** يجب التحقق من القاعدة المرتبطة بعد كل Remix.
4. **البيانات المثبتة (Pinned Data) تُربك الاختبار.** يجب إلغاء التثبيت قبل الاختبار الفعلي.
5. **الـ Duplicate يغيّر مسار الـ Webhook.** يجب مراجعة الـ Path قبل التفعيل.
6. **إدارة الأسرار تكون على الخادم فقط،** مع مطابقة القيمة في الطرفين وتغييرها عند انكشافها.
7. **عند محدودية الموارد يُختار الحل الأقل كلفة،** مع الحفاظ على مبدأ فصل المسؤوليات.

---

## 7. Preserving the Supabase Version for Future Development

أُبقي على مشروع **Supabase الشخصي** وجداوله (`project_requests` و`project_plans`) دون حذف، كما حُفظت نسخة من **n8n Workflow** الأصلي (المبني على عقد Supabase) في حالة **Inactive**، بهدف استكمال التطوير عليها بعد تجديد **Credits**.

**Proposed Migration Plan:**

1. إنشاء مشروع Lovable جديد وربطه بـ Supabase الشخصي **قبل** تفعيل Lovable Cloud.
2. توجيه Lovable إلى استخدام الجداول الموجودة وعدم إنشاء جداول جديدة.
3. إعادة تفعيل Workflow المبني على عقد Supabase باستخدام *Supabase API Credential* ومفتاح `service_role`.
4. إضافة **Authentication** وربط كل سجل بمالكه (**Per-user Ownership**).

---

## 8. Setup and Configuration

### 8.1 Prerequisites

- حساب Lovable مع تطبيق منشور (**Published**).
- حساب n8n Cloud مع **Credential** خاص بـ Google Gemini.

### 8.2 Configuration Parameters

| Location | Setting |
|---|---|
| Lovable (Secrets) | `N8N_WEBHOOK_URL` = رابط Production Webhook |
| Lovable (Secrets) | `N8N_WEBHOOK_SECRET` = سر عشوائي |
| n8n (HTTP Request) | Header `x-plancraft-secret` بنفس قيمة السر |
| n8n (Webhook) | Path = `PlanCraftGenerate`، Method = POST، Respond = Using Respond to Webhook Node |

### 8.3 Workflow Nodes

```text
Webhook → Edit Fields → Respond to Webhook → AI Agent (+ Gemini + Structured Output Parser) → HTTP Request
```

---

## 9. Security Considerations

- لا يتضمن التطبيق **Authentication System** حاليًا، لذا فهو مخصص للأغراض التعليمية وغير مهيأ للنشر العام متعدد المستخدمين.
- يُحظر وضع مفتاح `service_role` أو أسرار الـ Webhook في شيفرة الواجهة الأمامية.
- يُحظر استخدام سياسات وصول عامة غير مقيّدة (**Unrestricted Public Policies**).
- يجب تغيير قيمة `N8N_WEBHOOK_SECRET` قبل أي نشر عام، وعدم تضمين القيمة الحقيقية في هذا الملف.

---

## 10. Future Work

- [ ] إضافة **Authentication** (Sign up / Sign in) وملكية السجلات.
- [ ] نقل المشروع إلى Supabase الشخصي بعد تجديد Credits.
- [ ] إضافة **Failure Path** في n8n لتحديث الحالة إلى `failed` مع رسالة سبب واضحة.
- [ ] تحسين واجهة المستخدم وطريقة عرض الخطط.

---

## 11. Author

أُنجز هذا العمل كمشروع تعليمي يهدف إلى تعلّم تكامل **Lovable** و**Supabase** و**n8n** ضمن نظام **Workflow Automation** متكامل.

---

> **Note:** قبل رفع المشروع إلى GitHub، يُرجى التأكد من خلوّ الملف والشيفرة من أي مفاتيح أو أسرار حقيقية، واستبدالها بعبارات مثل `<YOUR_SECRET>` عند الحاجة.

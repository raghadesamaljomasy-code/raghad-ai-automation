# U1 — UNDERSTAND THE WORK

## Process & Data Thinking

> Before automating the work, understand the work.

في هذه الوحدة، أتعلم كيف أحوّل مشكلة واقعية إلى عملية واضحة يمكن تحليلها وتصميم حل مناسب لها باستخدام AI Automation.

الهدف ليس البدء بالأداة، بل فهم العمل أولًا، وتحديد المشكلة، وتحليل البيانات، وتصميم القرارات التي يحتاج النظام إلى اتخاذها.

---

## 🎯 Capability

**Process & Data Thinking**

القدرة على تحليل العمل وتفكيك العمليات وفهم المدخلات وتصميم منطق اتخاذ القرار قبل تنفيذ الأتمتة.

---

## 🧠 What I Learned

خلال هذه الوحدة، طوّرت قدرتي على:

- **Understand the Work:** فهم المشكلة وسياق العمل والنتيجة المطلوبة.
- **Decompose the Work:** تفكيك العملية إلى Workflow وTasks وSubtasks.
- **Understand the Input:** تحديد البيانات المطلوبة وفهم البيانات المنظمة وغير المنظمة.
- **Design the Decision:** تحديد متى نستخدم AI ومتى تكفي القواعد المحددة.
- **Handle Exceptions Safely:** التعامل مع الحالات غير المتوقعة وتحديد متى نحتاج إلى Human Review.
- **Design the Automation:** تحويل تصميم العملية إلى Workflow قابل للتنفيذ.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** AI Client Intake & Request Routing System

مشروع تطبيقي لتصميم نظام أتمتة يعالج طلبات العملاء المحتملين، ويستخرج المعلومات المهمة باستخدام الذكاء الاصطناعي، ويتحقق من اكتمالها، ويحدد مسار المعالجة المناسب.

- **Problem:** تستغرق معالجة طلبات العملاء يدويًا وقتًا في قراءة النصوص واستخراج المعلومات والتحقق منها، وقد تؤدي المعلومات الناقصة أو الغامضة إلى تأخير المتابعة.
- **Solution:** تصميم Workflow يستقبل الطلبات عبر Google Forms، ويستخدم AI by Zapier لاستخراج المعلومات وتنظيمها، ثم يوجّه الطلبات إلى مسار المتابعة أو المراجعة البشرية وفق قواعد التحقق، مع تخزين النتائج وإرسال الإشعارات ضمن التصميم المستهدف.
- **Tools:** Google Forms, Google Sheets, Zapier, AI by Zapier, Email.
- **Implementation Status:** يوثّق ملف المشروع التصميم المطلوب وخطة الاختبار؛ ويجب التحقق من التنفيذ الفعلي لكل خطوة قبل اعتبار النظام مكتملًا.

[Explore the Project](https://github.com/raghadesamaljomasy-code/raghad-ai-automation/tree/main/01-Process%20%26%20Data%20Thinking/course-project)

### 02 — Independent Project

**Project:** Meeting Management Automation

مشروع مستقل لتصميم سير عمل يساعد المساعد الإداري على تحويل ملاحظات الاجتماعات الأسبوعية غير المنظمة إلى معلومات واضحة ومنظمة، مع استخراج القرارات والمهام المذكورة صراحةً وحفظ النتائج في Google Sheets.

- **Problem:** تتطلب معالجة ملاحظات الاجتماعات يدويًا تنظيم النصوص واستخراج القرارات والمهام وتحديد المسؤولين والمواعيد عند ذكرها، مما يزيد الجهد الإداري ويصعّب مراجعة المعلومات.
- **Solution:** بناء Workflow باستخدام Google Forms وZapier وAI by Zapier لتنظيم الملاحظات واستخراج القرارات والمهام وتخزين النتائج في Google Sheets، مع إبقاء مراجعة المعلومات الناقصة أو الغامضة ضمن مسؤولية المستخدم.
- **Tools:** Google Forms, Google Sheets, Zapier, AI by Zapier.
- **Implementation Status:** تم اختبار أجزاء من سير العمل والمعالجة الأساسية، لكن ربط جميع تفاصيل المهام بأعمدتها المنفصلة والتحقق من التشغيل المتكامل ما زالا بحاجة إلى استكمال.

[Explore the Project](https://github.com/raghadesamaljomasy-code/raghad-ai-automation/tree/main/01-Process%20%26%20Data%20Thinking/independent-project)

---

## 🔍 Engineering Focus

في هذه الوحدة، أركز على الأسئلة التالية:

1. ما المشكلة الحقيقية التي نريد حلها؟
2. كيف تتم العملية حاليًا؟
3. ما المدخلات والبيانات التي يحتاجها النظام؟
4. أين نحتاج إلى AI، وأين تكفي القواعد المحددة؟
5. ماذا يحدث عندما تكون المعلومات ناقصة أو غير واضحة؟
6. كيف نحوّل التصميم إلى Automation Workflow؟

---

## 📌 Key Takeaway

**A good automation starts with a well-understood process, not with a tool.**

نجاح الأتمتة يبدأ بفهم العمل وتصميم العملية، وليس بمجرد اختيار الأداة.

---

## 📈 Capability Progress

- [x] فهم المشكلة والنتيجة المطلوبة.
- [x] تفكيك العمل إلى خطوات واضحة.
- [x] تحديد المدخلات والمخرجات.
- [x] تصميم قرارات AI وRule-Based Logic.
- [x] تحديد الحالات التي تحتاج إلى Human Review.
- [x] تطبيق المفاهيم في تصميم مشاريع أتمتة عملية.
- [ ] استكمال تنفيذ جميع الخطوات والتحقق من نتائج التشغيل النهائي للمشاريع.

---

## 🔗 Related Projects

- [Guided Project — AI Client Intake & Request Routing System](https://github.com/raghadesamaljomasy-code/raghad-ai-automation/tree/main/01-Process%20%26%20Data%20Thinking/course-project)
- [Independent Project — Meeting Management Automation](https://github.com/raghadesamaljomasy-code/raghad-ai-automation/tree/main/01-Process%20%26%20Data%20Thinking/independent-project)

---

**Next:** U2 — BUILD THE SYSTEM

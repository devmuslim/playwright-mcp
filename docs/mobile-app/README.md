# توثيق تطبيق الموبايل لإدارة الأموال فوق Firefly III

هذا المجلد يحتوي على وثائق تحليل وتصميم تطبيق موبايل احترافي (iOS + Android) يتكامل مع
**Firefly III** عبر الـ REST API. يُبنى التوثيق على مراحل، وكل وثيقة مستقلة وقابلة للاستخدام
مباشرة من فرق Product / Design / Development / QA / Security.

> **اللغة:** المحتوى بالعربية، مع إبقاء المصطلحات التقنية وأسماء الـ APIs والحقول
> والكيانات باللغة الإنجليزية كما هي في الكود.

---

## المصادر التي تم فحصها (Ground Truth)

| المصدر | الوصف | تاريخ اللقطة (Snapshot) |
|---|---|---|
| `firefly-iii/firefly-iii` @ `cdf1721e` | المستودع الرسمي (main) — الإصدار **6.6.6** | 2026-07-16 |
| `devmuslim/firefly-iii` @ `cdf1721e` | مستودعك (fork) — **مطابق تمامًا** للرسمي | 2026-07-16 |
| `firefly-iii/docs` | مستودع التوثيق الرسمي (mkdocs) | 2026-07-16 |
| `https://api-docs.firefly-iii.org/` | مواصفة Swagger/OpenAPI الرسمية (خارجية) | مرجع حي |

> **تنبيه منهجي:** كل نتيجة مهمة في هذه الوثائق مرفقة بمرجع برمجي (ملف / مسار / class / endpoint).
> ما لم يُتحقق منه من الكود موسوم صراحةً بـ **«غير مؤكد — يحتاج تحقق»**.

---

## حالة التنفيذ حسب المراحل

| # | الوثيقة | المرحلة | الحالة |
|---|---|---|---|
| — | `README.md` (هذا الملف) | فهرس | ✅ مكتمل |
| 00 | `00-executive-summary.md` | ملخص تنفيذي | 🟡 نسخة Phase 1 |
| 01 | `01-repository-assessment.md` | المرحلة 1 | ✅ مكتمل |
| 02 | `02-current-system-architecture.md` | المرحلة 1 | ✅ مكتمل |
| 03 | `03-domain-model.md` | المرحلة 2 | ✅ مكتمل |
| 04 | `04-financial-calculations.md` | المرحلة 3 | ⏳ لاحقًا |
| 05 | `05-product-requirements-document.md` | المرحلة 4 | ⏳ لاحقًا |
| 06 | `06-feature-catalog.md` | المرحلة 4 | ⏳ لاحقًا |
| 07 | `07-user-stories-and-acceptance-criteria.md` | المرحلة 4 | ⏳ لاحقًا |
| 08 | `08-mobile-information-architecture.md` | المرحلة 5 | ⏳ لاحقًا |
| 09 | `09-screen-inventory.md` | المرحلة 5 | ⏳ لاحقًا |
| 10 | `10-user-flows.md` | المرحلة 5 | ⏳ لاحقًا |
| 11 | `11-api-catalog.md` | المرحلة 6 | ⏳ لاحقًا |
| 12 | `12-api-request-response-examples.md` | المرحلة 6 | ⏳ لاحقًا |
| 13 | `13-api-gap-analysis.md` | المرحلة 7 | ⏳ لاحقًا |
| 14 | `14-proposed-mobile-architecture.md` | المرحلة 8 | ⏳ لاحقًا |
| 15 | `15-authentication-and-security.md` | المرحلة 9 | ⏳ لاحقًا |
| 16 | `16-offline-sync-strategy.md` | المرحلة 10 | ⏳ لاحقًا |
| 17 | `17-local-database-design.md` | المرحلة 11 | ⏳ لاحقًا |
| 18 | `18-error-handling.md` | المرحلة 12 | ⏳ لاحقًا |
| 19 | `19-non-functional-requirements.md` | المرحلة 13 | ⏳ لاحقًا |
| 20 | `20-test-strategy.md` | المرحلة 14 | ⏳ لاحقًا |
| 21 | `21-test-cases.md` | المرحلة 14 | ⏳ لاحقًا |
| 22 | `22-implementation-roadmap.md` | المرحلة 15 | ⏳ لاحقًا |
| 23 | `23-backlog.md` | المرحلة 15 | ⏳ لاحقًا |
| 24 | `24-risk-register.md` | المرحلة 15 | ⏳ لاحقًا |
| 25 | `25-open-questions.md` | مستمر | 🟡 نسخة Phase 1 |
| 26 | `26-traceability-matrix.md` | المرحلة 17 | ⏳ لاحقًا |
| 27 | `27-glossary.md` | مستمر | ⏳ لاحقًا |

مجلدات مرافقة:

- `diagrams/` — مخططات Mermaid مُصدّرة أو مصدرية.
- `api-examples/` — أمثلة Request/Response كاملة (JSON).
- `schemas/` — مخططات SQLite المحلية و DTO schemas.

---

## المرحلة الحالية: **Phase 1 — Repository Assessment & Architecture**

مكتمل في هذه الدفعة:

1. **Repository Assessment** — حالة المستودع، الفروع، الإصدار، الترخيص، المخاطر القانونية.
2. **Technology Stack** — استخراج دقيق من `composer.json` / `config/*` / `routes/*`.
3. **Current Architecture** — مخططات C4 + تدفقات (Auth / API / Transaction / …).
4. **API Discovery Approach** — منهجية استخراج الـ API في المرحلة 6.
5. **Open Questions** — قائمة الأسئلة المفتوحة الأولية.

> ❌ لم يبدأ أي عمل على شاشات الموبايل أو كوده. هذا مقصود ومطابق لقواعد المهمة
> (لا كود قبل اكتمال التحليل والاعتماد المنطقي للمتطلبات).

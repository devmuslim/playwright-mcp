# 00 — Executive Summary (نسخة Phase 1)

> **حالة الوثيقة:** 🟡 نسخة المرحلة 1. تُحدَّث بعد كل مرحلة. تعكس حاليًا نتائج
> **Repository Assessment + Technology Stack + Current Architecture** فقط.

---

## الهدف

بناء تطبيق موبايل احترافي (iOS + Android) لإدارة الأموال الشخصية يتكامل مع **Firefly III**
عبر الـ REST API، مع توثيق شامل يسبق كتابة الكود.

## أبرز نتائج المرحلة 1 (مؤكدة من الكود)

1. **المستودع:** `devmuslim/firefly-iii` هو **fork مطابق تمامًا** للرسمي
   (`firefly-iii/firefly-iii`) — نفس commit `cdf1721e`، **الإصدار 6.6.6**، بلا أي تعديل
   مخصّص. ⟶ يمكن اعتماد سلوك الرسمي كمرجع وحيد.

2. **الترخيص:** **AGPL-3.0** (Copyleft + شرط الشبكة §13). ⟶ **توصية جوهرية:** عدم تعديل
   الـ backend المنشور؛ بناء أي backend إضافي كـ **خدمة BFF مستقلة** تستهلك API. يتطلب
   **اعتمادًا قانونيًا** قبل التوزيع التجاري (OQ-01).

3. **التقنية:** Laravel 13 / PHP 8.5 / Passport (OAuth2 + PAT) / Fractal transformers /
   BCMath للحساب المالي / قواعد بيانات MySQL·PgSQL·SQLite·SQL Server.

4. **الـ API:** REST تحت **`/api/v1` فقط** (لا يوجد `/api/v2` في هذا الإصدار رغم أن نسخة
   المواصفة 2.1.0). حارس `auth:api` على كل المسارات. تنظيم واضح: System, Autocomplete,
   Models, Chart, Insight, Summary, Search, Data, User, Webhook.

5. **نموذج العملات:** كل كائن يحمل عملته (`currency_*`) + عملة أساسية (`primary_currency_*`)
   + قيم محوّلة (`pc_*`) تُملأ فقط عند تفعيل «convert to primary».

6. **قيد مالي مؤكد:** الحساب في الـ backend بـ **decimal (BCMath)** لا floating-point ⟶
   يُلزم الموبايل باستخدام decimal / minor-units.

## فجوات مؤكدة مبكرًا (تُحسم في المرحلة 7)

| الفجوة | الحالة |
|---|---|
| **Push notifications (FCM/APNs)** | ❌ غير موجودة في core — تحتاج backend إضافي |
| **Bank sync / Import** | ⚠️ عبر مشروع `data-importer` منفصل، ليست core API |
| **Dashboard aggregation في نداء واحد** | ❓ يحتاج تحقق (OQ-10) |
| **Idempotency / incremental-sync cursor** | ❓ حرج للـ offline — يحتاج تحقق (OQ-11, OQ-12) |
| Receipt OCR / auto-categorization | مؤجّلة على الأرجح خارج MVP |

## أهم القرارات المعمارية المبكّرة

- واجهة وحيدة = `/api/v1`.
- decimal/minor-units في كل طبقات الموبايل.
- أي backend إضافي = BFF مستقل (حماية من AGPL + صيانة أسهل).
- اعتماد سلوك الرسمي (fork مطابق).

## الحواجز (Blockers) قبل المرحلة التالية

الأسئلة الحرجة المفتوحة: **OQ-01** (قانوني/AGPL)، **OQ-05/06** (PKCE + tokens)،
**OQ-10/11/12** (aggregation + idempotency + sync cursor). تفاصيلها في
`25-open-questions.md`.

## نطاق ما لم يُنجز بعد (مقصود)

لا شاشات، لا كود موبايل، لا PRD نهائي، لا كتالوج API تفصيلي — هذه مخرجات المراحل 2–17
ولن تبدأ قبل إغلاق تعارضات المرحلة الحالية.

# 01 — Repository Assessment & Technology Stack

**المرحلة:** 1 — Discovery / Repository Assessment
**تاريخ اللقطة:** 2026-07-16
**الحالة:** ✅ مكتمل (مبني على فحص الكود الفعلي)

---

## 1. حالة المستودع (Repository Status)

### 1.1 هل المستودع Fork أم نسخة مستقلة؟

**النتيجة (مؤكدة):** `devmuslim/firefly-iii` هو **Fork مطابق تمامًا** للمستودع الرسمي
`firefly-iii/firefly-iii`، ولا يحتوي على أي تعديلات مخصّصة على فرع `main`.

الدليل البرمجي:

```
official firefly-iii/main  HEAD = cdf1721e1d2d97ba7749285b113e9a5a69bbd969
devmuslim/firefly-iii/main HEAD = cdf1721e1d2d97ba7749285b113e9a5a69bbd969
→ HEADs متطابقان بايتيًا (git rev-parse متساوٍ)
آخر Commit: "Change workflow a little." — 2026-07-16 06:55:26 +0200
```

> **الأثر على المشروع:** لا يوجد سلوك مخصّص في الـ backend يجب مراعاته. يمكن اعتماد
> السلوك الرسمي لـ Firefly III كمصدر وحيد للحقيقة (Single Source of Truth) للـ API.
> أي تخصيص مستقبلي في مستودعك سيتطلب تحديث هذه الوثيقة.

### 1.2 الفروع المتاحة (Branches)

| المستودع | الفروع |
|---|---|
| **الرسمي** `firefly-iii/firefly-iii` | `main`, `develop`, `v6.1`, `v6.2`, `adminlte`, `JC5-patch-1`, `JC5-patch-2`, `release-1768367136`, `release-1774106768` |
| **fork** `devmuslim/firefly-iii` | **`main` فقط** |

المصدر: `git ls-remote --heads` لكل مستودع.

### 1.3 الإصدار المستخدم (Version)

| العنصر | القيمة | المصدر |
|---|---|---|
| Firefly III version | **`6.6.6`** | `config/firefly.php` → `'version' => '6.6.6'` |
| API / OpenAPI spec version | **`2.1.0`** | `config/firefly.php` → `'api_version' => '2.1.0'` (معلّق: `// field is no longer used.`) |
| API URL prefix | **`/api/v1`** فقط | `routes/api.php` → كل المجموعات تستخدم `'prefix' => 'v1'` |

> ⚠️ **نقطة مهمة جدًا:** رغم أن نسخة مواصفة الـ API هي `2.1.0`، فإن **مسارات الـ API
> تُقدَّم تحت `/api/v1/` فقط** في هذا الإصدار. لا يوجد `/api/v2` في الكود
> (السلاسل `v2` الموجودة تخص layout واجهة الويب فقط: `config('view.layout')`،
> المصدر `app/Http/Controllers/HomeController.php:136`). أي وثيقة أو أداة تفترض
> `/api/v2` غير صحيحة لهذا الإصدار.

### 1.4 الاختلافات بين مستودعك والمستودع الرسمي

| البند | النتيجة |
|---|---|
| ملفات معدّلة | **لا يوجد** |
| ملفات مضافة | **لا يوجد** |
| ملفات محذوفة | **لا يوجد** |
| فرق في الشجرة (tree) | **صفر** (نفس commit hash) |
| حالة التحديث مقابل المصدر | **متزامن 100%** (up-to-date) |

### 1.5 الترخيص والاستخدام التجاري (License) — ⚠️ حرج

| البند | القيمة | المصدر |
|---|---|---|
| الترخيص | **AGPL-3.0** (GNU Affero General Public License v3) | `LICENSE`, `COPYING` |

**التحليل القانوني (من منظور Security/Legal — للمراجعة القانونية النهائية):**

- **AGPL-3.0 هو ترخيص Copyleft قوي مع "شرط الشبكة" (Network Clause / §13):** أي مستخدم
  يتفاعل مع البرنامج **عبر الشبكة** يحق له الحصول على الكود المصدري الكامل للنسخة المُعدّلة
  التي تخدمه.
- **الأثر على تطبيق الموبايل — نقطة الفصل الجوهرية:**
  - إذا كان تطبيق الموبايل **عميلًا منفصلًا (separate work)** يتواصل مع Firefly III عبر
    الـ HTTP API فقط، ولا يدمج أو يشتقّ من كود Firefly III، فإن الأرجح — **غير مؤكد
    قانونيًا، يحتاج مراجعة محامٍ** — أنه **لا يُعتبر "عملًا مشتقًا"** ولا تنطبق عليه AGPL.
    التفاعل عبر واجهة API عام يُعامَل عادةً كـ arm's-length interface.
  - إذا قمت **بتعديل Firefly III نفسه** (backend مخصّص، endpoints جديدة، aggregation
    layer داخل الكود) **ونشرته كخدمة**، **تنطبق AGPL** على تعديلاتك ويجب إتاحة مصدرها.
- **توصية معمارية مبنية على ذلك:** أي **Backend إضافي** مطلوب (انظر Gap Analysis لاحقًا:
  aggregation, push tokens, device registration) يُفضّل بناؤه كـ **خدمة مستقلة (BFF –
  Backend-for-Frontend)** تستهلك API الرسمي، **لا كـ fork معدّل** — لتقليل التزامات AGPL
  والحفاظ على مرونة التحديث من upstream.

> **قرار مطلوب من الـ Product/Legal (Open Question OQ-01):** هل الاستراتيجية هي
> «تطبيق موبايل + Firefly III رسمي غير معدّل + خدمة BFF مستقلة»؟ هذا الخيار هو الأقل
> مخاطرة قانونيًا والأسهل صيانةً. **يتطلب اعتماد قانوني رسمي قبل أي توزيع تجاري.**

### 1.6 المخاطر (قانونية / تقنية) في بناء تطبيق تجاري

| # | المخاطرة | النوع | الشدة | التخفيف المقترح |
|---|---|---|---|---|
| R-01 | التزامات AGPL عند تعديل الـ backend ونشره | قانوني | عالية | فصل أي backend كخدمة مستقلة تستهلك API؛ عدم تعديل fork المنشور |
| R-02 | استخدام العلامة/الاسم "Firefly III" تجاريًا | قانوني/علامات | متوسطة | تسمية مستقلة للتطبيق؛ ذكر التوافق لا التبعية |
| R-03 | كسر التوافق مع تحديثات upstream (breaking API changes) | تقني | متوسطة | تثبيت إصدار مدعوم؛ Contract tests؛ اكتشاف الإصدار عند الاتصال |
| R-04 | خصائص متاحة في الويب فقط وغير موجودة في API | تقني | متوسطة | Gap Analysis (المرحلة 7) قبل الاعتماد على أي خاصية |
| R-05 | غياب push notifications أصلًا في Firefly III | تقني | عالية للـ MVP | يتطلب backend إضافي — يُحسم في المرحلة 7 |
| R-06 | تعدّد قواعد البيانات وإعدادات الخوادم لدى المستخدمين | تقني | منخفضة | الاعتماد على API فقط، لا على DB مباشرة |

---

## 2. Technology Stack (مُستخرَج من الكود)

### 2.1 Backend Core

| العنصر | القيمة | المصدر |
|---|---|---|
| Backend framework | **Laravel `^13`** | `composer.json` → `laravel/framework` |
| Programming language | **PHP `>= 8.5`** | `composer.json` → `"php": ">=8.5"` |
| امتدادات PHP المطلوبة | bcmath, curl, intl, mbstring, openssl, pdo, xml, iconv, fileinfo, simplexml, tokenizer, xmlwriter, session | `composer.json` → `require` |
| Money / Decimal arithmetic | **`ext-bcmath`** (حساب عشري بدقة عشوائية) | `composer.json`; مستخدم في `app/Support/Steam.php`, `app/Support/Http/Api/ExchangeRateConverter.php`, `app/Support/Models/AccountBalanceCalculator.php` |
| HTTP client (خارجي) | **Guzzle `^7.11`** + `symfony/http-client ^8.0` | `composer.json` |

> ✅ **تأكيد متطلب مالي:** الـ backend يستخدم **BCMath (decimal strings)** وليس
> floating-point في العمليات المالية. هذا يُلزم تطبيق الموبايل باستخدام decimal/minor-units
> أيضًا (لا `float`/`double`). تفاصيل القواعد في `04-financial-calculations.md`.

### 2.2 Database & Persistence

| العنصر | القيمة | المصدر |
|---|---|---|
| Database engines المدعومة | **MySQL / MariaDB, PostgreSQL, SQLite, SQL Server** | `config/database.php` → connections: `mysql`, `pgsql`, `sqlite`, `sqlsrv` |
| Default connection | `mysql` | `config/database.php` |
| عدد الـ Migrations | **60** ملف | `database/migrations/` |
| عدد الـ Eloquent Models | **50** model | `app/Models/` |

### 2.3 Authentication & Authorization

| العنصر | القيمة | المصدر |
|---|---|---|
| API Authentication | **Laravel Passport `^13.0` (OAuth2)** | `composer.json`; `config/passport.php` |
| أنواع الاعتماد | **Personal Access Token (PAT)** + **OAuth2 (Authorization Code / Client Credentials)** | `config/passport.php`, `routes/api.php` middleware |
| API guard | `auth:api` (Passport) على كل مسارات `/api/v1` | `bootstrap/app.php:113` → `$middleware->group('api', [... 'auth:api'])` |
| صلاحيات إضافية | مجموعة `api-admin` لبعض مسارات الإدارة | `routes/api.php` (مثل `middleware => ['api-admin']`) |
| Two-Factor Auth | Google 2FA (`pragmarx/google2fa`, `jc5/google2fa-laravel`) | `composer.json` — **خاص بالويب، غير مؤكد توفره عبر API** |
| Web auth scaffolding | `laravel/ui ^4.2` | `composer.json` |
| Authorization model | User Roles + User Groups (multi-user administration) | `app/Models/UserRole.php`, `UserGroup.php`, `GroupMembership.php` |

> ملاحظة: `config/sanctum.php` موجود، لكن حارس الـ API الفعلي هو **Passport** (`auth:api`).
> يُحسم دور Sanctum (إن وجد) في مرحلة المصادقة (المرحلة 9).

### 2.4 API Layer

| العنصر | القيمة | المصدر |
|---|---|---|
| API style | REST / JSON | `routes/api.php` |
| API versioning | URL prefix `v1` (namespace `FireflyIII\Api\V1\Controllers`) | `routes/api.php:46-47` |
| Serialization | **`league/fractal 0.*`** (Transformers + DTO) | `composer.json`; `app/Transformers/` (**26** transformer) |
| Validation | Laravel **FormRequest** classes | `app/Api/V1/Requests/**` |
| Query parsing (search) | `gdbots/query-parser ^3.0` | `composer.json` |
| Pagination | Laravel LengthAwarePaginator (Fractal include) | Transformers/Controllers |

### 2.5 Currency, Date/Time & Localization

| العنصر | القيمة | المصدر |
|---|---|---|
| Default base currency | **`EUR`** | `config/firefly.php:196` → `'default_currency' => 'EUR'` |
| نموذج العملات | كل كائن يحمل عملته (`currency_*`) + عملة أساسية للإدارة (`primary_currency_*`) + قيم محوّلة (`pc_*`) | `docs/references/firefly-iii/api/index.md` |
| Exchange rates | `CurrencyExchangeRate` model + `ExchangeRateConverter` | `app/Models/CurrencyExchangeRate.php`, `app/Support/Http/Api/ExchangeRateConverter.php` |
| Date/time | Carbon (Laravel) | إطار العمل |
| Localization | Crowdin + ملفات `resources/lang` و `resources/locales` | `crowdin.yml`, `resources/lang/` |
| Markdown | `league/commonmark ^2` | `composer.json` |

### 2.6 Queue, Cache, Jobs & Events

| العنصر | القيمة | المصدر |
|---|---|---|
| Queue | افتراضي `sync`؛ يدعم `database`, `redis` | `config/queue.php:37` |
| Cache | افتراضي `file`؛ يدعم `redis` (`predis/predis ^3`) | `config/cache.php:39`; `composer.json` |
| Background Jobs | `CreateRecurringTransactions`, `DownloadExchangeRates`, `CreateAutoBudgetLimits`, `WarnAboutBills`, `SendWebhookMessage`, `MailError` | `app/Jobs/` |
| Events domains | Admin, Model, Preferences, Security, Test | `app/Events/` |
| Webhooks | نظام كامل (Webhook + WebhookMessage/Attempt/Delivery/Response/Trigger) | `app/Models/Webhook*.php` (6 models) |

### 2.7 Notifications, Files, Email

| العنصر | القيمة | المصدر |
|---|---|---|
| Notification channels | Mail, **Pushover** (`laravel-notification-channels/pushover ^5.0`), **Slack** (`laravel/slack-notification-channel ^3.3`), ntfy (معلّق/اختياري) | `composer.json`; `app/Notifications/` |
| ⚠️ Mobile Push (FCM/APNs) | **غير موجود أصلًا** | لا توجد تبعية FCM/APNs — فجوة تُحسم في المرحلة 7 |
| Email drivers | Mailgun (`symfony/mailgun-mailer`), MailerSend (`mailersend/laravel-driver`), SMTP | `composer.json` |
| File storage | Laravel Filesystem (local/…); Attachments | `app/Models/Attachment.php` |
| QR codes | `bacon/bacon-qr-code ^3.0` (لـ 2FA) | `composer.json` |
| CSV | `league/csv ^9.10` (import/export) | `composer.json` |

### 2.8 Import / Export

| العنصر | القيمة | المصدر |
|---|---|---|
| Export | endpoints تحت `/api/v1/data/export/*` (CSV) | `routes/api.php:173-189` → `ExportController` |
| Purge / Destroy data | `/api/v1/data/destroy`, `/api/v1/data/purge` | `routes/api.php:193-210` |
| Import (الاستيراد المصرفي/CSV) | **مشروع منفصل:** `firefly-iii/data-importer` (خارج هذا المستودع) | `readme.md`, التوثيق الرسمي |

> ⚠️ الاستيراد التلقائي (البنوك/CSV) **ليس جزءًا من core API**؛ يتم عبر مشروع
> `data-importer` المستقل. أي «bank sync» في الموبايل فجوة كبيرة (المرحلة 7).

### 2.9 Frontend (الويب) — للسياق فقط

| العنصر | القيمة | المصدر |
|---|---|---|
| Templating | **Twig** (`rcrowe/twigbridge ^0.14`) + Blade | `composer.json`; `resources/views/` |
| HTML helpers | `spatie/laravel-html ^3.13` | `composer.json` |
| Build | `patch-package` postinstall؛ `postcss` (الأصول قد تُبنى خارجيًا) | `package.json` |
| View layouts | `v1` (AdminLTE) و `v2` (layout أحدث) | `config('view.layout')` |

> واجهة الويب **ليست** مرجعًا للموبايل؛ المرجع هو الـ API. أي خاصية موجودة في الويب
> وغير مغطاة بـ API تُوسم في Gap Analysis.

### 2.10 Testing, Tooling, Deployment

| العنصر | القيمة | المصدر |
|---|---|---|
| Testing framework | **PHPUnit** (`phpunit.xml`) + `nunomaduro/collision ^8` | `phpunit.xml`, `composer.json`, `tests/` |
| Static analysis / quality | Mago (`mago.toml`), SonarQube (`sonar-project.properties`) | ملفات الجذر |
| Error tracking (backend) | `spatie/laravel-ignition ^2` | `composer.json` |
| Deployment | Docker (رسمي) + `nginx_app.conf` + `Procfile` (Heroku-style) + `server.php` | ملفات الجذر |
| Runtime entry | `public/index.php`, `artisan` | الجذر |

---

## 3. ملخص القرارات المبكّرة (Early Decisions)

| القرار | الأساس |
|---|---|
| اعتماد `/api/v1` كواجهة وحيدة | لا يوجد `/api/v2` في 6.6.6 |
| استخدام decimal/minor-units في الموبايل | الـ backend يستخدم BCMath |
| بناء أي backend إضافي كخدمة BFF مستقلة | تقليل التزامات AGPL + سهولة الصيانة |
| اعتبار Push/BankSync/OCR فجوات مؤكدة | لا توجد تبعياتها في الـ core |
| اعتماد سلوك الرسمي كمرجع (fork مطابق) | HEADs متطابقة بايتيًا |

**التالي:** `02-current-system-architecture.md` (المخططات + منهجية اكتشاف الـ API).

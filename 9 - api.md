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

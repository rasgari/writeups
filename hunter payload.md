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


# merge

===>>> open redirect & xss


payload: continue_url=attacker.com
payload: { javas%09cript:alert(1)}




==================================

در سال ۲۰۲۵، شکارچیان آسیب‌پذیری (Bug Bounty Hunters) اغلب موفق به کشف باگ‌هایی شده‌اند که از طریق تزریق payload در URLهای خاص و رایج اپلیکیشن‌های وب اتفاق افتاده‌اند. این URLها معمولاً در مسیرهایی قرار دارند که یا:

1. داده‌ای را از کاربر دریافت و رندر می‌کنند،


2. عملیات احراز هویت، بازنشانی، ریدایرکت یا تعامل با API انجام می‌دهند،


3. یا لاگیک سمت سرور یا کلاینت در آن‌ها حضور دارد.




---

📌 مهم‌ترین مسیرها (URLs) برای تزریق در ۲۰۲۵

بر اساس صدها write-up در HackerOne، Bugcrowd و گزارش‌های سال ۲۰۲۵، این URLها بیشترین آسیب‌پذیری‌های تزریقی را در خود داشته‌اند:


---

🔥 ۱. مسیرهای ریدایرکت، callback و return:

مسیر	توضیح	تست با

/redirect?url=FUZZ	آسیب‌پذیری Open Redirect / XSS	//evil.com، javascript:alert(1)
/auth/callback?next=FUZZ	حمله ریدایرکت یا cookie theft	URLهای شخص ثالث
/continue?redirect=FUZZ	Bypass احراز هویت	URL encoding


⛏ نکته: اغلب بدون validation کامل یا بدون allowlist هستند. می‌توان XSS یا session hijacking اجرا کرد.


---

💥 ۲. مسیرهای جستجو، فیلتر، sort یا نمایش نتایج:

مسیر	توضیح	تست با

/search?q=FUZZ	XSS / SQLi / LFI	"><img src=x onerror=alert(1)> یا '--
/filter?category=FUZZ	SSRF یا SQLi	http://127.0.0.1 یا UNION SELECT
/sort?by=FUZZ	RCE یا logic bypass	{{7*7}} یا $ne:null



---

🧠 ۳. مسیرهای API یا فایل JSON/XML:

مسیر	نوع حمله	نمونه تست

/api/user/update (POST)	Prototype Pollution	{"__proto__": {"isAdmin": true}}
/api/upload	File Upload	تغییر MIME و نام فایل
/graphql	Injection / Access	Mutation و introspection queries



---

🧨 ۴. مسیرهای احراز هویت:

مسیر	نوع آسیب‌پذیری	تست

/login, /register, /reset	SQLi، Logic Bypass	admin' --, یا تغییر role در JWT
/password/change	IDOR / CSRF	تست user ID‌های مختلف یا ارسال از دامنه دیگر
/2fa/verify?code=FUZZ	Rate-limit / brute force	ارسال سریع کدهای OTP



---

🧪 ۵. مسیرهای preview و embed:

مسیر	آسیب‌پذیری	نمونه تزریق

/preview?file=FUZZ	LFI / XSS	../../etc/passwd, javascript:
/embed?url=FUZZ	SSRF / Clickjacking	http://127.0.0.1 یا iframe مخرب



---

📚 منابعی که بر اساس آن‌ها این URLها اولویت‌بندی شده‌اند:

گزارش رسمی HackerOne 2025 Top Reports

مقالات top hunters مثل Pratham Shah و s0md3v

ریپورت‌های Bugcrowd و Integrity از راه‌یافتگان Hall of Fame

ابزارهای Fuzzer مدرن مثل ParamMiner, ffuf, Arjun, و Kiterunner



---

✅ پیشنهاد: ابزار اتوماتیک تست این مسیرها

برای اسکن سریع این مسیرها، می‌تونی از ابزار زیر استفاده کنی:

ffuf -u https://target.com/FUZZ -w paths.txt -mc 200,302

یا از Arjun برای شناسایی پارامترهای تزریقی:

python3 arjun.py -u https://target.com/page -o found.txt


---

🧭 اگر دامنه‌ای خاص داری (مثلاً CMS یا API خاص)، بگو تا مسیرهای اختصاصی اون رو تحلیل کنیم.

در غیر این صورت، پیشنهاد می‌کنم مسیرهای زیر را به‌عنوان تست اولیه در هر هدفی بررسی کنی:

/redirect?url=
/search?q=
/login?next=
/api/user/update
/embed?url=
/reset?email=
/preview?file=
/sort?order=

===============================


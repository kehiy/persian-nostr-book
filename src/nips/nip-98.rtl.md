
# نیپ شماره 98

## احرازهویت HTTP

`پیش نویس` ‍‍`دلبخواهی`

این نیپ یک رویداد گذرموقت را تعریف میکند که برای احرازهویت درخواست ها به HTTP سرور ها با استفاده از رویداد های نوستر استفاده میشود.

این نیپ برای خدمات HTTP ای که برای نوستر ساخته شده اند و با حساب های نوستر کاربران سر و کار دارند کاربردی است.

### رویداد نوستر

یک رویداد با گونه `27235` (به دلیل [RFC 7235](https://www.rfc-editor.org/rfc/rfc7235)) استفاده میشود.

فیلد `content` باید خالی باشد.

برچسب های زیر باید در رویداد وجود داشته باشند:

* `u` - url مطلق سرویس
* `method` - متد درخواست HTTP

رویداد نمونه:

```json
{
  "id": "fe964e758903360f28d8424d092da8494ed207cba823110be3a57dfe4b578734",
  "pubkey": "63fe6318dc58583cfe16810f86dd09e18bfd76aabc24a0081ce2856f330504ed",
  "content": "",
  "kind": 27235,
  "created_at": 1682327852,
  "tags": [
    ["u", "https://api.snort.social/api/v1/n5sp/list"],
    ["method", "GET"]
  ],
  "sig": "5ed9d8ec958bc854f997bdc24ac337d005af372324747efe4a00e24f4c30437ff4dd8308684bed467d9d6be3e5a517bb43b1732cc7d33949a3aaf86705c22184"
}
```

سرور باید بررسی های زیر را برای اعتبارسنجی رویداد انجام دهد:

۱. گونه باید 27235 باشد.
۲. تاریخ `created_at` باید در یک بازه زمانی منطقی باشد. (زمان پیشنهادی یک دقیقه است.)
۳. برچسب `u` باید url کامل مسیر صدا زده شده باشد. (با query parameter ها.)
۴. برچسب `method` باید با متد درخواست برابر باشد.

زمانی که درخواست شامل body است (متد های POST/PUT/PATCH) کلاینت ها باید یک هش SHA256 از آن را در تگ `payload` بر مبنای 16 قرار دهند. نمونه: (`["payload", "<sha256-hex>"]`). سرور ممکن است آن را بررسی کند تا اعتبارسنجی کند که payload درخواست احرازهویت شده است.

اگر هرکدام از بررسی ها ناموفق بود سرور باید درخواست با کد 401 احرازهویت نشده پاسخ دهد.

سرور ممکن است بررسی های افزوده مربوط به پیاده سازی خودش را هم انجام دهد.

### روند درخواست

با استفاده از `Authorization` HTTP header رویداد باید بر مبنای ۶۴ باشد و  Authorization scheme `Nostr` باشد.

نمونه  HTTP Authorization header:
```
Authorization: Nostr 
eyJpZCI6ImZlOTY0ZTc1ODkwMzM2MGYyOGQ4NDI0ZDA5MmRhODQ5NGVkMjA3Y2JhODIzMTEwYmUzYTU3ZGZlNGI1Nzg3MzQiLCJwdWJrZXkiOiI2M2ZlNjMxOGRjNTg1ODNjZmUxNjgxMGY4NmRkMDllMThiZmQ3NmFhYmMyNGEwMDgxY2UyODU2ZjMzMDUwNGVkIiwiY29udGVudCI6IiIsImtpbmQiOjI3MjM1LCJjcmVhdGVkX2F0IjoxNjgyMzI3ODUyLCJ0YWdzIjpbWyJ1IiwiaHR0cHM6Ly9hcGkuc25vcnQuc29jaWFsL2FwaS92MS9uNXNwL2xpc3QiXSxbIm1ldGhvZCIsIkdFVCJdXSwic2lnIjoiNWVkOWQ4ZWM5NThiYzg1NGY5OTdiZGMyNGFjMzM3ZDAwNWFmMzcyMzI0NzQ3ZWZlNGEwMGUyNGY0YzMwNDM3ZmY0ZGQ4MzA4Njg0YmVkNDY3ZDlkNmJlM2U1YTUxN2JiNDNiMTczMmNjN2QzMzk0OWEzYWFmODY3MDVjMjIxODQifQ
```

### پیاده سازی های مرجع

C# ASP.NET `AuthenticationHandler` [NostrAuth.cs](https://gist.github.com/v0l/74346ae530896115bfe2504c8cd018d3)
‍‍
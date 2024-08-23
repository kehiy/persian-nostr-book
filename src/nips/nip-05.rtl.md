# نیپ شماره 5

## نگاشت کلیدهای Nostr به شناسه های اینترنتی مبتنی بر DNS

`نهایی` `دلبخواهی`

در رویداد های گونه ۰ (متادیتا کاربر) یک کلید به نام `nip05` (https://datatracker.ietf.org/doc/html/rfc5322#section-3.4.1)[میتواند ادرس شناسه گر اینترنتی] (ادرسی ایمیل مانند) را به عنوان مقدار داشته باشد. هرچند که لینک به مشخصات شناسه گر اینترنتی در بالا وجود دارد اما نیپ ۵ تصور میکند که `<local-part>` به کاراکتر های `a-z0-9-_.` حساس است و حساستی به  کوچکی و بزرگی کاراکتر ها ندارد.

با دیدن این ادرس کلاینت ادرس را به دو قسمت `domain` و `<local-part>` تقسیم میکند و از این مقادیر برای ایجاد یک درخواست GET به `https://<domain>/.well-known/nostr.json?name=<local-part>` استفاده میکند.
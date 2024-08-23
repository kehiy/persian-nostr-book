# نیپ شماره ۳

## گواهی OpenTimestamps برای رویداد ها

`پیشنویس` `دلبخواهی`

این نیپ یک رویداد با گونه ‍`kind:1040` تعریف میکند که یک اثبات [OpenTimestamps](https://opentimestamps.org/) برای هر رویداد دیگیری دارد:

```json
{
  "kind": 1040
  "tags": [
    ["e", <event-id>, <relay-url>],
    ["alt", "opentimestamps attestation"]
  ],
  "content": <base64-encoded OTS file data>
}
```

- مدرک OpenTimestamps باید شناسه رویداد مرجع را به عنوان هش خود ثابت کند.

- محتوا (`content`) باید به شکل کامل محتوای یک فایل .ots باشد که حداقل شامل یک گواهی بیت کوین است. این فایل باید شامل یک گواهی بیت کوین باشد (زیرا وجود بیش از یک گواهی معتبر لازم نیست و حجم کمتر بهتر از بیشتر است.) و نباید به گواهی‌ های در انتظار اشاره کند زیرا آنها در این زمینه بی‌ فایده هستند.


### روند نمونه بررسی سلامت  OpenTimestamps

با استفاده از [`nak`](https://github.com/fiatjaf/nak), [`jq`](https://jqlang.github.io/jq/) و [`ots`](https://github.com/fiatjaf/ots):

```bash
~> nak req -i e71c6ea722987debdb60f81f9ea4f604b5ac0664120dd64fb9d23abc4ec7c323 wss://nostr-pub.wellorder.net | jq -r .content | ots verify
> using an esplora server at https://blockstream.info/api
- sequence ending on block 810391 is valid
timestamp validated at block [810391]
```

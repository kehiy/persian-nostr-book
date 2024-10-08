> **اخطار** `پیشنهاد نشده`: به دنبال نیپ شماره هفده منسوخ شده است.

# نیپ شماره 4

## پیام مستقیم رمزنگاری شده

`نهایی` `پیشنهاد نشده` ‍‍`دلبخواهی`

یک رویداد ویژه با گونه 4 به معنای پیام مستقیم رمزنگاری شده است. انتظار میرود که این رویداد ویژگی های زیر را داشته باشد:
 
متن باید برابر با رشته‌ای باشد که به صورت base64 رمزنگاری شده و با استفاده از aes-256-cbc رمزگذاری شده است و هر چیزی که یک کاربر می‌خواهد بنویسد را شامل می‌شود. این رمزگذاری با استفاده از یک رمز مشترک انجام می‌گیرد که با ترکیب کلید عمومی گیرنده و کلید خصوصی فرستنده ایجاد شده است. این متن باید به همراه رشته‌ ابتدایی (initialization vector) که به صورت base64 رمزنگاری شده است به عنوان یک پارامتر نام‌گذاری شده با عنوان "iv" اضافه شود. فرمت بدین صورت است: "content": "<encrypted_text>?iv=<initialization_vector>".

برچسب ها ممکن است یک شامل شناسه دریافت کننده پیام باشند (در این صورت رله ها ممکن است به طور طبیعی رویداد را به انها بفرستند.) به شکل: `["p", "<pubkey, as a hex string>"]`.

برچسب ها ممکن است شامل شناسه پیام قبلی مکالمه یا پیامی که ما صراحتا به ان پاسخ میدهیم (به طوری که ممکن است پیام های سازمان یافته تری رخ دهد.) به صورت `["e", "<event_id>"]` باشند.

**یادداشت:** به طور پیشفرض در پیاده سازی [libsecp256k1](https://github.com/bitcoin-core/secp256k1) ECDH کلید مخفی برابر با هش SHA256 نقطه مشترک (هر دو مختصات X و Y) است. در نوستر فقط مختصات X نقطه مشترک به عنوان کلید مخفی استفاده می‌شود و این مقدار هش نمی‌شود. اگر از کتابخانه libsecp256k1 استفاده می‌کنید، باید یک تابع سفارشی که مختصات X را کپی می‌کند به عنوان آرگومان hashfp در تابع secp256k1_ecdh منتقل کنید. [ببینید](https://github.com/bitcoin-core/secp256k1/blob/master/src/modules/ecdh/main_impl.h#L29).

کد منبع نمونه برای تولید چنین رویدادی با جاوااسکریپت:

```js
import crypto from 'crypto'
import * as secp from '@noble/secp256k1'

let sharedPoint = secp.getSharedSecret(ourPrivateKey, '02' + theirPublicKey)
let sharedX = sharedPoint.slice(1, 33)

let iv = crypto.randomFillSync(new Uint8Array(16))
var cipher = crypto.createCipheriv(
  'aes-256-cbc',
  Buffer.from(sharedX),
  iv
)
let encryptedMessage = cipher.update(text, 'utf8', 'base64')
encryptedMessage += cipher.final('base64')
let ivBase64 = Buffer.from(iv.buffer).toString('base64')

let event = {
  pubkey: ourPubKey,
  created_at: Math.floor(Date.now() / 1000),
  kind: 4,
  tags: [['p', theirPublicKey]],
  content: encryptedMessage + '?iv=' + ivBase64
}
```

## اخطار امنیتی

این استاندارد به هیچ وجه به چیزی که به عنوان آخرین پیشرفت‌ ها در ارتباطات رمزگذاری شده بین همتاها در نظر گرفته می‌شود نزدیک نمی‌شود و metadata را در رویدادها نشت می‌کند. بنابراین، نباید برای هیچ چیزی که به واقع نیاز دارید محرمانه بماند استفاده شود و فقط باید با رله‌هایی که از AUTH برای محدود کردن اینکه چه کسی می‌تواند رویدادهای نوع:4 شما را برداشت کند استفاده شود.


## اخطار پیاده سازی کلاینت

کلاینت نباید کلید های عمومی را در `.content` جستجو و جایگذاری کند. اگر مثل یک متن معمولی پردازش شود و `@npub...` با `#[0]` و یک برچسب `["p", "..."]` جایگذاری شود. برچسب ها به بیرون نشت می شود و کاربران نام برده پیام را در صندوق ورودی خود دریافت میکنند.

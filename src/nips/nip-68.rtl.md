
# نیپ شماره 68

## فید ها با محوریت تصویر

`پیش نویس` ‍‍`دلبخواهی`

 این نیپ یک رویداد با گونه ۲۰ برای کلاینت های تصویر محور معرفی میکند. تصاویر باید مستقل باشند. انها بصورت خارجی میزبانی میشوند و با برچسب imeta ارجاع داده میشوند. 

 ایده این است که این نوع رویداد به کلاینت های نوستر ای پاسخگو باشد که همچون پلتفرم‌هایی مانند اینستاگرام، فلیکر، اسنپ چت یا 9GAG هستند، جایی که خود تصویر در مرکز تجربه کاربر قرار می‌گیرد.

 ### رویداد تصویر

 رویداد های تصویر یک عنوان در برچسب title و یک توضیحات در .content دارند.

 آنها ممکن است چند عکس برای نمایش در یک پست داشته باشند.

 ```json
 {
  "id": <32-bytes lowercase hex-encoded SHA-256 of the the serialized event data>,
  "pubkey": <32-bytes lowercase hex-encoded public key of the event creator>,
  "created_at": <Unix timestamp in seconds>,
  "kind": 20,
  "content": "<description of post>",
  "tags": [
    ["title", "<short title of post>"],

    // Picture Data
    [
      "imeta",
      "url https://nostr.build/i/my-image.jpg",
      "m image/jpeg",
      "blurhash eVF$^OI:${M{o#*0-nNFxakD-?xVM}WEWB%iNKxvR-oetmo#R-aen$",
      "dim 3024x4032",
      "alt A scenic photo overlooking the coast of Costa Rica",
      "x <sha256 hash as specified in NIP 94>",
      "fallback https://nostrcheck.me/alt1.jpg",
      "fallback https://void.cat/alt1.jpg"
    ],
    [
      "imeta",
      "url https://nostr.build/i/my-image2.jpg",
      "m image/jpeg",
      "blurhash eVF$^OI:${M{o#*0-nNFxakD-?xVM}WEWB%iNKxvR-oetmo#R-aen$",
      "dim 3024x4032",
      "alt Another scenic photo overlooking the coast of Costa Rica",
      "x <sha256 hash as specified in NIP 94>",
      "fallback https://nostrcheck.me/alt2.jpg",
      "fallback https://void.cat/alt2.jpg",

      "annotate-user <32-bytes hex of a pubkey>:<posX>:<posY>" // Tag users in specific locations in the picture
    ],

    ["content-warning", "<reason>"], // if NSFW

    // Tagged users
    ["p", "<32-bytes hex of a pubkey>", "<optional recommended relay URL>"],
    ["p", "<32-bytes hex of a pubkey>", "<optional recommended relay URL>"],

    // Specify the media type for filters to allow clients to filter by supported kinds
    ["m", "image/jpeg"],

    // Hashes of each image to make them queryable
    ["x", "<sha256>"]

    // Hashtags
    ["t", "<tag>"],
    ["t", "<tag>"],

    // location
    ["location", "<location>"], // city name, state, country
    ["g", "<geohash>"],

    // When text is written in the image, add the tag to represent the language
    ["L", "ISO-639-1"],
    ["l", "en", "ISO-639-1"]
  ]
}
```

تگ imeta عه annotate-user کمک میکند تا یک کاربر در قسمتی از عکس منشن شود.

تنها media type های زیر قابل پذیرش اند:

- `image/apng`: Animated Portable Network Graphics (APNG)
- `image/avif`: AV1 Image File Format (AVIF)
- `image/gif`: Graphics Interchange Format (GIF)
- `image/jpeg`: Joint Photographic Expert Group image (JPEG)
- `image/png`: Portable Network Graphics (PNG)
- `image/webp`: Web Picture format (WEBP)

رویداد های تصویر محور ممکن است با رویداد ها گونه ۲۲ نیپ ۷۱ استفاده شوند تا ویدیو های عمودی را در همان فید نشان دهند.


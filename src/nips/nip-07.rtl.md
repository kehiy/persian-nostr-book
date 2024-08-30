# نیپ شماره 7

## قابلیت window.nostr برای مرورگر های وب

`پیشنویس` `دلبخواهی`

شی window.nostr ممکن است توسط مرورگر وب یا اکستنشن ها در دسترس قرار گیرد و وب اپلیکیشن ها و وبسایت ها ممکن است بعد از بررسی موجود بودن ان از ان استفاده کنند.

این شی (Object) باید متد های زیر را تعریف کند:

```
async window.nostr.getPublicKey(): string // returns a public key as hex
async window.nostr.signEvent(event: { created_at: number, kind: number, tags: string[][], content: string }): Event // takes an event object, adds `id`, `pubkey` and `sig` and returns it
```

در کنار دو متد ابتدایی بالا این متد ها بصورت دلبخواهی میتوانند پیاده سازی شوند:

```
async window.nostr.getRelays(): { [url: string]: {read: boolean, write: boolean} } // returns a basic map of relay urls to relay policies
async window.nostr.nip04.encrypt(pubkey, plaintext): string // returns ciphertext and iv as specified in nip-04 (deprecated)
async window.nostr.nip04.decrypt(pubkey, ciphertext): string // takes ciphertext and iv as specified in nip-04 (deprecated)
async window.nostr.nip44.encrypt(pubkey, plaintext): string // returns ciphertext as specified in nip-44
async window.nostr.nip44.decrypt(pubkey, ciphertext): string // takes ciphertext as specified in nip-44
```

## پیشنهاد برای توسعه دهندگان اکستنشن های مرورگر

برای اطمینان حاصل کردن از اینکه `nostr.window` برای کلاینت ها در حین بارگیری صفحه در دسترس است توسعه دهنده اکستنشن که اکتنشن هایی برای فایرفاکس و کرومیوم مینویسند باید اسکریپت خود را با مشخص کردن `"run_at": "document_end"` در ‍‍`manifest` اکستنشن خود بارگیری کنند.

## پیاده سازی ها

ببینید:
https://github.com/aljazceru/awesome-nostr#nip-07-browser-extensions

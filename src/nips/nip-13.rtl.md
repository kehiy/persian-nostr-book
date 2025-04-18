
# نیپ شماره 13

## اثبات کار

`پیش نویس` ‍‍`دلبخواهی`

این نیپ یک راه برای تولید و تفسیر یادداشت های اثبات کار تعریف میکند. اثبات کار یک روش برای افزودن اثبات کار محاسباتی به یک یادداشت است.این یک اثبات قابل حمل است که هر رله یا کلاینت میتواند با مقدار کمی کد در هرجایی ان را اعتبار سنجی کند. این اثبات میتواند به عنوان ابزاری برای جلوگیری از هرزنامه استفاده شود.

difficulty یک عدد که نشان دهنده صفر های اول در فیلد id یک رویداد است. برای نمونه ایدی 000000000e9d97a1ab09fc381030b346cdd7a142ad57e6df0b46dc9bef6c7e2d شامل difficulty 36 است با 36 بیت با مقدار صفر در ابتدا.

002f... همان 0000 0000 0010 1111... در مبنای دودویی است. که در ابتدا10 صفر دارد. فراموش نکنید که برای رقم های هگزادسیمال 7=> صفر های ابتدا را بشمارید.

### استخراج 

برای ایجاد یک رویداد اثبات کار یک از برچسب nonce استفاده میشود:

```json
{"content": "It's just me mining my own business", "tags": [["nonce", "1", "21"]]}
```
در حین استخراج مقدار دوم برچسب nonce بروزرسانی میشود و id دوباره محاسبه میشود. اگر ایدی شمار صفر های ابتدایی خواسته شده را داشت استخراج انجام شده. پیشنهاد میشود که crated_at هم همزمان در طی این فرایند بروزرسانی شود.

سومین مقدار در برچسب nonce باید شامل سختی (difficulty) هدف باشد. این امکان را به کلاینت ها می‌دهد که در برابر شرایطی که هرزنامه نویس ها بر روی یک سطح دشواری پایین با خوش‌شانسی رویدادشان با یک سطح دشواری بالاتر مطابقت پیدا می‌کند، محافظت کنند. به عنوان مثال، اگر شما نیاز به ۴۰ بیت برای پاسخ به تاپیک خود داشته باشید و هدف متعهدی را با عدد ۳۰ مشاهده کنید، می‌توانید به‌راحتی آن را رد کنید حتی اگر یادداشت دارای دشواری ۴۰ بیتی باشد. بدون یک تعهد به هدف دشواری، شما نمی‌توانید آن را رد کنید. متعهد شدن به یک دشواری هدف موضوعی است که تمام ماینرهای صادق باید با آن موافق باشند و مشتریان ممکن است یک یادداشت را که با دشواری هدف مطابقت دارد رد کنند اگر تعهد به دشواری در آن موجود نباشد.

### نمونه رویداد استخراج شده

```json
{
  "id": "000006d8c378af1779d2feebc7603a125d99eca0ccf1085959b307f64e5dd358",
  "pubkey": "a48380f4cfcc1ad5378294fcac36439770f9c878dd880ffa94bb74ea54a6f243",
  "created_at": 1651794653,
  "kind": 1,
  "tags": [
    ["nonce", "776797", "20"]
  ],
  "content": "It's just me mining my own business",
  "sig": "284622fc0a3f4f1303455d5175f7ba962a3300d136085b9566801bc2e0699de0c7e31e44c81fb40ad9049173742e904713c3594a1da0fc5d2382a25c11aba977"
}
```

### اعتبار سنجی

این یک کد مرجع C برای محاسبه سختی (شمارش صفر های ابتدا عدد) است:

```c
int zero_bits(unsigned char b)
{
        int n = 0;

        if (b == 0)
                return 8;

        while (b >>= 1)
                n++;

        return 7-n;
}

/* find the number of leading zero bits in a hash */
int count_leading_zero_bits(unsigned char *hash)
{
        int bits, total, i;
        for (i = 0, total = 0; i < 32; i++) {
                bits = zero_bits(hash[i]);
                total += bits;
                if (bits != 8)
                        break;
        }
        return total;
}
```

این یک کد جاوااسکریپت برای انجام همان کار است:

```js
int zero_bits(unsigned char b)
{
        int n = 0;

        if (b == 0)
                return 8;

        while (b >>= 1)
                n++;

        return 7-n;
}

/* find the number of leading zero bits in a hash */
int count_leading_zero_bits(unsigned char *hash)
{
        int bits, total, i;
        for (i = 0, total = 0; i < 32; i++) {
                bits = zero_bits(hash[i]);
                total += bits;
                if (bits != 8)
                        break;
        }
        return total;
}
```

### اثبات کار واگذار شده

از انجایی که فیلد id در نیپ ۱ به امضا مرتبط نیست انجام اثبات کار میتواند به اراعه دهنده های اثبات کار در ازای یک کارمزد برونسپاری شود. این یک راه برای کلاینت ها است تا رویداد های خود را به رله هایی که محدودیت اثبات کار دارند بدون انجام دادن کاری توسط خودشون بفرستند که برای دستگاه هایی همچون تلفن های همراه که محدودیت انرژی دارند کاربردی است.

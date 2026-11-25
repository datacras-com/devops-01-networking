---
marp: true
theme: default
paginate: true
backgroundColor: "#F7F9F8"
color: "#0F172A"
style: |
  section {
    font-family: 'Segoe UI', Arial, sans-serif;
    padding: 24px 44px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    box-sizing: border-box;
    overflow: hidden;
    background-color: #F7F9F8;
    color: #0F172A;
  }
  h1 { color: #00A859; font-size: 1.65em; margin: 0 0 6px 0; }
  h2 { color: #00A859; font-size: 1.2em; border-bottom: 2px solid #00A859; padding-bottom: 5px; margin: 0 0 8px 0; }
  h3 { color: #0F172A; font-size: 0.95em; margin: 5px 0; }
  strong { color: #00A859; }
  em { color: #848688; font-style: normal; }
  code { background: #E8F5EE; color: #00703A; padding: 2px 5px; border-radius: 4px; font-size: 0.78em; border: 1px solid #C3E4D0; }
  pre { background: #0F172A; color: #E2E8F0; padding: 8px 12px; border-radius: 8px; border-left: 4px solid #00A859; font-size: 0.50em; line-height: 1.4; margin: 5px 0; overflow: hidden; }
  pre code { font-size: 1em; padding: 0; background: transparent; color: #A7F3D0; border: none; }
  table { width: 100%; border-collapse: collapse; font-size: 0.8em; }
  th { background: #00A859; color: #ffffff; padding: 6px 10px; text-align: left; }
  td { background: #ffffff; padding: 6px 10px; border-bottom: 1px solid #D1E8DC; color: #0F172A; }
  tr:nth-child(even) td { background: #F0FAF4; }
  .columns { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  .columns pre { font-size: 0.48em; }
  blockquote { border-left: 4px solid #00A859; background: #E8F5EE; padding: 8px 14px; margin: 6px 0; border-radius: 0 8px 8px 0; color: #0F172A; font-style: normal; font-size: 0.84em; }
  ul { line-height: 1.55; margin: 5px 0; }
  li { margin-bottom: 2px; font-size: 0.86em; }
  br { display: block; margin: 4px 0; content: ''; }
  section::after { color: #848688; font-size: 0.75em; }
  .ai-box { background: #FFF8E1; border: 2px solid #F59E0B; border-radius: 10px; padding: 12px 16px; margin: 10px 0; font-size: 0.85em; }
  .ai-box h3 { color: #B45309; margin: 0 0 6px 0; font-size: 0.95em; }
  .ai-tip { background: #FFF8E1; border-left: 4px solid #F59E0B; padding: 8px 14px; margin: 8px 0; border-radius: 0 6px 6px 0; font-size: 0.82em; color: #78350F; }
  section img { display: block; margin: 6px auto; background: transparent; }
  section img.emoji, section img[data-marp-twemoji] { display: inline; margin: 0 2px; height: 1em; width: 1em; vertical-align: -0.12em; }
  pre code .hljs-attr, pre code .hljs-attribute, pre code .hljs-section, pre code .hljs-keyword, pre code .hljs-name, pre code .hljs-selector-tag { color: #7DD3FC; }
  pre code .hljs-string, pre code .hljs-bullet, pre code .hljs-symbol { color: #FCD34D; }
  pre code .hljs-comment, pre code .hljs-quote { color: #94A3B8; }
  pre code .hljs-number, pre code .hljs-literal, pre code .hljs-regexp { color: #F9A8D4; }
  pre code .hljs-variable, pre code .hljs-template-variable, pre code .hljs-meta, pre code .hljs-built_in, pre code .hljs-params { color: #A7F3D0; }
  pre code .hljs-title, pre code .hljs-type, pre code .hljs-class, pre code .hljs-function { color: #C4B5FD; }
  pre code .hljs-operator, pre code .hljs-punctuation, pre code .hljs-subst { color: #E2E8F0; }
---

# 🧮 Subnet, CIDR və Routing

## Bitlərdən hesab, qaydalardan yol seçimi

```
  192.168.0.45 /28   ──►  şəbəkə 192.168.0.32 ... 192.168.0.47     (hesab)

  Təyinat 10.40.1.15 ──►  [ routing cədvəli ]  ──►  ən dəqiq qayda qalib   (yol)
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 06 · Chapter 3 — Ünvanlama dərinləşməsi və Yönləndirmə

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Bit və bayt:** ikilik say sisteminin qısa girişi
- **Subnet mask bitlərlə:** `/24`, `/16`, `/28` əslində nə deməkdir
- **CIDR hesabı addım-addım:** şəbəkə ünvanı, broadcast, host sayı
- **Şəbəkəni bölmək:** `/24`-ü iki `/25`-ə, dörd `/26`-ya
- **RFC 1918:** private aralıqlar və niyə `172.16`–`172.31`
- **`ip route`:** cədvəli sətir-sətir oxumaq, **longest prefix match**, `default`, `ip route get`
- **Real ssenari:** Docker və VPN CIDR toqquşması, AWS VPC planı

> Sessiya 02-də `/24`-ün "ilk 24 bit şəbəkədir" demək olduğunu öyrəndin. Bu dərsdə **bitlərə baxıb hesablamağı** və **paketin yolunu seçməyi** öyrənirik.

---

## 💡 Niyə hesab bilmək lazımdır?

| Real vəziyyət | Hansı bilik lazımdır |
|---|---|
| AWS VPC üçün subnet planı | CIDR hesabı |
| Docker şəbəkəsi korporativ DB-ni "gizlədir" | routing cədvəli |
| VPN qoşulur, amma serverə çatmır | üst-üstə düşən CIDR |
| İki mühit arasında peering qurulur | **toqquşmayan** aralıqlar |
| "Bu IP mənim şəbəkəmdədir?" | mask hesabı |

> Yanlış CIDR planı **sonradan düzəltməsi ən çətin** səhvlərdəndir: şəbəkə artıq qurulub, hər şey ona bağlıdır. Ona görə hesabı əvvəldən düz öyrənirik.

---

## 💡 Bit və bayt: işıq açarları

Kompüter yalnız **iki vəziyyəti** bilir. **Bit** = bir işıq açarı: söndü (`0`) və ya yandı (`1`).

```
  1 bit   :  [0]                       2 vəziyyət   (0 və ya 1)
  2 bit   :  [0][1]                    4 vəziyyət   (00, 01, 10, 11)
  8 bit   :  [0][1][0][0][1][1][0][1]  256 vəziyyət (2 üstü 8)
```

- **8 bit = 1 bayt** (IP-də buna **oktet** də deyirik)
- 8 açarın **256** fərqli kombinasiyası var: `0`-dan `255`-ə qədər rəqəm
- Məhz buna görə IP-nin hər rəqəmi **0–255** arasındadır

> Dərsin qalan hissəsində bir sual verəcəyik: "bu 8 açarın hansıları şəbəkəyə, hansıları host-a aiddir?"

---

## 🔢 Bitləri rəqəmə çevirmək

Hər açarın **çəkisi** var. Soldan sağa: `128, 64, 32, 16, 8, 4, 2, 1`. **Yanan açarların çəkisini topla**.

| Çəki | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|---|---|---|---|---|---|---|---|---|
| `192` | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| `168` | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 |
| `15` | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 1 |

- `192` = 128 + 64 → `11000000`
- `168` = 128 + 32 + 8 → `10101000`
- `15` = 8 + 4 + 2 + 1 → `00001111`

> Bunu əzbərləmək lazım deyil. Lazım olanda `python3 -c "print(bin(168))"` yazarsan. Məqsəd **məntiqi** başa düşməkdir.

---

## 🎭 Mask bitlərlə: 1-lər şəbəkə, 0-lar host

IP **32 bitdir** (4 bayt). Mask da **32 bitdir**. Qayda sadədir:

- Maskda **1** olan yer = IP-nin həmin biti **şəbəkəyə** aiddir
- Maskda **0** olan yer = IP-nin həmin biti **host-a** aiddir
- `/24` = maskda **ilk 24 bit 1-dir**, qalanı 0

```
  IP    192.168.0.15   11000000.10101000.00000000.00001111
  Mask  /24            11111111.11111111.11111111.00000000
                       └───── şəbəkə: 24 bit ────┘└─host──┘
```

> Analogiya: mask kağız üzərinə qoyulmuş **deşikli şablondur**. 1 olan yerlər "bu hissə şəbəkə adıdır", 0 olan yerlər "bu hissə ev nömrəsidir" deyir.

---

## 📏 `/24`, `/16`, `/28`: üç mask yan-yana

| Yazılış | Mask (ikilik) | Mask (onluq) | Host bitləri |
|---|---|---|---|
| `/24` | `11111111.11111111.11111111.`**`00000000`** | `255.255.255.0` | 8 |
| `/16` | `11111111.11111111.`**`00000000.00000000`** | `255.255.0.0` | 16 |
| `/28` | `11111111.11111111.11111111.`**`11110000`** | `255.255.255.240` | 4 |

- `/16` = **16 bit şəbəkə**, 16 bit host → böyük şəbəkə (65 536 ünvan)
- `/24` = **24 bit şəbəkə**, 8 bit host → orta (256 ünvan)
- `/28` = **28 bit şəbəkə**, 4 bit host → kiçik (16 ünvan)

> Prefiks (`/N`) **böyüdükcə** şəbəkə **kiçilir**: çünki host-a az bit qalır.

---

## 🧩 Oktet ortasında kəsilən mask

`/28`-də 4-cü oktetin **ilk 4 biti** 1-dir: `11110000`. Onluq: `128+64+32+16 = 240`.

| Prefiks | 4-cü oktet (ikilik) | Maskın son rəqəmi | Blok ölçüsü |
|---|---|---|---|
| `/25` | `10000000` | 128 | 128 |
| `/26` | `11000000` | 192 | 64 |
| `/27` | `11100000` | 224 | 32 |
| `/28` | `11110000` | 240 | 16 |
| `/29` | `11111000` | 248 | 8 |
| `/30` | `11111100` | 252 | 4 |

> **Blok ölçüsü = 256 − maskın son rəqəmi.** `/28` üçün `256 − 240 = 16`. Bu rəqəm növbəti slaydlarda hesabı sürətləndirir.

---

## 🔍 Şəbəkə ünvanı necə tapılır? (AND)

**AND** qaydası: nəticə `1` olur **yalnız ikisi də 1 olanda**. Qalan hallarda `0`. Mask 0 olan bitləri **sıfırlayır**.

`192.168.0.45/28` üçün yalnız son oktet dəyişir:

```
  IP     45  =  0010 1101
  Mask  240  =  1111 0000
  AND        =  0010 0000   = 32         → şəbəkə ünvanı 192.168.0.32
```

- Host bitləri (son 4) **hamısı 0** → şəbəkənin **ilk** ünvanı
- Host bitləri **hamısı 1** (`0010 1111` = 47) → **broadcast** ünvanı

> Yəni: **şəbəkə ünvanı** = host bitləri 0. **Broadcast** = host bitləri 1. Aralarındakılar cihazlara verilir.

---

## 🧮 Neçə host? Düstur

Host bitlərinin sayı `h = 32 − prefiks`. Ünvan sayı `2 üstü h`. **İki ünvan ayrılır**: şəbəkə ünvanı və broadcast.

| Prefiks | Host bitləri | Cəmi ünvan | Cihaza verilən |
|---|---|---|---|
| `/24` | 8 | 256 | **254** |
| `/25` | 7 | 128 | **126** |
| `/26` | 6 | 64 | **62** |
| `/27` | 5 | 32 | **30** |
| `/28` | 4 | 16 | **14** |
| `/30` | 2 | 4 | **2** |

> Düstur: **cihaz sayı = 2^(32−prefiks) − 2**. `/30` iki cihazlıq xəttdir (router-router linki). `/32` tək ünvandır (bir maşın).

---

## 🪜 Addım-addım: `192.168.0.45/28`

| Addım | Nə edirik | Nəticə |
|---|---|---|
| 1 | Host bitləri: `32 − 28` | 4 bit, **blok = 16** |
| 2 | Blokları yaz: 0, 16, 32, 48 ... | 45 **32–47** blokunda |
| 3 | Blokun ilk ünvanı = şəbəkə | **192.168.0.32** |
| 4 | Blokun son ünvanı = broadcast | **192.168.0.47** |
| 5 | Aralarındakılar = host | **.33 – .46** (14 ədəd) |

> Yoxla: `ipcalc 192.168.0.45/28` və ya `python3 -c "import ipaddress as i; n=i.ip_network('192.168.0.45/28', strict=False); print(n, n.broadcast_address)"`. AI-nin və öz hesabını **həmişə alətlə tutuşdur**.

---

## ✂️ `/24`-ü iki `/25`-ə bölmək

`192.168.0.0/24` → **bir bit** host-dan götürüb şəbəkəyə əlavə et (24-cü bit əvəzinə 25-ci bit). Son oktetin **ilk biti** hansı yarı olduğunu seçir:

```
  0xxxxxxx  →  .0   – .127        (ilk bit 0)
  1xxxxxxx  →  .128 – .255        (ilk bit 1)
```

| Subnet | Şəbəkə | Host aralığı | Broadcast |
|---|---|---|---|
| `192.168.0.0/25` | `192.168.0.0` | `.1` – `.126` | `192.168.0.127` |
| `192.168.0.128/25` | `192.168.0.128` | `.129` – `.254` | `192.168.0.255` |

> Hər yarıda **126** cihaz. Cəmi 254 yox, **252** oldu: çünki **hər subnet öz şəbəkə və broadcast ünvanını itirir**.

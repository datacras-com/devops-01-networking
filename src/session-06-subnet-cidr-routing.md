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

---

## ✂️ Daha çox bölmək: dörd `/26`

Hər əlavə bit subnet sayını **2 dəfə artırır**, hər subnet-in ölçüsünü **yarıya endirir**.

| Subnet | Şəbəkə | Host aralığı | Broadcast |
|---|---|---|---|
| `192.168.0.0/26` | `.0` | `.1` – `.62` | `.63` |
| `192.168.0.64/26` | `.64` | `.65` – `.126` | `.127` |
| `192.168.0.128/26` | `.128` | `.129` – `.190` | `.191` |
| `192.168.0.192/26` | `.192` | `.193` – `.254` | `.255` |

- 2 bit əlavə = `2² = 4` subnet, hər biri 64 ünvan
- Sərhədlər **blok ölçüsünün** (64) misallarıdır: 0, 64, 128, 192

> Yoxla: `python3 -c "import ipaddress as i; print(*i.ip_network('192.168.0.0/24').subnets(2), sep='\n')"`

---

## 🏠 RFC 1918: private aralıqlar

| Aralıq | Nə qədər | Harada görürsən |
|---|---|---|
| `10.0.0.0/8` | `10.0.0.0` – `10.255.255.255` | böyük şirkət, **AWS VPC** |
| `172.16.0.0/12` | `172.16.0.0` – `172.31.255.255` | **Docker** (`172.17.0.0/16`) |
| `192.168.0.0/16` | `192.168.0.0` – `192.168.255.255` | ev, ofis |

**Niyə `172.16`-dan `172.31`-ə qədər?** `/12` = ikinci oktetin yalnız ilk 4 biti sabitdir:

```
  172.16 = 172.0001 0000        172.31 = 172.0001 1111
           └─sabit─┘└─sərbəst─┘
  172.32 = 172.0010 0000   →   sabit hissə fərqlidir, bu artıq PUBLIC-dir
```

---

## 🗺️ Routing: paket hara getsin?

**Problem:** Cihazın bir neçə yolu var (interfeys, gateway, VPN). Hər paket üçün **birini seçmək** lazımdır.

**Həll:** **routing cədvəli** = "hansı təyinat hansı yolla" qaydalar siyahısı. Analogiya: **yol işarələri**.

| Həyatda | Şəbəkədə |
|---|---|
| Öz küçəm | eyni şəbəkə, **birbaşa** çatıram |
| "Başqa şəhərlər →" işarəsi | `default` qaydası, **gateway**-ə ver |
| Xüsusi işarə ("Aeroport →") | **xüsusi marşrut** (VPN, daxili şəbəkə) |

> Sessiya 02-dəki "eyni şəbəkədəyik?" sualının cavabı **məhz bu cədvəldədir**. Cədvəli `ip route` (qısa `ip r`) göstərir.

---

## 📖 `ip route`: bütöv cədvəl

```bash
$ ip route
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100        # <- 1
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown        # <- 2
192.168.0.0/24 dev enp0s3 proto kernel scope link src 192.168.0.15 metric 100    # <- 3
```

| Sətir | Qayda | Mənası |
|---|---|---|
| 1 | `default via 192.168.0.1` | qalan **hər şey** gateway-ə |
| 2 | `172.17.0.0/16 dev docker0` | **Docker** şəbəkəsi, `docker0` interfeysində |
| 3 | `192.168.0.0/24 dev enp0s3` | **öz şəbəkəm**, birbaşa |

> Bu **nümunə çıxışdır**. 3 sətir = 3 qayda. Sətir **sırası** qərar vermir: seçimi **prefiks uzunluğu** edir (bir neçə slayd sonra).

---

## 🔬 Sətir 1: `default` qaydası

```
  default   via 192.168.0.1   dev enp0s3   proto dhcp   src 192.168.0.15   metric 100
```

| Hissə | Mənası |
|---|---|
| `default` | "başqa heç bir qayda uyğun gəlməyən **hər şey**" (`0.0.0.0/0`) |
| `via 192.168.0.1` | paketi bu **gateway**-ə ver |
| `dev enp0s3` | bu interfeysdən çıxar |
| `proto dhcp` | qaydanı **DHCP** yazıb (`kernel`, `static` də ola bilər) |
| `src 192.168.0.15` | paketdə **göndərən** kimi bu IP yazılır |
| `metric 100` | eyni qayda iki dəfə olsa, **kiçik rəqəm** üstündür |

> `default via ...` sətrindəki IP = **default gateway** (Sessiya 02). İnternetə gedən hər şey buradan çıxır.

---

## 🔬 Sətir 3: öz şəbəkəm üçün qayda

```
  192.168.0.0/24   dev enp0s3   proto kernel   scope link   src 192.168.0.15
```

| Hissə | Mənası |
|---|---|
| `192.168.0.0/24` | şəbəkə: `.0` – `.255` |
| `via` **yoxdur** | gateway lazım deyil, **birbaşa** çatırıq (ARP ilə) |
| `proto kernel` | qaydanı **kernel özü** yaratdı (IP verəndə avtomatik) |
| `scope link` | yalnız **eyni şəbəkə seqmentində** keçərli |

> **Əsas fərq:** 1-ci sətirdə `via` var (gateway), 3-cü sətirdə **yoxdur** (birbaşa). Sətir 2 (`docker0`, `linkdown`) eyni quruluşdadır: Docker öz şəbəkəsini yaradıb, hazırda aktiv konteyner yoxdur.

---

## 🚶 Gəzinti: paket yolunu seçir

Host `192.168.0.15` iki ünvana paket göndərir. Cədvəl əvvəlki slayddakıdır.

```
  A) Təyinat 192.168.0.21               B) Təyinat 93.184.216.34
  1. 192.168.0.0/24-ə uyğundur? HƏ      1. 192.168.0.0/24-ə uyğundur? YOX
  2. 172.17.0.0/16-ya uyğundur? YOX     2. 172.17.0.0/16-ya uyğundur? YOX
  3. Qalib: 192.168.0.0/24              3. Qalib: default (hər şeyə uyğundur)
  4. via yoxdur -> BİRBAŞA              4. via 192.168.0.1 -> GATEWAY-ə ver
  5. ARP ilə .21-in MAC-ı tapılır       5. ARP ilə .1-in MAC-ı tapılır
```

- A: paket birbaşa serverə gedir, **MAC = serverin**
- B: paket gateway-ə gedir, **MAC = router-in**, **IP = 93.184.216.34** qalır

> Sessiya 02-dəki qayda: **IP = nəhayət haraya**, **MAC = növbəti addım kimə**. Routing cədvəli "növbəti addımı" seçir.

---

## 🏆 Bir neçə qayda uyğun gəlsə? Longest prefix match

**Problem:** `10.40.1.15` üçün həm geniş, həm dar qayda uyğun gələ bilər. Hansı seçilir?

**Həll:** **ən dəqiq** (ən uzun prefiksli) qayda qalib gəlir. Analogiya: kuryerə iki təlimat var: "Bakıya get" və "Bakı, Nizami küç. 5-ə get". **Dəqiq ünvan** qalib gəlir.

| Prefiks | Əhatə etdiyi ünvan | Dəqiqlik |
|---|---|---|
| `/0` (`default`) | **hamısı** | ən zəif |
| `/16` | 65 536 ünvan | orta |
| `/24` | 256 ünvan | yaxşı |
| `/32` | **bir** ünvan | ən dəqiq |

> Qayda iki mərhələlidir: 1) **uyğun gələnləri** seç, 2) onların arasında **prefiksi ən uzun** olanı götür. `metric` yalnız **eyni prefiks** arasında hakimdir.

---

## 🧪 Longest prefix match nümunələri

Hipotetik cədvəl (**nümunə**):

```
  default         via 192.168.0.1                       # /0
  10.40.0.0/16    via 192.168.0.50                      # /16
  10.40.1.0/24    via 192.168.0.60                      # /24
```

| Təyinat | Uyğun gələnlər | Qalib | Gateway |
|---|---|---|---|
| `10.40.1.15` | default, /16, /24 | `/24` | `192.168.0.60` |
| `10.40.9.9` | default, /16 | `/16` | `192.168.0.50` |
| `1.1.1.1` | yalnız default | `default` | `192.168.0.1` |
| `10.41.0.1` | yalnız default | `default` | `192.168.0.1` |

> Sonuncuya diqqət: `10.41.x.x` `10.40.0.0/16`-ya **aid deyil** (ikinci oktet 40 deyil). Prefiks **bitlərlə** tutuşdurulur, ona görə əvvəlki hesab burada lazımdır.

---

## ❓ `ip route get`: kernel-dən birbaşa soruş

**Problem:** Cədvəl uzundur, longest prefix match-i əllə hesablamaq yorucudur.
**Həll:** `ip route get <IP>` kernel-ə "bu ünvana hansı yolla gedərdin?" deyə soruşur. **Paket göndərmir**, yalnız qərarı göstərir.

```bash
$ ip route get 192.168.0.21
192.168.0.21 dev enp0s3 src 192.168.0.15 uid 1000          # <- via YOXDUR: birbaşa

$ ip route get 93.184.216.34
93.184.216.34 via 192.168.0.1 dev enp0s3 src 192.168.0.15 uid 1000   # <- via VAR: gateway
```

| Hissə | Mənası |
|---|---|
| `via 192.168.0.1` | paket **gateway**-ə gedir (yoxdursa birbaşa) |
| `dev enp0s3` | hansı interfeysdən çıxacaq |
| `src 192.168.0.15` | göndərən kimi hansı IP yazılacaq |

> Çıxışın sonunda `uid ...` və `cache` sətri ola bilər. Vacib olan **ilk sətirdir**. Bu, ən faydalı diaqnostika əmrlərindən biridir.

---

## 🧰 `ip route get` və `default` olmayanda

```bash
$ ip route get 127.0.0.1
local 127.0.0.1 dev lo src 127.0.0.1 uid 1000       # <- "local" = özümə

$ ip route get 8.8.8.8          # default silinibsə:
RTNETLINK answers: Network is unreachable           # <- bu IP üçün QAYDA YOXDUR
```

| Cavab | Mənası |
|---|---|
| `... dev X src ...` | öz şəbəkəm, **birbaşa** |
| `... via G dev X ...` | **gateway** vasitəsilə |
| `local ...` | ünvan **mənimdir** |
| `Network is unreachable` | uyğun qayda **yoxdur** (çox vaxt `default` yoxdur) |

> `Network is unreachable` ilə `100% packet loss` fərqlidir: birincisi **paket heç çıxmır** (route yoxdur), ikincisi **çıxır, amma cavab gəlmir** (Sessiya 04-dəki `timeout`).

---

## 🔥 Ssenari: Docker VPN-dən DB-ni "oğurlayır"

**Vəziyyət:** Server korporativ DB-yə (`172.20.5.10`) gateway `192.168.0.1` vasitəsilə çatırdı. Dünən yeni `docker compose` qalxdı. İndi **DB-yə çatmaq olmur**. Başqa serverlərdə problem yoxdur.

```
  Əvvəl:  172.20.5.10 --> default --> gateway 192.168.0.1 --> DB     [OK]

  İndi:   172.20.5.10 --> 172.20.0.0/16 dev br-3f2a... (Docker)      [X]
                          (yeni Docker şəbəkəsi eyni aralığı götürüb)
```

**Niyə?** Docker `compose` üçün **avtomatik** `172.20.0.0/16` seçdi. Bu, `default`-dan **daha dəqiqdir** (`/16` > `/0`), deməli longest prefix match-ə görə **qalib gəlir**. Paket DB-yə yox, **boş Docker bridge-ə** gedir.

> VPN-də eyni hadisə: **evdəki** `192.168.0.0/24` ilə **ofisdəki** `192.168.0.0/24`. Lokal qayda həmişə qalib gəlir, ofis şəbəkəsi **görünməz** olur.

---

## 🔎 Xətanı necə tapırıq?

```bash
$ ip route
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
172.20.0.0/16 dev br-3f2a1b9c4d5e proto kernel scope link src 172.20.0.1  # <- YENİ!

$ ip route get 172.20.5.10
172.20.5.10 dev br-3f2a1b9c4d5e src 172.20.0.1 uid 1000    # <- gateway yox, Docker bridge

$ docker network inspect <şəbəkə> | grep Subnet
        "Subnet": "172.20.0.0/16"
```

- 1-ci addım: `ip route` ilə **yeni sətri** tap
- 2-ci addım: `ip route get` ilə **təsdiqlə**
- 3-ci addım: `docker network inspect` ilə **kim yaratdığını** tap

> Çıxışlar **nümunədir**: interfeys adı (`br-...`) hər dəfə fərqlidir.

---

## 🛠️ Həll: Docker-ə aralığı özün de

**Variant 1: konkret şəbəkə** (`docker-compose.yml`):

```yaml
networks:
  default:
    ipam:
      config:
        - subnet: 10.200.1.0/24
```

**Variant 2: Docker-in bütün yeni şəbəkələri** (`/etc/docker/daemon.json`):

```json
{ "default-address-pools": [ { "base": "10.200.0.0/16", "size": 24 } ] }
```

> ⚠️ `daemon.json` dəyişəndən sonra Docker **restart** olur və konteynerlər dayana bilər (əvvəl faylın **ehtiyat nüsxəsini** götür). **Mövcud** şəbəkələr köhnə aralığı saxlayır: onları `docker compose down` ilə silib yenidən qaldır. Geri qaytarma: faylı köhnə halına qaytar, Docker-i restart et.

---

## ☁️ AWS VPC: subnet planı

| Subnet | CIDR | Verilən IP | Məqsəd |
|---|---|---|---|
| public-a | `10.40.0.0/24` | 251 | load balancer, NAT |
| public-b | `10.40.1.0/24` | 251 | ikinci AZ |
| db-a | `10.40.4.0/24` | 251 | verilənlər bazası |
| db-b | `10.40.5.0/24` | 251 | ikinci AZ |
| private-a | `10.40.16.0/20` | 4091 | tətbiq, **EKS pod-ları** |
| private-b | `10.40.32.0/20` | 4091 | ikinci AZ |

> VPC `10.40.0.0/16` (AWS-də VPC `/16`–`/28` arası olur). AWS **hər subnet-də 5 ünvanı özü saxlayır**, ona görə `/24` = 251. Private böyükdür: Kubernetes-də **hər pod** bir IP götürür, kiçik subnet tez dolur.

---

## 📋 Planlama qaydaları: toqquşma olmasın

Bu aralıqların **hamısı bir-birindən ayrı** olmalıdır:

| Yer | CIDR |
|---|---|
| Ofis şəbəkəsi | `192.168.0.0/24` |
| VPN | `10.8.0.0/24` |
| AWS VPC | `10.40.0.0/16` |
| Docker (default) | `172.17.0.0/16` |
| K8s Pod / Service | `10.244.0.0/16` / `10.96.0.0/12` |

- **Sənədləşdir:** bir cədvəldə "kim hansı aralığı işlədir"
- Peering, VPN, Transit birləşməsindən **əvvəl** yoxla

> `10.96.0.0/12` = `10.96.0.0` – `10.111.255.255`. Bu, VPC-nin `10.40.x.x` aralığı ilə **kəsişmir**, amma `10.100.0.0/16` seçsən kəsişərdi.

---

## 🔀 İki CIDR üst-üstə düşürmü?

**Qayda:** iki blok **kəsişirsə** (ortaq ünvan varsa), toqquşma var. Aralığı yaz və müqayisə et.

```
  10.40.0.0/20   →  10.40.0.0  –  10.40.15.255
  10.40.8.0/21   →  10.40.8.0  –  10.40.15.255        # <- /20-nin içindədir
```

- `/21` (8–15) `/20`-nin (0–15) **tam içindədir** → **toqquşur**
- `10.40.0.0/20` ilə `10.40.16.0/20` → `0–15` və `16–31` → **toqquşmur**

```bash
$ python3 -c "import ipaddress as i; print(i.ip_network('10.40.0.0/20').overlaps(i.ip_network('10.40.8.0/21')))"
True
```

> Böyük blok kiçiyi **ya tam əhatə edir, ya heç kəsişmir**: CIDR blokları yarım-yarımçıq üst-üstə düşmür.

---

## 🧪 Lab 6A — cədvəli oxu (sudo lazım deyil)

Interfeys adını `ip -br a`-dan götür.

```bash
$ ip -br a                       # interfeys və IP-lər
$ ip route                       # bütün cədvəl: neçə sətir? hansı Docker-dir?
$ ip route show default          # yalnız default gateway
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100
$ ip route get 192.168.0.1       # eyni şəbəkə -> via YOXDUR
$ ip route get 93.184.216.34     # internet    -> via 192.168.0.1
$ ip route get 127.0.0.1         # özüm        -> local ... dev lo
```

**Gözlənilən:** ilk sorğuda `via` yoxdur, ikincidə var, üçüncüdə `local`. Əgər `via` olan IP-nin gateway-in IP-si ilə eyni olduğunu görürsənsə, **cədvəli oxumağı bacarırsan**.

> Cavablar **nümunədir**: sənin interfeys adın və IP-n fərqli ola bilər.

---

## 🧪 Lab 6B — longest prefix match canlı

> ⚠️ **Yalnız VM-də.** `default`-a toxunmuruq, SSH kəsilməz. **Geri qaytarma:** `sudo ip link del dummy0` (IP və route özü silinir). Reboot da hər şeyi təmizləyir.

```bash
$ sudo ip link add dummy0 type dummy        # saxta interfeys
$ sudo ip link set dummy0 up
$ sudo ip addr add 10.77.0.1/24 dev dummy0
$ ip route | grep 10.77                     # kernel özü /24 qaydasını yaratdı
10.77.0.0/24 dev dummy0 proto kernel scope link src 10.77.0.1
$ sudo ip route add 10.77.0.0/16 via 10.77.0.2 dev dummy0
$ ip route get 10.77.0.9                    # /24 qalib: gateway YOXDUR
$ ip route get 10.77.5.5                    # /16 qalib: via 10.77.0.2
$ ip route get 10.78.0.1                    # hər ikisi uyğun gəlmir: default
```

**Gözlənilən:** 1-ci sorğuda `dev dummy0` (via yox), 2-cidə `via 10.77.0.2 dev dummy0`, 3-cüdə `via 192.168.0.1 dev enp0s3`.

---

## 🧪 Lab 6C — toqquşmanı özün yarat

Əvvəlki lab-ın davamı. Docker ssenarisini təhlükəsiz təqlid edirik. ⚠️ VM-də `172.20.x.x` artıq işlənirsə, `172.28.0.0/16` seç.

```bash
$ ip route get 172.20.5.10                  # əvvəl: via 192.168.0.1 dev enp0s3
$ sudo ip link add dummy1 type dummy && sudo ip link set dummy1 up
$ sudo ip addr add 172.20.0.1/16 dev dummy1
$ ip route get 172.20.5.10                  # indi: dev dummy1 src 172.20.0.1
$ sudo ip link del dummy1                   # təmizlə
$ sudo ip link del dummy0                   # Lab 6B-dən qalan
$ ip route get 172.20.5.10                  # yenə: via 192.168.0.1
```

**Gözlənilən:** eyni ünvan üçün yol **yalnız bir interfeys əlavə olunanda** dəyişdi. Bu, Docker/VPN toqquşmasının **kiçik modelidir**.

> İndi `ip route get`-in niyə "ilk baxılacaq əmr" olduğunu görürsən: **dəyişikliyin təsirini dərhal göstərir**.

---

## ⚠️ Tipik səhvlər

| Səhv | Niyə olur | Düzgünü |
|---|---|---|
| `.0` və `.255`-i host-a vermək | şəbəkə/broadcast olduğunu unudurlar | `/24`-də `.1`–`.254` |
| `/24` və `/16`-nı qarışdırmaq | "eyni şəbəkədir" sanırsan | mask-a **həmişə** bax |
| `172.16`–`172.31` yerinə hər `172.x` | `/12`-ni bilmirlər | `172.32` **public**-dir |
| Docker/K8s default aralığına güvənmək | toqquşma sonra çıxır | aralığı **özün** seç |
| `ip route del default` SSH-da | əlaqə kəsilir | konsoldan və ya `replace` |
| Gateway başqa şəbəkədə | `via` IP-yə çatılmır | gateway **eyni şəbəkədə** olmalıdır |

> `ip route add`/`del` ilə edilən dəyişiklik **müvəqqətidir**: reboot-da itir. Daimi etmək üçün şəbəkə konfiqinə (`netplan`, `NetworkManager`) yazılır.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"`192.168.0.77/26` üçün şəbəkə ünvanını, broadcast-ı və host aralığını bitlərlə addım-addım hesabla."*
- *"VPC üçün `10.40.0.0/16`-dan dev/stage/prod, hər birində public/private/db subnet ilə CIDR planı cədvəl şəklində hazırla. K8s Pod `10.244.0.0/16` ilə toqquşmasın."*
- *"`ip route` və `ip route get 172.20.5.10` çıxışımı izah et, paket hansı interfeysdən niyə çıxır? (çıxışı yapışdır)"*

</div>

<div class="ai-tip">💡 AI CIDR hesabında <b>səhv edə bilər</b>. Cavabı həmişə <code>ipcalc</code> və ya Python <code>ipaddress</code> ilə yoxla.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **Bit / bayt** | 8 bit = 1 bayt, `0`–`255`. IP = 32 bit |
| **Mask** | 1-lər **şəbəkə**, 0-lar **host** (`/24` = 24 ədəd 1) |
| **Hesab** | blok = `2^(32−N)`. Şəbəkə = blokun ilki, broadcast = sonu |
| **Host sayı** | `2^(32−N) − 2` |
| **Bölmək** | +1 bit = 2 subnet, hər biri yarı ölçüdə |
| **RFC 1918** | `10/8`, `172.16/12` (`.16`–`.31`), `192.168/16` |
| **`ip route`** | `default via` = gateway, `via` yoxdur = birbaşa |
| **Longest prefix** | **ən uzun** prefiks qalib, `default` ən sonda |
| **`ip route get`** | "bu IP-yə hansı yolla?" sualına cavab |

**Növbəti dərs:** Sessiya 07 — **DNS: adı IP-yə çevirmək**.

---

## ✍️ Tapşırıqlar

1. **İkilik:** `10.40.1.15`-i hər oktet üçün 8 bitlə yaz (əl ilə, sonra `bin()` ilə yoxla).
2. **Mask:** `/27` üçün maskın onluq yazılışını, blok ölçüsünü və cihaz sayını tap.
3. **Hesab:** `192.168.0.100/26` üçün şəbəkə, broadcast və host aralığını hesabla. `ipcalc` ilə yoxla.
4. **Bölmə:** `192.168.0.0/24`-ü **8 bərabər** subnet-ə böl (hansı prefiks?). İlk üçünü yaz.
5. **RFC 1918:** hansı private-dir? `172.31.255.1` · `172.32.0.1` · `10.255.0.1` · `192.169.1.1`
6. **Longest prefix:** cədvəldə `default`, `10.40.0.0/16`, `10.40.1.0/24` var. `10.40.1.15`, `10.40.200.9`, `10.41.0.1` hansı qayda ilə gedir?
7. **Lab:** Lab 6B-ni et. `ip route get` ilə üç ünvanın yolunu yaz.
8. **Plan:** `10.40.0.0/16`-dan 3 mühit üçün subnet planı hazırla. Pod `10.244.0.0/16`, VPN `10.8.0.0/24` ilə toqquşmadığını göstər.

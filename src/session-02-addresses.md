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

# 🏷️ Şəbəkədə ünvanlar

## MAC · IP · subnet mask · gateway · ARP · DHCP

```
  MAC   08:00:27:4e:66:a1        ← cihazın "şəxsiyyət vəsiqəsi nömrəsi"
  IP    192.168.0.15 / 24           ← cihazın "yaşayış ünvanı" və şəbəkənin ölçüsü
  GW    192.168.0.1                 ← şəbəkədən çıxış qapısı
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 02 · Chapter 1 — Şəbəkə təməli

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **MAC ünvanı** nədir və niyə IP-dən başqa ikinci ünvan var
- **IP ünvanı** necə oxunur: 4 rəqəm, ikilik görünüş
- **Subnet mask və `/24`**: "mənim şəbəkəm" haradan haradadır
- **Private və public IP**: nə fərqi var və niyə hamı `192.168.x.x` işlədir
- **Default gateway**: şəbəkədən çıxış qapısı
- **ARP**: IP-dən MAC-ı necə tapırıq
- **DHCP**: cihaz IP-ni haradan alır
- `ip a`, `ip neigh`, `ip r` çıxışlarını **sətir-sətir oxumaq**

> Sessiya 01-də "paket ünvana göndərilir" demişdik. Bu dərsdə **o ünvanların nə olduğunu** öyrənirik.

---

## 🪪 Niyə iki ünvan var? MAC və IP

| | MAC ünvanı | IP ünvanı |
|---|---|---|
| Analogiya | **şəxsiyyət vəsiqəsi nömrəsi** | **yaşayış ünvanı** |
| Kim verir | istehsalçı (şəbəkə kartına yazılır) | şəbəkə (DHCP və ya əllə) |
| Dəyişir? | adətən **dəyişmir** | **yer dəyişəndə dəyişir** |
| Harada işləyir | yalnız **eyni şəbəkədə** | **şəbəkələr arasında** |
| Kim istifadə edir | **switch** | **router** |

> Eyni şəbəkədəki cihazlar bir-birini **MAC** ilə tapır. Başqa şəbəkəyə çatmaq üçün **IP** lazımdır. Hər ikisi lazımdır və **bir paketdə yan-yana yazılır** (başlıqda).

---

## 🔌 MAC ünvanı

```
  08 : 00 : 27 : 4e : 66 : a1
  └──────┬─────┘ └─────┬─────┘
    istehsalçı        cihazın
    (ilk 3 bayt)      özünün nömrəsi
```

- **48 bit** = 6 bayt, 16-lıq (hex) yazılır
- **İlk 3 bayt istehsalçını** göstərir (məsələn, `08:00:27` = VirtualBox)
- Hər **şəbəkə interfeysinin** öz MAC-ı var (Wi-Fi kartı, kabel kartı, virtual kart)
- Yalnız **eyni şəbəkə seqmentində** məna daşıyır: paket router-dən keçəndə MAC dəyişir

```bash
$ ip link show enp0s3
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ... state UP ...
    link/ether 08:00:27:4e:66:a1 brd ff:ff:ff:ff:ff:ff
```

> `link/ether` = MAC. `ff:ff:ff:ff:ff:ff` = **hamıya** göndərmək ünvanı (broadcast). MAC proqramla **dəyişdirilə bilər**, ona görə onu təhlükəsizlik üçün təkbaşına etibarlı saymırıq.

---

## 🔢 IP ünvanı (IPv4)

**4 rəqəm**, hər biri **0–255**, nöqtə ilə ayrılır. Əslində **32 bit**-dir (4 × 8 bit):

```
  192      .   168      .    0       .   31
  11000000 . 10101000 . 00000000 . 00011111
  └─ 8 bit ─┘                         (hər hissə = 1 bayt = "oktet")
```

| Fakt | Dəyər |
|---|---|
| Ümumi ölçü | **32 bit** |
| Hər oktet | 0–255 (2⁸ = 256 dəyər) |
| Mümkün ünvan sayı | təxminən **4,3 milyard** (2³²) |

> 4,3 milyard dünyanın cihazlarına **çatmır**. Buna görə **private IP + NAT** və **IPv6** var (sonrakı slaydlar və Sessiya 05).

---

## 🧩 IP iki hissədən ibarətdir

Analogiya: **küçə adı + ev nömrəsi**.

```
  192.168.0.   31
  └───┬────┘  └┬┘
   ŞƏBƏKƏ     HOST
  (küçə)    (ev nömrəsi)
```

- **Şəbəkə hissəsi**: eyni şəbəkədəki **bütün cihazlarda eynidir**
- **Host hissəsi**: şəbəkə daxilində **hər cihaz üçün fərqlidir**
- Nə qədər hissənin şəbəkə, nə qədərinin host olduğunu **subnet mask** deyir

> Eyni küçədə ev nömrələri fərqlidir, küçə adı eynidir. Şəbəkədə də belədir.

---

## 🎭 Subnet mask və `/24`

Mask IP-nin **neçə bitinin şəbəkəyə aid olduğunu** göstərir.

| Yazılış | Mask | Şəbəkə hissəsi | Ünvan sayı |
|---|---|---|---|
| `/8` | `255.0.0.0` | ilk **1** oktet | 16 777 216 |
| `/16` | `255.255.0.0` | ilk **2** oktet | 65 536 |
| `/24` | `255.255.255.0` | ilk **3** oktet | **256** |

```
  192.168.0.31/24   →  ilk 24 bit (192.168.0.) = şəbəkə
                       son 8 bit  (.31)         = host
```

- `/24` = "ilk **24 biti** şəbəkədir" (`CIDR` yazılışı)
- Şəbəkənin **ilk ünvanı** (`.0`) şəbəkənin adıdır, **sonuncusu** (`.255`) broadcast-dır. Cihazlara **254** ünvan qalır
- Dərin hesab və bölmələr üçün: **Sessiya 06 (Subnet, CIDR və routing)**

---

## ❓ "Eyni şəbəkədəyik?" — ən vacib sual

Cihaz paket göndərməzdən əvvəl bunu yoxlayır. `/24` üçün **ilk 3 oktetə bax**:

| Mənim IP | Təyinat IP | Eyni şəbəkə? | Nə edəcək |
|---|---|---|---|
| `192.168.0.31/24` | `192.168.0.50` | ✅ hə (`192.168.0.x`) | **birbaşa** göndər |
| `192.168.0.31/24` | `192.168.1.50` | ❌ yox (`192.168.1.x`) | **gateway**-ə ver |
| `192.168.0.31/24` | `8.8.8.8` | ❌ yox | **gateway**-ə ver |

> Cavab **hə**-dirsə: MAC ilə birbaşa çatdır (**ARP** lazımdır). **Yox**-dursa: **gateway**-ə ver. Bu qərarı verən qaydalar siyahısı `ip r`-dir (Linux S26B).

---

## 🏠 Private və public IP

| | Private IP | Public IP |
|---|---|---|
| Harada işləyir | **yalnız daxili şəbəkədə** | **internetdə** |
| Unikaldır? | yox, **hər yerdə təkrarlana bilər** | **bəli**, dünyada tək |
| Analogiya | binadakı **otaq nömrəsi** | **küçə ünvanı** |
| Kim verir | sən özün / DHCP | provayder (ISP) |

**Private aralıqlar** (RFC 1918):

| Aralıq | Ölçü |
|---|---|
| `10.0.0.0/8` | `10.x.x.x` |
| `172.16.0.0/12` | `172.16.x.x` – `172.31.x.x` |
| `192.168.0.0/16` | `192.168.x.x` |

> Ev və ofis şəbəkələrinin hamısı `192.168.x.x` işlədə bilər, çünki internetdə **görünmürlər**. Daxili ünvanın internetə çıxması üçün **NAT** lazımdır (Sessiya 05).

---

## ⚠️ Xüsusi ünvanlar

| Ünvan | Mənası | Harada görürsən |
|---|---|---|
| `127.0.0.1` | **özüm** (`localhost`, loopback) | `curl localhost` |
| `0.0.0.0` | **bütün interfeyslər** (server dinləyəndə) | `ss`: `0.0.0.0:80` |
| `169.254.x.x` | **DHCP tapılmadı**, sistem özü ünvan verdi | "IP aldı, amma işləmir" |
| `x.x.x.255` (`/24`-də) | **broadcast**: şəbəkədəki hamıya | `ip a`: `brd 192.168.0.255` |

> **`169.254.x.x` görürsənsə**, əsas səbəb **DHCP serverinə çata bilməməkdir**. Bu, ən sürətli diaqnostika ipuclarından biridir.

---

## 🚪 Default gateway

**Gateway** = öz şəbəkəndən **kənara çıxmaq üçün qapı**. Adətən **router-in** daxili şəbəkədəki IP-sidir (çox vaxt `.1` və ya `.2`).

```bash
$ ip r | head -1
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100
```

- `default via 192.168.0.1` = "**başqa heç bir qayda uyğun gəlməyən hər şey** `192.168.0.1`-yə get"
- **Gateway mənim şəbəkəmdə olmalıdır.** Məsələn, `192.168.0.15/24` üçün gateway `192.168.0.x` olmalıdır
- Gateway yanlışdırsa: **yerli şəbəkə işləyir, internet işləmir**

> Ətraflı: **Linux kursu, Sessiya 26B (`ip r`)**. Orada bütün `ip r` qaydalarını oxuyuruq.

---

## 🔎 ARP — IP-dən MAC-ı necə tapırıq?

**Problem:** IP-ni bilirəm (`192.168.0.1`), amma **eyni şəbəkədə çatdırma MAC ilə** olur. MAC-ı necə tapım?

```
  Noutbuk (192.168.0.31)                         Router (192.168.0.1)
        │                                              │
        │ ① ARP sorğusu (HAMIYA, broadcast):           │
        │   "192.168.0.1 kimdir? Mənə MAC-ını de"      │
        │ ──────────────────────────────────────────►  │
        │                                              │
        │ ② ARP cavabı (yalnız mənə):                  │
        │   "Mənəm, MAC-ım 52:54:00:12:35:02"          │
        │ ◄──────────────────────────────────────────  │
        │                                              │
        │ ③ İndi paketi bu MAC ilə göndərirəm          │
```

> **ARP yalnız eyni şəbəkədə** işləyir. İnternetdəki serverin MAC-ını **soruşmuruq**, gateway-in MAC-ını soruşuruq.

---

## 📒 ARP cədvəli — `ip neigh`

Cihaz cavabları **yadda saxlayır**, hər dəfə soruşmur:

```bash
$ ip neigh
192.168.0.1 dev enp0s3 lladdr 52:54:00:12:35:02 REACHABLE
192.168.0.3 dev enp0s3 lladdr 52:54:00:12:35:03 STALE
```

| Hissə | Mənası |
|---|---|
| `192.168.0.1` | IP |
| `lladdr 52:54:…` | həmin IP-nin **MAC**-ı |
| `REACHABLE` | yaxınlarda **cavab verib** (canlıdır) |
| `STALE` | köhnəlib, lazım olanda **yenidən yoxlanacaq** |
| `FAILED` | **cavab gəlmədi** (cihaz yoxdur və ya bağlıdır) |

> `ip neigh`-də **`FAILED`** görürsənsə, həmin IP-yə **eyni şəbəkədə çatılmır**. Bu, "gateway-ə niyə çatmıram?" sualına cavab verir.

---

## 🔁 Başqa şəbəkəyə gedəndə IP və MAC nə olur?

`192.168.0.31` → `8.8.8.8` (başqa şəbəkədir, **gateway** vasitəsilə):

| Addım | Mənbə IP | Təyinat IP | Mənbə MAC | Təyinat MAC |
|---|---|---|---|---|
| Noutbuk → router | `192.168.0.31` | `8.8.8.8` | noutbukun | **router-in** |
| Router → növbəti router | `192.168.0.31`* | `8.8.8.8` | router-in | **növbəti router-in** |

```
  IP  ─────  son təyinatı göstərir (yol boyu EYNİ qalır)
  MAC ─────  yalnız NÖVBƏTİ addımı göstərir (HƏR HOP-da dəyişir)
```

> **Qayda:** IP = "**nəhayət haraya**", MAC = "**növbəti addım kimə**". *Ev routerində **NAT** mənbə IP-ni dəyişir. Bunu **Sessiya 05**-də açacağıq.

---

## 📬 DHCP — IP-ni haradan alırıq?

Noutbuku şəbəkəyə qoşanda **heç kim IP yazmır**. Cihaz **DHCP serverindən** (adətən router) **icarəyə** alır.

```
  Cihaz                                          DHCP server
    │
    │ ① DISCOVER: "Kimsə IP verə bilər?" (hamıya)
    │ ────────────────────────────────────────────────►
    │ ② OFFER:    "Sənə 192.168.0.31 verirəm"
    │ ◄────────────────────────────────────────────────
    │ ③ REQUEST:  "Bəli, 192.168.0.31-i istəyirəm"
    │ ────────────────────────────────────────────────►
    │ ④ ACK:      "Təsdiq. 24 saatlığa sənindir"
    │ ◄────────────────────────────────────────────────
```

- **DHCP nə verir:** **IP**, **mask**, **gateway**, **DNS** server, **icarə müddəti** (lease)
- İcarə bitəndə cihaz **yeniləyir**
- **Serverlər** adətən **sabit (static)** IP alır, çünki ünvanı dəyişməməlidir

---

## 📖 `ip a` çıxışını sətir-sətir oxu

```bash
$ ip a show enp0s3
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP
    link/ether 08:00:27:4e:66:a1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.15/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86212sec preferred_lft 86212sec
    inet6 fe80::a00:27ff:fe4e:66a1/64 scope link
```

| Hissə | Mənası |
|---|---|
| `enp0s3`, `state UP` | interfeys adı, **qalxıb** |
| `link/ether 08:00:27:…` | **MAC** ünvanı |
| `inet 192.168.0.15/24` | **IPv4** ünvanı və **mask** |
| `brd 192.168.0.255` | şəbəkənin **broadcast** ünvanı |
| `dynamic` | IP **DHCP ilə** alınıb (`static` yox) |
| `valid_lft 86212sec` | **icarənin** qalan vaxtı |
| `inet6 fe80::…` | **IPv6** ünvanı (`fe80` = yalnız yerli şəbəkə) |

> ⚠️ Bu **nümunə çıxışdır** (`192.168.0.0/24` şəbəkəsi). Sənin interfeys adın, IP və MAC-ın fərqli olacaq.

---

## 🌐 IPv6 — qısa tanışlıq

IPv4 ünvanları **bitib** (4,3 milyard çatmır). IPv6 bunun həllidir.

| | IPv4 | IPv6 |
|---|---|---|
| Ölçü | 32 bit | **128 bit** |
| Yazılış | `192.168.0.31` | `2001:db8::1` |
| Ünvan sayı | ~4,3 milyard | **astronomik** (2¹²⁸) |
| Köhnə mexanizm | ARP | **NDP** |

- `fe80::…` = **link-local**, yalnız **eyni şəbəkədə** işləyir (hər interfeysdə avtomatik var)
- Praktikada bu gün **IPv4 hələ əsasdır**, çoxu sistem hər ikisini **birlikdə** işlədir (dual-stack)
- Bu kursda **IPv4**-ə fokuslanırıq

---

## 🧪 Lab 2 — öz ünvanlarını tap

Ubuntu VM-də (interfeys adını `ip -br a`-dan götür):

```bash
$ ip link show enp0s3 | grep link/ether      # MAC
    link/ether 08:00:27:4e:66:a1 brd ff:ff:ff:ff:ff:ff

$ ip -4 a show enp0s3 | grep inet            # IPv4 və mask
    inet 192.168.0.15/24 brd 192.168.0.255 scope global dynamic enp0s3

$ ip r | head -1                             # gateway
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100

$ ip neigh                                   # ARP cədvəli (əvvəl boş ola bilər)
$ ping -c 1 192.168.0.1                         # gateway-ə bir paket
$ ip neigh                                   # indi gateway-in MAC-ı görünür
192.168.0.1 dev enp0s3 lladdr 52:54:00:12:35:02 REACHABLE
```

> `ping` **ARP-nı işə salır**: paketi göndərməzdən əvvəl cihaz gateway-in MAC-ını soruşur. Bu səbəbdən **`ping`-dən sonra** `ip neigh`-də yeni sətir görünür.

---

## 🪜 Ünvan problemləri: simptom → səbəb

| Simptom | Ehtimal olunan səbəb | Yoxla |
|---|---|---|
| **IP yoxdur** (`inet` sətri yoxdur) | DHCP işləmir, interfeys düşüb | `ip -br a`, kabel/Wi-Fi |
| **`169.254.x.x`** | DHCP serverinə **çatmadı** | router/DHCP işləyir? |
| Yerli şəbəkə var, **internet yox** | **gateway yoxdur və ya yanlışdır** | `ip r` → `default via` |
| `ip neigh` → **`FAILED`** | həmin IP **eyni şəbəkədə cavab vermir** | cihaz açıqdır? IP düzgündür? |
| Bəzən qoşulur, bəzən yox | **IP toqquşması** (iki cihaz eyni IP) | `ip neigh`-də MAC dəyişir |
| "Eyni şəbəkədir" sanır, **çatmır** | **mask yanlışdır** | `/24` yoxsa `/16`? |

> Qayda: **əvvəl IP və mask, sonra gateway, sonra ARP**. Növbəti dərslərdə yuxarıya doğru gedəcəyik.

---

## 🛠️ DevOps-da bu ünvanlar harada çıxır?

| Mühit | Ünvan nümunəsi | Nəyə diqqət et |
|---|---|---|
| **Docker** | `172.17.0.0/16` (`docker0`) | private aralıq, ev şəbəkəsi ilə **toqquşa bilər** |
| **Kubernetes** | pod-lar üçün ayrıca CIDR | pod və node aralıqları **üst-üstə düşməməlidir** |
| **AWS VPC** | `10.0.0.0/16` kimi seçirsən | düzgün **ölçü** seçmək lazımdır |
| **VPN** | iki ofis eyni `192.168.0.0/24` | **toqquşma** → marşrut işləmir (Sessiya 06) |

> Ən çox rast gəlinən real problem: **iki şəbəkə eyni private aralığı işlədir** və birləşdirəndə (VPN, peering) marşrut çaşır. Ona görə şəbəkə qurmazdan əvvəl **ünvan planı** qurulur.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"`192.168.0.31/24` üçün şəbəkə ünvanını, broadcast-ı və istifadə oluna bilən ünvan sayını addım-addım hesabla."*
- *"`ip neigh` çıxışımı izah et: REACHABLE, STALE və FAILED nə deməkdir? (çıxışı yapışdır)"*
- *"MAC və IP ünvanı arasındakı fərqi poçt analogiyası ilə izah et. Paket router-dən keçəndə hansı dəyişir?"*

</div>

<div class="ai-tip">💡 AI-nin hesabını <b>özün yoxla</b>: mask hesabında bir bit səhvi bütün cavabı pozur. Nəticəni slayddakı cədvəllə tutuşdur.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **MAC** | cihazın fiziki ünvanı, **eyni şəbəkədə** işləyir, `link/ether` |
| **IP** | məntiqi ünvan, **32 bit**, şəbəkə hissəsi + host hissəsi |
| **Mask / `/24`** | neçə bit **şəbəkəyə** aiddir (`/24` = 256 ünvan) |
| **Private / public** | daxili (`10.`, `172.16–31.`, `192.168.`) / internet |
| **Gateway** | şəbəkədən çıxış qapısı (`default via`) |
| **ARP** | IP → MAC (`ip neigh`) |
| **DHCP** | IP, mask, gateway, DNS **icarəyə** verir (DORA) |
| **IP vs MAC yolda** | IP **eyni qalır**, MAC **hər hop-da dəyişir** |

**Növbəti dərs:** Sessiya 03 — **OSI və TCP/IP modeli**: bu ünvanlar hansı **qatlarda** işləyir.

---

## ✍️ Tapşırıqlar

1. **MAC:** Öz VM-nin MAC-ını tap. İlk 3 baytı (istehsalçı) yaz.
2. **IP:** `ip -4 a` ilə IP-ni və maskı yaz. `/24`-dürsə, **şəbəkə ünvanını** və **broadcast-ı** hesabla.
3. **Eyni şəbəkədir?** (cavabı hər cüt üçün **hə/yox** yaz)
   - `192.168.0.15/24` və `192.168.0.20`
   - `192.168.1.5/24` və `192.168.2.5`
   - `172.16.5.4/16` və `172.16.200.9`
4. **Private, yoxsa public?** `8.8.8.8` · `172.20.1.1` · `172.32.0.1` · `192.168.5.5`
5. **ARP:** `ip neigh`-ə bax. `ping -c 1 <gateway>` et, sonra yenidən bax. **Nə dəyişdi?**
6. **DHCP:** 4 addımı (DISCOVER, OFFER, REQUEST, ACK) **düzgün sıra ilə** yaz və hər birini **öz sözünlə** izah et.
7. **Sual:** Paket `8.8.8.8`-ə gedəndə **təyinat IP** niyə dəyişmir, amma **təyinat MAC** hər addımda dəyişir?

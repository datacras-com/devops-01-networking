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

# 🌐 Şəbəkə nədir?

## Cihazlar, şəbəkə növləri və bir paketin yolu

<style>
.nw{display:block;width:100%;height:auto;margin:10px 0 6px;color:#0F172A}
.nw .n{font:600 17px 'Segoe UI',Arial,sans-serif;fill:#0F172A}
.nw .s{font:14px 'Segoe UI',Arial,sans-serif;fill:#848688}
.nw .l{font:14px 'Segoe UI',Arial,sans-serif;fill:#848688}
.nw .ic{fill:none;stroke:#0F172A;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
.nw .fillbg{fill:#F7F9F8}
.nw .led{fill:#00A859;stroke:none}
.nw .link{stroke:#848688;stroke-width:2;fill:none}
.nw .wan{stroke:#00A859;stroke-width:2;fill:none}
</style>
<svg class="nw" viewBox="0 0 1140 210" role="img" aria-label="Noutbuk switch və router vasitəsilə internetə, oradan data mərkəzinə və serverə qoşulur">
<g class="link"><line x1="122" y1="80" x2="218" y2="80"/><line x1="302" y1="80" x2="398" y2="80"/><line x1="942" y1="80" x2="1018" y2="80"/></g>
<g class="wan"><line x1="482" y1="76" x2="598" y2="76"/><line x1="482" y1="84" x2="598" y2="84"/><line x1="682" y1="76" x2="818" y2="76"/><line x1="682" y1="84" x2="818" y2="84"/></g>
<g class="l" text-anchor="middle"><text x="170" y="68">Ethernet</text><text x="350" y="68">Ethernet</text><text x="540" y="66" style="fill:#00A859">ISP / WAN</text><text x="750" y="66" style="fill:#00A859">VPN / uplink</text><text x="980" y="68">LAN</text></g>
<g transform="translate(80,80)" class="ic"><rect x="-22" y="-28" width="44" height="30" rx="3"/><rect x="-17" y="-23" width="34" height="20" rx="1"/><path d="M-30 10H30L26 20H-26Z"/></g>
<g transform="translate(260,80)" class="ic"><rect x="-38" y="-14" width="76" height="28" rx="3"/>
<rect x="-30" y="-2" width="7" height="9"/><rect x="-20" y="-2" width="7" height="9"/><rect x="-10" y="-2" width="7" height="9"/><rect x="0" y="-2" width="7" height="9"/><rect x="10" y="-2" width="7" height="9"/><rect x="20" y="-2" width="7" height="9"/>
<circle class="led" cx="-26" cy="-8" r="1.8"/><circle class="led" cx="-16" cy="-8" r="1.8"/><circle class="led" cx="-6" cy="-8" r="1.8"/></g>
<g transform="translate(440,80)" class="ic"><path d="M-18 -4L-26 -28M18 -4L26 -28"/><rect x="-38" y="-4" width="76" height="26" rx="3"/>
<circle class="led" cx="-26" cy="9" r="2"/><circle class="led" cx="-18" cy="9" r="2"/><line x1="8" y1="9" x2="30" y2="9"/><path d="M-10 -14a14 14 0 0 1 20 0"/></g>
<g transform="translate(640,80)" class="ic"><path class="fillbg" d="M-26 20H22A13 13 0 0 0 24 -6A19 19 0 0 0 -12 -12A15 15 0 0 0 -26 20Z"/>
<circle cx="-2" cy="4" r="9"/><ellipse cx="-2" cy="4" rx="4" ry="9"/><line x1="-11" y1="4" x2="7" y2="4"/></g>
<g transform="translate(880,80)" class="ic"><rect x="-34" y="-34" width="68" height="68" rx="4"/>
<rect x="-26" y="-26" width="52" height="14" rx="2"/><rect x="-26" y="-7" width="52" height="14" rx="2"/><rect x="-26" y="12" width="52" height="14" rx="2"/>
<circle class="led" cx="-19" cy="-19" r="2"/><circle class="led" cx="-19" cy="0" r="2"/><circle class="led" cx="-19" cy="19" r="2"/>
<line x1="-8" y1="-19" x2="18" y2="-19"/><line x1="-8" y1="0" x2="18" y2="0"/><line x1="-8" y1="19" x2="18" y2="19"/></g>
<g transform="translate(1060,80)" class="ic"><rect x="-24" y="-32" width="48" height="18" rx="2"/><rect x="-24" y="-9" width="48" height="18" rx="2"/><rect x="-24" y="14" width="48" height="18" rx="2"/>
<circle class="led" cx="-15" cy="-23" r="2"/><circle class="led" cx="-15" cy="0" r="2"/><circle class="led" cx="-15" cy="23" r="2"/>
<line x1="-6" y1="-23" x2="16" y2="-23"/><line x1="-6" y1="0" x2="16" y2="0"/><line x1="-6" y1="23" x2="16" y2="23"/></g>
<g text-anchor="middle">
<text class="n" x="80" y="148">Noutbuk</text><text class="s" x="80" y="166">istifadəçi cihazı</text>
<text class="n" x="260" y="148">Switch</text><text class="s" x="260" y="166">L2 giriş</text>
<text class="n" x="440" y="148">Router</text><text class="s" x="440" y="166">gateway</text>
<text class="n" x="640" y="148">İnternet</text><text class="s" x="640" y="166">ictimai şəbəkə</text>
<text class="n" x="880" y="148">Data mərkəzi</text><text class="s" x="880" y="166">kolokasiya</text>
<text class="n" x="1060" y="148">Server</text><text class="s" x="1060" y="166">tətbiq hostu</text>
</g>
</svg>

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 01 · Chapter 1 — Şəbəkə təməli

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Şəbəkə nədir** və niyə DevOps mühəndisi onu bilməlidir
- **Host, switch, router, modem, firewall** — hər cihaz nə edir
- **LAN, WAN, İnternet** — fərq nədir
- **Paket** nədir və məlumat necə gedir
- **Client və server** — rol, cihaz deyil
- **Bir sorğunun yolu:** brauzerdə sayt açanda nə baş verir (qısa xəritə)
- **Gecikmə və bant eni** — "yavaşdır" demək nə deməkdir
- **Diaqnostika nərdivanı** — "bağlantı yoxdur" olanda hardan başlamalı

> Bu dərs **təməldir**. Burada gördüyün hər söz sonrakı dərslərdə ətraflı açılacaq. **Hər şeyi indi yadda saxlamaq lazım deyil.**

---

## 🧩 Şəbəkə nədir?

**Şəbəkə** = iki və ya daha çox cihazın bir-biri ilə **məlumat mübadiləsi** etməsi üçün bir-birinə qoşulması.

| Şəhərdə | Şəbəkədə |
|---|---|
| Ev, ofis | **cihaz** (host) |
| Ev ünvanı | **IP ünvanı** |
| Yollar | **kabel**, **Wi-Fi** |
| Yol kəsişməsi, dövrə | **switch**, **router** |
| Göndərilən bağlama | **paket** |
| Poçt şöbəsi, yol göstəricisi | **gateway**, **routing cədvəli** |

> Bütün şəbəkə mövzusu bir suala gəlib çıxır: *"Bu cihazdan həmin cihaza məlumat necə çatır və yolda nə problem ola bilər?"*

---

## 🖥️ Host, client, server

**Host** = şəbəkəyə qoşulmuş **hər cihaz** və ünvanı olan hər şey: noutbuk, telefon, server, printer, virtual maşın, konteyner.

| Rol | Nə edir | Nümunə |
|---|---|---|
| **Client** | **sorğu göndərir** | brauzer, `curl`, `ssh`, `kubectl` |
| **Server** | sorğunu **qəbul edib cavab verir** | nginx, `sshd`, PostgreSQL, Kubernetes API |

- **Client və server cihaz deyil, roldur.** Eyni maşın həm biri, həm digəri ola bilər
- Nginx serveri PostgreSQL-ə qoşulanda **nginx client** rolundadır
- Tək maşında hər iki rolu sına bilərsən: `curl localhost` (client və server eyni maşındadır)

---

## 🔌 Şəbəkə cihazları — kim nə edir?

| Cihaz | Nə edir | Analogiya |
|---|---|---|
| **Switch** | **eyni şəbəkədəki** cihazları birləşdirir | bina daxilindəki koridor |
| **Router** | **fərqli şəbəkələri** birləşdirir, yol seçir | bina ilə küçə arasındakı qapı + yol göstəricisi |
| **Modem / ONT** | provayderin xətti ilə şəbəkəni uyğunlaşdırır | şəhərə çıxan magistral girişi |
| **Access Point (Wi-Fi)** | simsiz cihazları kabelli şəbəkəyə qoşur | binaya girişdəki qəbul masası |
| **Firewall** | qaydalara görə **keçirir** və ya **bağlayır** | təhlükəsizlik postu |

> **Ev routeri** bunların bir neçəsini birlikdə edir (router + switch + Wi-Fi + firewall). Ona görə şəbəkə cihazlarını qarışdırmaq asandır.

---

## 🔀 Switch vs Router — ən çox qarışdırılan cüt

| | Switch | Router |
|---|---|---|
| Nəyi birləşdirir | **eyni** şəbəkədəki cihazları | **fərqli** şəbəkələri |
| Qərarı nəyə görə verir | **MAC ünvanı** (cihazın fiziki ünvanı) | **IP ünvanı** |
| Nümunə | ofisdəki kompüterləri bir-birinə bağlamaq | ofis şəbəkəsini internetə çıxarmaq |

```
  [PC1]──┐
  [PC2]──┼── switch ──── router ──── İnternet
  [PC3]──┘    (daxili)    (şəbəkələr arası)
```

> Yadda saxla: **switch = daxili**, **router = xaricə çıxış**. MAC və IP-nin nə olduğunu **Sessiya 02**-də görəcəyik.

---

## 📏 LAN, WAN və İnternet

| Ad | Nədir | Nümunə |
|---|---|---|
| **LAN** (Local Area Network) | **bir yerdəki** şəbəkə | ev, ofis, data mərkəzin daxili şəbəkəsi |
| **WAN** (Wide Area Network) | **uzaq LAN-ları** birləşdirən şəbəkə | ofis ↔ data mərkəzi, provayderin xətti |
| **İnternet** | **minlərlə müstəqil şəbəkənin** bir-birinə qoşulması | hamısının birləşdiyi ümumi şəbəkə |

```
  LAN (ofis)  ═══ WAN / VPN ═══  LAN (data mərkəzi)
       │                              │
       └────────── İnternet ──────────┘
```

> **İnternet = şəbəkələrin şəbəkəsidir.** Onu bir şirkət idarə etmir: provayderlər (ISP) öz şəbəkələrini bir-birinə qoşur.

---

## 📦 Məlumat necə gedir? — paket

Məlumat **bütöv göndərilmir**, **kiçik hissələrə (paketlərə)** bölünür. Hər paket bir **bağlama** kimidir.

```
  ┌────────────── PAKET ──────────────┐
  │  BAŞLIQ (header)   │   MƏLUMAT    │
  │  kimdən · kimə ·   │  (payload)   │
  │  sıra nömrəsi      │              │
  └────────────────────┴──────────────┘
```

| Hissə | Analogiya |
|---|---|
| **Başlıq** | bağlamanın üstündəki **ünvan etiketi** |
| **Məlumat** | bağlamanın **içindəki** şey |
| Paketin ölçüsü | adətən **ən çox ~1500 bayt** (Ethernet-də) |

> Niyə bölünür? Böyük fayl bir hissəsi itəndə **yalnız itən hissəni** yenidən göndərmək olur və bir neçə nəfər eyni xətti **növbə ilə** istifadə edə bilir.

---

## 🧭 Bir paketin yolu

```
  💻 noutbuk
     │  1) Wi-Fi / kabel
  📡 ev routeri            ← "bu ünvan mənim şəbəkəmdə deyil, provayderə ötür"
     │  2)
  🌐 provayderin routeri
     │  3)
  🌍 İnternetin bir neçə routeri   ← hər biri paketi bir addım irəli ötürür
     │  4)
  🏢 data mərkəzin routeri
     │  5)
  🖥️ server
```

- Paketin keçdiyi **hər router** bir **"hop"** (addım) sayılır
- Hər router yalnız **növbəti addımı** bilir, bütün yolu yox
- Hop sayını `traceroute` ilə görəcəyik (**Sessiya 11**)

---

## 🤝 Client–Server: sorğu və cavab

```
  CLIENT                          SERVER
  (brauzer)  ──── sorğu ────►    (nginx)
             ◄─── cavab ────
```

| Client | Server | Sorğu |
|---|---|---|
| brauzer | nginx / web server | "bu səhifəni ver" |
| `ssh` | `sshd` | "mənə terminal ver" |
| `psql` | PostgreSQL | "bu sorğunu icra et" |
| `kubectl` | Kubernetes API server | "pod-ları göstər" |

> DevOps-da çox vaxt **client də, server də sənin tərəfindədir.** Problemin **hansı tərəfdə** olduğunu tapmaq işin yarısıdır.

---

## 🔁 Brauzerdə sayt açanda nə baş verir? (xəritə)

`https://tm.datacras.com` yazırsan. **6 addım:**

| # | Addım | Hansı dərsdə |
|---|---|---|
| 1 | **Ad → IP:** `tm.datacras.com` hansı IP-dir? (**DNS**) | Sessiya 07 |
| 2 | **Bu IP mənim şəbəkəmdədir?** Yox → **gateway**-ə ver | Sessiya 02 |
| 3 | Paket **router-lərdən** keçib serverə çatır | Sessiya 01 (indi), 06 |
| 4 | **TCP bağlantısı** qurulur | Sessiya 04 |
| 5 | **HTTPS sorğusu** göndərilir | Sessiya 03 və 08 |
| 6 | Server **cavab** qaytarır | — |

> Bu **xəritə**dir. İndi hər addımı bilmək lazım deyil. Kurs boyu hər addım ayrıca açılacaq və sonda bu cədvəli **özün izah edə biləcəksən**.

---

## ⏱️ Gecikmə və bant eni — "yavaşdır" nə deməkdir?

İki **fərqli** ölçü var. Çoxları onları qarışdırır.

| Ölçü | Sual | Vahid | Yol analogiyası |
|---|---|---|---|
| **Gecikmə** (latency) | məlumat **nə qədər tez** çatır? | **millisaniyə (ms)** | yolun **uzunluğu** |
| **Bant eni** (bandwidth) | **nə qədər** məlumat sığır? | **Mbit/s, Gbit/s** | yolun **eni** (zolaq sayı) |

- Gecikmə **yüksək**, bant eni **geniş** ola bilər (uzaq, amma geniş yol)
- Gecikmə **aşağı**, bant eni **dar** ola bilər (yaxın, amma dar yol)
- `ping` **gecikməni** ölçür (`time=12.4 ms`)
- Bant enini `iperf` ölçür (bonus, sonra)

> "Sayt yavaşdır" iki ayrı səbəbdən ola bilər: **uzaqdadır** (gecikmə) və ya **kanal doludur** (bant eni). Həlli fərqlidir.

---

## 🧪 Lab 1 — öz maşınında baxaq

Ubuntu VM-də (VirtualBox) terminal aç:

```bash
$ hostname                       # bu host-un adı
ubuntu-vm

$ ip -br a                       # interfeyslər və IP-lər (qısa forma)
lo      UNKNOWN  127.0.0.1/8 ::1/128
enp0s3  UP       192.168.0.15/24 fe80::a00:27ff:fe4e:66a1/64

$ ip r | head -2                 # default gateway hansıdır?
default via 192.168.0.1 dev enp0s3 proto dhcp src 192.168.0.15 metric 100
192.168.0.0/24 dev enp0s3 proto kernel scope link src 192.168.0.15
```

> ⚠️ Bu **nümunə çıxışdır** (bizim `192.168.0.0/24` şəbəkəsində adətən belədir). **Sənin interfeys adın və IP-lərin fərqli ola bilər**, bu normaldır. `ip -br a` və `ip r`-in **Sessiya 02**-də ətraflı izahı var.

---

## 🧪 Lab 1 — `ping` ilə yoxla

```bash
$ ping -c 3 192.168.0.1         # gateway-ə: öz şəbəkəm
64 bytes from 192.168.0.1: icmp_seq=1 ttl=… time=0.5 ms
...
3 packets transmitted, 3 received, 0% packet loss

$ ping -c 3 8.8.8.8             # internetə: Google-un açıq DNS-i
64 bytes from 8.8.8.8: icmp_seq=1 ttl=… time=14.2 ms
...
3 packets transmitted, 3 received, 0% packet loss
rtt min/avg/max/mdev = 13.8/14.2/14.9/0.4 ms
```

| Sətir | Mənası |
|---|---|
| `time=14.2 ms` | **gecikmə** (paket gedib qayıtdı) |
| `3 received, 0% packet loss` | **itki yoxdur** |
| `rtt min/avg/max` | gecikmənin ən az / orta / ən çox dəyəri |

> Gateway-ə gecikmə **çox azdır** (yaxındır), internetə **daha çoxdur** (uzaqdır). Bu fərq paketin **daha çox router-dən** keçməsindən gəlir.

---

## 🪜 Diaqnostika nərdivanı: "bağlantı yoxdur"

Aşağıdan yuxarı yoxla. **İlk uğursuz addım** problemin yerini göstərir.

| # | Yoxla | Əmr | Uğursuzdursa |
|---|---|---|---|
| 1 | Öz maşınımda IP var? | `ip -br a` | interfeys qalxmayıb |
| 2 | **Gateway**-ə çatıram? | `ping <gateway>` | yerli şəbəkə problemi |
| 3 | **İnternetə** çatıram? | `ping 8.8.8.8` | provayder / routing |
| 4 | **Ad** işləyir? | `ping google.com` | **DNS** problemi |

```
  1 ✅ → 2 ✅ → 3 ✅ → 4 ❌    →  "internet var, amma DNS işləmir"
```

> ⚠️ Bəzi serverlər `ping`-i **bağlayır** (firewall). `ping` uğursuzdursa, server **mütləq dayanıb** demək deyil, başqa üsulla yoxla (məsələn, `curl`).

---

## 🧠 DevOps niyə şəbəkə bilməlidir?

Real problemlər, hər biri **şəbəkə sualıdır**:

| Simptom | Şəbəkə sualı |
|---|---|
| "Sayt açılmır" | **DNS** düzgündür? Port açıqdır? |
| "Servis serverə qoşula bilmir" | **Firewall / security group** bağlayıb? |
| "Pod başqa pod-a çatmır" | **Routing / şəbəkə qaydası**? |
| "Deploy sonra yavaşlayıb" | **Gecikmə**, yoxsa **bant eni**? |
| "VPN qoşulur, amma sistem görünmür" | **IP aralığı toqquşur**? |

> Şəbəkəni bilməyən mühəndis problemi **təxmin edir**, bilən isə **addım-addım izləyir**. Bu modul sənə ikincisini öyrədəcək.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Switch ilə router-in fərqini 3 cümlə ilə, poçt analogiyası ilə izah et."*
- *"`ping -c 3 8.8.8.8` çıxışını sətir-sətir izah et: `ttl`, `time`, `packet loss` nə deməkdir?"*
- *"Mənim ev şəbəkəmdə hansı cihazlar var? Onların hər birinin rolunu cədvəllə ver."*

</div>

<div class="ai-tip">💡 AI-dən cavabı <b>əmrlə yoxla</b>: izah etdiyi şeyi öz VM-ində icra et və nəticə ilə tutuşdur. AI səhv edə bilər.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| Şəbəkə | cihazların bir-biri ilə məlumat mübadiləsi |
| Host | şəbəkədəki hər cihaz (client və ya server **rolu**) |
| Switch / Router | **daxili** bağlantı / **şəbəkələr arası** bağlantı |
| LAN / WAN / İnternet | bir yer / uzaq yerlər / şəbəkələrin şəbəkəsi |
| Paket | başlıq (kimdən-kimə) + məlumat |
| Gecikmə / bant eni | yolun **uzunluğu** / yolun **eni** |
| Nərdivan | IP → gateway → internet → ad |

**Növbəti dərs:** Sessiya 02 — **ünvanlar**: MAC, IP, subnet mask, gateway, ARP, DHCP.

---

## ✍️ Tapşırıqlar

1. **Öz maşının:** `ip -br a` işlət. Interfeys adını və IP-ni yaz. IP hansı şəbəkədədir (`/24` nə deməkdir, bilmirsənsə, yaz: "bilmirəm", Sessiya 02-də cavablandırılacaq)?
2. **Gateway:** `ip r` ilə `default via …` sətrini tap. Həmin IP-yə `ping -c 3` et. Orta gecikmə neçə ms-dir?
3. **Gecikmə fərqi:** `ping -c 4 8.8.8.8` və `ping -c 4 <gateway>` nəticələrini müqayisə et. Hansının gecikməsi böyükdür və **niyə**?
4. **Rəsm:** Evindəki internet qoşulmasını çək: noutbuk → … → internet. Hansı cihazlar var və hər birinin rolu nədir?
5. **Sual:** Switch ilə router arasındakı fərqi **öz sözlərinlə** 2 cümlə ilə yaz.

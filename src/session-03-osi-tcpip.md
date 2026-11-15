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

# 🧱 OSI və TCP/IP modeli

## Şəbəkə qatlarla necə işləyir?

```
  7  Application    ← proqram (HTTP, SSH, DNS)
  6  Presentation
  5  Session
  4  Transport      ← TCP / UDP, port
  3  Network        ← IP, router
  2  Data Link      ← MAC, switch
  1  Physical       ← kabel, Wi-Fi, siqnal
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 03 · Chapter 1 — Şəbəkə təməli

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Niyə şəbəkə qatlara bölünür**
- **OSI modeli:** 7 qat, hər qat nə edir
- **TCP/IP modeli:** praktikada işlətdiyimiz 4 qat, OSI ilə əlaqəsi
- **Encapsulation:** məlumat aşağı enəndə hər qat **başlıq** əlavə edir
- **Hansı cihaz hansı qata** baxır (switch, router, firewall, load balancer)
- **Hansı əmr hansı qatı yoxlayır**
- **"Problem hansı qatdadır?"** deyə düşünmək

> Sessiya 01-də cihazları, Sessiya 02-də ünvanları (MAC, IP) gördük. İndi onları **qatlara yerləşdiririk**: MAC hansı qatdadır, IP hansı qatdadır, port hansı qatdadır.

---

## 🧩 Niyə qatlar var?

**Mürəkkəb problemi hissələrə bölürük.** Məktub göndərməyə bənzəyir:

| Mərhələ | Kim edir | Nə bilir |
|---|---|---|
| Məktubu **yazırsan** | sən | məzmun |
| **Zərfə qoyub** ünvan yazırsan | sən | kimə |
| Poçt şöbəsi **şəhərlər arası** yol seçir | poçt | hansı şəhərə |
| Yerli kuryer **qapıya** çatdırır | kuryer | hansı bina |
| Maşın, qatar, təyyarə **daşıyır** | nəqliyyat | fiziki yol |

- **Hər qat yalnız öz işini bilir** və yalnız **yanındakı qatla** danışır
- Bir qatı **dəyişəndə** digərləri pozulmur: kabeldən Wi-Fi-a keçirsən, **brauzer eyni işləyir**
- **Diaqnostika asanlaşır:** "problem **hansı qatdadır**?" sualı axtarışı kiçildir

---

## 🏗️ OSI modeli — 7 qat

| # | Qat | Nə edir | Nümunə |
|---|---|---|---|
| 7 | **Application** | proqramın şəbəkə ilə danışdığı yer | HTTP, SSH, DNS |
| 6 | **Presentation** | format, şifrələmə, sıxma | TLS, JSON/ASCII |
| 5 | **Session** | əlaqə sessiyasını açıb-bağlamaq | sessiya idarəsi |
| 4 | **Transport** | proqramlar arası çatdırma, **port** | TCP, UDP |
| 3 | **Network** | şəbəkələr arası ünvan və yol | IP, router |
| 2 | **Data Link** | eyni şəbəkədə çatdırma, **MAC** | Ethernet, switch |
| 1 | **Physical** | fiziki siqnal, **bit** | kabel, Wi-Fi |

> **Yadda saxlamaq üçün** (1 → 7): *Please Do Not Throw Sausage Pizza Away*. Qatları həm **aşağıdan yuxarı** (1 → 7), həm də **yuxarıdan aşağı** say.

---

## 🔗 Qatlar bir-birinə necə yardım edir?

Hər qat **aşağıdakı qatın çatışmazlığını həll edir** və **yuxarıdakına xidmət göstərir**:

| Qat | Aşağıdakı qat nə verir | Bu qat nə əlavə edir |
|---|---|---|
| **1** Physical | — | **siqnalı daşıyır** (0 və 1), amma nə dediyini bilmir |
| **2** Data Link | siqnal | siqnalları **mənalı çərçivəyə** salır, **MAC** ilə qonşuya düzgün çatdırır |
| **3** Network | qonşuya çatdırma | **çoxlu şəbəkə** arasında yolu tapır (**IP**, router) |
| **4** Transport | cihaza çatdırma | **hansı proqrama** çatdırır (**port**), TCP ilə **etibarlı** edir |
| **5–7** | proqrama çatdırma | **proqramın dili** (HTTP, SSH, DNS) |

> IP **ev ünvanıdırsa**, MAC həmin evdə yaşayan **konkret şəxsin şəxsiyyət vəsiqəsidir**. Hər ikisi lazımdır: IP evi tapır, MAC isə evdə **dəqiq kimə** çatdığını göstərir.

---

## 🔌 Qat 1: Physical — fiziki qat

**Ən aşağı qat.** Kompüterin məlumatı **0 və 1-lərdən (bit)** ibarətdir. Bitlər kabeldə və ya havada birbaşa gedə bilmir, ona görə **fiziki siqnala** çevrilir.

- **Göndərərkən (bit → siqnal):** bitlər **elektrik impulsuna**, **işıq şüasına** və ya **radio dalğasına** çevrilir
- **Qəbul edərkən (siqnal → bit):** kabeldən və ya havadan gələn siqnal yenidən **0 və 1-ə** çevrilir

```
  Göndərən: 0 1 0 1 1 0 ──► [ siqnal ] ══ kabel / hava ══► [ siqnal ] ──► 0 1 0 1 1 0 :Qəbul edən
```

| Mühit | Siqnal | Nümunə |
|---|---|---|
| **Mis kabel** | elektrik impulsu | Ethernet kabeli |
| **Optik kabel** | işıq şüası | fiber internet |
| **Hava** | radio dalğası | Wi-Fi |

> Qat 1 **nə dediyini bilmir**, yalnız **daşıyır**.

---

## 🔌 Qat 1: Physical — necə yoxlayırıq?

**Tipik problem:** kabel çıxıb, Wi-Fi zəifdir, port işləmir.

```bash
$ ip link show enp0s3
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> ... state UP ...
```

- `LOWER_UP` = **fiziki əlaqə var** (kabel qoşulub)
- `NO-CARRIER` = **fiziki əlaqə yoxdur** (kabel çıxıb, switch söndürülüb)

> Ən sadə səbəb ən çox rast gəlinəndir. İnternet yoxdursa, **əvvəl kabelə və Wi-Fi-a bax**.

---

## 🔗 Qat 2: Data Link — frame və MAC

Qat 1 yalnız siqnal daşıyır. **Qat 2 həmin siqnalları mənalı formaya (çərçivəyə) salır və qarşı tərəfə düzgün çatdırır.**

**Əsas vəzifələr:**

- **Framing:** qat 3-dən gələn **paketi çərçivəyə (frame) qablaşdırır**, hər çərçivəyə **göndərən və alanın MAC ünvanını** yazır
- **MAC ilə ünvanlama:** məlumatın **hansı cihaza** gedəcəyi **MAC**-a baxılaraq müəyyən olunur
- **Xəta yoxlaması:** zədəli çərçivə **FCS** (Frame Check Sequence, çərçivə yoxlama ardıcıllığı) ilə aşkarlanıb atılır

```
  ┌──────────┬──────────┬──────┬─────────────┬─────┐
  │ Təyinat  │ Mənbə    │ Tip  │   MƏLUMAT   │ FCS │
  │ MAC (6B) │ MAC (6B) │ (2B) │ (IP paketi) │ (4B)│
  └──────────┴──────────┴──────┴─────────────┴─────┘
```

**Cihaz:** switch · **Protokol:** Ethernet, Wi-Fi, ARP (IP → MAC) · **Problem:** `ip neigh` → `FAILED`

> IP **ev ünvanıdırsa**, MAC həmin evdə yaşayan **şəxsin şəxsiyyət vəsiqəsidir**.

---

## 🌐 Qat 3: Network — IP və routing

Qat 2 yalnız **eyni şəbəkədəki qonşuya** çatdıra bilir. **Qat 3 bu məhdudiyyəti aşır:** **bir çox şəbəkə arasında** son təyinata gedən yolu tapır.

**Əsas vəzifələr:**

- **IP ünvanlama:** hər cihaza **məntiqi ünvan** verir (Sessiya 02)
- **Marşrutlaşdırma:** router paketi **növbəti addıma** ötürür. `ip r` qərar verir: **birbaşa**, yoxsa **gateway**-ə?

| Nə | Nümunə |
|---|---|
| Vahid | **paket** (packet) |
| Cihaz | **router** |
| Protokol | **IP**, **ICMP** (`ping`) |
| Tipik problem | **gateway yoxdur**, yanlış marşrut, IP toqquşması |

> **Qat 3 = "hara?"**: paketin yolu. `ping` bu qatı yoxlayır (ICMP işlədir). **`ping` işləyirsə, qat 3-ə qədər hər şey qaydasındadır.**

---

## 🚚 Qat 4: Transport — port, TCP və UDP

Qat 3 paketi **cihaza** çatdırır, amma cihazdakı **hansı proqrama** aid olduğunu bilmir. **Qat 4 məlumatı doğru proqrama çatdırır və lazım olsa etibarlı edir.**

**Əsas vəzifələr:**

- **Port ilə proqram seçimi:** `80` veb server, `22` SSH, `443` HTTPS. **IP** = hansı **bina**, **port** = binada hansı **otaq**
- **Böyük məlumatı bölmək:** TCP onu **segmentlərə** bölür, **sıra nömrəsi** verir, itəni **yenidən göndərir**
- **Seçim:** **TCP** (etibarlı, bağlantılı) və ya **UDP** (sürətli, bağlantısız)

| Nə | Nümunə |
|---|---|
| Vahid | **segment** (TCP) / **datagram** (UDP) |
| Tipik problem | port **bağlıdır** (`refused`), firewall port-u **bloklayır** |

> TCP və UDP-ni **Sessiya 04**-də ətraflı öyrənəcəyik. İndi bilmək kifayətdir: **transport qatı proqramı seçir**.

---

## 🖥️ Qat 4 və Qat 5–7: kuryer və məktubun dili

Poçt misalı: məktub göndərirsən.

| Qat | Poçtda kim | Nə edir |
|---|---|---|
| **4** Transport | **kuryer** (karqo) | qutunu **salamat çatdırır**. İçində resept, yoxsa tətil şəkli olduğunu **bilmir** |
| **5–7** Session, Presentation, Application | **məktubun dili və zərfi** | proqramların **bir-biri ilə necə danışdığını** müəyyən edir: dil (protokol), şifrələmə (TLS), əlaqənin idarəsi |

> **Qısası:** Qat 4 məlumatı mənasını bilmədən **daşıyan fəhlədir**. Qat 5–7 həmin məlumatı **proqramların başa düşdüyü dildə hazırlayan** şöbədir.

**"Praktikada üçü bir yerdə düşünülür" nə deməkdir?** Brauzerdə və ya `curl`-da xəta axtaranda 5, 6 və 7-ni ayrı-ayrı müzakirə etmirik. Hamısına bir ad veririk: **tətbiq səviyyəsi**.

---

## 🖥️ Qat 5, 6, 7: Session, Presentation, Application

| OSI qat | Nə | Nümunə |
|---|---|---|
| **7 Application** | proqramın protokolu | **HTTP**, **SSH**, **DNS**, **SMTP**, **DHCP** |
| **6 Presentation** | şifrələmə, format | **TLS** (HTTPS-in şifrəsi) |
| **5 Session** | sessiyanın idarəsi | giriş sessiyası, bağlantı bərpası |

- **Application qatı**: `curl`, brauzer, `ssh` danışdığı protokol **buradadır**
- Real protokollar **dəqiq bir qata sığmır** (məsələn, TLS 5-6-7 arasındadır). OSI **istinad modelidir**, sərt qayda deyil
- Praktikada çox vaxt **"qat 7"** = **proqram səviyyəsi**

> `curl` səhv verirsə (`403`, `502`) və `ping` işləyirsə, problem çox vaxt **qat 7-dədir**, yəni şəbəkədə yox, **tətbiqdədir**.

---

## 🧭 OSI niyə yetərli deyil?

- **OSI nəzəri modeldir** (1980-lər, ISO təşkilatı). Anlamaq üçün əladır, amma real sistemdə **ağır və mürəkkəbdir**
- **Real protokollar 7 qata dəqiq sığmır.** Məsələn, **TLS** OSI-də 6-cı qatdır, praktikada isə tətbiq qatının **içində** işləyir
- **TCP/IP** isə nəzəriyyə ilə yox, **"işləyirsə, deməli doğrudur"** prinsipi ilə real həyatda yaradılıb (ABŞ-ın DARPA layihəsi). **İnternet onun üzərində qurulub**

> **Qızıl qayda:** öyrənəndə və analiz edəndə **OSI (7 qat)**, sistem quranda və sazlayanda **TCP/IP (4 qat)** ilə düşünürük.

---

## 🧭 TCP/IP modeli: 4 qat və real protokollar

| TCP/IP qatı | OSI qarşılığı | Real protokollar |
|---|---|---|
| **Application** | 5, 6, 7 | **HTTP/HTTPS**, **DNS**, **SSH**, **TLS** |
| **Transport** | 4 | **TCP**, **UDP** |
| **Internet** | 3 | **IP** (IPv4/IPv6), **ICMP** (`ping`) |
| **Network Access** | 1, 2 | **Ethernet**, **Wi-Fi**, **MAC** ünvanları |

> Bəzi dərsliklərdə **5 qatlı** versiya da var (Physical və Data Link ayrı). Mahiyyət eynidir.

---

## 🎁 Encapsulation — hər qat başlıq əlavə edir

Göndərəndə məlumat **yuxarıdan aşağı** enir. **Hər qat öz başlığını** əlavə edir (zərfin içində zərf):

```
  Application:  [ HTTP: GET /index.html ]                    ← məlumat
  Transport:    [ TCP başlıq | HTTP məlumat ]                ← segment
  Network:      [ IP başlıq | TCP | HTTP ]                   ← paket
  Data Link:    [ Eth başlıq | IP | TCP | HTTP | FCS ]      ← frame
  Physical:     0101 0011 0100 … (bit)                       ← bit
```

| Qat | Başlığa nə yazır |
|---|---|
| **Transport** | **mənbə və təyinat portu**, sıra nömrəsi |
| **Network** | **mənbə və təyinat IP** |
| **Data Link** | **mənbə və təyinat MAC** |

> Qəbul edən **tərsinə** açır (**decapsulation**): frame → paket → segment → məlumat. Hər qat **öz başlığını oxuyub atır**.

---

## 🎬 Canlı: iki kompüter TCP/IP ilə necə danışır?

<style>
.enc{display:block;width:100%;height:auto;cursor:pointer;user-select:none}
.enc .pk{transition:transform .9s ease}
.enc .bk{transition:opacity .5s,transform .5s;transform-box:fill-box;transform-origin:center;opacity:0;transform:scale(.6)}
.enc .bx{fill:#fff;stroke:#C3E4D0;stroke-width:2;transition:fill .4s,stroke .4s}
.enc .dt{opacity:0;transition:opacity .4s}
.enc-t{font:700 12px "Segoe UI",Arial,sans-serif;fill:#fff;text-anchor:middle}
.enc-s{font:11px "Segoe UI",Arial,sans-serif;fill:#fff;text-anchor:middle}
.enc-l{font:600 13px "Segoe UI",Arial,sans-serif;fill:#848688}
.enc-h{font:700 16px "Segoe UI",Arial,sans-serif;fill:#0F172A;text-anchor:middle}
.enc-dh{font:700 19px "Segoe UI",Arial,sans-serif;fill:#00703A}
.enc-dl{font:15px "Segoe UI",Arial,sans-serif;fill:#0F172A}
@keyframes enc-flow{to{stroke-dashoffset:-24}}
.enc .ar{stroke-dasharray:8 4;animation:enc-flow .7s linear infinite}
.enc-nav{display:flex;justify-content:center;align-items:center;gap:14px;margin-top:6px}
.enc-nav button{font:700 16px "Segoe UI",Arial,sans-serif;padding:7px 20px;border-radius:8px;border:2px solid #00A859;background:#00A859;color:#fff;cursor:pointer}
.enc-nav button.sec{background:#fff;color:#00703A}
.enc-nav button:disabled{opacity:.4;cursor:default}
.enc-nav span{font:600 16px "Segoe UI",Arial,sans-serif;color:#848688;min-width:110px;text-align:center}
.enc[data-step="0"] .pk{transform:translate(349px,86px)}
.enc[data-step="1"] .pk{transform:translate(349px,86px)}
.enc[data-step="2"] .pk{transform:translate(349px,170px)}
.enc[data-step="3"] .pk{transform:translate(349px,254px)}
.enc[data-step="4"] .pk{transform:translate(349px,338px)}
.enc[data-step="5"] .pk{transform:translate(1069px,338px)}
.enc[data-step="6"] .pk{transform:translate(1069px,338px)}
.enc[data-step="7"] .pk{transform:translate(1069px,254px)}
.enc[data-step="8"] .pk{transform:translate(1069px,170px)}
.enc[data-step="9"] .pk{transform:translate(1069px,86px)}
.enc .pk{transform:translate(349px,86px)}
.enc[data-step="1"] .data,.enc[data-step="2"] .data,.enc[data-step="3"] .data,.enc[data-step="4"] .data,.enc[data-step="5"] .data,.enc[data-step="6"] .data,.enc[data-step="7"] .data,.enc[data-step="8"] .data,.enc[data-step="9"] .data{opacity:1;transform:scale(1)}
.enc[data-step="2"] .tcp,.enc[data-step="3"] .tcp,.enc[data-step="4"] .tcp,.enc[data-step="5"] .tcp,.enc[data-step="6"] .tcp,.enc[data-step="7"] .tcp{opacity:1;transform:scale(1)}
.enc[data-step="3"] .ip,.enc[data-step="4"] .ip,.enc[data-step="5"] .ip,.enc[data-step="6"] .ip{opacity:1;transform:scale(1)}
.enc[data-step="4"] .eth,.enc[data-step="5"] .eth{opacity:1;transform:scale(1)}
.enc[data-step="1"] #encA0{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="2"] #encA1{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="3"] #encA2{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="4"] #encA3{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="6"] #encB3{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="7"] #encB2{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="8"] #encB1{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="9"] #encB0{fill:#E8F5EE;stroke:#00A859}
.enc[data-step="0"] .d0{opacity:1}
.enc[data-step="1"] .d1{opacity:1}
.enc[data-step="2"] .d2{opacity:1}
.enc[data-step="3"] .d3{opacity:1}
.enc[data-step="4"] .d4{opacity:1}
.enc[data-step="5"] .d5{opacity:1}
.enc[data-step="6"] .d6{opacity:1}
.enc[data-step="7"] .d7{opacity:1}
.enc[data-step="8"] .d8{opacity:1}
.enc[data-step="9"] .d9{opacity:1}
</style>
<svg class="enc" id="encSvg" data-step="0" viewBox="0 0 1192 500" role="img" aria-label="Interaktiv diaqram: klik etdikcə məlumat göndərən kompüterdə qatlardan keçərək başlıq qazanır, qəbul edəndə başlıqlar çıxarılır">
<defs><marker id="encArr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0 0L10 5L0 10z" fill="#00A859"/></marker></defs>
<text class="enc-h" x="236" y="28">Kompüter A: göndərən (192.168.0.15)</text><text class="enc-h" x="956" y="28">Kompüter B: qəbul edən (192.168.0.21)</text>
<rect class="bx" id="encA0" x="16" y="44" width="440" height="64" rx="8"/>
<text class="enc-l" x="26" y="60">Application · data</text>
<rect class="bx" id="encA1" x="16" y="128" width="440" height="64" rx="8"/>
<text class="enc-l" x="26" y="144">Transport · segment</text>
<rect class="bx" id="encA2" x="16" y="212" width="440" height="64" rx="8"/>
<text class="enc-l" x="26" y="228">Internet · paket</text>
<rect class="bx" id="encA3" x="16" y="296" width="440" height="64" rx="8"/>
<text class="enc-l" x="26" y="312">Network Access · frame</text>
<rect class="bx" id="encB0" x="736" y="44" width="440" height="64" rx="8"/>
<text class="enc-l" x="746" y="60">Application · data</text>
<rect class="bx" id="encB1" x="736" y="128" width="440" height="64" rx="8"/>
<text class="enc-l" x="746" y="144">Transport · segment</text>
<rect class="bx" id="encB2" x="736" y="212" width="440" height="64" rx="8"/>
<text class="enc-l" x="746" y="228">Internet · paket</text>
<rect class="bx" id="encB3" x="736" y="296" width="440" height="64" rx="8"/>
<text class="enc-l" x="746" y="312">Network Access · frame</text>
<line class="ar" x1="36" y1="110" x2="36" y2="126" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="1156" y1="126" x2="1156" y2="110" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="36" y1="194" x2="36" y2="210" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="1156" y1="210" x2="1156" y2="194" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="36" y1="278" x2="36" y2="294" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="1156" y1="294" x2="1156" y2="278" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<line class="ar" x1="466" y1="338" x2="726" y2="338" stroke="#00A859" stroke-width="3" marker-end="url(#encArr)"/>
<text class="enc-l" x="596" y="304" text-anchor="middle">kabel / Wi-Fi</text>
<g class="pk"><g class="bk data"><rect x="-52" y="-18" width="104" height="36" rx="4" fill="#00A859"/><text class="enc-t" x="0.0" y="-3">HTTP</text><text class="enc-s" x="0.0" y="12">GET /index.html</text></g><g class="bk tcp"><rect x="-134" y="-18" width="78" height="36" rx="4" fill="#F59E0B"/><text class="enc-t" x="-95.0" y="-3">TCP</text><text class="enc-s" x="-95.0" y="12">41234 → 80</text></g><g class="bk ip"><rect x="-230" y="-18" width="92" height="36" rx="4" fill="#3B82F6"/><text class="enc-t" x="-184.0" y="-3">IP</text><text class="enc-s" x="-184.0" y="12">.15 → .21</text></g><g class="bk eth"><rect x="-318" y="-18" width="84" height="36" rx="4" fill="#8B5CF6"/><text class="enc-t" x="-276.0" y="-3">ETH</text><text class="enc-s" x="-276.0" y="12">MAC A → B</text></g><g class="bk eth"><rect x="56" y="-18" width="36" height="36" rx="4" fill="#8B5CF6"/><text class="enc-t" x="74.0" y="5">FCS</text></g></g>
<rect x="16" y="376" width="1160" height="118" rx="8" fill="#fff" stroke="#C3E4D0" stroke-width="2"/>
<text class="enc-dh dt d0" x="36" y="406">Başlayaq: Kompüter A, Kompüter B-yə web səhifə sorğusu göndərir</text><text class="enc-dl dt d0" x="36" y="434">A = 192.168.0.15 (brauzer), B = 192.168.0.21 (web server, port 80).</text><text class="enc-dl dt d0" x="36" y="458">Sorğu A-da yuxarıdan aşağı qatlardan keçərkən hər qat öz başlığını əlavə edəcək.</text><text class="enc-dl dt d0" x="36" y="482">Növbəti addım üçün düyməyə və ya diaqrama klikləyin.</text>
<text class="enc-dh dt d1" x="36" y="406">① Application qatı: proqram MƏLUMATI hazırlayır</text><text class="enc-dl dt d1" x="36" y="434">DATA = HTTP sorğusu: GET /index.html HTTP/1.1</text><text class="enc-dl dt d1" x="36" y="458">Brauzer nə istədiyini yazdı. Bu qatda hələ nə IP var, nə port: yalnız məlumat.</text>
<text class="enc-dh dt d2" x="36" y="406">② Transport qatı: TCP başlıq əlavə olunur → segment</text><text class="enc-dl dt d2" x="36" y="434">TCP başlıq: mənbə port 41234 → təyinat port 80</text><text class="enc-dl dt d2" x="36" y="458">Port = cihazdakı hansı proqrama çatdırmaq lazımdır (80 = web server). Sıra nömrəsi də burada yazılır.</text>
<text class="enc-dh dt d3" x="36" y="406">③ Internet qatı: IP başlıq əlavə olunur → paket</text><text class="enc-dl dt d3" x="36" y="434">IP başlıq: mənbə IP 192.168.0.15 → təyinat IP 192.168.0.21</text><text class="enc-dl dt d3" x="36" y="458">IP ünvanı paketi doğru cihaza çatdırır (Sessiya 02).</text>
<text class="enc-dh dt d4" x="36" y="406">④ Network Access qatı: Ethernet başlıq və FCS əlavə olunur → frame</text><text class="enc-dl dt d4" x="36" y="434">Ethernet başlıq: mənbə MAC 08:00:27:aa:bb:01 → təyinat MAC 08:00:27:cc:dd:02</text><text class="enc-dl dt d4" x="36" y="458">MAC yerli şəbəkədə cihazı tapır (təyinat MAC ARP ilə öyrənilir). FCS = frame zədələnməyib yoxlaması.</text>
<text class="enc-dh dt d5" x="36" y="406">⑤ Physical qat: frame bit-lərə (0 və 1) çevrilir və siqnal kimi gedir</text><text class="enc-dl dt d5" x="36" y="434">Kabeldə elektrik, optik kabeldə işıq, Wi-Fi-da radio dalğası.</text><text class="enc-dl dt d5" x="36" y="458">Bütün başlıqlar və məlumat birlikdə yoldadır.</text>
<text class="enc-dh dt d6" x="36" y="406">⑥ B alır: Network Access qatı Ethernet başlığı yoxlayır (decapsulation)</text><text class="enc-dl dt d6" x="36" y="434">Təyinat MAC 08:00:27:cc:dd:02 mənimkidir? Hə. FCS düzgündür? Hə.</text><text class="enc-dl dt d6" x="36" y="458">Ethernet başlıq və FCS çıxarılır. Qalan = IP paketi, yuxarı qata verilir.</text>
<text class="enc-dh dt d7" x="36" y="406">⑦ Internet qatı IP başlığı yoxlayır</text><text class="enc-dl dt d7" x="36" y="434">Təyinat IP 192.168.0.21 mənimkidir? Hə → IP başlıq çıxarılır.</text><text class="enc-dl dt d7" x="36" y="458">Qalan = TCP segmenti. Mənbə IP 192.168.0.15 cavab üçün yadda saxlanır.</text>
<text class="enc-dh dt d8" x="36" y="406">⑧ Transport qatı TCP başlığı yoxlayır</text><text class="enc-dl dt d8" x="36" y="434">Təyinat port 80: bu portu web server proqramı dinləyir.</text><text class="enc-dl dt d8" x="36" y="458">TCP başlıq çıxarılır, məlumat web serverə verilir. Mənbə port 41234 cavabın qayıdacağı yerdir.</text>
<text class="enc-dh dt d9" x="36" y="406">⑨ Application qatı: web server öz məlumatını alır</text><text class="enc-dl dt d9" x="36" y="434">GET /index.html HTTP/1.1 oxunur və cavab hazırlanır.</text><text class="enc-dl dt d9" x="36" y="458">Hər qat yalnız öz başlığını oxuyub atdı, server yalnız ilk yazılan DATA-nı gördü.</text><text class="enc-dl dt d9" x="36" y="482">Cavab eyni yolla tərsinə gedəcək (IP-lər və portlar yer dəyişir).</text>
</svg>
<div class="enc-nav"><button class="sec" id="encPrev" type="button">◀ Geri</button><span id="encCnt">Addım 0 / 9</span><button id="encNext" type="button">Növbəti addım ▶</button><button class="sec" id="encRst" type="button">↻ Yenidən</button></div>
<script>
(function(){var s=document.getElementById('encSvg'),p=document.getElementById('encPrev'),n=document.getElementById('encNext'),r=document.getElementById('encRst'),c=document.getElementById('encCnt'),k=0,M=9;
function u(){s.setAttribute('data-step',k);c.textContent='Addım '+k+' / '+M;p.disabled=k===0;n.disabled=k===M;}
function go(d){k=Math.max(0,Math.min(M,k+d));u();}
n.addEventListener('click',function(){go(1);});p.addEventListener('click',function(){go(-1);});r.addEventListener('click',function(){k=0;u();});
s.addEventListener('click',function(){if(k!==M){go(1);}else{k=0;u();}});
[n,p,r].forEach(function(b){b.addEventListener('keydown',function(e){e.stopPropagation();});});
u();})();
</script>

---

## 🌍 Fərqli şəbəkələr arasında: router-lər və internet

<style>
.nw{display:block;width:100%;height:auto;cursor:pointer;user-select:none}
.nw .pk{transition:transform .9s ease;transform:translate(90px,46px)}
.nw .ck{transition:opacity .5s;opacity:0}
.nw .bd{fill:#F0FAF4;stroke:#C3E4D0;stroke-width:2}
.nw .nd{fill:#fff;stroke:#C3E4D0;stroke-width:2;transition:fill .4s,stroke .4s}
.nw .dt{opacity:0;transition:opacity .4s}
.nw .ni{fill:none;stroke:#0F172A;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
.nw .ld{fill:#00A859;stroke:none}
.nw-t{font:700 12px "Segoe UI",Arial,sans-serif;fill:#fff;text-anchor:middle}
.nw-n{font:700 14px "Segoe UI",Arial,sans-serif;fill:#0F172A;text-anchor:middle}
.nw-s{font:12px "Segoe UI",Arial,sans-serif;fill:#848688;text-anchor:middle}
.nw-b{font:700 14px "Segoe UI",Arial,sans-serif;fill:#00703A}
.nw-m{font:14.5px Menlo,"SF Mono",Consolas,monospace;fill:#0F172A}
.nw-m.c{fill:#C2410C;font-weight:700}
.nw-m.g{fill:#848688;text-decoration:line-through}
.nw-tg{font:700 13px "Segoe UI",Arial,sans-serif}
.nw-dh{font:700 19px "Segoe UI",Arial,sans-serif;fill:#00703A}
.nw-dl{font:15px "Segoe UI",Arial,sans-serif;fill:#0F172A}
.nw-hd{font:700 13px "Segoe UI",Arial,sans-serif;fill:#848688}
@keyframes nw-flow{to{stroke-dashoffset:-24}}
.nw .ln{stroke:#00A859;stroke-width:3;stroke-dasharray:8 4;animation:nw-flow .7s linear infinite}
.nw-nav{display:flex;justify-content:center;align-items:center;gap:14px;margin-top:6px}
.nw-nav button{font:700 16px "Segoe UI",Arial,sans-serif;padding:7px 20px;border-radius:8px;border:2px solid #00A859;background:#00A859;color:#fff;cursor:pointer}
.nw-nav button.sec{background:#fff;color:#00703A}
.nw-nav button:disabled{opacity:.4;cursor:default}
.nw-nav span{font:600 16px "Segoe UI",Arial,sans-serif;color:#848688;min-width:110px;text-align:center}
.nw[data-step="0"] .pk{transform:translate(90px,46px)}
.nw[data-step="0"] .d0{opacity:1}
.nw[data-step="1"] .pk{transform:translate(90px,46px)}
.nw[data-step="1"] .d1{opacity:1}
.nw[data-step="2"] .pk{transform:translate(340px,46px)}
.nw[data-step="2"] .d2{opacity:1}
.nw[data-step="3"] .pk{transform:translate(340px,46px)}
.nw[data-step="3"] .d3{opacity:1}
.nw[data-step="4"] .pk{transform:translate(603px,46px)}
.nw[data-step="4"] .d4{opacity:1}
.nw[data-step="5"] .pk{transform:translate(840px,46px)}
.nw[data-step="5"] .d5{opacity:1}
.nw[data-step="6"] .pk{transform:translate(1090px,46px)}
.nw[data-step="6"] .d6{opacity:1}
.nw[data-step="7"] .pk{transform:translate(340px,46px)}
.nw[data-step="7"] .d7{opacity:1}
.nw[data-step="8"] .pk{transform:translate(90px,46px)}
.nw[data-step="8"] .d8{opacity:1}
.nw[data-step="1"] .k-eth,.nw[data-step="2"] .k-eth,.nw[data-step="3"] .k-eth,.nw[data-step="4"] .k-eth,.nw[data-step="5"] .k-eth,.nw[data-step="7"] .k-eth,.nw[data-step="8"] .k-eth{opacity:1}
.nw[data-step="1"] .k-ip,.nw[data-step="2"] .k-ip,.nw[data-step="3"] .k-ip,.nw[data-step="4"] .k-ip,.nw[data-step="5"] .k-ip,.nw[data-step="7"] .k-ip,.nw[data-step="8"] .k-ip{opacity:1}
.nw[data-step="1"] .k-tcp,.nw[data-step="2"] .k-tcp,.nw[data-step="3"] .k-tcp,.nw[data-step="4"] .k-tcp,.nw[data-step="5"] .k-tcp,.nw[data-step="7"] .k-tcp,.nw[data-step="8"] .k-tcp{opacity:1}
.nw[data-step="1"] .k-http,.nw[data-step="2"] .k-http,.nw[data-step="3"] .k-http,.nw[data-step="4"] .k-http,.nw[data-step="5"] .k-http,.nw[data-step="6"] .k-http,.nw[data-step="7"] .k-http,.nw[data-step="8"] .k-http{opacity:1}
.nw[data-step="1"] #nwNA{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="2"] #nwNR1{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="3"] #nwNR1{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="4"] #nwNR2{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="5"] #nwNR3{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="6"] #nwNS{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="7"] #nwNR1{fill:#E8F5EE;stroke:#00A859}
.nw[data-step="8"] #nwNA{fill:#E8F5EE;stroke:#00A859}
</style>
<svg class="nw" id="nwSvg" data-step="0" viewBox="0 0 1192 500" role="img" aria-label="Interaktiv diaqram: paket ev şəbəkəsindən router-lər və internet vasitəsilə başqa şəbəkədəki serverə gedir; hər hop-da Ethernet başlıq dəyişir, NAT mənbə IP və portu dəyişir, serverin IP-si isə eyni qalır">
<rect class="bd" x="14" y="6" width="416" height="190" rx="10"/><rect class="bd" x="446" y="6" width="314" height="190" rx="10" style="fill:#EEF4FF;stroke:#C7D7F5"/><rect class="bd" x="776" y="6" width="402" height="190" rx="10"/>
<text class="nw-hd" x="26" y="24">Ev şəbəkəsi 192.168.0.0/24</text><text class="nw-hd" x="458" y="24">İnternet (ISP router-ləri)</text><text class="nw-hd" x="788" y="24">Server şəbəkəsi 93.184.216.0/24</text>
<line class="ln" x1="138" y1="112" x2="292" y2="112"/>
<line class="ln" x1="388" y1="112" x2="555" y2="112"/>
<line class="ln" x1="651" y1="112" x2="792" y2="112"/>
<line class="ln" x1="888" y1="112" x2="1042" y2="112"/>
<rect class="nd" id="nwNA" x="44" y="74" width="92" height="76" rx="10"/>
<rect class="nd" id="nwNR1" x="294" y="74" width="92" height="76" rx="10"/>
<rect class="nd" id="nwNR2" x="557" y="74" width="92" height="76" rx="10"/>
<rect class="nd" id="nwNR3" x="794" y="74" width="92" height="76" rx="10"/>
<rect class="nd" id="nwNS" x="1044" y="74" width="92" height="76" rx="10"/>
<g class="ni" transform="translate(90,106)"><rect x="-20" y="-26" width="40" height="28" rx="3"/><rect x="-15" y="-21" width="30" height="18"/><path d="M-27 8H27L23 18H-23Z"/></g>
<g class="ni" transform="translate(340,112)"><path d="M-16 -2L-22 -24M16 -2L22 -24"/><rect x="-32" y="-2" width="64" height="24" rx="3"/><circle class="ld" cx="-20" cy="10" r="2"/><circle class="ld" cx="-12" cy="10" r="2"/><line x1="6" y1="10" x2="24" y2="10"/></g>
<g class="ni" transform="translate(603,112)"><path d="M-16 -2L-22 -24M16 -2L22 -24"/><rect x="-32" y="-2" width="64" height="24" rx="3"/><circle class="ld" cx="-20" cy="10" r="2"/><circle class="ld" cx="-12" cy="10" r="2"/><line x1="6" y1="10" x2="24" y2="10"/></g>
<g class="ni" transform="translate(840,112)"><path d="M-16 -2L-22 -24M16 -2L22 -24"/><rect x="-32" y="-2" width="64" height="24" rx="3"/><circle class="ld" cx="-20" cy="10" r="2"/><circle class="ld" cx="-12" cy="10" r="2"/><line x1="6" y1="10" x2="24" y2="10"/></g>
<g class="ni" transform="translate(1090,112)"><rect x="-22" y="-30" width="44" height="18" rx="2"/><rect x="-22" y="-8" width="44" height="18" rx="2"/><rect x="-22" y="14" width="44" height="18" rx="2"/><circle class="ld" cx="-14" cy="-21" r="2"/><circle class="ld" cx="-14" cy="1" r="2"/><circle class="ld" cx="-14" cy="23" r="2"/></g>
<text class="nw-n" x="90" y="172">Kompüter A</text><text class="nw-s" x="90" y="189">192.168.0.15</text>
<text class="nw-n" x="340" y="172">R1 (ev router-i)</text><text class="nw-s" x="340" y="189">192.168.0.1 · 203.0.113.10</text>
<text class="nw-n" x="603" y="172">ISP router-ləri</text><text class="nw-s" x="603" y="189">(bir neçə hop)</text>
<text class="nw-n" x="840" y="172">R3 (server router-i)</text><text class="nw-s" x="840" y="189">gateway</text>
<text class="nw-n" x="1090" y="172">Web server</text><text class="nw-s" x="1090" y="189">93.184.216.34</text>
<g class="pk"><g class="ck k-eth"><rect x="-78" y="-11" width="36" height="22" rx="4" fill="#8B5CF6"/><text class="nw-t" x="-60.0" y="4">ETH</text></g><g class="ck k-ip"><rect x="-38" y="-11" width="36" height="22" rx="4" fill="#3B82F6"/><text class="nw-t" x="-20.0" y="4">IP</text></g><g class="ck k-tcp"><rect x="2" y="-11" width="36" height="22" rx="4" fill="#F59E0B"/><text class="nw-t" x="20.0" y="4">TCP</text></g><g class="ck k-http"><rect x="42" y="-11" width="36" height="22" rx="4" fill="#00A859"/><text class="nw-t" x="60.0" y="4">HTTP</text></g></g>
<rect x="14" y="208" width="1164" height="152" rx="8" fill="#fff" stroke="#C3E4D0" stroke-width="2"/>
<text class="nw-hd" x="190" y="230">Mənbə (kim göndərir)</text><text class="nw-hd" x="520" y="230">Təyinat (kimə)</text><text class="nw-hd" x="830" y="230">Bu addımda</text>
<rect x="28" y="244" width="140" height="26" rx="5" fill="#8B5CF6"/><text class="nw-t" x="98" y="262">ETH · qat 2: MAC</text>
<rect x="28" y="278" width="140" height="26" rx="5" fill="#3B82F6"/><text class="nw-t" x="98" y="296">IP · qat 3: IP</text>
<rect x="28" y="312" width="140" height="26" rx="5" fill="#F59E0B"/><text class="nw-t" x="98" y="330">TCP · qat 4: port</text>
<text class="nw-m c dt d1" x="190" y="262">08:00:27:aa:bb:01</text>
<text class="nw-m c dt d1" x="520" y="262">52:54:00:11:11:01</text>
<text class="nw-m dt d1" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d1" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m c dt d1" x="190" y="296">192.168.0.15</text>
<text class="nw-m c dt d1" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d1" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d1" x="830" y="296" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m c dt d1" x="190" y="330">41234</text>
<text class="nw-m c dt d1" x="520" y="330">80</text>
<text class="nw-m dt d1" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d1" x="830" y="330" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m dt d2" x="190" y="262">08:00:27:aa:bb:01</text>
<text class="nw-m dt d2" x="520" y="262">52:54:00:11:11:01</text>
<text class="nw-m dt d2" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d2" x="830" y="262" style="fill:#848688">dəyişmədi</text>
<text class="nw-m dt d2" x="190" y="296">192.168.0.15</text>
<text class="nw-m dt d2" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d2" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d2" x="830" y="296" style="fill:#848688">dəyişmədi</text>
<text class="nw-m dt d2" x="190" y="330">41234</text>
<text class="nw-m dt d2" x="520" y="330">80</text>
<text class="nw-m dt d2" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d2" x="830" y="330" style="fill:#848688">dəyişmədi</text>
<text class="nw-m c dt d3" x="190" y="262">52:54:00:11:11:02</text>
<text class="nw-m c dt d3" x="520" y="262">00:1b:21:22:22:01</text>
<text class="nw-m dt d3" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d3" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m c dt d3" x="190" y="296">203.0.113.10</text>
<text class="nw-m dt d3" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d3" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d3" x="830" y="296" style="fill:#C2410C">mənbə IP dəyişdi (NAT)</text>
<text class="nw-m c dt d3" x="190" y="330">50001</text>
<text class="nw-m dt d3" x="520" y="330">80</text>
<text class="nw-m dt d3" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d3" x="830" y="330" style="fill:#C2410C">mənbə port dəyişdi (NAT)</text>
<text class="nw-m c dt d4" x="190" y="262">00:1b:21:22:22:02</text>
<text class="nw-m c dt d4" x="520" y="262">00:1b:21:33:33:01</text>
<text class="nw-m dt d4" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d4" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m dt d4" x="190" y="296">203.0.113.10</text>
<text class="nw-m dt d4" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d4" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d4" x="830" y="296" style="fill:#848688">dəyişmədi</text>
<text class="nw-m dt d4" x="190" y="330">50001</text>
<text class="nw-m dt d4" x="520" y="330">80</text>
<text class="nw-m dt d4" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d4" x="830" y="330" style="fill:#848688">dəyişmədi</text>
<text class="nw-m c dt d5" x="190" y="262">00:1b:21:33:33:02</text>
<text class="nw-m c dt d5" x="520" y="262">52:54:00:44:44:01</text>
<text class="nw-m dt d5" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d5" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m dt d5" x="190" y="296">203.0.113.10</text>
<text class="nw-m dt d5" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d5" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d5" x="830" y="296" style="fill:#848688">dəyişmədi</text>
<text class="nw-m dt d5" x="190" y="330">50001</text>
<text class="nw-m dt d5" x="520" y="330">80</text>
<text class="nw-m dt d5" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d5" x="830" y="330" style="fill:#848688">dəyişmədi</text>
<text class="nw-m g dt d6" x="190" y="262">00:1b:21:33:33:02</text>
<text class="nw-m g dt d6" x="520" y="262">52:54:00:44:44:01</text>
<text class="nw-m dt d6" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d6" x="830" y="262" style="fill:#00703A">çıxarıldı</text>
<text class="nw-m g dt d6" x="190" y="296">203.0.113.10</text>
<text class="nw-m g dt d6" x="520" y="296">93.184.216.34</text>
<text class="nw-m dt d6" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d6" x="830" y="296" style="fill:#00703A">çıxarıldı</text>
<text class="nw-m g dt d6" x="190" y="330">50001</text>
<text class="nw-m g dt d6" x="520" y="330">80</text>
<text class="nw-m dt d6" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d6" x="830" y="330" style="fill:#00703A">çıxarıldı</text>
<text class="nw-m c dt d7" x="190" y="262">00:1b:21:22:22:01</text>
<text class="nw-m c dt d7" x="520" y="262">52:54:00:11:11:02</text>
<text class="nw-m dt d7" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d7" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m c dt d7" x="190" y="296">93.184.216.34</text>
<text class="nw-m c dt d7" x="520" y="296">203.0.113.10</text>
<text class="nw-m dt d7" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d7" x="830" y="296" style="fill:#C2410C">mənbə və təyinat yer dəyişdi</text>
<text class="nw-m c dt d7" x="190" y="330">80</text>
<text class="nw-m c dt d7" x="520" y="330">50001</text>
<text class="nw-m dt d7" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d7" x="830" y="330" style="fill:#C2410C">portlar yer dəyişdi</text>
<text class="nw-m c dt d8" x="190" y="262">52:54:00:11:11:01</text>
<text class="nw-m c dt d8" x="520" y="262">08:00:27:aa:bb:01</text>
<text class="nw-m dt d8" x="492" y="262" style="fill:#848688">→</text>
<text class="nw-tg dt d8" x="830" y="262" style="fill:#C2410C">yeni başlıq</text>
<text class="nw-m dt d8" x="190" y="296">93.184.216.34</text>
<text class="nw-m c dt d8" x="520" y="296">192.168.0.15</text>
<text class="nw-m dt d8" x="492" y="296" style="fill:#848688">→</text>
<text class="nw-tg dt d8" x="830" y="296" style="fill:#C2410C">təyinat IP dəyişdi (NAT geri)</text>
<text class="nw-m dt d8" x="190" y="330">80</text>
<text class="nw-m c dt d8" x="520" y="330">41234</text>
<text class="nw-m dt d8" x="492" y="330" style="fill:#848688">→</text>
<text class="nw-tg dt d8" x="830" y="330" style="fill:#C2410C">təyinat port dəyişdi (NAT geri)</text>
<rect x="14" y="368" width="1164" height="124" rx="8" fill="#fff" stroke="#C3E4D0" stroke-width="2"/>
<text class="nw-dh dt d0" x="34" y="396">Fərqli şəbəkədəki serverə sorğu: A və server eyni şəbəkədə deyil</text><text class="nw-dl dt d0" x="34" y="424">A = 192.168.0.15 (ev şəbəkəsi). Web server = 93.184.216.34 (internetdə, başqa şəbəkədə).</text><text class="nw-dl dt d0" x="34" y="448">Aralarında router-lər var. IP və MAC ünvanları bu yolda necə dəyişir? Addım-addım baxaq.</text><text class="nw-dl dt d0" x="34" y="472">Növbəti addım üçün düyməyə və ya diaqrama klikləyin.</text>
<text class="nw-dh dt d1" x="34" y="396">① A paketi hazırlayır: server mənim şəbəkəmdə deyil</text><text class="nw-dl dt d1" x="34" y="424">93.184.216.34 şəbəkəmdə (192.168.0.0/24) deyil, deməli paketi default gateway-ə verirəm: R1 (192.168.0.1).</text><text class="nw-dl dt d1" x="34" y="448">IP təyinatı = serverin IP-si (son məqsəd). Ethernet təyinatı = R1-in MAC-ı (yalnız növbəti addım, ARP ilə tapılır).</text>
<text class="nw-dh dt d2" x="34" y="396">② Frame R1-ə (gateway) çatır</text><text class="nw-dl dt d2" x="34" y="424">R1 yoxlayır: təyinat MAC mənimdir. Ethernet başlığı çıxarır, IP-yə baxır: təyinat IP mənim deyil.</text><text class="nw-dl dt d2" x="34" y="448">Deməli paket mənim üçün deyil, onu başqa şəbəkəyə ötürməliyəm (routing, Sessiya 06).</text>
<text class="nw-dh dt d3" x="34" y="396">③ R1 paketi internetə ötürür: NAT və yeni Ethernet başlıq</text><text class="nw-dl dt d3" x="34" y="424">NAT: mənbə IP 192.168.0.15 → açıq IP 203.0.113.10, mənbə port 41234 → 50001 (Sessiya 05). Private IP internetdə işləmir.</text><text class="nw-dl dt d3" x="34" y="448">Ethernet başlıq tamam yenidir: növbəti hop ISP router-idir. IP təyinatı (server) dəyişmədi.</text>
<text class="nw-dh dt d4" x="34" y="396">④ ISP router-i: yalnız Ethernet başlıq dəyişir</text><text class="nw-dl dt d4" x="34" y="424">Router IP təyinatına baxır, routing cədvəlindən növbəti hop-u seçir və yeni Ethernet başlıq yazır.</text><text class="nw-dl dt d4" x="34" y="448">IP və TCP dəyişmir. Bu addım internetdə bir neçə dəfə təkrarlanır (hər router bir hop).</text>
<text class="nw-dh dt d5" x="34" y="396">⑤ Server şəbəkəsinin router-i: son hop</text><text class="nw-dl dt d5" x="34" y="424">Server artıq bu router-in şəbəkəsindədir, ona görə Ethernet təyinatı birbaşa serverin MAC-ıdır.</text><text class="nw-dl dt d5" x="34" y="448">IP və TCP yenə dəyişməyib.</text>
<text class="nw-dh dt d6" x="34" y="396">⑥ Server alır: bütün başlıqlar çıxarılır</text><text class="nw-dl dt d6" x="34" y="424">MAC mənimdir, IP təyinatı mənimdir, TCP port 80-i web server dinləyir. HTTP sorğusu oxunur: GET /index.html.</text><text class="nw-dl dt d6" x="34" y="448">Server mənbə kimi 203.0.113.10:50001 görür. 192.168.0.15-i heç vaxt görmür.</text>
<text class="nw-dh dt d7" x="34" y="396">⑦ Cavab geri gəlir: mənbə və təyinat yer dəyişir</text><text class="nw-dl dt d7" x="34" y="424">IP: 93.184.216.34 → 203.0.113.10, port: 80 → 50001. Cavab R1-ə çatır.</text><text class="nw-dl dt d7" x="34" y="448">Təyinat hələ açıq IP-dir. R1-in NAT cədvəli bilir ki, 203.0.113.10:50001 hansı daxili cihaza məxsusdur.</text>
<text class="nw-dh dt d8" x="34" y="396">⑧ R1 NAT-ı geri çevirir və cavabı A-ya verir</text><text class="nw-dl dt d8" x="34" y="424">NAT cədvəli: 203.0.113.10:50001 ⇄ 192.168.0.15:41234. Təyinat IP və port daxili dəyərlərə qayıdır.</text><text class="nw-dl dt d8" x="34" y="448">Qayda: IP son nöqtəni göstərir (NAT-dan başqa dəyişmir), MAC isə yalnız növbəti hop-u və hər hop-da yenilənir.</text>
</svg>
<div class="nw-nav"><button class="sec" id="nwPrev" type="button">◀ Geri</button><span id="nwCnt">Addım 0 / 8</span><button id="nwNext" type="button">Növbəti addım ▶</button><button class="sec" id="nwRst" type="button">↻ Yenidən</button></div>
<script>
(function(){var s=document.getElementById('nwSvg'),p=document.getElementById('nwPrev'),n=document.getElementById('nwNext'),r=document.getElementById('nwRst'),c=document.getElementById('nwCnt'),k=0,M=8;
function u(){s.setAttribute('data-step',k);c.textContent='Addım '+k+' / '+M;p.disabled=k===0;n.disabled=k===M;}
function go(d){k=Math.max(0,Math.min(M,k+d));u();}
n.addEventListener('click',function(){go(1);});p.addEventListener('click',function(){go(-1);});r.addEventListener('click',function(){k=0;u();});
s.addEventListener('click',function(){if(k!==M){go(1);}else{k=0;u();}});
[n,p,r].forEach(function(b){b.addEventListener('keydown',function(e){e.stopPropagation();});});
u();})();
</script>

---

## 🔍 Hansı cihaz hansı qata qədər baxır?

Cihaz **paketi açır**, amma **lazım olan qata qədər**:

| Cihaz | Hansı qata qədər | Nəyə baxır |
|---|---|---|
| **Hub** | 1 | heç nəyə (siqnalı təkrarlayır) |
| **Switch** | **2** | **MAC** |
| **Router** | **3** | **IP** |
| **Firewall** | **3–4** (bəzən 7) | **IP, port** |
| **Load balancer** | **4** və ya **7** | **port** / **HTTP məzmunu** |
| **Proxy, WAF** | **7** | **HTTP məzmunu** |

- "**Layer 2 switch**", "**Layer 3 switch**", "**L4 load balancer**", "**L7 load balancer**" deyəndə məhz **bu qatlar** nəzərdə tutulur
- **Router frame-in başlığını açıb yenisini yazır**: MAC hər hop-da dəyişir, IP eyni qalır (Sessiya 02)

---

## 📦 Bir HTTP sorğusu qatlarla

`curl http://example.com` işlədirsən:

| Qat | Nə baş verir |
|---|---|
| **7 Application** | `GET / HTTP/1.1` sorğusu hazırlanır |
| **4 Transport** | **TCP**, təyinat **port 80** |
| **3 Network** | təyinat **IP** = example.com-un IP-si; mənbə = sənin IP-n |
| **2 Data Link** | təyinat **MAC = gateway-in MAC-ı** (ARP ilə tapılıb) |
| **1 Physical** | frame **bit** kimi kabel/Wi-Fi ilə gedir |

> **Real həyatda:** DNS əvvəl `example.com` → IP çevirir (qat 7). Sonra bu cədvəl işləyir. Qatlar **yuxarıdan aşağı** iş görür, qəbul edəndə **aşağıdan yuxarı**.

---

## 🛠️ Hansı əmr hansı qatı yoxlayır?

**Diaqnostika xəritəsi:** problemi **aşağıdan yuxarı** axtar.

| Qat | Sual | Əmr |
|---|---|---|
| **1** Physical | kabel/Wi-Fi qoşuludur? | `ip link` (`LOWER_UP`) |
| **2** Data Link | gateway-in MAC-ı tapılır? | `ip neigh` |
| **3** Network | IP və gateway var? çatır? | `ip a`, `ip r`, `ping` |
| **4** Transport | port açıqdır? | `ss -tln`, `nc -vz host port` |
| **7** Application | proqram cavab verir? | `curl -v`, `dig` |

```
  1 ✅ → 2 ✅ → 3 ✅ → 4 ❌    →  "IP-yə çatırıq, amma port bağlıdır"
```

> **İlk uğursuz addım** problemin qatını göstərir. Sessiya 01-dəki "nərdivan" indi **qatlara bağlanıb**.

---

## 🧪 Lab 3 — qat-qat yoxla

Ubuntu VM-də (interfeys adını `ip -br a`-dan götür):

```bash
$ ip link show enp0s3 | grep -o "state [A-Z]*"      # qat 1-2
state UP

$ ip neigh | head -2                                 # qat 2
192.168.0.1 dev enp0s3 lladdr 52:54:00:12:35:02 REACHABLE

$ ping -c 2 192.168.0.1                                 # qat 3: gateway
2 packets transmitted, 2 received, 0% packet loss

$ ping -c 2 8.8.8.8                                  # qat 3: internet
2 packets transmitted, 2 received, 0% packet loss

$ nc -vz example.com 80                              # qat 4: port açıqdır?
Connection to example.com (…) 80 port [tcp/http] succeeded!

$ curl -sI http://example.com | head -1              # qat 7: HTTP cavabı
HTTP/1.1 200 OK
```

> ⚠️ **Nümunə çıxışdır.** `nc` yoxdursa: `sudo apt install netcat-openbsd`. Nəticələr sənin şəbəkənə görə fərqli ola bilər.

---

## 🪜 Simptom → hansı qat?

| Simptom | Qat | Ehtimal olunan səbəb |
|---|---|---|
| `ip link` → `NO-CARRIER` | **1** | kabel çıxıb, Wi-Fi söndürülüb |
| IP var, gateway-in `ip neigh`-i `FAILED` | **2** | eyni şəbəkədə cavab yoxdur |
| Gateway-ə `ping` var, **`8.8.8.8`-ə yox** | **3** | gateway/routing/provayder problemi |
| `8.8.8.8` var, **`google.com`** yox | **7** | **DNS** (ad işləmir) |
| IP-yə `ping` var, `nc -vz IP 80` → `refused` | **4** | port bağlıdır, servis işləmir |
| Port açıq, `curl` → `403` / `502` | **7** | **tətbiq** səhvi, şəbəkə yox |

> **Qayda:** aşağı qat sağlam deyilsə, yuxarı qatı yoxlamağın mənası yoxdur. Əvvəl aşağını düzəlt.

---

## 🛠️ DevOps terminləri hansı qata aiddir?

| Termin | Qat | Mənası |
|---|---|---|
| **Security Group / NACL** (AWS) | **3–4** | IP və portlara görə icazə |
| **iptables / ufw** | **3–4** | host firewall (Sessiya 09) |
| **L4 load balancer** (AWS NLB) | **4** | portda trafiki paylayır, məzmuna baxmır |
| **L7 load balancer** (AWS ALB) | **7** | HTTP-ə baxır: yol, host adı |
| **Ingress** (Kubernetes) | **7** | HTTP trafikini idarə edir |
| **VPN** (WireGuard) | **3** | IP paketlərini şifrələnmiş tunneldə daşıyır |
| **WAF** | **7** | HTTP hücumlarını süzür |

> "L4 yoxsa L7?" sualı DevOps-da tez-tez çıxır. **L4** = sürətli, sadə, məzmunu görmür. **L7** = ağıllı, HTTP-yə baxır, amma daha çox resurs işlədir.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"OSI-nin 7 qatını poçt analogiyası ilə izah et. Hər qatda **hansı ünvan** işləyir (MAC, IP, port)?"*
- *"`curl https://example.com` zamanı hər qatda nə baş verir? Qat-qat yaz."*
- *"Sayt açılmır, `ping` işləyir, `curl` `502` verir. Problem hansı qatdadır və niyə?"*

</div>

<div class="ai-tip">💡 AI qatları bəzən <b>fərqli saya bilər</b> (5, 6 və ya 7 qat). Sənin cədvəlinlə tutuşdur: bu kursda <b>7 qatlı OSI</b> və <b>4 qatlı TCP/IP</b> işlədirik.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **Qat nəyə lazımdır?** | mürəkkəbi bölmək, problemi qatda tapmaq |
| **OSI** | 7 qat (nəzəri), **TCP/IP** 4 qat (real) |
| **1 / 2 / 3 / 4** | bit-kabel / **MAC**-frame-switch / **IP**-paket-router / **port**-TCP/UDP |
| **5-6-7** | praktikada **proqramın özü** (HTTP, SSH, DNS, TLS) |
| **Encapsulation** | hər qat **öz başlığını** əlavə edir |
| **Cihazlar** | switch **2**, router **3**, firewall **3–4**, load balancer **4/7** |
| **Diaqnostika** | **aşağıdan yuxarı**, ilk uğursuz qat = problemin yeri |

**Növbəti dərs:** Sessiya 04 — **Transport**: TCP, UDP, port, handshake.

---

## ✍️ Tapşırıqlar

1. **Qatlar:** 7 OSI qatını **düzgün sıra ilə** yaz və hər birinə bir **nümunə** (protokol, cihaz və ya ünvan) qoş.
2. **Ünvanlar:** MAC, IP və port **hansı qatın** ünvanlarıdır? Hər birini bir cümlə ilə izah et.
3. **Cihazlar:** switch, router və load balancer hansı qata qədər baxır? **Niyə** router MAC-a yox, IP-yə baxır?
4. **Lab 3:** Sənin VM-də Lab 3-ün hər əmrini icra et. **Hansı qat** hansı əmrlə yoxlanır?
5. **Diaqnostika:** `ping 8.8.8.8` işləyir, `ping google.com` işləmir. **Hansı qat** və **ən ehtimal olunan səbəb** nədir?
6. **Encapsulation:** `curl http://example.com` üçün hər qatın başlığına **nə yazıldığını** (port, IP, MAC) öz sözünlə izah et.
7. **Sual:** "L7 load balancer L4-dən niyə daha ağıllıdır?" Üstünlüyü və çatışmazlığı yaz.

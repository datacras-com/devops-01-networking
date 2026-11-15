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

**Ən aşağı qat.** Bit-ləri (0 və 1) **siqnal** kimi daşıyır: kabeldə **elektrik**, optik kabeldə **işıq**, Wi-Fi-da **radio dalğası**. Nə dediyini **bilmir**, yalnız **daşıyır**.

**Əsas vəzifə:** göndərəndə **bit → siqnal**, qəbul edəndə **siqnal → bit**.

| Nə | Nümunə |
|---|---|
| Mühit | **mis kabel**, **optik kabel**, **Wi-Fi** (radio) |
| Vahid | **bit** |
| Tipik problem | kabel çıxıb, Wi-Fi zəifdir, portu işləmir |

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

## 🖥️ Qat 5, 6, 7: Session, Presentation, Application

Qat 4 yalnız **baytları daşıyır**, mənasını bilmir. **Qat 5–7 proqramın öz dilidir:** sorğunu və cavabı proqramların **başa düşdüyü formada** yazır. Praktikada üçü **bir yerdə** düşünülür.

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

## 🧭 OSI niyə yetərli deyil? TCP/IP modeli

- **OSI** = **nəzəri model** (1980-lər, təşkilatlar yazıb). Anlamaq üçün əladır
- **İnternet isə TCP/IP üzərində qurulub**, yəni real protokollar onun 4 qatına uyğundur

```
   OSI (7 qat)                      TCP/IP (4 qat)
  ┌───────────────┐
  │ 7 Application │
  │ 6 Presentation│ ──────────►   ┌──────────────────┐
  │ 5 Session     │               │   Application    │
  ├───────────────┤               ├──────────────────┤
  │ 4 Transport   │ ──────────►   │   Transport      │
  ├───────────────┤               ├──────────────────┤
  │ 3 Network     │ ──────────►   │   Internet       │
  ├───────────────┤               ├──────────────────┤
  │ 2 Data Link   │               │   Network Access │
  │ 1 Physical    │ ──────────►   │   (Link)         │
  └───────────────┘               └──────────────────┘
```

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
10.0.2.2 dev enp0s3 lladdr 52:54:00:12:35:02 REACHABLE

$ ping -c 2 10.0.2.2                                 # qat 3: gateway
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
| **iptables / ufw** | **3–4** | host firewall (Linux S41) |
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

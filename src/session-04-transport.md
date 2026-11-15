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

# 🚚 Transport qatı

## TCP · UDP · port — məlumat doğru proqrama necə çatır?

```
  Cihaz: 192.168.0.15
  ┌────────────────────────────────────────────┐
  │  :22   sshd        ◄── ssh sorğusu         │
  │  :80   nginx       ◄── brauzer sorğusu     │
  │  :5432 PostgreSQL  ◄── verilənlər bazası   │
  └────────────────────────────────────────────┘
        IP cihazı tapır, PORT proqramı seçir
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 04 · Chapter 2 — Transport və Təhlükəsizlik

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Qat 3-dən sonra nə çatışmır?** Transport qatı hansı problemləri həll edir
- **Port və socket:** `IP:port`, client portu necə seçilir, bağlantını necə tanıyırıq
- **TCP** (Transmission Control Protocol) və **UDP** (User Datagram Protocol): fərq və seçim
- **TCP necə etibarlıdır:** handshake, sıra nömrəsi, ACK, itənin təkrarı, sürət nəzarəti
- **TCP bağlantısının ömrü:** açmaq, bağlamaq, `ss` ilə vəziyyətlər
- **`refused` və `timeout`:** iki fərqli xəta, iki fərqli səbəb
- **Canlı lab:** `nc` və `ss` ilə bağlantını özün gör

> Linux kursunda **S27** (port siyahısı, `ss`, `telnet`) var. HTTP tərəfi **Sessiya 08**-dədir. Burada **niyə belə işlədiyini** və **problemi necə tapacağını** öyrənirik.

---

## 🧩 Qat 3-dən sonra nə çatışmır?

Qat 3 (IP) paketi **cihaza** çatdırır. Amma iki sual cavabsız qalır:

| Problem | Nə üçün çətindir | Kim həll edir |
|---|---|---|
| **Cihazdakı hansı proqrama?** | bir cihazda onlarla proqram var (brauzer, SSH, DB) | **port** |
| **Paket itərsə? Qarışarsa?** | IP "ən yaxşı cəhd" edir, **zəmanət vermir** | **TCP** (etibarlı) |

```
  Qat 3 (IP):        "Paketi 192.168.0.15 cihazına çatdırdım"
  Qat 4 (Transport): "Hansı proqrama? Hamısı düzgün gəlib?"
```

> **IP zəmanət vermir:** paket itə, təkrarlana, səhv sırada gələ bilər. İtkini düzəltmək işini **qat 4** (TCP) öz üzərinə götürür. İstəməyən proqram isə **UDP** seçir.

---

## 🚪 Port — cihazdakı "otaq nömrəsi"

- **IP** = hansı **bina**, **port** = binada hansı **otaq**
- Port **16 bit** nömrədir: **0–65535**
- **`IP:port`** cütünə **socket** (soket, şəbəkə ucu) deyilir: `192.168.0.15:80`

| Aralıq | Adı | Kim istifadə edir |
|---|---|---|
| **0–1023** | well-known (məşhur) | sistem servisləri: `22` SSH, `80` HTTP, `443` HTTPS |
| **1024–49151** | registered (qeydə alınmış) | tətbiqlər: `3306` MySQL, `5432` PostgreSQL, `8080` |
| **49152–65535** | dynamic (müvəqqəti) | **client**-lərin müvəqqəti portları |

> Rəsmi bölgünü **IANA** (Internet Assigned Numbers Authority) edir. **Linux**-da müvəqqəti port aralığı bir az fərqlidir: `32768–60999` (`cat /proc/sys/net/ipv4/ip_local_port_range`). Port siyahısı üçün: **Linux S27**.

---

## 🎲 Client portu necə seçilir?

**Server** portu **sabitdir** (`443`). **Client** isə **hər bağlantı üçün müvəqqəti port** seçir:

```
  Brauzer (tab 1)  192.168.0.15:41234 ───────────►  93.184.216.34:443
  Brauzer (tab 2)  192.168.0.15:41240 ───────────►  93.184.216.34:443
  curl             192.168.0.15:41300 ───────────►  93.184.216.34:443
```

Hər bağlantı **dörd dəyərlə** tanınır:

| Mənbə IP | Mənbə port | Təyinat IP | Təyinat port |
|---|---|---|---|
| `192.168.0.15` | **`41234`** | `93.184.216.34` | `443` |
| `192.168.0.15` | **`41240`** | `93.184.216.34` | `443` |

> **Eyni serverə eyni portla iki bağlantı** mümkündür, çünki **mənbə port fərqlidir**. Cavab da doğru tab-a bu səbəbdən qayıdır.

---

## ⚖️ TCP və UDP — əsas fərq

| | TCP | UDP |
|---|---|---|
| Analogiya | **qeydli məktub** (təhvil alındı bildirişi ilə) | **açıqca** (atdın, getdi) |
| Bağlantı | **var** (əvvəl razılaşma) | **yoxdur** |
| Etibarlılıq | **var** (itən təkrar göndərilir) | **yoxdur** |
| Sıra | **qorunur** | qorunmur |
| Sürət | daha yavaş | **daha sürətli** |
| Başlıq | **20 bayt** (minimum) | **8 bayt** |
| Nümunə | web, SSH, e-poçt, DB | DNS sorğusu, video zəng, oyun |

> UDP **"fire and forget"** (göndər və unut) kimidir: göndərir, çatıb-çatmadığına **baxmır**. Nə üçün? **Gözləmək bahalıdır**: video zəngdə köhnə kadrı yenidən göndərməkdənsə, atıb yenisini göstərmək yaxşıdır.

---

## 🛡️ TCP etibarlılığı: 4 mexanizm

| # | Mexanizm | Nə edir | Analogiya |
|---|---|---|---|
| 1 | **Bağlantı** (handshake) | göndərməzdən **əvvəl razılaşır** | zəng edib "eşidirsən?" soruşmaq |
| 2 | **Sıra nömrəsi** | hər hissəyə nömrə verir | bağlamaları **nömrələmək** |
| 3 | **ACK** (acknowledgement, təsdiq) + **təkrar** | alınanı təsdiqləyir, itəni **yenidən göndərir** | "aldım" bildirişi |
| 4 | **Sürət nəzarəti** (window) | qəbul edənin tutumuna görə sürəti tənzimləyir | qəbul edən "yavaş ol" deyir |

```
  Göndərən:  1  2  3  4  5   ───►   hər hissə nömrəlidir
  Qəbul edən:"1,2 aldım" "3 itdi, 3 göndər" ← təsdiq + tələb
```

> TCP-ni **"hər şey çatdırılana qədər çalışan"** protokol kimi düşün. Bu etibar **gecikmə hesabına** gəlir: hər addım təsdiq gözləyir.

---

## 🤝 3-way handshake — bağlantının açılması

**SYN** (synchronize, "sinxronlaşaq"), **ACK** (təsdiq). **Üç addım:**

```
  Client                                    Server
    │
    │  ① SYN        "Bağlanmaq istəyirəm"
    │ ─────────────────────────────────────────►
    │  ② SYN-ACK    "Hazıram, sən də hazırsan?"
    │ ◄─────────────────────────────────────────
    │  ③ ACK        "Hazıram, başlayaq"
    │ ─────────────────────────────────────────►
    │            İNDİ MƏLUMAT GEDƏ BİLƏR

```

| Addım | Nə baş verir |
|---|---|
| ① **SYN** | client **başlanğıc sıra nömrəsini** göndərir |
| ② **SYN-ACK** | server **təsdiqləyir** və **öz** nömrəsini göndərir |
| ③ **ACK** | client **təsdiqləyir**. **İki tərəf də** göndərə və qəbul edə bildiyini bilir |

> Üç addım nə üçündür? **Hər iki tərəf** bir-birini eşitdiyini və **başlanğıc nömrələrini** razılaşdırdığını bilməlidir. HTTP-nin bunun üstündə necə işlədiyi: **Sessiya 08**.

---

## 🔢 Sıra nömrəsi və itirilmiş paket

Sadələşdirilmiş nümunə (real TCP **bayt** nömrəsi işlədir, burada **hissə** kimi göstəririk):

```
  Göndərən                                   Qəbul edən
    │  hissə 1 ───────────────────────────►  aldım
    │  hissə 2 ───────────────────────────►  aldım
    │  hissə 3 ───────────────X (yolda itdi)
    │  hissə 4 ───────────────────────────►  aldım (amma 3 yoxdur!)
    │ ◄────────── ACK: "3-ü gözləyirəm"      │
    │  hissə 3 (YENİDƏN) ─────────────────►  aldım
    │ ◄────────── ACK: "5-i gözləyirəm"      │
```

- Qəbul edən **ardıcıl gələnləri** təsdiqləyir. İtən hissəni **gözlədiyini** bildirir
- Göndərən **cavab gəlmədikdə** həmin hissəni **yenidən göndərir** (retransmission)
- Nəticədə proqram **hər şeyi düzgün sırada** alır, itki onun gözündən **gizlənir**

> İtki **aşağı qatda** düzəlir, **proqram bilmir**. Ona görə TCP üzərindəki proqram (məsələn, `scp`) sadəcə "fayl gəldi" görür. İtki çoxdursa, **yavaşlıq** hiss olunur.

---

## 🚦 Sürət nəzarəti: window

**Problem:** göndərən çox sürətli, qəbul edən yavaş olarsa, qəbul edənin **buferi dolur**.

| Mexanizm | Kim nəyə baxır | Mənası |
|---|---|---|
| **Flow control** (window) | **qəbul edən** | "buferimdə **X bayt yer** var, bundan çox göndərmə" |
| **Congestion control** | **şəbəkənin yükü** | şəbəkə dolu görünəndə göndərən **özü yavaşlayır** |

```
  Qəbul edən:  "window = 64 KB"   →  göndərən 64 KB göndərib təsdiq gözləyir
  Qəbul edən:  "window = 0"        →  "dayan, yerim yoxdur"
```

> "Sürətli kanal var, amma **fayl yavaş yüklənir**" problemi tez-tez **window**, **itki** və **gecikmə** ilə bağlıdır. Bunu **Sessiya 11**-də alətlərlə ölçəcəyik.

---

## 🔚 Bağlantının ömrü və `ss` vəziyyətləri

```
  LISTEN  ──(client SYN göndərir)──►  ESTAB  ──(FIN)──►  TIME-WAIT  ──►  bağlandı
  server                          bağlantı var    bağlanır       ~60 san gözləyir
```

| Vəziyyət | Mənası |
|---|---|
| **LISTEN** | server **dinləyir**, bağlantı gözləyir |
| **ESTAB** | bağlantı **quruldu**, məlumat gedir |
| **TIME-WAIT** | bağlantı **bağlandı**, son paketləri gözləyir (Linux-da ~60 san) |
| **CLOSE-WAIT** | qarşı tərəf bağladı, **sənin proqram hələ bağlamayıb** |

```bash
$ ss -tan
State      Recv-Q Send-Q  Local Address:Port    Peer Address:Port
LISTEN     0      4096    0.0.0.0:22            0.0.0.0:*
ESTAB      0      0       192.168.0.15:41234       93.184.216.34:443
TIME-WAIT  0      0       192.168.0.15:41230       93.184.216.34:443
```

> Çoxlu `TIME-WAIT` **normaldır**. Çoxlu **`CLOSE-WAIT`** isə adətən **proqram səhvidir** (bağlantını bağlamır).

---

## 📨 UDP — bağlantısız, sürətli

UDP **handshake etmir**, **təsdiq gözləmir**, **itəni təkrar göndərmir**. Üstünlüyü: **sürət və sadəlik**.

| Protokol / servis | Port | Niyə UDP? |
|---|---|---|
| **DNS** (Domain Name System) | `53` | kiçik sorğu-cavab, sürətli olmalıdır |
| **DHCP** | `67/68` | cihazın hələ IP-si yoxdur, bağlantı qurmaq çətindir |
| **WireGuard** (VPN) | seçilir | tunnel gecikməsi aşağı olmalıdır |
| **QUIC** (HTTP/3) | `443` | UDP üzərində **öz etibarlılığını** qurur |
| **Video zəng, oyun** | müxtəlif | köhnə kadrı təkrar göndərmək mənasızdır |

> UDP **etibarsız** deyil, sadəcə **etibarlılığı proqrama buraxır**. DNS böyük cavablarda **TCP**-yə də keçə bilər (hər ikisi `53`-də).

---

## 🧭 Hansını seçməli?

| Sual | Cavab | Seçim |
|---|---|---|
| Məlumat **itməməlidir**? (fayl, DB, web səhifə) | hə | **TCP** |
| **Sıra** vacibdir? | hə | **TCP** |
| **Gecikmə** kritikdir, itki **dözülür**? (zəng, oyun) | hə | **UDP** |
| Kiçik **sorğu-cavab**, hər dəfə bağlantı bahadır? (DNS) | hə | **UDP** |

> Praktikada **çox şey TCP-dir** (web, SSH, DB, e-poçt). UDP **xüsusi hallar** üçündür. DevOps-da qarşılaşdığın əksər servis TCP-dir.

---

## ❌ `refused` və `timeout` — iki fərqli xəta

Əvvəlcə **qat 3** çatır (`ping` işləyir). İndi **port** üçün iki ayrı cavab ola bilər:

| Xəta | Nə baş verdi | Mənası | Ehtimal olunan səbəb |
|---|---|---|---|
| **`Connection refused`** | cihaz **cavab verdi**: "bu portda heç kim yoxdur" (**RST**, reset) | **cihaza çatırıq, amma servis yoxdur** | servis dayanıb, **yanlış port** |
| **`Timed out`** | **heç bir cavab gəlmədi** | paket **itir** və ya **atılır** | **firewall DROP**, cihaz yoxdur, marşrut yoxdur |

```bash
$ nc -vz 192.168.0.15 9999
nc: connect to 192.168.0.15 port 9999 (tcp) failed: Connection refused      # cavab var, servis yox

$ curl -m 5 http://192.168.0.99
curl: (28) Connection timed out after 5001 milliseconds                   # cavab yoxdur
```

> **`refused` = qapı var, ev boşdur. `timeout` = qapı yoxdur və ya keşikçi səni ötürmür.** Bu fərq diaqnostikanın yarısıdır: `refused` olanda **servisi yoxla**, `timeout` olanda **firewall və marşrutu yoxla**.

---

## 🧪 Lab 4a — öz serverini qur

**İki terminal** aç (VM-də). `nc` (netcat) kiçik server və client kimi işləyir.

```bash
# TERMINAL 1: server (9000-ci portu dinlə)
$ nc -l 127.0.0.1 9000

# TERMINAL 2: dinləyirmi?
$ ss -tln | grep 9000
LISTEN 0  1  127.0.0.1:9000  0.0.0.0:*

# TERMINAL 2: client kimi qoşul
$ nc 127.0.0.1 9000
salam                          ← yaz, Enter bas. Terminal 1-də görünməlidir
```

| Nə görürsən | Mənası |
|---|---|
| `LISTEN ... 127.0.0.1:9000` | server **dinləyir** |
| Yazdığın mətn **digər terminalda** çıxır | TCP bağlantısı **qurulub**, məlumat gedir |

> ⚠️ `nc` yoxdursa: `sudo apt install netcat-openbsd`. Çıxışlar **nümunədir**. `Ctrl+C` ilə dayandır.

---

## 🧪 Lab 4b — bağlantıya baxaq: `ESTAB` və `refused`

Bağlantı hələ açıqkən (Terminal 3-də):

```bash
$ ss -tn | grep 9000
ESTAB  0  0  127.0.0.1:9000   127.0.0.1:43512      ← server tərəfi
ESTAB  0  0  127.0.0.1:43512  127.0.0.1:9000       ← client tərəfi (müvəqqəti port)
```

`Ctrl+C` ilə **serveri dayandır**, sonra yenidən qoşulmağa çalış:

```bash
$ nc -vz 127.0.0.1 9000
nc: connect to 127.0.0.1 port 9000 (tcp) failed: Connection refused
```

| Müşahidə | Nəyi göstərir |
|---|---|
| **Client portu** `43512` (müvəqqəti) | client **hər dəfə fərqli port** seçir |
| Server dayananda **`refused`** | cihaz **cavab verir**, amma **dinləyən yoxdur** |

> Server işləyəndə `nc -vz 127.0.0.1 9000` → `succeeded!`. Dayanandan sonra → `refused`. Bu, "servis dayanıb" simptomudur.

---

## 🪜 Transport problemləri: simptom → səbəb

| Simptom | Ehtimal olunan səbəb | Yoxla |
|---|---|---|
| **`Connection refused`** | servis **dayanıb** və ya port **yanlışdır** | `ss -tlnp`: dinləyən var? |
| **`Timed out`** | **firewall DROP**, marşrut yoxdur, cihaz yoxdur | `ping`, firewall qaydaları |
| **`Address already in use`** (server başlamır) | port **başqa proses** tərəfindən tutulub | `ss -tlnp \| grep :PORT` |
| Qoşulur, sonra **`Connection reset`** | qarşı tərəf bağlantını **kəsdi** (RST) | server logları |
| Çoxlu **`CLOSE-WAIT`** | proqram bağlantıları **bağlamır** | proqram kodu |
| Yavaş yüklənir, kanal boşdur | **itki / gecikmə / window** | `ping`, `iperf` (Sessiya 11) |

> **Qayda:** əvvəl `refused` yoxsa `timeout`-u ayır. İki ayrı problemdir, **iki ayrı yerdə** axtarılır.

---

## 🛠️ DevOps-da port və transport

| Mühit | Harada görünür | Diqqət |
|---|---|---|
| **Docker** | `-p 8080:80` (host:container) | port **xaricdə** `8080`, **konteynerdə** `80` |
| **Kubernetes** | Service `port` / `targetPort` | iki fərqli port: Service-in, pod-un |
| **AWS Security Group** | Inbound rule: TCP 443 | **hansı port və protokol** icazəlidir |
| **Load balancer** | health check: TCP `80` | **TCP bağlantısı** qurulursa, "sağlam" sayılır |
| **Firewall** (Sessiya 09) | `ufw allow 22/tcp` | `tcp` və `udp` **ayrıca** açılır |

> **TCP health check** yalnız portun **açıq olduğunu** yoxlayır, **tətbiqin işlədiyini yox**. Port açıqdır, amma tətbiq xətalıdır: bunu **HTTP health check** (qat 7) tutur.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"TCP 3-way handshake-i addım-addım izah et. SYN, SYN-ACK, ACK arasında nə dəyişir?"*
- *"`Connection refused` və `Connection timed out` arasındakı fərq nədir? Hər biri üçün ən ehtimal olunan 3 səbəbi yaz."*
- *"`ss -tan` çıxışımı izah et: ESTAB, TIME-WAIT, CLOSE-WAIT nə deməkdir? (çıxışı yapışdır)"*

</div>

<div class="ai-tip">💡 AI cavabını <b>öz VM-ində yoxla</b>: <code>nc</code> və <code>ss</code> ilə eyni vəziyyəti yarat və görüntünü cavabla tutuşdur.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **Qat 4 nə edir?** | **hansı proqrama** (port) və **etibarlı** (TCP) |
| **Port** | 16 bit (0–65535). **`IP:port`** = socket |
| **Client portu** | **müvəqqəti** (49152+ və ya Linux-da 32768+) |
| **Bağlantı** | 4 dəyərlə tanınır (mənbə/təyinat IP və port) |
| **TCP** | bağlantılı, etibarlı: handshake, sıra, ACK, təkrar, window |
| **UDP** | bağlantısız, sürətli, itki proqramın işi |
| **Vəziyyətlər** | LISTEN, ESTAB, TIME-WAIT, CLOSE-WAIT (`ss -tan`) |
| **`refused`** | cihaz cavab verdi, **servis yoxdur** |
| **`timeout`** | **cavab yoxdur** (firewall, marşrut) |

**Növbəti dərs:** Sessiya 05 — **NAT, firewall, load balancer**.

---

## ✍️ Tapşırıqlar

1. **Port:** Nə üçün **eyni serverə eyni portla iki bağlantı** mümkündür? Bağlantını **hansı 4 dəyər** fərqləndirir?
2. **TCP vs UDP:** Video zəng üçün **hansını** seçərdin və **niyə**? Fayl yükləmə üçün hansını?
3. **Handshake:** 3 addımı sıra ilə yaz və **hər addımda kimin nə dediyini** öz sözünlə izah et.
4. **Lab 4a:** `nc` ilə server və client yarat. İki terminal arasında mesaj göndər. `ss -tln` ilə dinləyən portu göstər.
5. **Lab 4b:** Bağlantı açıqkən `ss -tn` işlət: **client portunu** tap. Serveri dayandırıb `nc -vz` et: hansı xəta çıxır və **niyə**?
6. **Diaqnostika:** `curl` `refused` verir. Digər `curl` `timeout` verir. Hər birində **ilk nəyi yoxlayarsan**?

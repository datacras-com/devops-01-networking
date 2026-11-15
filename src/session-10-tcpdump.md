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

# 🦈 tcpdump ilə trafik analizi

## Şəbəkənin "təhlükəsizlik kamerası": paketi öz gözünlə gör

```
  192.168.0.15 (sən)                                         192.168.0.21 (server)
        │                                                          │
        │ ──── SYN ──────────────────────────────────────────────► │
        │ ◄─── SYN-ACK ─────────────────────────────────────────── │
        │ ──── ACK ──────────────────────────────────────────────► │
        │                                                          │
        └──────────────► [ tcpdump ] ◄─────────────────────────────┘
                          paketləri "çəkir" və sətir-sətir göstərir
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 10 · Chapter 5 — Diaqnostika

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **`tcpdump` nədir** və niyə "loglar yalan danışa bilər, paket yalan danışmır"
- **Necə işə salırıq:** `sudo`, interfeys seçimi (`-i`), əsas flaglar
- **Filtrlər (BPF):** milyonlarla paketdən yalnız lazım olanı seçmək
- **Çıxışı sətir-sətir oxumaq:** vaxt, ünvanlar, `Flags`, `seq`, `ack`, `length`
- **TCP bayraqları** `[S] [S.] [.] [P.] [F.] [R.]` paket səviyyəsində
- **Əsas ssenari:** "SYN gedir, cavab yoxdur" (3 fərqli hal)
- **DNS və HTTP tutmaq**, `.pcap` faylı yazmaq və Wireshark-a körpü
- **Təhlükəsizlik və performans** qaydaları, canlı lab

> **Sessiya 04**-də handshake, bayraqlar və `refused`/`timeout` anlayışını gördük. Bu dərsdə **eyni şeyi birbaşa paketlərin üzərində** görəcəksən.

---

## 🧩 Problem: nəyə inanaq?

Server problemi olanda adətən **loglara** baxırıq. Amma:

| Mənbə | Nə deyir | Problem |
|---|---|---|
| **Tətbiq logu** | "Heç bir sorğu gəlmədi" | tətbiq **yalnız özünə çatanı** görür |
| **Dashboard** | "Server sağlamdır" | **yoxlamanın** nəyi yoxladığına bağlıdır |
| **Developer** | "Bizdə hər şey işləyir" | **öz tərəfini** görür |
| **Paket** | gəldi / gəlmədi / cavab verdi | **tək həqiqət**, yalan danışmır |

> Tətbiq logu **hekayə danışır**, paket isə **faktı göstərir**. Sorğu serverə **çatmayıbsa**, tətbiq logunda heç nə olmaz, amma şəbəkədə **iz qalır**.

---

## 📹 tcpdump nədir?

**Analogiya:** binanın girişindəki **təhlükəsizlik kamerası**. Kim, nə vaxt, hara gəldi: hamısını **yazır**. Telefon xəttinə qoşulmuş **dinləyici** kimi də düşünə bilərsən.

| tcpdump | nə edir | nə etmir |
|---|---|---|
| **izləyir** | şəbəkə kartından keçən paketləri **göstərir** | paketi **dəyişmir** |
| **süzür** | filtrə uyğun paketi seçir | problemi **düzəltmir** |
| **yazır** | `.pcap` faylına saxlaya bilir | **şifrəni açmır** (HTTPS məzmunu görünmür) |

> `tcpdump` **qara qutu** kimidir: nəyin baş verdiyini göstərir, **niyə** baş verdiyini isə **sən** çıxarırsan. Bu dərs məhz **oxumağı** öyrədir.

---

## ⏰ Nə vaxt istifadə olunur?

| Sual | tcpdump nəyi göstərir |
|---|---|
| "Sorğu serverə **çatır**?" | paket serverin kartında görünürmü |
| "Cavab niyə **gəlmir**?" | `SYN` gedir, `SYN-ACK` yoxdur |
| "5 saniyə **gecikmə** haradadır?" | iki paket arasında vaxt fərqi |
| "DNS **yavaşdır**?" | sorğu ilə cavab arasındakı vaxt |
| "Bu server **kimlə danışır**?" | hansı IP hansı porta qoşulur |

> Qayda: `ping`, `curl`, `ss` **nəticəni** göstərir (işləyir/işləmir). `tcpdump` isə **səbəbi**: paket **harada və niyə** dayandı.

---

## 🔑 Niyə `sudo` lazımdır?

Adi istifadəçi **yalnız öz proqramının** məlumatını görür. Bütün şəbəkə kartını izləmək isə **başqalarının trafikini də** görmək deməkdir.

```
  Adi istifadəçi:   "öz zəngimi eşidirəm"
  root (sudo):      "xəttdəki bütün zəngləri dinləyə bilərəm"  ← icazə lazımdır
```

```bash
$ sudo apt install -y tcpdump          # yoxdursa quraşdır
$ tcpdump -i eth0
tcpdump: eth0: You don't have permission to capture on that device   ← sudo yoxdur
$ sudo tcpdump -i eth0                 # düzgün
```

> Paket tutmaq **raw (xam) şəbəkə girişi** tələb edir, bunu Linux **yalnız root-a** (və ya xüsusi icazəyə) verir.

---

## 🔌 İnterfeys seçimi: harada dinləyirik?

Kamera **hansı qapıya** baxır? Əvvəl şəbəkə kartlarını (**interface**, interfeys) tap:

```bash
$ ip -br a
lo       UNKNOWN   127.0.0.1/8
eth0     UP        192.168.0.15/24          <- əsas kart, trafik buradan keçir
docker0  DOWN      172.17.0.1/16            <- Docker körpüsü
$ tcpdump -D                                <- tcpdump-un görə bildiyi interfeyslər
1.eth0 [Up, Running]
2.any (Pseudo-device that captures on all interfaces)
```

| Seçim | Mənası |
|---|---|
| `-i eth0` | **yalnız bu kart** (dəqiq, az səs-küy) |
| `-i any` | **bütün kartlar** (hansı kartdan getdiyini bilmirsənsə) |
| `-i lo` | **loopback** (`127.0.0.1`, eyni maşın daxilində) |

> `-i` yazmasan, tcpdump **özü bir kart seçir**, bu isə səni **yanlış yerə** baxdıra bilər. `-i any` yazanda sətirlərə **`eth0 In`/`eth0 Out`** (gələn/gedən) sözləri əlavə olunur.

---

## ▶️ İlk əmr: 5 paket tut

`-c 5` = "5 paketdən sonra **dayan**" (`count`). Başqa terminalda `ping -c 2 192.168.0.1` işlət:

```bash
$ sudo tcpdump -i eth0 -c 5
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes     <- dinləməyə başladı
14:02:11.201345 IP 192.168.0.15 > gateway: ICMP echo request, id 7, seq 1, length 64
14:02:11.202110 IP gateway > 192.168.0.15: ICMP echo reply, id 7, seq 1, length 64
14:02:12.203001 IP 192.168.0.15 > gateway: ICMP echo request, id 7, seq 2, length 64
14:02:12.203790 IP gateway > 192.168.0.15: ICMP echo reply, id 7, seq 2, length 64
14:02:12.350004 ARP, Request who-has 192.168.0.1 tell 192.168.0.15, length 28    <- Sessiya 02-dəki ARP
5 packets captured                      <- ekrana çıxan
5 packets received by filter            <- filtrə düşən
0 packets dropped by kernel             <- itirilən (0 yaxşıdır)
```

> Nümunədir: vaxtlar və `id` sənin çıxışında fərqli olacaq. Diqqət: **`gateway`** sözü gördün, çünki tcpdump **IP-ni adla əvəz etdi**. Bu, növbəti slaydın mövzusudur.

---

## 🔢 `-n` və `-nn`: adlar yox, rəqəmlər

Susmaya görə tcpdump IP-ni **ada**, portu **xidmət adına** çevirməyə çalışır (`93.184.216.34` → `example.com`, `443` → `https`). Bunun **iki problemi** var:

| Problem | Nəticə |
|---|---|
| Ad tapmaq üçün tcpdump **özü DNS sorğusu göndərir** | **trafikə əlavə paket qarışır**, çıxış yavaşlayır |
| DNS işləmirsə, **hər sətir gecikir** | pis vaxtda **ən çox lazım olanda** donur |

| Flag | Nə edir |
|---|---|
| `-n` | **IP-ni** adla əvəz etmə |
| `-nn` | **IP-ni də, portu da** adla əvəz etmə (`443`, `https` yox) |

> **Həmişə `-nn` yaz.** Rəqəm dəqiqdir: `443` yazılıbsa, nə olduğunu bilirsən. Bu dərsdən sonra əmrlərin hamısı **`sudo tcpdump -nn`** ilə başlayacaq.

---

## 🧰 Əsas flaglar

| Flag | Nə edir | Nümunə |
|---|---|---|
| `-i` | hansı interfeys | `-i eth0`, `-i any` |
| `-nn` | ad həll etmə, rəqəm göstər | `-nn` |
| `-c N` | N paketdən sonra dayan | `-c 20` |
| `-v`, `-vv` | **daha ətraflı** (`ttl`, `id`, yoxlama cəmi) | `-vv` |
| `-S` | `seq` **mütləq** göstər (susmaya görə nisbi) | `-S` |
| `-s N` | paketin **neçə baytını** götür (`snaplen`) | `-s 0` = hamısı |
| `-A` / `-X` | **məzmunu** göstər (ASCII / hex+ASCII) | `-A` |
| `-w` / `-r` | fayla **yaz** / fayldan **oxu** | `-w a.pcap` |

> Flaglar **birləşir**: `sudo tcpdump -nn -i eth0 -c 10 -vv`. Filtr (növbəti slaydlar) **sonda** yazılır.

---

## 🔍 `-A`, `-X` və `-s`: paketin içinə baxmaq

Susmaya görə tcpdump yalnız **başlığı** (zərfi) göstərir. **Məktubu** (məzmun) görmək üçün:

| Flag | Nə göstərir | Nə vaxt |
|---|---|---|
| `-A` | məzmunu **ASCII** (mətn) kimi | HTTP, mətn protokolları |
| `-X` | məzmunu **hex + ASCII** yanbayan | ikili (binary) protokollar |
| `-s 0` | paketi **tam** götür (kəsmə) | məzmunu tam görmək lazımdırsa |

```bash
$ sudo tcpdump -nn -i eth0 -c 1 -X 'port 80'
        0x0000:  4500 0082 1c46 4000 4006 b1e6 c0a8 000f  E....F@.@.......   <- sol: hex, sağ: ASCII
        0x0010:  c0a8 0015 c822 0050 a7d3 4f01 0000 0001  ....."P..O.....
        0x0020:  8018 01f6 8a3c 0000 0101 080a 0000 0000  .....<..........
```

> `c0a8 000f` = `192.168.0.15` (hex). Hex-i əl ilə oxumaq **lazım deyil**, əsas məqsəd: **`-A` ilə mətni görmək** (HTTP slaydında).

---

## 🎛️ Filtr nədir? (BPF)

Real serverdə saniyədə **minlərlə paket** keçir. Hamısını oxumaq **mümkün deyil**. **Filtr** = "yalnız bunu göstər".

**Analogiya:** kamera yazısında **yalnız qırmızı maşını** axtarırsan. Filtrin adı **BPF** (Berkeley Packet Filter): paket **kerneldə süzülür**, ona görə sürətlidir.

```bash
$ sudo tcpdump -nn -i eth0 host 192.168.0.21           <- bu IP-yə gələn/ondan gedən
$ sudo tcpdump -nn -i eth0 port 443                    <- bu port (mənbə VƏ YA təyinat)
$ sudo tcpdump -nn -i eth0 host 192.168.0.21 and port 443    <- ikisi birlikdə
```

| Filtr | Nə seçir |
|---|---|
| `host 192.168.0.21` | mənbə **və ya** təyinat bu IP |
| `port 443` | mənbə **və ya** təyinat portu `443` |

> **Filtr həmişə əmrin sonunda** yazılır, `-i`, `-nn`, `-c` kimi flaglardan **sonra**.

---

## ➡️ `src`, `dst` və protokol filtrləri

**Kimdən?** (`src`, source) **Kimə?** (`dst`, destination) **Hansı dildə?** (`tcp`, `udp`, `icmp`)

| Filtr | Mənası |
|---|---|
| `src 192.168.0.15` | yalnız **bu cihazdan gedən** |
| `dst 192.168.0.21` | yalnız **bu cihaza gedən** |
| `src port 51234` / `dst port 443` | **mənbə** portu / **təyinat** portu |
| `tcp`, `udp`, `icmp` | yalnız bu **protokol** (`icmp` = `ping`) |
| `arp` | yalnız ARP (Sessiya 02) |

```bash
$ sudo tcpdump -nn -i eth0 icmp                          <- yalnız ping
$ sudo tcpdump -nn -i eth0 src 192.168.0.15 and dst port 443    <- 192.168.0.15-in HTTPS-ə getdiyi
```

> `host` iki istiqamətdə işləyir. **Bir istiqaməti** istəyirsənsə, `src` və ya `dst` yaz.

---

## 🔗 `and`, `or`, `not` və subnet

Filtrləri **mətn kimi birləşdir**: `and` (və), `or` (və ya), `not` (deyil).

```bash
$ sudo tcpdump -nn -i eth0 tcp and port 5432                     <- TCP VƏ port 5432
$ sudo tcpdump -nn -i eth0 'port 80 or port 443'                 <- HTTP VƏ YA HTTPS
$ sudo tcpdump -nn -i eth0 not port 22                           <- SSH XARİC hər şey
$ sudo tcpdump -nn -i eth0 net 192.168.0.0/24                    <- bütün subnet (Sessiya 02-nin /24-ü)
$ sudo tcpdump -nn -i eth0 'host 192.168.0.21 and (port 80 or port 443)'   <- mötərizə: əvvəl bu
```

| Qayda | Niyə |
|---|---|
| **`not port 22`** | SSH ilə qoşulubsansa, **öz sessiyanın trafiki ekranı doldurur** |
| **Dırnaq `' '`** | mötərizə və `or` shell-də **xüsusi məna** daşıyır, dırnaqsız xəta verir |

> Kiçik filtr yaz, **tədricən genişləndir**: əvvəl `host`, sonra `and port`, lazımsa `not`.

---

## 📖 Çıxış sətrini oxumaq: böyük şəkil

Bir paket = **bir sətir**. Bu sətri **sütun-sütun** oxumağı öyrənəcəyik:

```
14:02:11.201345 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [S], seq 2810203519, win 64240, length 0
```

| Hissə | Sualı |
|---|---|
| `14:02:11.201345` | **Nə vaxt?** |
| `IP` | **Hansı qat?** |
| `192.168.0.15.51234 > 192.168.0.21.80` | **Kimdən kimə?** |
| `Flags [S]` | **Nə dedi?** |
| `seq`, `win`, `length` | **Hansı nömrə, nə qədər yer, nə qədər data?** |

> Növbəti iki slaydda **hər hissəni ayrıca** açırıq. Bu sətri oxuya bilirsənsə, **tcpdump-un 90%-ni bilirsən**.

---

## 🔬 Sütun-sütun (1/2): vaxt, protokol, ünvanlar

```
14:02:11.201345 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [S], seq 2810203519, win 64240, length 0
```

```
14:02:11.201345        <- VAXT: saat:dəqiqə:saniyə.mikrosaniyə (paketin tutulduğu an)
IP                     <- PROTOKOL: IPv4 (IPv6 olsaydı "IP6" yazılardı)
192.168.0.15.51234     <- MƏNBƏ: IP + "." + port. Axırıncı nöqtədən sonrakı rəqəm PORTDUR (51234)
>                      <- İSTİQAMƏT oxu: "buradan ... bura"
192.168.0.21.80        <- TƏYİNAT: IP + "." + port (80 = HTTP serveri)
```

> Diqqət: IP-də **4 nöqtəli** rəqəm var, **portla birlikdə 5-ci nöqtə** görünür: `192.168.0.15.51234` = IP `192.168.0.15` + port `51234`. Çox vaxt **səhv oxunan** yer budur. Sessiya 04-dən xatırla: **client portu müvəqqətidir** (`51234`), server portu sabit (`80`).

---

## 🔬 Sütun-sütun (2/2): bayraq, seq, win, length

```
Flags [S], seq 2810203519, win 64240, length 0
```

```
Flags [S]       <- BAYRAQ: bu paket SYN-dir ("bağlanmaq istəyirəm"), cədvəl növbəti slaydda
seq 2810203519  <- SIRA NÖMRƏSİ: göndərənin "bu paketdən başlayıram" nömrəsi
ack 1001        <- (varsa) TƏSDİQ: "bu nömrəyə qədər aldım, növbətini gözləyirəm"
win 64240       <- PƏNCƏRƏ: göndərənin qəbul buferində neçə bayt yeri var (Sessiya 04, window)
length 0        <- DATA ölçüsü: paketdəki faydalı məlumat (bayt). SYN-də 0, çünki məlumat yoxdur
```

| Sahə | Praktik məna |
|---|---|
| `length 0` | **boş idarə paketi** (handshake, ACK) |
| `length 78` | **78 bayt** data (məsələn HTTP sorğusu) |
| `win 0` | "**yerim yoxdur**, dayan" (yavaşlıq səbəbi ola bilər) |

> `seq` susmaya görə **nisbi** göstərilir: handshake-dən sonra `seq 1:79` kimi **kiçik** rəqəmlər görəcəksən. Mütləq rəqəmlər üçün `-S`.

---

## 🚩 TCP bayraqları: tcpdump-da necə yazılır

Sessiya 04-dən SYN, ACK, FIN, RST bayraqlarını xatırla. tcpdump onları **qısa işarə** ilə yazır:

| tcpdump | Bayraq | Mənası (sadə dillə) |
|---|---|---|
| `[S]` | SYN | "bağlanmaq istəyirəm" |
| `[S.]` | SYN-ACK | "hazıram, sən də hazırsan?" |
| `[.]` | ACK | "aldım" (boş təsdiq) |
| `[P.]` | PUSH + ACK | "**data** göndərirəm" (sorğu/cavab) |
| `[F.]` | FIN + ACK | "bağlayıram" |
| `[R]` / `[R.]` | RST | "**rədd edirəm** / dərhal kəs" |

> **Nöqtə `.` = ACK** bayrağıdır. Yəni `[S.]` = SYN + ACK, `[P.]` = PUSH + ACK. Gedişatda **demək olar hər paket ACK daşıyır**, ona görə nöqtəni çox görəcəksən.

---

## 🚶 Gəzinti 1: handshake paketlərdə

`curl http://192.168.0.21` zamanı **ilk 3 paket** (`sudo tcpdump -nn -i eth0 'host 192.168.0.21 and port 80'`):

```
① 14:02:11.201 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [S],  seq 2810203519, win 64240, length 0   <- SYN
② 14:02:11.202 IP 192.168.0.21.80 > 192.168.0.15.51234: Flags [S.], seq 1231234567, ack 2810203520, win 65160, length 0   <- SYN-ACK
③ 14:02:11.202 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [.],  ack 1, win 502, length 0   <- ACK
```

| Addım | Nəyə baxırsan |
|---|---|
| ① `[S]` | **mənbə** `192.168.0.15.51234` **təyinat** `.80`-ə yazır |
| ② `[S.]` | **istiqamət çevrildi** (`.80 > ...51234`). `ack 2810203520` = SYN nömrəsi **+1** |
| ③ `[.]` | yenə client. `ack 1` (nisbi) = "sənin SYN-ACK-ını aldım" |

> **Nümunədir:** `seq` və port rəqəmləri sənin çıxışında **fərqli** olacaq. Dəyişməyən: **ardıcıllıq `[S]` → `[S.]` → `[.]`** və `ack` = qarşı tərəfin `seq`-i **+1**. Vaxt fərqi `0.001` san = şəbəkə **sürətlidir**.

---

## 🚶 Gəzinti 2: data və bağlanma

Handshake-dən sonra HTTP sorğusu və bağlanma:

```
④ 14:02:11.203 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [P.], seq 1:79, ack 1, win 502, length 78: HTTP: GET / HTTP/1.1   <- sorğu
⑤ 14:02:11.204 IP 192.168.0.21.80 > 192.168.0.15.51234: Flags [.],  ack 79, win 508, length 0   <- "78 bayt aldım"
⑥ 14:02:11.210 IP 192.168.0.21.80 > 192.168.0.15.51234: Flags [P.], seq 1:613, ack 79, length 612: HTTP: HTTP/1.1 200 OK   <- cavab
⑦ 14:02:11.211 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [F.], seq 79, ack 613, length 0   <- client bağlayır
⑧ 14:02:11.211 IP 192.168.0.21.80 > 192.168.0.15.51234: Flags [F.], seq 613, ack 80, length 0   <- server bağlayır
```

| Addım | Nə öyrəndin |
|---|---|
| ④ | `seq 1:79` = **78 bayt** (79 - 1). `length 78` ilə **üst-üstə düşür** |
| ⑤ | `ack 79` = "79-cu baytı gözləyirəm", yəni **78-i aldım** |
| ⑥ | server `612` bayt cavab verdi, `200 OK` |
| ⑦⑧ | **`[F.]` iki dəfə**: hər tərəf öz tərəfini bağlayır |

---

## ❌ `refused` paket səviyyəsində: RST

Sessiya 04-də `refused` = "cihaz cavab verdi: bu portda heç kim yoxdur". Paketdə:

```bash
$ curl --connect-timeout 3 http://192.168.0.21:9999
curl: (7) Failed to connect to 192.168.0.21 port 9999: Connection refused    <- proqram nə görür

$ sudo tcpdump -nn -i eth0 'host 192.168.0.21 and port 9999'
14:05:01.100 IP 192.168.0.15.51300 > 192.168.0.21.9999: Flags [S],  seq 3456789012, win 64240, length 0   <- SYN gedir
14:05:01.101 IP 192.168.0.21.9999 > 192.168.0.15.51300: Flags [R.], seq 0, ack 3456789013, win 0, length 0   <- RST dərhal qayıdır
```

| Nə görürsən | Mənası |
|---|---|
| `[S]` → **dərhal** `[R.]` | cihaz **yaşayır**, amma portda **dinləyən yoxdur** |
| cavab vaxtı `0.001` san | **tez** xəta: gözləmə yoxdur |

> **Qapı var, ev boşdur**: qapıçı "burada heç kim yaşamır" deyir. Yoxlayacağın yer: **servis işləyir? port düzgündür?** (`ss -tlnp`, Sessiya 04).

---

## ⏳ `timeout` paket səviyyəsində: sükut

`timeout` = **heç bir cavab yoxdur**. Paketdə fərq **dramatikdir**:

```bash
$ curl --connect-timeout 5 http://192.168.0.21:8080
curl: (28) Failed to connect to 192.168.0.21 port 8080: Connection timed out

$ sudo tcpdump -nn -i eth0 'host 192.168.0.21 and port 8080'
10:15:00.000 IP 192.168.0.15.51400 > 192.168.0.21.8080: Flags [S], seq 77001, win 64240, length 0   <- 1-ci cəhd
10:15:01.003 IP 192.168.0.15.51400 > 192.168.0.21.8080: Flags [S], seq 77001, win 64240, length 0   <- +1 san: təkrar
10:15:03.007 IP 192.168.0.15.51400 > 192.168.0.21.8080: Flags [S], seq 77001, win 64240, length 0   <- +2 san: təkrar
10:15:07.015 IP 192.168.0.15.51400 > 192.168.0.21.8080: Flags [S], seq 77001, win 64240, length 0   <- +4 san: təkrar
```

> Eyni `seq 77001`, **eyni port**: bu **yeni sorğu deyil, təkrardır** (retransmission). Fasilələr **ikiqat artır**: 1, 2, 4, 8 saniyə. Cavab **heç gəlmir**. Ağıllı qayda: **təkrarlanan `[S]` = itirilir və ya atılır**.

---

## 🚨 Ssenari: "SYN gedir, cavab yoxdur"

**Vəziyyət:** app `192.168.0.15` yeni DB serverinə `192.168.0.21:5432` qoşula bilmir (`timed out`). DBA deyir: "PostgreSQL işləyir, `ss -tlnp` 5432-də `LISTEN` göstərir."

Əsas sual: **paket harada itir?** Üç mümkün hal var:

| Hal | Client-də görünən | Mənası |
|---|---|---|
| **1** | **`[S]` heç görünmür** | paket **heç çıxmayıb** (client tərəfi) |
| **2** | `[S]` var, **cavab yox** | `[S]` yolda **itir** və ya serverdə **atılır** |
| **3** | `[S]` və `[S.]` var, **ACK yox** | **cavab yolu** və ya client tərəfi problemli |

> **İki tərəfdə eyni anda tut:** client-də və serverdə. Hər tərəf **"mən nə gördüm?"** deyir, fərq **itkinin yerini** göstərir. Növbəti slaydlarda hər hal.

---

## 🔎 Hal 1 və Hal 2: SYN yoxdur / SYN var, cavab yox

**Hal 1: client-də `[S]` yoxdur** (tutmaq: `sudo tcpdump -nn -i eth0 host 192.168.0.21`):

```
(heç nə çıxmır)       <- proqram heç paket göndərmir
```
Səbəb: **yanlış IP/ad** (DNS), **yanlış marşrut** (`ip r`), **local firewall egress qaydası** və ya proqram **başqa IP-yə** qoşulur. Yoxla: `ip r get 192.168.0.21`, `getent hosts ad`.

**Hal 2: client-də `[S]` təkrarlanır.** İndi **serverdə** tut: `sudo tcpdump -nn -i eth0 port 5432`

| Serverdə `[S]` | Nə deməkdir | Haradadır problem |
|---|---|---|
| **yoxdur** | paket **serverə çatmır** | **aradakı** firewall, security group, marşrut |
| **var**, `[S.]` **yoxdur** | server paketi **alır**, amma cavab vermir | **server host firewall** (`DROP`) |
| **var**, `[R.]` qayıdır | server **rədd edir** | `refused`: servis/port problemidir |

> Əhəmiyyətli: tcpdump **gələn paketi firewall atmazdan əvvəl** görür. Yəni "serverdə `[S]` görünür amma cavab yoxdur" = paket **kartdadır, sistem onu atır** (host firewall). Gedən paketi isə **local firewall bloklayıbsa**, tcpdump onu **görməyə bilər**.

---

## 🔎 Hal 3: SYN-ACK var, ACK yoxdur

Client-də **`[S]` və `[S.]` görünür**, amma `[.]` yoxdur. Server `[S.]` göndərməyi **təkrarlayır**:

```
10:30:00.000 IP 192.168.0.15.51500 > 192.168.0.21.5432: Flags [S],  seq 900, win 64240, length 0     <- SYN
10:30:00.001 IP 192.168.0.21.5432 > 192.168.0.15.51500: Flags [S.], seq 7000, ack 901, length 0      <- SYN-ACK gəldi
10:30:01.002 IP 192.168.0.21.5432 > 192.168.0.15.51500: Flags [S.], seq 7000, ack 901, length 0      <- server təkrarlayır: ACK gəlmir
10:30:03.004 IP 192.168.0.21.5432 > 192.168.0.15.51500: Flags [S.], seq 7000, ack 901, length 0      <- yenə
```

| Kimdə görünür | Mənası |
|---|---|
| **Client** `[S.]` görür, **ACK göndərmir** | client **host firewall** gələn cavabı atır (stateful qayda yoxdur) |
| **Client** ACK göndərir, **serverdə görünmür** | ACK **yolda itir**: **asimmetrik marşrut** və ya ara firewall |

> Server **`[S.]`-ni təkrarlayır** = "ACK almadım". Bu hal **nadirdir**, ən çox **asimmetrik marşrut** (gedən və qayıdan yol fərqli) və ya **yarımçıq firewall qaydası** ilə olur. Sessiya 05-dəki **stateful** anlayışını xatırla.

---

## 🧭 3 haldan nəticə: harada axtarmalı?

| Müşahidə | Ehtimal | Növbəti addım |
|---|---|---|
| Client-də **`[S]` yox** | client tərəfi | `ip r get`, DNS, local qaydalar |
| `[S]` var, serverdə **yox** | **aradakı** firewall / SG / marşrut | SG, NACL, router qaydaları (Sessiya 05) |
| Serverdə `[S]` var, **cavab yox** | server **host firewall** DROP | `iptables`/`ufw` qaydaları |
| `[S]` → **`[R.]`** | **servis/port** | `ss -tlnp`, portu yoxla |
| `[S]`, `[S.]`, ACK yox | **cavab yolu**, asimmetrik | marşrutlar, stateful qayda |

> **Dərs:** `ss -tlnp` "servis dinləyir" desə də, **paket serverə çatmaya bilər**. "İşləyir" ilə "əlçatandır" **fərqli şeylərdir**. Paketi **hər iki ucda** gör, itkinin yeri bəlli olar.

---

## 🌐 DNS sorğusunu tutmaq (`port 53`)

DNS adətən **UDP 53** (Sessiya 04). Başqa terminalda `dig example.com` et:

```bash
$ sudo tcpdump -nn -i eth0 port 53
14:10:05.100 IP 192.168.0.15.43210 > 192.168.0.1.53: 4711+ A? example.com. (29)       <- sorğu: "example.com-un IPv4-ü nədir?"
14:10:05.112 IP 192.168.0.1.53 > 192.168.0.15.43210: 4711 1/0/0 A 93.184.216.34 (45)  <- cavab
```

| Hissə | Mənası |
|---|---|
| `4711` | sorğu **ID**-si: sorğu və cavab **eyni ID** daşıyır |
| `+` | "rekursiya istəyirəm" (resolver özü axtarsın) |
| `A?` | **A record** sorğusu (adın IPv4 ünvanı) |
| `1/0/0` | cavabda **1** cavab, 0 səlahiyyətli, 0 əlavə |
| `(29)`, `(45)` | DNS mesajının **bayt** ölçüsü |

> **Vaxt fərqi** (`.100` → `.112` = 12 ms) = **DNS gecikməsi**. Cavab **heç gəlmirsə** (yalnız sorğu sətri var), resolver (soruşan DNS server) **əlçatmazdır**. Çox "yavaş sayt" şikayəti əslində **DNS**-dir.

---

## 🔓 HTTP görünür, HTTPS görünmür

`-A` paketin məzmununu **mətn** kimi göstərir. **HTTP** (şifrəsiz) tamamilə oxunur:

```bash
$ sudo tcpdump -nn -i eth0 -A -s 0 'tcp port 80 and host 192.168.0.21'
14:20:05.311 IP 192.168.0.15.51234 > 192.168.0.21.80: Flags [P.], seq 1:79, ack 1, length 78: HTTP: GET /login HTTP/1.1
GET /login HTTP/1.1                       <- sorğu sətri açıq mətn
Host: 192.168.0.21                        <- başlıqlar açıq
User-Agent: curl/8.5.0
Authorization: Basic dXNlcjpwYXNz         <- "user:pass" (base64, şifrə DEYİL!)
```

| Protokol | Port | `-A` ilə nə görürsən |
|---|---|---|
| **HTTP** | `80` | sorğu, başlıqlar, **parol/cookie** açıq |
| **HTTPS** | `443` | **şifrəli zibil** (`.....x.\..`). Yalnız IP, port, ölçü, vaxt |

> HTTPS-də məzmun **görünmür**, amma tcpdump hələ də **handshake, `[R.]`, gecikmə, paket ölçüsü** göstərir. Yəni **bağlantı problemini** tapırsan, **məzmunu oxumursan**. Bu **yaxşıdır**: şifrələmə məhz bunun üçündür.

---

## 💾 `.pcap` yazmaq və oxumaq

Canlı axın **sürətlə keçir**. Faylı yaz, **sonra sakit oxu** və ya başqasına göndər:

```bash
$ sudo tcpdump -nn -i eth0 -w /tmp/web.pcap -c 500 'port 80 or port 443'    <- YAZ (ekrana çıxmır)
500 packets captured
$ tcpdump -nn -r /tmp/web.pcap | head                                      <- OXU (sudo lazım deyil)
$ tcpdump -nn -r /tmp/web.pcap 'tcp[tcpflags] & tcp-rst != 0'              <- yalnız RST paketləri
$ scp user@192.168.0.21:/tmp/web.pcap .                                    <- Wireshark üçün yüklə
```

| Alət | Nə edir |
|---|---|
| **tcpdump** `-w`/`-r` | serverdə **tut**, terminalda **süz** |
| **Wireshark** | `.pcap` faylını **qrafik** açır: rəngli paketlər, **"Follow TCP Stream"** (söhbəti bütöv göstərir) |

> Praktik iş bölgüsü: **serverdə** (GUI yoxdur) `tcpdump -w`, **öz kompüterində** Wireshark ilə analiz. Wireshark ayrıca alətdir, burada yalnız **körpünü** qururuq.

---

## ⚠️ Təhlükəsizlik və performans qaydaları

| Qayda | Niyə |
|---|---|
| **Yalnız öz sistemində / icazə ilə** | başqasının trafikini tutmaq **hüquqi və etik pozuntudur** |
| **`.pcap` = həssas fayl** | HTTP-də **parol, token, cookie** ola bilər. Paylaşma, işi bitəndə **sil** |
| **AI-yə göndərmə** | həssas hissələri **sil**, yalnız **başlıq sətirlərini** ver (`-A`-sız) |
| **`-c` və ya vaxt limiti** | **fayl və disk dolmasın** |
| **Dar filtr** (`host`, `port`) | CPU yükü azalır, **az məxfi məlumat** tutulur |
| **`-s` ilə ölçü** | yalnız başlıq lazımdırsa kiçik götür (məs. `-s 96`), **az disk** |

> Yüksək yüklü production serverdə **filtrsiz** tutmaq **paket itkisinə** və yük artımına səbəb ola bilər. Əvvəl **dar filtr + `-c`**, sonra genişlət.

---

## 🧪 Lab 10a — handshake-i canlı gör

**Məqsəd:** `curl` zamanı `[S]` → `[S.]` → `[.]` ardıcıllığını tutmaq. **İki terminal** aç.

```bash
# TERMINAL 1: dinlə (loopback, lokal server üçün)
$ sudo tcpdump -nn -i lo -c 10 'port 8080'

# TERMINAL 2: kiçik server qaldır və sorğu göndər
$ python3 -m http.server 8080 --bind 127.0.0.1 &       <- fonda server
$ curl -s -o /dev/null http://127.0.0.1:8080/
```

| Gözlənilən nəticə | Mənası |
|---|---|
| Terminal 1-də `Flags [S]` → `[S.]` → `[.]` | **handshake** tamamdır |
| Sonra `[P.]` (data), `[.]` (təsdiq), `[F.]` (bağlanma) | sorğu, cavab, **bağlanma** |
| `127.0.0.1.8080` təyinat, **müvəqqəti** mənbə port | **client portu** hər dəfə fərqlidir |

> Çıxışlar **nümunədir**. İş bitəndə serveri dayandır: `kill %1`. **Tapşırıq:** hər sətrin `seq`/`ack` rəqəmlərini **kağızda** izlə: `ack` = qarşı tərəfin `seq`-i + data uzunluğu.

---

## 🧪 Lab 10b — `refused` və `timeout` yarat

> ⚠️ **Diqqət:** `iptables` qaydası **yalnız `lo` (loopback)** üzərindədir, SSH-a toxunmur. Yenə də **qaydanı sonda mütləq sil**. Geri qaytarma: `sudo iptables -D INPUT -i lo -p tcp --dport 8080 -j DROP`.

```bash
# TERMINAL 1:   sudo tcpdump -nn -i lo 'port 8080'
# TERMINAL 2:
$ kill %1                                                 # serveri dayandır (Lab 10a-dan)
$ curl --connect-timeout 2 http://127.0.0.1:8080          # 1) REFUSED: [S] -> [R.] dərhal
$ python3 -m http.server 8080 --bind 127.0.0.1 &          # serveri geri qaldır
$ sudo iptables -I INPUT -i lo -p tcp --dport 8080 -j DROP    # firewall: DROP
$ curl --connect-timeout 2 http://127.0.0.1:8080          # 2) TIMEOUT: [S] təkrarlanır
$ sudo iptables -D INPUT -i lo -p tcp --dport 8080 -j DROP    # QAYDANI SİL
$ curl -s -o /dev/null http://127.0.0.1:8080/             # 3) yenə işləyir
```

| Hal | `curl` | tcpdump |
|---|---|---|
| 1) server **yoxdur** | `Connection refused` | `[S]` → **`[R.]`** |
| 2) **DROP** qaydası | `timed out` | `[S]` **təkrarlanır** (1, 2, 4 san) |
| 3) qayda silindi | cavab gəlir | tam handshake |

> Üç vəziyyət, **üç fərqli paket nümunəsi**. Bu fərq **real problemdə** səbəbi göstərir. Lab sonunda `sudo iptables -S INPUT` ilə qaydanın **silindiyini** yoxla.

---

## 🪜 Tipik səhvlər

| Səhv | Nəticə | Düzgün |
|---|---|---|
| **`sudo` yazmamaq** | `permission` xətası | `sudo tcpdump ...` |
| **`-nn` yazmamaq** | yavaş, DNS paketi qarışır | həmişə `-nn` |
| **Filtrsiz tutmaq** | ekran dolur, vacib sətir itir | `host`/`port` |
| **`-i` yazmamaq** | **yanlış kartı** dinləyirsən | `-i eth0` və ya `-i any` |
| **SSH-da `not port 22` yazmamaq** | öz sessiyanın trafiki ekranı doldurur | `not port 22` |
| **Yalnız bir tərəfdə tutmaq** | itkinin yerini bilmirsən | **iki ucda** tut |
| **`.pcap`-ı paylaşmaq** | parol/token sızır | həssas fayl kimi saxla |

---

## 🔁 Simptom → paketdə nə görürsən

| Simptom | Paketdə | Mənası |
|---|---|---|
| `Connection refused` | `[S]` → dərhal `[R.]` | **servis/port** problemi |
| `timed out` | `[S]` **təkrarlanır** | **DROP**: firewall, SG, marşrut |
| Qoşulur, sonra `reset` | data sonra **`[R]`** | qarşı tərəf bağlantını **kəsdi** |
| Yavaş, kanal boş | `win 0` və ya təkrarlanan **seq** | **window / itki** |
| Sayt yavaş açılır | DNS sorğu→cavab arası **uzun** | **DNS** problemi |
| Bağlantı qurulur, tətbiq xətalıdır | handshake **normal**, `[P.]` sonra xəta | problem **şəbəkədə deyil**, tətbiqdədir |

> Məntiq: **paket səviyyəsində normaldırsa** (handshake tamam), şəbəkəni **təmizlə**, tətbiq logunu yoxla.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Bu `tcpdump` çıxışını sütun-sütun izah et: vaxt, mənbə, təyinat, Flags, seq, ack, length. Problemi tap: [yapışdır]"*
- *"`192.168.0.0/24` subnet-dən gələn, port 443-ə gedən və yalnız SYN paketlərini tutan `tcpdump` filtri yaz. Hər hissəni izah et."*
- *"Client və serverin `tcpdump` nəticələri: [A], [B]. `[S]` gedir, cavab yoxdur. Paket harada itir?"*

</div>

<div class="ai-tip">💡 Paketlərdə <b>parol, token, cookie</b> ola bilər. AI-yə göndərməzdən əvvəl həssas hissələri <b>sil</b> və ya yalnız <b>başlıq sətirlərini</b> ver (<code>-A</code> olmadan).</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **tcpdump** | şəbəkənin "kamerası": paketi **göstərir** (`sudo` lazımdır) |
| **İnterfeys** | `-i eth0`, `-i any`. Tapmaq: `ip -br a` |
| **Əsas flaglar** | `-nn` (həmişə), `-c N`, `-v`, `-A`, `-X`, `-s`, `-w`, `-r` |
| **Filtr (BPF)** | `host`, `port`, `src`, `dst`, `tcp/udp/icmp`, `and/or/not`, `net` |
| **Sətir** | vaxt, **mənbə.port > təyinat.port**, `Flags`, `seq`, `ack`, `win`, `length` |
| **Bayraqlar** | `[S]` `[S.]` `[.]` `[P.]` `[F.]` `[R.]` |
| **`refused`** | `[S]` → dərhal `[R.]` |
| **`timeout`** | `[S]` **təkrarlanır** (1, 2, 4 san), cavab yox |
| **Qayda** | **iki ucda** tut, dar filtr, `-c`, `.pcap` həssasdır |

**Növbəti dərs:** Sessiya 11 — **diaqnostika alətləri**: `ping` → `traceroute`/`mtr` → `ss` → `curl` → `dig`.

---

## ✍️ Tapşırıqlar

1. **Oxu:** `192.168.0.15.51234 > 192.168.0.21.80: Flags [S.], seq 500, ack 101` sətrində **mənbə IP, mənbə port, təyinat port** və bayraq nədir?
2. **Filtr:** `192.168.0.21` ilə **yalnız `5432` portunda** olan trafiki tutan əmri yaz. SSH-ı **necə xaric edərsən**?
3. **Handshake:** `curl` zamanı `[S]`, `[S.]`, `[.]` paketlərini tut. `ack` rəqəmlərinin **niyə +1** olduğunu izah et.
4. **DNS:** `port 53` ilə `dig` sorğusunu və cavabını tut. **Sorğu-cavab vaxt fərqini** hesabla.
5. **`.pcap`:** `-w` ilə yaz, `-r` ilə oxu, yalnız **`[R]`** paketlərini süz.
6. **Lab 10b:** `refused` və `timeout` yarat. Hər biri üçün **tcpdump sətrini** yaz və fərqi izah et.
7. **Ssenari:** Client-də `[S]` **təkrarlanır**, serverdə `[S]` **görünmür**. Problem **haradadır** və **ilk nəyi yoxlayarsan**?
8. **Təhlükəsizlik:** HTTP və HTTPS-i `-A` ilə tut. **Hansında parol görünür**, niyə? `.pcap` faylı ilə **nə etməməlisən**?

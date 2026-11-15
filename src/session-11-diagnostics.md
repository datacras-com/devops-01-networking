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

# 🧰 Diaqnostika alətləri

## "İşləmir" deməkdən "burada qırılır" deməyə: alətləri düzgün ardıcıllıqla işlət

```
  Sən ──► [ gateway ] ──► [ provayder ] ──► [ internet ] ──► [ server ] ──► [ tətbiq ]
   │          │                │                 │               │              │
  ip a      ping GW        traceroute         ping IP          ss / nc        curl
  ip r                       mtr              ping ad         (port açıqdır?) (cavab düzgündür?)
                                               dig
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 11 · Chapter 5 — Diaqnostika

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Sistemli yanaşma:** "sayt açılmır" problemini **aşağıdan yuxarı** addım-addım daralt
- **`ping`:** cihaz **çatır?** gecikmə və itki
- **`traceroute` və `mtr`:** paket yolda **harada** itir və ya yavaşlayır
- **`ss` və `nc`:** server portu **dinləyir?** bağlantı **qurulur?**
- **`curl`:** sorğunun **hansı mərhələsi** yavaşdır (DNS, TCP, TLS, server)
- **`dig`:** ad **düzgün** IP-yə çevrilir?
- **Ssenarilər:** "sayt açılmır" və "yavaşdır" üçün qərar ağacı
- **3 canlı lab:** nərdivan, `curl` vaxt ölçmə, **gizli problemi tap**

> **Sessiya 01**-də diaqnostika nərdivanını, **Sessiya 04**-də `refused` və `timeout` fərqini, **Sessiya 10**-da `tcpdump`-u gördük. Bu dərsdə **hamısını bir araya gətirib** real iş axınına çeviririk.

---

## 🧩 Problem: "işləmir" faydasız hesabatdır

| Kim nə deyir | Problem |
|---|---|
| "Sayt açılmır" | **hansı** hissə? DNS, şəbəkə, port, tətbiq? |
| "İnternet yoxdur" | **kimdə**? yalnız səndə, yoxsa **hamıda**? |
| "Server yavaşdır" | **harada** yavaşdır? şəbəkədə, yoxsa tətbiqdə? |

**Yanlış yol:** təsadüfi əmrlər yazıb nəticəyə **baxmadan** server restart etmək.
**Düzgün yol:** hər addımda **bir sual** ver, cavab **bəli/xeyir** olsun, problemi **yarıya böl**.

> Diaqnostika **ovçuluq** deyil, **axtarış** işidir: hər alət **bir sualı** cavablandırır. Cavab "bəli"-dirsə, **növbəti suala** keç. "Xeyir"-dirsə, **problemin yerini tapdın**.

---

## 🪜 Diaqnostika nərdivanı (tam versiya)

| # | Sual | Alət | "Xeyir" olarsa |
|---|---|---|---|
| 1 | **Mənim** interfeysim və IP-m var? | `ip a` | kabel, Wi-Fi, DHCP |
| 2 | **Gateway** var və çatır? | `ip r`, `ping GW` | yerli şəbəkə |
| 3 | **İnternetə** çatıram? | `ping 8.8.8.8` | provayder, routing |
| 4 | **Ad** IP-yə çevrilir? | `dig ad` | **DNS** |
| 5 | Yol **harada** qırılır? | `traceroute`, `mtr` | aradakı hop |
| 6 | **Port** açıqdır? | `nc -vz IP PORT` | firewall, servis |
| 7 | Servis **dinləyir?** (serverdə) | `ss -tlnp` | servis dayanıb |
| 8 | **HTTP** cavabı düzgündür? | `curl -v` | tətbiq xətası |
| 9 | Hələ də aydın deyil | `tcpdump` | paketə bax (S10) |

> Əvvəlcə **sadə və ucuz** alətlər (`ip`, `ping`), ən sonda **ağır** alət (`tcpdump`). Nərdivanı **yuxarıdan aşağı** oxu: hər pillə əvvəlkinin **üstündə** qurulub.

---

## 🗺️ Hansı sual, hansı alət?

| Sual | Alət | Hansı qat |
|---|---|---|
| Cihaz **çatır?** | `ping` | 3 |
| Yol **hansı** hop-lardan keçir? | `traceroute`, `mtr` | 3 |
| Port **açıqdır?** | `nc -vz`, `curl` | 4 |
| Kim **hansı portu** dinləyir? | `ss -tlnp` | 4 |
| Ad **hansı IP**-dir? | `dig` | 7 (DNS) |
| HTTP cavabı **nədir**, **hansı mərhələ** yavaşdır? | `curl -v`, `curl -w` | 7 |
| **Kanal sürəti** nədir? | `iperf3` | 4 |
| Paket **harada** itdi? | `tcpdump` | 2–4 |

> Bu cədvəl **Sessiya 03**-dəki qatlarla eynidir. Hər alətin **bir qatı** yoxlayır: alət seçmək = **qat seçmək**.

---

## 📡 `ping`: cihaz çatır?

`ping` hədəfə **ICMP** (Internet Control Message Protocol, şəbəkənin "xidməti mesaj" protokolu) **echo** sorğusu göndərir. Hədəf **echo cavabı** qaytarır.

```bash
$ ping -c 3 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=116 time=14.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=116 time=13.9 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=116 time=14.8 ms

--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 13.9/14.3/14.8/0.38 ms
```

| Hissə | Mənası |
|---|---|
| `time=14.2 ms` | **RTT** (Round-Trip Time): sorğu gedib cavab qayıdana qədər vaxt |
| `ttl=116` | **TTL** (Time To Live): paketin qalan "ömrü" (hər router 1 azaldır) |
| `0% packet loss` | itən paketlərin **faizi** |
| `mdev` | gecikmənin **dəyişkənliyi** (böyükdürsə, bağlantı **qeyri-sabitdir**) |

---

## 🔎 `ping` xətalarını oxumaq

| Çıxış | Mənası | Növbəti addım |
|---|---|---|
| `Destination Host Unreachable` (**From sənin IP-dən**) | **yerli** şəbəkədə hədəf tapılmadı (ARP cavabı yox) | IP/maska, kabel, hədəf yoxdur |
| `Destination Net Unreachable` | marşrut **yoxdur** | `ip r` |
| **Heç nə** (cavab yox, `100% packet loss`) | paket itir **və ya** ICMP bağlıdır | `traceroute`, başqa üsul (`nc`, `curl`) |
| `Name or service not known` | ad IP-yə **çevrilmədi** | **DNS**: `dig` |
| `ping: connect: Network is unreachable` | **default gateway yoxdur** | `ip r`, DHCP |

> ⚠️ **`ping` cavabsızdırsa, server dayanıb demək deyil.** Çox firewall və Security Group ICMP-ni **bağlayır**. Əvvəl **başqa üsulla** yoxla: `nc -vz IP 443` və ya `curl`.

> `ping` işləyir amma sayt açılmır? Qat 3 **sağlamdır**, problem **yuxarıdadır**: port, firewall, tətbiq.

---

## 🛣️ `traceroute`: yol hansı hop-lardan keçir?

**Hop** = paketin yolda keçdiyi **hər router**. `traceroute` **TTL** ilə hiylə işlədir: TTL = 1 olan paket 1-ci router-də **ölür**, router "vaxtı bitdi" mesajı göndərir. Sonra TTL = 2, TTL = 3...

```
  TTL=1 ──► [ R1 ] ✖ "vaxt bitdi" ──► sən R1-i görürsən
  TTL=2 ──► [ R1 ] ──► [ R2 ] ✖ ───► sən R2-ni görürsən
  TTL=3 ──► [ R1 ] ──► [ R2 ] ──► [ R3 ] ✖ ──► ... hədəfə çatana qədər
```

```bash
$ sudo apt install -y traceroute mtr-tiny      # yoxdursa quraşdır
$ traceroute -n 8.8.8.8                        # -n: ad axtarma, IP göstər
```

> Hər hop üçün **3 ölçmə** göstərilir. Beləcə **hansı hop-dan sonra gecikmə sıçrayır** və ya **cavab kəsilir** görürsən.

---

## 👀 `traceroute` çıxışını oxumaq

```bash
$ traceroute -n 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.0.1    1.2 ms   1.0 ms   1.1 ms      <- gateway (yerli)
 2  10.20.0.1      4.5 ms   4.3 ms   4.8 ms      <- provayderin ilk router-i
 3  * * *                                          <- bu hop cavab vermir
 4  172.16.5.9    12.1 ms  12.4 ms  12.0 ms      <- yol davam edir
 5  8.8.8.8       14.2 ms  14.0 ms  14.5 ms      <- hədəf
```

| Gördüyün | Mənası |
|---|---|
| `* * *` **ortada**, sonra yol davam edir | router ICMP-yə cavab **vermir**, normaldır |
| `* * *` **sonuna qədər** | paket **bu hop-dan sonra** itir (firewall/marşrut) |
| gecikmə **bir hop-da kəskin artır** və sonra da yüksək qalır | **həmin hop** və ya ondan sonrakı link yavaşdır |
| yalnız **bir hop** yüksək, sonrakılar normal | router öz cavabını **gec verir**, real problem **deyil** |

> Qayda: **ortadakı `* * *` problem deyil.** Problem yalnız **sonuna qədər davam edən** `* * *`-dir.

---

## 📈 `mtr`: canlı traceroute + ping

`mtr` (My Traceroute) hər hop-a **təkrar-təkrar** paket göndərir və **statistika** yığır. Tək `traceroute` bir anlıq şəkildir, `mtr` isə **film**.

```bash
$ mtr -rwn -c 10 8.8.8.8          # -r: hesabat, -w: tam yaz, -n: IP, -c 10: 10 paket
HOST: vm01             Loss%   Snt   Last    Avg   Best   Wrst  StDev
  1.|-- 192.168.0.1     0.0%    10    1.1    1.2    0.9    2.0    0.3
  2.|-- 10.20.0.1       0.0%    10    4.5    4.6    4.2    5.1    0.3
  3.|-- ???            100.0    10    0.0    0.0    0.0    0.0    0.0
  4.|-- 172.16.5.9      0.0%    10   12.1   12.3   11.9   13.0    0.4
  5.|-- 8.8.8.8         0.0%    10   14.2   14.3   13.9   14.9    0.3
```

| Sütun | Mənası |
|---|---|
| `Loss%` | həmin hop-da **itki** faizi |
| `Snt` | göndərilən paket sayı |
| `Last / Avg / Best / Wrst` | son, orta, ən yaxşı, ən pis gecikmə (ms) |
| `StDev` | **dəyişkənlik** (böyükdürsə, qeyri-sabit) |

> **Ən vacib qayda:** yalnız **sonuncu hop-da** itki varsa, **real itki** var. Ortada `100%` itki, amma **sonrakı hop-larda 0%**-dirsə, router sadəcə ICMP cavabını **məhdudlaşdırır**. Problem **yoxdur**.

---

## 🔌 `ss`: server portu dinləyir?

`ss` (socket statistics) **serverin öz daxilində** hansı portun **açıq** olduğunu və **kimin** dinlədiyini göstərir. Sessiya 04-də `ss -tan` ilə bağlantıları görmüşdük, indi **dinləyənlərə** baxırıq.

```bash
$ sudo ss -tlnp
State   Recv-Q  Send-Q   Local Address:Port    Peer Address:Port   Process
LISTEN  0       4096           0.0.0.0:22           0.0.0.0:*     users:(("sshd",pid=812,fd=3))
LISTEN  0       511            0.0.0.0:80           0.0.0.0:*     users:(("nginx",pid=930,fd=6))
LISTEN  0       244          127.0.0.1:5432         0.0.0.0:*     users:(("postgres",pid=1010,fd=5))
```

| Flag | Mənası |
|---|---|
| `-t` | yalnız **TCP** |
| `-l` | yalnız **LISTEN** (dinləyən) |
| `-n` | **rəqəm** göstər (ad axtarma) |
| `-p` | **proses** adı (`sudo` lazımdır) |

> 🔑 **`Local Address` çox vacibdir:** `0.0.0.0:80` = **bütün** kartlardan qəbul edir. `127.0.0.1:5432` = **yalnız server özü**. Kənardan `5432`-yə qoşulmaq istəyirsən, amma `127.0.0.1`-dədirsə, `refused` alacaqsan. Bu **ən tipik** "servis işləyir, amma çatmır" səbəbidir.

---

## 🚪 `nc` və `curl`: kənardan port yoxla

`ss` **serverin içindən** baxır. Client tərəfdən **real çatır?** sualı üçün `nc` (netcat) işlədirik:

```bash
$ nc -vz 192.168.0.21 443          # -v: ətraflı, -z: yalnız qoşul, data göndərmə
Connection to 192.168.0.21 443 port [tcp/https] succeeded!      <- AÇIQDIR

$ nc -vz 192.168.0.21 5432
nc: connect to 192.168.0.21 port 5432 (tcp) failed: Connection refused   <- servis YOXDUR

$ nc -vz -w 3 192.168.0.21 8443    # -w 3: 3 saniyə gözlə
nc: connect to 192.168.0.21 port 8443 (tcp) timed out: Operation now in progress   <- DROP
```

| Nəticə | Mənası | Növbəti |
|---|---|---|
| `succeeded` | **port açıq**, TCP qurulur | tətbiq qatına bax (`curl`) |
| `refused` | cihaz cavab verdi, **servis yoxdur** | serverdə `ss -tlnp` |
| `timed out` | **cavab yoxdur** | firewall, SG, marşrut: `traceroute`, `tcpdump` |

> Bu cədvəl **Sessiya 04**-dəki `refused` və `timeout` qaydasıdır. İndi onu **bir əmrlə** yoxlayırsan.

---

## 🌐 `curl`: HTTP cavabı və mərhələ vaxtları

`curl -v` sorğunun **bütün addımlarını** göstərir. Ən vacib hissə:

```bash
$ curl -sv https://tm.datacras.com -o /dev/null 2>&1 | grep -E '^\* (Trying|Connected|TLS)|^< HTTP'
*   Trying 203.0.113.10:443...                    <- DNS işlədi, IP tapıldı
* Connected to tm.datacras.com (203.0.113.10) port 443    <- TCP qurulub
* TLSv1.3 (OUT), TLS handshake, Client hello (1):   <- TLS başlayır
< HTTP/2 200                                          <- server cavab verdi
```

**Hansı mərhələ yavaşdır?** `-w` ilə hər mərhələnin **vaxtını** ölç:

```bash
$ curl -s -o /dev/null -w 'dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ilk_bayt=%{time_starttransfer} cəmi=%{time_total}\n' https://tm.datacras.com
dns=0.012 tcp=0.031 tls=0.078 ilk_bayt=0.142 cəmi=0.150
```

> ⚠️ Bu vaxtlar **yığılandır** (kumulativ): `tcp=0.031` = DNS + TCP **cəmi**. Hər mərhələnin öz vaxtı = **fərq**: TCP = 0.031 − 0.012 = **19 ms**.

---

## ⏱️ `curl` vaxtlarını oxumaq

| Mərhələ | Hesabla | Böyükdürsə |
|---|---|---|
| **DNS** | `time_namelookup` | DNS server yavaşdır / əlçatmazdır |
| **TCP** | `time_connect − time_namelookup` | **şəbəkə gecikməsi**, paket itkisi |
| **TLS** | `time_appconnect − time_connect` | sertifikat zənciri, CPU, TLS ayarı |
| **Server** | `time_starttransfer − time_appconnect` | **tətbiq yavaşdır** (verilənlər bazası, kod) |
| **Yükləmə** | `time_total − time_starttransfer` | **cavab böyükdür** və ya bant eni az |

```
  dns=2.100  tcp=2.120  tls=2.170  ilk_bayt=2.230  cəmi=2.240
   └─ 2.1 san DNS-dədir! Qalan hər şey sürətlidir.  → problem DNS-dir, server yox
```

> Bu bir əmrlə **"sayt yavaşdır"** şikayətini **dəqiq mərhələyə** bağlayır. Developer-lə mübahisə əvəzinə **rəqəm** göstərirsən.

---

## 🔍 `dig`: ad düzgün IP-yə çevrilir?

```bash
$ dig +short tm.datacras.com                  # yalnız cavab
203.0.113.10

$ dig @8.8.8.8 +short tm.datacras.com         # konkret DNS serverdən soruş
203.0.113.10

$ dig tm.datacras.com | grep -E 'status|Query time|SERVER'
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41233
;; Query time: 12 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
```

| `status` | Mənası |
|---|---|
| `NOERROR` | uğurlu (cavab boş ola bilər: **tip yoxdur**) |
| `NXDOMAIN` | belə ad **mövcud deyil** (yazılış səhvi və ya qeyd yoxdur) |
| `SERVFAIL` | DNS server **cavab verə bilmədi** (zona xətası, DNSSEC) |
| **cavab yoxdur** | DNS server **əlçatmazdır** |

> **Müqayisə hiyləsi:** `dig ad` (sistem resolver) və `dig @8.8.8.8 ad` (kənar) **fərqli** cavab verirsə, problem **sənin DNS serverindədir**, adın özündə yox. DNS-in tam izahı: **Sessiya 07**.

---

## 🧭 `ip`: yerli vəziyyət

Hər diaqnostika **öz maşınından** başlayır. Üç əmr kifayətdir:

```bash
$ ip -br a                         # interfeys və IP-lər (qısa)
lo      UNKNOWN  127.0.0.1/8
eth0    UP       192.168.0.15/24

$ ip r                             # marşrut cədvəli
default via 192.168.0.1 dev eth0
192.168.0.0/24 dev eth0 proto kernel scope link src 192.168.0.15

$ ip r get 8.8.8.8                 # bu hədəfə paket HARADAN çıxacaq?
8.8.8.8 via 192.168.0.1 dev eth0 src 192.168.0.15 uid 1000
```

| Yoxla | Problem varsa |
|---|---|
| `eth0` **`UP`**, IP **var**? | kabel, DHCP, interfeys `DOWN` |
| `default via ...` **var**? | gateway yoxdur → internet **yoxdur** |
| `ip r get HEDEF` **düzgün** interfeysi göstərir? | **yanlış marşrut** (məsələn, VPN trafiki udur) |

> `ip r get` **ən az bilinən, ən faydalı** əmrdir: paket **hansı yolla** gedəcəyini **göndərmədən** göstərir.

---

## 🚀 `iperf3`: kanal sürətini ölç (bonus)

`ping` **gecikməni** ölçür, `iperf3` isə **bant enini** (saniyədə nə qədər data keçir). Hər iki ucda **quraşdırılmalıdır**.

```bash
# SERVERDƏ:                      # CLIENT-DƏ:
$ iperf3 -s                      $ iperf3 -c 192.168.0.21 -t 5
                                 [ ID] Interval        Transfer     Bitrate
                                 [  5] 0.00-5.00 sec   562 MBytes   943 Mbits/sec   sender
                                 [  5] 0.00-5.00 sec   561 MBytes   941 Mbits/sec   receiver
```

| Nəticə | Mənası |
|---|---|
| Bitrate **gözlənilənə yaxın** (1 Gbit kartda ~940 Mbit/s) | kanal **sağlamdır** |
| Çox **aşağıdır** | kanal **dolu**, kabel, **duplex** problemi, itki |
| `iperf3` yaxşıdır, **tətbiq yavaşdır** | problem **şəbəkədə deyil** |

> ⚠️ `iperf3` kanalı **doldurur**. Production-da **icazəsiz** və **iş saatlarında** işlətmə. Test üçün **qısa** (`-t 5`) götür.

---

## 🎬 Ssenari 1: "Sayt açılmır"

```
  1. ip a, ip r            ──► IP və gateway yoxdur?       ──► BƏLİ: şəbəkə/DHCP problemi
        │ yox
  2. ping gateway          ──► cavab yoxdur?               ──► BƏLİ: yerli şəbəkə (kabel, switch)
        │ var
  3. ping 8.8.8.8          ──► cavab yoxdur?               ──► BƏLİ: provayder / routing
        │ var                                                    (traceroute ilə harada)
  4. dig ad                ──► NXDOMAIN / cavab yoxdur?    ──► BƏLİ: DNS problemi
        │ düzgün IP
  5. nc -vz IP 443         ──► refused / timeout?          ──► refused: servis dayanıb
        │ succeeded                                            timeout: firewall/SG
  6. curl -v               ──► 5xx / 4xx / sertifikat?     ──► BƏLİ: tətbiq / TLS problemi
        │ 200
  7. Hamısı yaxşıdır       ──► problem səndə (brauzer cache, proxy, VPN)
```

> Hər addım **bir sualı** cavablandırır. **İlk "bəli"** problemin yeridir. Qalan addımlara **keçməyə ehtiyac yoxdur**.

---

## 🎬 Ssenari 2: "Yavaşdır"

| Müşahidə | Alət | Nəticə → səbəb |
|---|---|---|
| `curl -w`: **dns** böyükdür | `dig`, `resolvectl` | DNS server yavaşdır |
| `curl -w`: **tcp** böyükdür | `ping`, `mtr` | **gecikmə / itki** yolda |
| `mtr`: **son hop-da Loss%** var | `mtr -c 50` | real paket itkisi → provayderə yaz |
| `curl -w`: **tls** böyükdür | `openssl s_client` | TLS ayarı / sertifikat |
| `curl -w`: **ilk_bayt** böyükdür | tətbiq logları, DB | **server/tətbiq** yavaşdır, şəbəkə yox |
| Hər şey normal, **yükləmə** uzun | `iperf3` | bant eni / **window** (S04) |
| Yalnız **bəzən** yavaşdır | `mtr` uzun müddət, `ping` `mdev` | **qeyri-sabit** link |

> 80% "yavaşdır" şikayətində problem **şəbəkədə deyil**, `ilk_bayt` mərhələsindədir (tətbiq/verilənlər bazası). `curl -w` bunu **ilk dəqiqədə** göstərir.

---

## 🧪 Lab 11a — nərdivanı işlət

**Məqsəd:** öz VM-ində 6 pilləni ardıcıl yoxla. Hər addımda **nəticəni yaz**.

```bash
$ ip -br a                          # 1) interfeys və IP
$ ip r | grep default               # 2) gateway-i tap
$ ping -c 3 $(ip r | awk '/default/ {print $3}')    # 3) gateway-ə ping
$ ping -c 3 8.8.8.8                 # 4) internetə ping
$ dig +short google.com             # 5) DNS
$ nc -vz -w 3 google.com 443        # 6) port 443
$ curl -sI https://google.com | head -1             # 7) HTTP cavabı
```

| Addım | Gözlənilən | Cavab ver |
|---|---|---|
| 1–2 | `eth0 UP`, `default via ...` | gateway IP-n nədir? |
| 3–4 | `0% packet loss` | gateway və internet **gecikmələri** neçədir? |
| 5–6 | IP-lər, `succeeded` | neçə IP qayıtdı? hansı **protokol** adı çıxdı? |
| 7 | `HTTP/2 200` və ya `301` | niyə `301` ola bilər? |

> **Tapşırıq:** bu 7 əmri **bir skriptdə** yaz. Hər addım uğursuz olarsa, **dayan** və hansı pillənin **qırıldığını** çap et. (Bash kursunda dəqiqləşdirəcəyik.)

---

## 🧪 Lab 11b — `curl` ilə mərhələ vaxtlarını ölç

```bash
$ curl -s -o /dev/null -w 'dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ilk_bayt=%{time_starttransfer} cəmi=%{time_total}\n' https://google.com
$ curl -s -o /dev/null -w 'dns=%{time_namelookup} tcp=%{time_connect} cəmi=%{time_total}\n' http://neverssl.com
```

1. **Hər mərhələnin öz vaxtını** hesabla (fərqi götür). Kağıza **cədvəl** çək.
2. `https` və `http` arasında **hansı mərhələ əlavə** olur?
3. Eyni əmri **5 dəfə** təkrarla. Hansı mərhələ **ən çox dəyişir**?
4. `mtr -rwn -c 10 8.8.8.8` işlət. **Son hop-da** itki var?

> Cavabı **rəqəmlə** ver, "yavaşdır" yox. DevOps-da **rəqəm** sübutdur.

---

## 🧪 Lab 11c — gizli problemi tap

> ⚠️ **Yalnız `lo` (loopback) üzərində.** SSH-a toxunmur. Sonda **mütləq** qaydanı sil: `sudo iptables -D INPUT -i lo -p tcp --dport 8080 -j DROP`.

Partnyor **problemi yaradır**, sən **tapırsan**. Yaradan şəxs üç ssenaridən **birini** seçir:

```bash
# A) Servis dayanıb:         heç nə qaldırma (8080-də server yoxdur)
# B) Yanlış bind:            python3 -m http.server 8080 --bind 127.0.0.2 &
# C) Firewall DROP:          python3 -m http.server 8080 --bind 127.0.0.1 &
#                            sudo iptables -I INPUT -i lo -p tcp --dport 8080 -j DROP
```

Axtaran şəxs yalnız bunları işlədir: `nc -vz -w 2 127.0.0.1 8080`, `ss -tlnp | grep 8080`, `curl --connect-timeout 2`. **Səbəbi və sübutu** kağıza yazır.

---

## 🧪 Lab 11c — nəticəni oxu

| Simptom | Səbəb | Necə tapdın? |
|---|---|---|
| `refused`, `ss`-də 8080 **yoxdur** | A: servis dayanıb | `ss -tlnp` boşdur |
| `refused`, `ss`-də `127.0.0.2:8080` | B: **yanlış ünvana** bağlanıb | `Local Address` fərqlidir |
| `timed out`, `ss`-də 8080 **var** | C: **firewall DROP** | `sudo iptables -S INPUT` |

> **Dərs:** eyni simptom (`curl` işləmir), **üç fərqli səbəb**. Fərqi **alətlər** göstərir, təxmin yox.

---

## 🪜 Tipik səhvlər

| Səhv | Nəticə | Düzgün |
|---|---|---|
| **`ping` işləmədi → server dayanıb** | ICMP bağlı ola bilər | `nc -vz`, `curl` ilə yoxla |
| **Ortadakı `* * *`-i problem saymaq** | boş yerə vaxt itirmək | yalnız **sonadək** davam edəni say |
| **`mtr`-də ortadakı itkiyə panika** | router ICMP-ni məhdudlaşdırır | **son hop**-a bax |
| **`-n` yazmamaq** | alət DNS axtarır, **yavaşlayır** | `traceroute -n`, `mtr -n`, `ss -n` |
| **`ss`-i yalnız `LISTEN`-ə baxmaq** | **`127.0.0.1` və `0.0.0.0`** fərqini görməmək | `Local Address`-i oxu |
| **Yalnız client tərəfdə yoxlamaq** | serverin daxilini bilmirsən | **iki ucdan** yoxla: `nc` + `ss` |
| **Sürətlə "restart"** | səbəb **itir**, təkrar olar | əvvəl **diaqnoz**, sonra əməliyyat |
| **Nəticəni yazmamaq** | hansı addımda nə gördüyünü unudursan | hər addımın **cavabını qeyd et** |

---

## 🔁 Simptom → ilk alət

| Simptom | İlk əmr | Sonra |
|---|---|---|
| Heç nə açılmır | `ip a`, `ip r` | `ping GW` → `ping 8.8.8.8` |
| Yalnız **ad ilə** açılmır, IP ilə açılır | `dig ad` | `dig @8.8.8.8 ad` |
| `Connection refused` | serverdə `ss -tlnp` | servis, `Local Address` |
| `timed out` | `nc -vz -w 3` | `traceroute`, firewall, `tcpdump` |
| `502 / 504` | `curl -v` | **upstream** (S05, S08) |
| Yavaşdır | `curl -w` (vaxtlar) | mərhələyə görə alət |
| Bəzən qopur | `mtr -c 100` | `ping` `mdev`, kabel |
| Başqaları açır, **mən** yox | `ip r get`, `dig` | VPN, proxy, yerli DNS |
| Heç nə aydın deyil | `tcpdump` (S10) | **iki ucda** tut |

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Bu `mtr` çıxışını oxu. Hansı hop-da real problem var, hansı sadəcə ICMP məhdudiyyətidir? Səbəbini izah et: [yapışdır]"*
- *"`curl -w` çıxışım: dns=2.1 tcp=2.12 tls=2.17 ilk_bayt=2.23. Hər mərhələnin öz vaxtını hesabla və yavaşlığın səbəbini de."*
- *"`nc -vz` `timed out` verir, amma serverdə `ss -tlnp` portu göstərir. Mümkün səbəbləri **yoxlama sırası** ilə sırala (hər biri üçün əmr ver)."*

</div>

<div class="ai-tip">💡 AI-yə <b>çıxışı</b> ver, yalnız "işləmir" yazma. Həmişə <b>hansı əmri</b>, <b>hansı maşından</b> işlətdiyini yaz. Daxili IP və hostname-ləri göndərməzdən əvvəl <b>maskala</b>.</div>

---

## 📝 Xülasə

| Alət | Sual | Yadda saxla |
|---|---|---|
| **`ip a`, `ip r`** | yerli vəziyyət | `ip r get HEDEF` yolu göstərir |
| **`ping`** | cihaz çatır? | cavab yoxdur ≠ server dayanıb (ICMP bağlı ola bilər) |
| **`traceroute`** | yol harada qırılır? | ortadakı `* * *` normaldır |
| **`mtr -rwn`** | yol + itki | yalnız **son hop**-un itkisi real |
| **`ss -tlnp`** | kim dinləyir? | `127.0.0.1` ≠ `0.0.0.0` |
| **`nc -vz`** | port açıqdır? | `refused` vs `timed out` |
| **`curl -w`** | hansı mərhələ yavaş? | vaxtlar **yığılandır**, fərq götür |
| **`dig`** | ad → IP düzgündür? | `NXDOMAIN`, `SERVFAIL`, `@server` müqayisəsi |

> **Qayda:** **aşağıdan yuxarı**, hər addımda **bir sual**, nəticəni **yaz**. `iperf3` kanal sürətini ölçür (yalnız icazə ilə).

**Növbəti dərs:** Sessiya 12 — **VPN və tunnel**: şifrələnmiş tunnel, WireGuard-a körpü.

---

## ✍️ Tapşırıqlar

1. **Xəritə:** 8 alətin hər biri üçün **bir sual** yaz (cədvəl **yadda saxlamadan**) və **hansı qatı** yoxladığını de.
2. **Ping:** `ping 192.168.0.250` (mövcud olmayan IP) və `ping -c 2 google.invalid` nəticələrini müqayisə et. **Hansı xəta** hansı problemi göstərir?
3. **Traceroute:** `traceroute -n 8.8.8.8` işlət. **Neçə hop** var? **Gecikmə harada ən çox** artır? `* * *` varsa, **problemdir** yoxsa yox?
4. **ss:** `python3 -m http.server 9000 --bind 127.0.0.1 &` qaldır. `ss -tlnp | grep 9000`-ə bax. **Başqa maşından** niyə qoşula bilməzsən? `--bind 0.0.0.0` ilə **nə dəyişir**?
5. **curl vaxtı:** 3 fərqli saytın `curl -w` vaxtlarını ölç. **Hansı mərhələ** hamısında ən böyükdür?
6. **dig:** mövcud olmayan ad (`dig nonexistent.example.invalid`) üçün `status` nədir? `dig @8.8.8.8` ilə **müqayisə et**.
7. **Ssenari:** "`curl https://app.example.com` **30 saniyə** gözləyib `timed out` verir. `dig` düzgün IP qaytarır. `ping` cavab vermir." **Növbəti 3 əmrin** hansıdır və niyə?
8. **Qərar ağacı:** Ssenari 1 diaqramını **öz sözlərinlə** yenidən çək və **hər qərar nöqtəsinə** konkret əmr yaz.

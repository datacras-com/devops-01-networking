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

# 🌐 HTTP, HTTPS və sorğunun tam həyat dövrü

## Brauzerdə sayt açanda arxada nə baş verir?

```
  192.168.0.15                                                93.184.216.34
  ┌──────────┐  1.DNS  2.TCP  3.TLS  4.HTTP sorğu  ┌─────────────────────┐
  │ brauzer  │ ──────► ────► ────► ────────────────►│  web server (:443)  │
  │ / curl   │ ◄─────────────────────  5.cavab ◄────│                     │
  └──────────┘                                      └─────────────────────┘
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 08 · Chapter 4 — Tətbiq qatı və Təhlükəsizlik

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **HTTP nədir:** sorğu və cavab, metodlar, başlıqlar, gövdə
- **Status kodları:** `301`, `401`, `403`, `404`, `429`, `500`, `502`, `503`, `504` və **kimin günahıdır**
- **`curl`:** `-v`, `-I`, `-L`, `-H`, `-X POST -d`, `-w`
- **HTTPS və TLS:** sertifikat, niyə lazımdır, müddətini `openssl` ilə yoxlamaq
- **HTTP/1.1 və HTTP/2**, **reverse proxy**, **`502` və `504` fərqi**
- **Bütöv xəritə:** DNS → TCP → TLS → HTTP
- **Simptom cədvəli:** `refused`, `timed out`, `502`

> Sessiya 03-də qatları, Sessiya 04-də TCP handshake və portları öyrəndik. Burada onların **üstündəki** qata baxırıq: proqramların **bir-birinə nə dediyinə**.

---

## 🍽️ HTTP nədir? Restoran analogiyası

**HTTP** (HyperText Transfer Protocol) = client ilə server arasında **danışıq qaydası**. Restoranı düşün:

| Restoranda | HTTP-də |
|---|---|
| Sən masada **sifariş verirsən** | client (brauzer, `curl`) **sorğu** göndərir |
| Ofisiant **mətbəxə** aparır | şəbəkə sorğunu **serverə** çatdırır |
| Mətbəx **yeməyi** hazırlayır | server sorğunu **emal edir** |
| "Hazırdır" və ya "bu yeməyimiz bitib" | **cavab** + **status kodu** |

> HTTP **həmişə** bu sıra ilə gedir: **əvvəl client soruşur, sonra server cavab verir**. Server özbaşına sənə yazmır. Tək bir sorğu = tək bir cavab.

---

## 📨 Sorğunun quruluşu

HTTP mətn formatındadır. Sorğu 3 hissədən ibarətdir:

```
GET /api/health HTTP/1.1          <- 1) ilk sətir: METOD  YOL  VERSİYA
Host: app.example.com             <- 2) başlıqlar (headers): "Ad: dəyər"
User-Agent: curl/8.4.0
Accept: */*
                                  <- boş sətir: "başlıqlar bitdi"
(gövdə, body)                     <- 3) gövdə: GET-də adətən BOŞ olur
```

- **Metod:** nə etmək istəyirsən (oxu, yarat, sil)
- **Yol (path):** serverdə **hansı** resurs (`/api/health`)
- **Başlıqlar:** sorğu haqqında **əlavə məlumat** (kimsən, nə formatda istəyirsən)

---

## 📩 Cavabın quruluşu

Cavab da eyni məntiqlə qurulub:

```
HTTP/1.1 200 OK                   <- 1) status sətri: VERSİYA  KOD  İZAH
Content-Type: application/json    <- 2) başlıqlar
Content-Length: 15
                                  <- boş sətir
{"status":"ok"}                   <- 3) gövdə (body): əsl məzmun
```

| Hissə | Sualı | Nümunə |
|---|---|---|
| **Status kodu** | iş **alındı**mı? | `200`, `404`, `502` |
| **Başlıqlar** | məzmun **nədir**? | `Content-Type`, `Content-Length` |
| **Gövdə** | **özü** | HTML, JSON, şəkil |

> Sən çox vaxt **status kodundan** başlayırsan: o, problemin **kimdə** olduğunu göstərir.

---

## 🔧 Metodlar: sorğunun "feli"

| Metod | Mənası | Restoranda | Gövdə? |
|---|---|---|---|
| `GET` | **oxu** (məlumatı gətir) | "menyunu göstər" | yox |
| `POST` | **yarat** / göndər | "yeni sifariş ver" | var |
| `PUT` | **əvəz et** (yenilə) | "sifarişi dəyiş" | var |
| `DELETE` | **sil** | "sifarişi ləğv et" | adətən yox |

- `GET` **heç nəyi dəyişmir**, ona görə təhlükəsiz təkrarlanır
- `POST`, `PUT`, `DELETE` **serverdə nəsə dəyişir**
- Başqa metodlar da var (`HEAD` = yalnız başlıqlar, `PATCH`), amma bu 4 əsasdır

---

## 🚦 Status kodları: 5 qrup

| Qrup | Mənası | Sadə izah |
|---|---|---|
| `1xx` | məlumat | "gözlə, davam edirəm" (nadir görürsən) |
| `2xx` | **uğur** | "buyur, hazırdır" |
| `3xx` | **yönləndirmə** | "yer dəyişib, bura get" |
| `4xx` | **client xətası** | "sorğun düzgün deyil" |
| `5xx` | **server xətası** | "bizdə nəsə sınıb" |

> **Ən vacib qayda:** `4xx` = **sorğuya** bax (client günahkardır). `5xx` = **serverə** bax (server tərəfi günahkardır). Yoxlamağa **hara** baxacağını bu rəqəm deyir.

---

## 🔍 Ən çox rast gəlinənlər (1/2): `3xx` və `4xx`

| Kod | Adı | Mənası | Kimin günahı? |
|---|---|---|---|
| `301` | Moved Permanently | ünvan **həmişəlik** dəyişib (`Location` başlığına bax) | normaldır, yönləndirmə |
| `401` | Unauthorized | **kimliyin məlum deyil** (token/parol yoxdur və ya səhvdir) | **client** |
| `403` | Forbidden | kimsən bilirik, amma **icazən yoxdur** | **client** (icazə) |
| `404` | Not Found | bu **yol** serverdə yoxdur | **client** (yol səhvi) |
| `429` | Too Many Requests | çox tez-tez sorğu göndərdin (rate limit) | **client** (yavaşla) |

> `401` = "sən kimsən?", `403` = "səni tanıyıram, **girmək olmaz**". Çox qarışdırılır.

---

## 🔍 Ən çox rast gəlinənlər (2/2): `5xx`

| Kod | Adı | Mənası | Kimin günahı? |
|---|---|---|---|
| `500` | Internal Server Error | tətbiq **özü** xəta verdi (bug, exception) | **tətbiq** (loga bax) |
| `502` | Bad Gateway | proxy backend-dən **pis/boş cavab** aldı, və ya backend-ə qoşula bilmədi | **backend** |
| `503` | Service Unavailable | servis **müvəqqəti** cavab verə bilmir (yüklənib, bağlanıb) | **server / backend** |
| `504` | Gateway Timeout | proxy backend-i **gözlədi, cavab gəlmədi** | **backend** (yavaş/donub) |

> `502`, `503`, `504` adətən **proxy / load balancer** ilə gəlir. Sən **proxy-ni** görürsən, problem isə **onun arxasındadır**. Fərqi `502` vs `504` slaydında görəcəyik.

---

## 🏷️ Başlıqlar (headers): ən vacib 5-i

| Başlıq | Hansı tərəf | Nə deyir |
|---|---|---|
| `Host` | sorğu | **hansı sayt** istənir (`app.example.com`). Bir IP-də çox sayt ola bilər |
| `User-Agent` | sorğu | **kimsən** (`curl/8.4.0`, brauzer adı) |
| `Content-Type` | hər ikisi | gövdə **hansı formatdadır** (`application/json`, `text/html`) |
| `Authorization` | sorğu | **kimlik** (`Bearer <token>`) |
| `Cache-Control` | cavab | cavabı **nə qədər** saxlamaq olar (`max-age=3600`, `no-store`) |

> Sessiya 07-dən: DNS adı **IP-yə** çevirir. Amma server **hansı saytı** istədiyini bilmir, `Host` başlığı bunu deyir. Buna görə `curl http://192.168.0.21` bəzən **başqa cavab** verir.

---

## 🔬 `curl -v` sətir-sətir (1/2): sorğu

`-v` (verbose) = `curl`-ün **nə etdiyini** göstər. Sadəlik üçün əvvəl `http://` ilə:

```
$ curl -v http://example.com/
* Host example.com:80 was resolved.          <- 1) DNS bitdi (Sessiya 07)
*   Trying 93.184.216.34:80...               <- 2) TCP qoşulma cəhdi
* Connected to example.com (93.184.216.34) port 80   <- TCP handshake OK
> GET / HTTP/1.1                             <- 3) SORĞU (> = göndərilən)
> Host: example.com                          <- başlıq: hansı sayt
> User-Agent: curl/8.4.0                     <- başlıq: kimsən
> Accept: */*                                <- başlıq: hər format olar
>                                            <- boş sətir: sorğu bitdi
```

> Çıxış **nümunədir**, `curl` versiyasına görə sətirlər bir az fərqlənə bilər. Əsas qayda: `*` = `curl`-ün öz qeydi, **`>` = sən göndərirsən**, **`<` = server göndərir**.

---

## 🔬 `curl -v` sətir-sətir (2/2): cavab

```
< HTTP/1.1 200 OK                  <- 4) CAVAB (< = gələn). Status: 200 uğur
< Content-Type: text/html          <- gövdə HTML-dir
< Content-Length: 1256             <- gövdə 1256 bayt
< Cache-Control: max-age=604800    <- 7 gün saxlamaq olar
<                                  <- boş sətir: başlıqlar bitdi
<!doctype html>...                 <- 5) gövdə (HTML)
* Connection #0 to host example.com left intact   <- bağlantı bağlanmadı
```

- **Tapmaq üçün:** `curl -v` yazanda əvvəl `>` sətirlərinə (sən nə göndərdin?), sonra `<` sətirlərinə (server nə dedi?) bax
- **Yalnız başlıqlar** lazımdırsa: gövdəni gizlət `curl -sv -o /dev/null URL`
- Çox yazırsa: `curl -v URL 2>&1 | head -25` (`-v` çıxışı **stderr**-ə gedir)

---

## 🧰 Gündəlik `curl`: `-I`, `-L`, `-H`

```
$ curl -I http://example.com/          # -I: yalnız başlıqlar (HEAD sorğusu)
HTTP/1.1 200 OK
Content-Type: text/html

$ curl -I http://github.com/           # http -> https yönləndirməsi
HTTP/1.1 301 Moved Permanently
Location: https://github.com/          <- "bura get"

$ curl -L http://github.com/           # -L: Location-u izlə (yönləndirməyə get)
$ curl -H "Host: app.example.com" http://192.168.0.21/   # -H: başlıq əlavə et
```

> Cavab başlıqları **nümunədir**. `-I` = "mənə yalnız vizit kartını göstər, məzmunu yox". `-H "Host: ..."` ilə **DNS-siz** saytı test edirsən (ingress, load balancer yoxlayanda çox lazımdır).

---

## 📮 `curl -X POST -d`: gövdə göndərmək

```
$ curl -X POST http://192.168.0.21:8080/api/users \
       -H "Content-Type: application/json" \
       -d '{"name":"aygun"}'
{"id":7,"name":"aygun"}                     <- server yaratdığı obyekti qaytardı
```

| Hissə | Nə edir |
|---|---|
| `-X POST` | metodu `POST` et |
| `-H "Content-Type: application/json"` | gövdənin **JSON** olduğunu söylə |
| `-d '...'` | **gövdə** (body) |
| `-H "Authorization: Bearer <TOKEN>"` | lazım olsa kimlik əlavə et |

> `-d` yazanda `curl` metodu **özü** `POST` edir, `-X POST` çox vaxt artıqdır. **Tokeni** terminal tarixçəsinə və chat-ə yapışdırma.

---

## ⏱️ `curl -w`: haradan yavaşdır?

Sorğunu mərhələlərə bölüb **vaxt** ölçürük (saniyə, **yığılan** cəmdir):

```
$ curl -o /dev/null -s -w "dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ttfb=%{time_starttransfer} total=%{time_total} kod=%{http_code}\n" https://example.com
dns=0.012 tcp=0.034 tls=0.071 ttfb=0.120 total=0.125 kod=200
```

| Sahə | Nə vaxta qədər | Böyükdürsə problem hardadır |
|---|---|---|
| `dns` | ad tapılana | **DNS** (Sessiya 07) |
| `tcp` | TCP qoşuldu | **şəbəkə / firewall** |
| `tls` | TLS bitdi | **sertifikat / TLS** |
| `ttfb` | ilk bayt gəldi | **server / backend yavaşdır** |

> Hər rəqəm əvvəlkini **də** daxil edir: TCP-nin öz vaxtı = `tcp - dns`. Nümunədə TLS ≈ `0.071 - 0.034`.

---

## 🔐 Problem: HTTP açıqdır

HTTP mətn kimi gedir. Yolda olan **hər kəs** (Wi-Fi, router, provayder) onu **oxuya və dəyişə** bilər:

```
  192.168.0.15 ──── router ──── provayder ──── server
       "parol=12345"    [X] hamı oxuya bilir
```

| Təhlükə | Nümunə |
|---|---|
| **Dinləmə** | parol, token açıq görünür |
| **Dəyişmə** | yolda reklam və ya zərərli kod əlavə olunur |
| **Saxta server** | "bank" kimi görünən başqa server |

> **HTTPS = HTTP + TLS.** TLS (Transport Layer Security) HTTP-ni **qapalı zərfə** qoyur. Port `443`-dür (HTTP `80`). Sessiya 04-dəki portlar.

---

## 📜 Sertifikat: serverin "şəxsiyyət vəsiqəsi"

Brauzer necə bilir ki, `example.com` **həqiqətən** odur? **Sertifikat** ilə:

```
   Kök CA (brauzerdə hazır)  ──imzalayır──►  Aralıq CA  ──imzalayır──►  example.com sertifikatı
   (güvənilən qurum)                                                     "bu açar example.com-undur"
```

- **CA** (Certificate Authority, sertifikat verən qurum) = vəsiqəni verən **dövlət idarəsi** kimidir
- Sertifikatda var: **kimə** verilib (domen), **kim** verib, **bitmə tarixi**, serverin **açıq açarı**
- Brauzer yoxlayır: ① imza **güvənilən CA-dandır**, ② **domen uyğundur**, ③ **müddəti bitməyib**
- Biri pozulsa: `SSL certificate problem` / brauzerdə qırmızı xəbərdarlıq

---

## 🤝 TLS handshake: qısa

TCP handshake-dən **sonra**, HTTP-dən **əvvəl** TLS "tanışlığı" olur:

```
  client                                          server
    │ ── 1. Salam, bu TLS versiyaları var ───────────►│
    │ ◄── 2. Salam, bu mənim sertifikatım ────────────│
    │    (client sertifikatı yoxlayır: CA? domen? müddət?)
    │ ── 3. Birgə gizli açar razılaşdıraq ───────────►│
    │ ◄══ 4. Hazırdır. Bundan sonra HAMISI şifrəli ══►│
```

- TLS 1.3-də bu **bir gediş-gəliş** (round trip) ilə olur, ona görə sürətlidir
- Razılaşdırılan açar ilə HTTP **şifrələnir**, kənar adam yalnız **qarışıq bayt** görür
- Ona görə `curl -w`-də `tls` vaxtı ayrıca görünür

---

## 🔬 `curl -v https://...`: TLS sətirləri

```
$ curl -v https://example.com/ 2>&1 | head -22
* Host example.com:443 was resolved.            <- 1) DNS
*   Trying 93.184.216.34:443...                 <- 2) TCP
* Connected to example.com port 443             <- TCP OK
* TLSv1.3 (OUT), TLS handshake, Client hello    <- 3) TLS: biz salam dedik
* TLSv1.3 (IN), TLS handshake, Server hello     <- server cavab verdi
* Server certificate:                           <- sertifikatı göstərir
*  subject: CN=example.com                      <- kimə verilib
*  expire date: Mar 14 23:59:59 2027 GMT        <- bitmə tarixi
* SSL certificate verify ok.                    <- 4) yoxlama KEÇDİ
> GET / HTTP/2                                  <- 5) indi HTTP (artıq şifrəli)
< HTTP/2 200                                    <- cavab
```

> Çıxış **nümunədir**. `SSL certificate verify ok` yoxdursa və xəta varsa, **problem TLS-dədir**, HTTP-yə hələ çatmayıb.

---

## 📅 Sertifikat müddəti: `openssl s_client`

Sertifikat bitəndə sayt "açılmır". Müddəti **əmrlə** yoxla:

```
$ echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null \
    | openssl x509 -noout -subject -issuer -dates
subject=CN = example.com               <- kimə verilib
issuer=C = US, O = DigiCert Inc ...    <- kim verib
notBefore=Jan 13 00:00:00 2026 GMT     <- nə vaxtdan keçərlidir
notAfter=Mar 14 23:59:59 2027 GMT      <- nə vaxta qədər  <-- BUNA BAX
```

- `-connect host:port`: hara qoşulsun; `-servername`: `Host` kimi, **hansı sayt** (SNI)
- `echo |` bağlantını **dərhal** bağlayır (yoxsa gözləyir)
- Qalan günü yoxlamaq: `... | openssl x509 -noout -checkend 864000` (10 gün)
- Bu əmr **monitorinq alertlərinin** əsasıdır: "sertifikat 14 gündən az qalıb"

---

## ⚡ HTTP/1.1 və HTTP/2: qısa

| | HTTP/1.1 | HTTP/2 |
|---|---|---|
| **Format** | **mətn** | **ikili** (binary) |
| **Bir bağlantıda** | sorğular **növbə ilə** | **eyni anda çox** sorğu |
| **Başlıqlar** | hər dəfə **tam** yazılır | **sıxılır** |
| **Brauzerdə** | hələ də işlənir | **adətən** HTTPS üzərində |

- **Məna dəyişmir:** metodlar, status kodları, başlıqlar **eynidir**. Yalnız **daşınma üsulu** dəyişib
- `curl -v` çıxışında `GET / HTTP/2` görürsənsə, server HTTP/2 danışıb
- Məcbur etmək: `curl --http1.1 URL` və ya `curl --http2 URL`
- **HTTP/3** də var (UDP üzərində, QUIC), amma bu dərsdə lazım deyil

---

## 🔀 Reverse proxy: serverin qarşısındakı qapıçı

Real sistemdə sorğu **birbaşa** tətbiqə getmir. Qarşıda **reverse proxy** durur (`nginx`, ingress, load balancer, Sessiya 05):

```
  client ──HTTPS──► [ nginx 192.168.0.10 ] ──HTTP──► [ app 192.168.0.21:8080 ]
                      reverse proxy (qapıçı)          backend (iş görən)
```

| Proxy nə edir | Niyə lazım |
|---|---|
| TLS-i **özü açır** | tətbiq sertifikatla məşğul olmur |
| Sorğunu **backend-ə ötürür** | bir ünvan, arxada çox servis |
| Yükü **bölür** | Sessiya 05 |
| Xəta olsa **özü** `502/503/504` qaytarır | client **proxy-ni** görür |

> **Vacib:** `curl` ilə `502` alırsansa, proxy **işləyir** (cavab verdi). Problem **onun arxasındadır**.

---

## 🧭 `502` vs `504`: proxy backend-ə nə dedi?

Proxy backend-ə sorğu atdı. İki fərqli şey ola bilər:

```
  502 Bad Gateway                          504 Gateway Timeout
  proxy ──► backend                        proxy ──► backend
  [X] qoşulma YOX / cavab pis              [OK] qoşuldu, amma...
  (backend ölüdür, port bağlıdır)          ...cavab GƏLMİR (gözləyir, gözləyir)
```

| | `502` | `504` |
|---|---|---|
| **Backend** | **ölüdür** və ya **pis cavab** verdi | **canlıdır**, amma **çox yavaş / donub** |
| **Sürət** | **tez** gəlir | **gec** gəlir (timeout bitəndə) |
| **Yoxla** | backend **işləyirmi**? port, `systemctl status` | backend **niyə yavaşdır**? DB, loqlar |
| **Analogiya** | mətbəx **bağlıdır** | mətbəx **açıqdır**, yemək gəlmir |

---

## 🗺️ Brauzerdə sayt açanda tam yol

`https://example.com/` yazıb Enter basanda (hər addım **ayrıca** sına bilər):

| № | Addım | Nə baş verir | Sına bilər: simptom |
|---|---|---|---|
| 1 | **DNS** | `example.com` -> `93.184.216.34` (Sessiya 07) | `Could not resolve host` |
| 2 | **TCP** | handshake `:443`-ə (Sessiya 04) | `timed out` / `refused` |
| 3 | **TLS** | sertifikat yoxlanır, açar razılaşır | `SSL certificate problem` |
| 4 | **HTTP sorğu** | `GET / HTTP/2` + `Host` | `4xx` (sorğu səhvdir) |
| 5 | **Proxy / backend** | sorğu arxaya ötürülür | `502` / `503` / `504` |
| 6 | **HTTP cavab** | `200` + gövdə | `5xx`, yavaş cavab |

> **DNS-in nəticəsi** (IP ünvanı) HTTP sorğusunun **ilk addımıdır**: IP olmadan TCP qoşulmaq olmur. Debug = **hansı addımda dayandığını** tapmaq.

---

## 🚨 Simptom cədvəli: `refused`, `timed out`, `502`

| Simptom | Hansı addım | Mənası | İlk yoxla |
|---|---|---|---|
| `Could not resolve host` | 1 DNS | ad IP-yə çevrilmir | `dig`, yazılış səhvi |
| `Connection timed out` | 2 TCP | SYN-ə **cavab yox** (firewall DROP / marşrut) | `nc -vz`, SG/firewall |
| `Connection refused` | 2 TCP | **RST** gəldi: port bağlıdır, servis yoxdur | `ss -tln` |
| `SSL certificate problem` | 3 TLS | sertifikat bitib / domen səhvdir | `openssl s_client` |
| `502 Bad Gateway` | 5 proxy | proxy var, **backend ölüdür** | backend `systemctl`, port |
| `504 Gateway Timeout` | 5 proxy | backend **yavaşdır** | backend log, DB |
| `401` / `403` / `404` | 4 HTTP | **sorğu** (kimlik/icazə/yol) | token, yol, icazə |

---

## 🧪 Lab 8a: sadə server və `curl`

Heç nəyi dəyişməyən təhlükəsiz lab (yalnız `localhost`):

```bash
$ mkdir -p /tmp/lab8 && echo "salam" > /tmp/lab8/index.html
$ python3 -m http.server 8080 --directory /tmp/lab8 &      # server fonda
$ curl -v http://localhost:8080/ 2>&1 | grep -E '^[<>]'    # >, < sətirləri
$ curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080/yoxdur
$ kill %1                                                   # serveri dayandır
$ curl -sv http://localhost:8080/ 2>&1 | grep -i refused
```

| Addım | Gözlənilən nəticə |
|---|---|
| 3-cü əmr | `> GET / HTTP/1.1`, `< HTTP/1.0 200 OK` (Python serveri `1.0` yaza bilər) |
| 4-cü əmr | `404` |
| 6-cı əmr | `Connection refused` (Sessiya 04-dən tanış) |

---

## 🧪 Lab 8b: `502` və `504` yarat

`nginx` proxy-ni backend portuna yönəldirik (`127.0.0.1:9000`). **Config-i dəyişmədən əvvəl** köhnəsini saxla (`sudo cp`):

```nginx
# /etc/nginx/conf.d/lab8.conf
server { listen 8081;
  location / { proxy_pass http://127.0.0.1:9000; proxy_read_timeout 5s; } }
```

```bash
$ sudo nginx -t && sudo nginx -s reload
$ curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8081/    # 9000-də heç kim yox
$ nc -l 9000 &        # qoşulmanı QƏBUL edir, cavab VERMİR (bəzi nc-də: nc -l -p 9000)
$ curl -s -o /dev/null -w "%{http_code} %{time_total}s\n" http://localhost:8081/
```

| Addım | Gözlənilən nəticə |
|---|---|
| 1-ci `curl` | `502` **dərhal** (backend yoxdur) |
| 2-ci `curl` | `504` təxminən **5 saniyə** sonra (backend qəbul etdi, susdu) |

> Bitəndə: `kill %1`, `sudo rm /etc/nginx/conf.d/lab8.conf`, `sudo nginx -s reload`.

---

## 🧯 Ssenari: "Sayt `504` verir"

| Vaxt | Hadisə |
|---|---|
| 14:02 | Monitorinq: `https://shop.example.com` yavaşdır, bəzən `504` |
| 14:05 | Dev: "server işləyir" (`systemctl status` aktivdir) |

Addım-addım gedirik, hər addımda **ən ucuz yoxlamanı** edirik:

```
$ curl -o /dev/null -s -w "dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ttfb=%{time_starttransfer}\n" https://shop.example.com
dns=0.01 tcp=0.03 tls=0.07 ttfb=60.01      <- dns/tcp/tls normal, TTFB 60 san.!
```

- DNS, TCP, TLS **sağlamdır**, proxy cavab verir -> problem **arxadadır**
- `504` (`502` yox) -> backend **canlıdır**, amma **gecikir**
- Backend loqunda: **yavaş SQL sorğusu** tapıldı. **Kök səbəb:** DB indeksi yoxdur. **Dərs:** `-w` vaxtları problemi **addıma** bölür

---

## ⚠️ Tipik səhvlər və "niyə belə olur?"

| Səhv / sual | Niyə belə olur |
|---|---|
| "`curl http://IP` işləmir, amma ad ilə işləyir" | server `Host` başlığına baxır. `-H "Host: ..."` əlavə et |
| `502`-də **proxy-ni** düzəltməyə çalışmaq | proxy sağlamdır, **backend-ə** bax |
| `401` və `403`-ü qarışdırmaq | `401` = kimlik yoxdur, `403` = icazə yoxdur |
| `curl -k` ilə "həll etmək" | sertifikat yoxlamasını **söndürür**, problemi gizlədir (yalnız test) |
| `http://` işləyir, `https://` işləmir | `:443` açıq deyil və ya sertifikat problemi |
| Sertifikat "birdən" bitir | müddət izlənmir. **Alert** qur |
| Tokeni `curl -H` ilə chat-ə yapışdırmaq | tokeni **sızdırırsan**, əvvəl sil |

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Bu `curl -v` çıxışımda hansı sətir DNS-dir, hansı TCP, TLS və HTTP? Harada dayandığını izah et: [çıxışı yapışdır, tokeni sil]"*
- *"`502`, `503` və `504` arasındakı fərq nədir? `nginx` arxasında hər biri üçün **ilk 3 yoxlamanı** yaz."*
- *"Bu `curl -w` nəticələrinə bax: `dns=0.01 tcp=0.03 tls=0.07 ttfb=8.2`. Problem hardadır və növbəti hansı əmri işlədim?"*

</div>

<div class="ai-tip">💡 AI cavabını <b>öz maşınında yoxla</b>: Lab 8b ilə <code>502</code> və <code>504</code> yarat, cavabın sənin müşahidənlə uyğun gəlib-gəlmədiyinə bax.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **HTTP** | client soruşur, server cavab verir (sorğu + cavab) |
| **Sorğu** | metod + yol + başlıqlar + gövdə |
| **Cavab** | status kodu + başlıqlar + gövdə |
| **`4xx` / `5xx`** | **client** günahı / **server** günahı |
| **`401` / `403`** | kimlik yoxdur / icazə yoxdur |
| **`502` / `504`** | backend **ölüdür** / backend **yavaşdır** |
| **HTTPS** | HTTP + TLS, port `443`, sertifikat ilə |
| **Sertifikat** | CA imzası, domen, **bitmə tarixi** |
| **`curl -v`** | `*` qeyd, `>` göndərilən, `<` gələn |
| **Tam yol** | DNS -> TCP -> TLS -> HTTP -> proxy/backend |

**Növbəti dərs:** Sessiya 09 — **Linux firewall**: `ufw`, `iptables`, `nftables`.

---

## ✍️ Tapşırıqlar

1. **Analogiya:** HTTP sorğu və cavabını **öz sözlərinlə** izah et. Sorğunun 3 hissəsi hansıdır?
2. **Status kodları:** `401`, `403`, `404` fərqini **hər biri üçün bir real nümunə** ilə yaz.
3. **Kimin günahı:** `429`, `500`, `502`, `504` hər biri üçün **client yoxsa server**, və **ilk yoxlama** nədir?
4. **`curl -v`:** istənilən `https://` saytına `curl -v` et. **DNS, TCP, TLS, HTTP** sətirlərini tap və yaz.
5. **Vaxt:** `curl -w` ilə `dns`, `tcp`, `tls`, `ttfb` ölç. **Ən böyük** mərhələ hansıdır?
6. **Sertifikat:** `openssl s_client` ilə bir saytın `notAfter` tarixini tap. **Neçə gün** qalıb?
7. **Lab 8b:** `502` və `504` yarat. Hər birinin **nə qədər gec** gəldiyini yaz və **niyə** fərqli olduğunu izah et.
8. **Diaqnostika:** Üç ayrı `curl`: biri `Connection refused`, biri `timed out`, biri `502` verir. Hər biri **hansı addımda** (DNS/TCP/TLS/HTTP/proxy) dayanıb?

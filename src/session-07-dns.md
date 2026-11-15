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

# 📖 DNS: adı IP-yə çevirmək

## İnsan ad yazır, şəbəkə IP ilə işləyir. Aradakı tərcüməçi DNS-dir.

```
  Sən yazırsan:   curl example.com
                       |
                       |   "example.com hansı IP-dir?"
                       v
                  [ DNS sistemi ]
                       |
                       |   "93.184.216.34"
                       v
  Paket gedir:    192.168.0.15 ------> 93.184.216.34 : 443
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 07 · Chapter 3 — Ünvanlama dərinləşməsi və Yönləndirmə

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **DNS niyə var:** ad yadda qalır, IP yox. DNS "telefon kitabçasıdır"
- **Kim kimdən soruşur:** root, TLD, authoritative, recursive resolver
- **Bir sorğunun tam yolu:** cache, `/etc/hosts`, resolver, root, `.com`, ad serveri
- **Record tipləri:** `A`, `AAAA`, `CNAME`, `MX`, `TXT`, `NS`, `PTR`
- **TTL və cache:** DNS dəyişikliyi niyə dərhal görünmür
- **Alətlər:** `dig` (sətir-sətir), `getent`, `resolvectl`
- **DevOps konteksti:** Kubernetes DNS, TTL outage ssenarisi
- **Xəta oxumaq:** `NXDOMAIN`, `SERVFAIL`, `timeout` fərqi

> Sessiya 04-də DNS-i **UDP/53** nümunəsi kimi gördük. Sessiya 05-də `timeout` və `refused` fərqini öyrəndik. İndi **53-ün arxasında nə olduğuna** baxırıq.

---

## 🧩 Problem: IP yadda qalmır

Sessiya 02-dən bilirik: paket yalnız **IP ünvana** gedir. Amma insan `93.184.216.34` yox, `example.com` yadda saxlayır.

| | Problem | Həll |
|---|---|---|
| **Yadda saxlama** | yüzlərlə sayt, hər birinin IP-si | ad: `example.com` |
| **IP dəyişir** | server köçür, IP başqa olur | ad **eyni qalır**, IP yenilənir |
| **Bir ad, çox server** | yük bir neçə serverə bölünür | ad **bir neçə IP**-yə baxa bilər |

> **Analogiya:** telefon kitabçası. Dostunun **adını** bilirsən, **nömrəsini** kitabçadan tapırsan. Nömrə dəyişsə, kitabçanı yeniləyirlər, sən yenə eyni adı yığırsan.

---

## 🌳 DNS hierarchy: ad sağdan sola oxunur

`www.example.com.` sonunda gizli **nöqtə** var (root). Ad **sağdan sola** oxunur:

```
                .               <- root (kök)
                |
              .com              <- TLD (Top Level Domain, ən üst səviyyə)
                |
          example.com           <- domen (sənin şirkətin)
                |
        www.example.com         <- host (konkret ad)
```

- **Heç bir server hamısını bilmir.** Hər səviyyə yalnız **növbəti səviyyəni** göstərir
- Kitabxanaya bənzəyir: giriş stendi (root) → rəf (`.com`) → konkret kitab (`example.com`)
- Belə bölgü DNS-i **dünya miqyasında** işlək saxlayır

---

## 🏛️ Hierarchy-də 3 səviyyə kimdir?

| Səviyyə | Nə bilir | Nümunə |
|---|---|---|
| **Root server** | `.com` kimdədir? | `a.root-servers.net` (13 ünvan qrupu) |
| **TLD server** | `example.com`-un ad serveri hansıdır? | `a.gtld-servers.net` |
| **Authoritative** | domenin **həqiqi** cavabı (IP-lər) | `a.iana-servers.net` |

- **TLD** nümunələri: `.com`, `.net`, `.org`, `.az`
- **Authoritative** = "yetkili" server: cavabı **o verir**, başqasından soruşmur
- Domeni sən qeydiyyatdan keçirəndə **ad serverlərini** (NS) göstərirsən. Zəncir buradan bağlanır

> Root və TLD **IP-ni vermir**, yalnız **"get ona sor"** deyir (yönləndirir). IP-ni yalnız **authoritative** verir.

---

## 🙋 Kim soruşur? Resolver və authoritative

Sən hər dəfə root-a yox, **bir köməkçiyə** zəng edirsən. Bu köməkçi **`resolver`** (soruşub tapan proqram) adlanır.

| Rol | Nə edir | Nümunə |
|---|---|---|
| **Stub resolver** | maşındakı kiçik sorucu, sorğunu **irəli ötürür** | `127.0.0.53` |
| **Recursive resolver** | **bütün zənciri** gəzib cavabı tapır, **yadda saxlayır** | `8.8.8.8`, `1.1.1.1` |
| **Authoritative** | yalnız **öz domeninin** cavabını bilir | `a.iana-servers.net` |

> **Analogiya:** sən (stub) **məlumat xidmətinə** (recursive) zəng edirsən. O, kataloqlara baxıb nömrəni tapır və **növbəti dəfə üçün yadında saxlayır**. Nömrənin **sahibi** isə authoritative-dir.

---

## 🚶 Walkthrough 1: sorğu maşından çıxmamış

`curl example.com` yazdın. Sorğu **əvvəl öz maşınında** axtarılır:

```
  1. curl: "example.com nədir?"              (proqram sistemə sual verir)
  2. Lokal cache-də var?        -> var: DAYAN, cavab hazırdır
  3. /etc/hosts-da yazılıb?     -> var: DAYAN, DNS-ə getmir
  4. /etc/resolv.conf oxunur    -> "nameserver 127.0.0.53"  (stub)
  5. Stub öz cache-ində axtarır -> yox: sorğu 192.168.0.1 və ya 8.8.8.8-ə gedir
```

- Addım 2-3 **şəbəkəyə çıxmadan** cavab verir. Bu, DNS-in ən sürətli halıdır
- Addım 3-ün sırasını `/etc/nsswitch.conf` müəyyən edir (bir az sonra)
- Addım 5: sorğu **UDP/53** ilə **recursive resolver**-ə gedir (Sessiya 04)

---

## 🚶 Walkthrough 2: resolver zənciri gəzir

Resolver cache-də tapmadı, **sıfırdan** soruşur:

```
  6. resolver -> root:           "example.com hardadır?"
                 root cavab:     "bilmirəm, .com serverlərinə sor"  (yönləndirmə)
  7. resolver -> .com (TLD):     "example.com hardadır?"
                 TLD cavab:      "a.iana-servers.net-ə sor"          (yönləndirmə)
  8. resolver -> authoritative:  "example.com üçün A qeydi nədir?"
                 cavab:          "93.184.216.34, TTL 3600"           (HƏQİQİ cavab)
  9. resolver cavabı cache edir və stub-a göndərir
 10. stub -> curl:               93.184.216.34   ->  curl TCP bağlantısı açır
```

- Addım 6-7 **yönləndirmədir**, addım 8 **real cavabdır**
- 9-cu addımdan sonra **eyni ad üçün** növbəti sorğular TTL bitənə qədər **cache-dən** gəlir
- Addım 10-da DNS **bitir**, TCP başlayır (Sessiya 04)

---

## 📮 Record tipləri 1: ünvanı verənlər

DNS "kitabçasında" hər sətir bir **record** (qeyd) adlanır. **Tip** nəyi soruşduğunu bildirir.

| Tip | Nə qaytarır | Nümunə |
|---|---|---|
| **A** | **IPv4** ünvan | `app.example.com  A  192.168.0.21` |
| **AAAA** | **IPv6** ünvan ("dörd A", 4 dəfə uzun) | `app.example.com  AAAA  2001:db8::21` |
| **CNAME** | **başqa ad**-a alias (ləqəb) | `www  CNAME  app.example.com.` |
| **PTR** | **IP-dən ada** (tərs sorğu) | `21.0.168.192.in-addr.arpa  PTR  app.example.com.` |

> **CNAME** "əlavə ad kartı" kimidir: `www` öz IP-sini saxlamır, "**`app.example.com`-a bax**" deyir. IP dəyişəndə **bir yeri** yeniləyirsən. CNAME domenin **kökündə** (`example.com`-un özündə) ola bilməz.

---

## 📮 Record tipləri 2: xidmət və idarəetmə qeydləri

| Tip | Nə qaytarır | Nümunə |
|---|---|---|
| **MX** | domenin **poçt serveri** (prioritetlə) | `example.com  MX  10 mail.example.com.` |
| **TXT** | **mətn**: SPF, domen doğrulaması | `"v=spf1 include:_spf.example.com ~all"` |
| **NS** | domenin **ad serverləri** (authoritative) | `example.com  NS  a.iana-servers.net.` |

- **MX** prioriteti: **kiçik rəqəm** əvvəl sınanır (`10` əvvəl, `20` ehtiyat)
- **TXT** ilə sən domenin **sahibi** olduğunu sübut edirsən (sertifikat, e-poçt təhlükəsizliyi)
- **NS** zənciri bağlayır: TLD "bu domenin NS-i budur" deyəndə **bu qeydə** baxır
- Hamısını `dig example.com MX` kimi **tipi yazaraq** soruşuruq

---

## 🔁 Bir adın bir neçə IP-si

Bir ad **bir neçə `A` qeydinə** baxa bilər. Resolver siyahını hər dəfə **fərqli sırada** verə bilər:

```
  api.example.com.   300  IN  A  192.168.0.21
  api.example.com.   300  IN  A  192.168.0.22
  api.example.com.   300  IN  A  192.168.0.23
```

- Buna **DNS round-robin** deyilir: sadə yük bölgüsü, hər client başqa birincini seçə bilər
- Üstünlük: **sadədir**. Çatışmazlıq: DNS **serverin sağ olub-olmadığını bilmir**, ölü IP-ni də verir
- Ona görə ciddi sistemdə **load balancer** (Sessiya 05) və health check lazımdır

> **IPv4 + IPv6:** bir ad həm `A`, həm `AAAA` qaytara bilər. Proqram adətən IPv6 işləyirsə əvvəlcə onu sınayır.

---

## ⏳ TTL: cache nə qədər yaşayır?

Hər cavabın yanında **TTL** (Time To Live, yaşama müddəti) saniyə ilə yazılır. Resolver cavabı **bu müddət qədər** yadında saxlayır.

| TTL | Üstünlük | Çatışmazlıq |
|---|---|---|
| **Uzun** (`86400` = 1 gün) | az sorğu, sürətli | dəyişiklik **gec** yayılır |
| **Qısa** (`60`-`300`) | dəyişiklik **tez** görünür | çox sorğu, authoritative yüklənir |

> **Analogiya:** dostun nömrəsini dəftərinə yazırsan və "**1 gün yoxlama**" deyirsən. Dostun nömrəsi dəyişsə, sən **sabaha qədər köhnə nömrəni** yığacaqsan. Cache də eynidir. Həm sənin maşının, həm resolver, həm brauzer **ayrı-ayrı** yadda saxlaya bilər.

---

## 🕰️ Niyə DNS dəyişikliyi dərhal görünmür?

Sən authoritative-də `A` qeydini `192.168.0.21` → `192.168.0.31` dəyişdin. TTL `3600` idi:

```
  Vaxt      Resolver A (cache doludur)     Resolver B (cache boşdur)
  10:00     192.168.0.21  (TTL 3600)        -
  10:05     [siz qeydi dəyişdin]
  10:10     192.168.0.21  (köhnə!)          192.168.0.31  (yeni)
  11:00     cache bitdi, yenidən soruşur    -
  11:01     192.168.0.31  (yeni)            192.168.0.31
```

- Dəyişiklik **yayılmır**, sadəcə hər cache **öz vaxtında** bitir
- Nəticə: bir müddət **bəzi istifadəçilər köhnə, bəziləri yeni** serverə gedir
- **Qayda:** köçməzdən **əvvəl** TTL-i `60`-a endir, köç, sonra qaldır

---

## 📒 `/etc/hosts` və `nsswitch`: lokal sıra

Maşın DNS-dən **əvvəl** özünün kiçik "kitabçası" `/etc/hosts`-a baxa bilər. Sıranı `/etc/nsswitch.conf` müəyyən edir:

```bash
$ grep ^hosts /etc/nsswitch.conf
hosts:          files dns        # <- soldan sağa: əvvəl "files", sonra "dns"
$ cat /etc/hosts
127.0.0.1       localhost
192.168.0.21    app.local        # <- əl ilə yazılmış qeyd
```

| Söz | Mənası |
|---|---|
| `files` | `/etc/hosts` faylı |
| `dns` | `/etc/resolv.conf`-dakı resolver |

> **İlk tapılan qalib gəlir.** `files` əvvəl olduğu üçün `/etc/hosts` qeydi DNS-i **üstələyir**. Bu yaxşı test üsuludur, amma **unudulmuş qeyd** təhlükəli sirrdir (Lab 7b).

---

## 🔬 `dig`: DNS-ə birbaşa sual

`dig` (Domain Information Groper) DNS serverdən **özü soruşur**, `/etc/hosts`-a baxmır. Qurulum: `sudo apt install dnsutils`.

| Əmr | Nə edir |
|---|---|
| `dig example.com` | **tam** çıxış (A qeydi) |
| `dig +short example.com` | **yalnız cavab**: `93.184.216.34` |
| `dig example.com MX` | **tipi** seç (`A`, `AAAA`, `MX`, `NS`, `TXT`) |
| `dig @1.1.1.1 example.com` | **konkret serverdən** soruş |
| `dig -x 93.184.216.34` | **tərs** sorğu (IP → ad, PTR) |
| `dig +noall +answer example.com` | yalnız `ANSWER` bölməsi (TTL ilə) |

> `+short` skriptlərdə rahatdır, `+noall +answer` isə **TTL-i də** göstərir.

---

## 📖 `dig` çıxışı sətir-sətir (1/2): başlıq

Nümunə çıxış (sənin çıxışın fərqli ola bilər):

```
$ dig example.com
; <<>> DiG 9.18.28 <<>> example.com
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4711      <- 1) nəticə statusu
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1   <- 2) bayraqlar
;; QUESTION SECTION:
;example.com.                  IN      A                   <- 3) nə soruşduq
```

| Hissə | Mənası |
|---|---|
| `status: NOERROR` | sorğu **uğurlu** oldu (`NXDOMAIN`, `SERVFAIL` sonrakı slaydda) |
| `qr` / `rd` / `ra` | `qr` = bu **cavabdır**, `rd` = **rekursiya istədik**, `ra` = resolver **bunu edə bilir** |
| `ANSWER: 1` | cavabda **1 qeyd** var |
| `aa` bayrağı | olsaydı, cavabı **authoritative** vermişdi (burada yoxdur: resolver cache-dən verib) |

---

## 📖 `dig` çıxışı sətir-sətir (2/2): cavab

```
;; ANSWER SECTION:
example.com.     3600   IN   A   93.184.216.34     <- 4) cavabın özü
;; Query time: 23 msec                              <- 5) sorğu müddəti
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)          <- 6) kimdən soruşduq
;; MSG SIZE rcvd: 56                                <- 7) cavabın ölçüsü (bayt)
```

| Sütun / sətir | Mənası |
|---|---|
| `example.com.` | ad (sonda **nöqtə** = tam ad) |
| `3600` | **TTL**: cache-də **qalan** saniyə (təkrar sorğuda azalır) |
| `IN` | "Internet" sinfi (həmişə `IN`) |
| `A` / `93.184.216.34` | qeyd tipi / **IP ünvan** |
| `SERVER: ...#53 (UDP)` | **hansı resolver**, **hansı port** (`53`), **UDP** (Sessiya 04) |

> `Query time: 0-1 msec` adətən **cache** deməkdir. `SERVER: 127.0.0.53` göstərir ki, sorğu **stub-a** getdi.

---

## 🚦 `status`: NXDOMAIN, SERVFAIL, timeout

Üç fərqli "cavab alınmadı" halı var. Üçü **üç ayrı səbəbdir**:

| Nəticə | Nə baş verdi | Yoxla |
|---|---|---|
| **NOERROR** | cavab var. `ANSWER: 0` ola bilər: ad var, **bu tip yoxdur** | düzgün tipi soruşduqmu? |
| **NXDOMAIN** | belə **ad yoxdur** (authoritative "tanımıram" dedi) | **yazılışı**, qeyd əlavə edilibmi? |
| **SERVFAIL** | resolver **cavab ala bilmədi** (authoritative sınıqdır, DNSSEC xətası) | `dig @authoritative` ilə birbaşa yoxla |
| **timeout** | cavab **heç gəlmədi** (`;; connection timed out`) | resolver IP, firewall `UDP/53` |

> Sessiya 05-dəki məntiq eynidir: **`NXDOMAIN` = "kitabçada yoxdur"** (aydın cavab), **`timeout` = "telefona heç kim baxmır"**. `SERVFAIL` isə "məlumat xidməti **cavab tapa bilmədi**".

---

## 🔍 `dig +trace`: zənciri öz gözünlə gör

`+trace` resolver cache-ni **atlayır** və root-dan başlayıb hər pilləni göstərir (nümunə, qısaldılıb):

```
$ dig +trace example.com
.              518400 IN NS  a.root-servers.net.   <- 1) root siyahısı
com.           172800 IN NS  a.gtld-servers.net.   <- 2) root dedi: "com-u gtld bilir"
example.com.   172800 IN NS  a.iana-servers.net.   <- 3) TLD dedi: "example.com-u bu bilir"
example.com.   3600   IN A   93.184.216.34         <- 4) authoritative: HƏQİQİ cavab
;; Received 56 bytes from 199.43.135.53#53(a.iana-servers.net) in 24 ms
```

- Hər `Received ... from X` = **hansı serverə** sorduq, cavab gəldi
- 1-3 **yönləndirmədir**, 4 **real cavabdır**. Bu, Walkthrough 2-nin canlı versiyasıdır
- `NS` qeydlərinin TTL-i uzundur (`172800` = 2 gün), `A` isə qısa ola bilər

---

## 🧭 Tətbiq nə görür? `getent`, `resolvectl`

`curl`, `ssh`, `nginx` adı **sistemin sırası** (`files` → `dns`) ilə həll edir. `dig` isə yox.

| Əmr | Kimdən soruşur | `/etc/hosts`-u oxuyur? |
|---|---|---|
| `getent hosts example.com` | sistem sırası (tətbiqin gördüyü) | **hə** |
| `dig example.com` | birbaşa DNS | **yox** |
| `resolvectl query example.com` | `systemd-resolved` (stub + cache) | DNS tərəfi |

```bash
$ getent hosts example.com        # tətbiq nə görür?
$ dig +short example.com          # DNS nə deyir?
$ resolvectl status               # hansı DNS server (Current DNS Server)?
$ sudo resolvectl flush-caches    # lokal cache-i təmizlə
```

> **Qayda:** "`dig` işləyir, amma proqram işləmir" → `getent hosts`. **Fərq varsa:** `/etc/hosts`, `nsswitch` və ya cache.

---

## ☸️ Kubernetes DNS qısa

Klaster daxilində pod-lar bir-birini **Service adı** ilə tapır. Bunu klasterin **CoreDNS** servisi həll edir.

```
  <service>.<namespace>.svc.cluster.local
  myapp.default.svc.cluster.local   ->   Service-in ClusterIP-si (məs. 10.96.x.x)
```

```bash
$ kubectl exec -it mypod -- cat /etc/resolv.conf
nameserver 10.96.0.10                                  # <- CoreDNS servisi
search default.svc.cluster.local svc.cluster.local cluster.local
options ndots:5                                        # <- qısa ad əvvəl "search" ilə sınanır
```

- Eyni namespace-də sadəcə `myapp` yazmaq kifayətdir (`search` tamamlayır)
- Başqa namespace: `myapp.prod`, və ya tam ad
- Pod-dan servisə çata bilmirsən? `dig`/`nslookup myapp.default.svc.cluster.local` ilə **DNS-i** yoxla

---

## 🔥 Ssenari: DNS TTL outage (1/2)

Cümə gecəsi servis `192.168.0.21`-dən yeni serverə **köçürüldü**, `A` qeydi dəyişdi. TTL `86400` (1 gün) idi və **əvvəlcədən endirilməmişdi**.

```
  Cümə 22:00   Köhnə server söndürüldü, A qeydi -> 192.168.0.31
  Cümə 22:10   Bəzi istifadəçilər işləyir, bəziləri xəta alır
  Cümə 22:30   Yeni serverdə trafik azdır, köhnə serverə hələ sorğu gəlir
```

| Kim | Nə görür | Səbəb |
|---|---|---|
| Cache-i **boş** olan | yeni IP, **işləyir** | dərhal yeni qeydi alır |
| Cache-i **dolu** olan | köhnə IP, **xəta** | `86400` saniyə köhnəni saxlayır |

> Problem **DNS xətası deyil**: DNS **düzgün** işləyir, sadəcə **köhnə cavabı** saxlayır.

---

## 🔥 Ssenari: araşdırma və dərs (2/2)

**Üç yerdən soruş, cavabları tutuşdur:**

```bash
$ dig +noall +answer app.example.com                       # lokal resolver
app.example.com.  80000 IN A 192.168.0.21                  # <- KÖHNƏ, TTL azalır
$ dig +noall +answer @8.8.8.8 app.example.com              # ictimai resolver
app.example.com.  61000 IN A 192.168.0.21                  # <- KÖHNƏ
$ dig +noall +answer @a.iana-servers.net app.example.com   # authoritative
app.example.com.  86400 IN A 192.168.0.31                  # <- YENİ (həqiqət)
```

- **Authoritative yenidir, resolver-lər köhnədir** = cache problemi (TTL)
- **Düzəliş:** köhnə serveri **müvəqqəti geri qaldır**, TTL bitənə qədər saxla
- **Dərs:** köçmədən **əvvəl** TTL-i `60`-a endir, **köhnə TTL qədər gözlə**, köç, **köhnə serveri TTL bitənə qədər söndürmə**, sonra TTL-i qaldır

---

## 🧪 Lab 7a: DNS-i oxu

Tələb: `dig` qurulub (`sudo apt install dnsutils`). Çıxışlar **nümunədir**.

```bash
$ dig +short example.com                 # gözlənilən: bir IP, məs. 93.184.216.34
$ dig example.com MX +short              # gözlənilən: prioritet + poçt serveri (və ya boş)
$ dig example.com NS +short              # gözlənilən: ad serverləri
$ dig +noall +answer example.com         # 1-ci dəfə: TTL, məs. 3600
$ dig +noall +answer example.com         # 2-ci dəfə: TTL AZALIB (cache işləyir)
$ dig @1.1.1.1 +short example.com        # gözlənilən: eyni və ya yaxın cavab
$ dig +trace example.com                 # gözlənilən: root -> TLD -> authoritative
```

> **Yoxla:** 2-ci sorğuda TTL niyə **azaldı**? (Cavab: cache vaxt keçdikcə qalan müddəti göstərir.)
> **Əlavə:** `dig +noall +comments yoxdur-xyz.example.com | grep status` → `status: NXDOMAIN` görməlisən.

---

## 🧪 Lab 7b: `/etc/hosts` səhvi və `dig` ilə `getent` fərqi

**Xəbərdarlıq:** `/etc/hosts` sistem faylıdır. **Əvvəl ehtiyat nüsxə** al, işin bitəndə **geri qaytar**.

```bash
$ sudo cp /etc/hosts /etc/hosts.bak                    # 1) ehtiyat nüsxə
$ echo "192.168.0.99 example.com" | sudo tee -a /etc/hosts   # 2) yanlış qeyd
$ getent hosts example.com        # gözlənilən: 192.168.0.99  example.com  (hosts qalib)
$ dig +short example.com          # gözlənilən: 93.184.216.34 (DNS hosts-a baxmır)
$ curl -v --connect-timeout 3 http://example.com   # gözlənilən: 192.168.0.99-a, timeout
$ sudo cp /etc/hosts.bak /etc/hosts                    # 3) GERİ QAYTAR
$ getent hosts example.com        # gözlənilən: yenə 93.184.216.34
```

> **Sual:** niyə `dig` düzgün, `curl` isə səhv nəticə verdi? Cavab: `curl` sistem sırasına (`files` → `dns`) tabedir, `dig` yox.

---

## 🪜 Tipik səhvlər: simptom → səbəb

| Simptom | Ehtimal olunan səbəb | Yoxla |
|---|---|---|
| `NXDOMAIN` | ad **səhv yazılıb** və ya qeyd **yoxdur** | yazılışı, `dig +trace` |
| `SERVFAIL` | authoritative **cavab vermir** / DNSSEC xətası | `dig @ns-server` birbaşa |
| `timeout` (`dig`) | resolver **çatmır**, `UDP/53` bağlıdır | `resolvectl status`, firewall |
| `dig` düzgün, `curl` səhv | `/etc/hosts`, cache, `nsswitch` | `getent hosts` |
| Bir istifadəçi işləyir, digəri yox | **cache** / TTL fərqi | `dig @8.8.8.8` vs `@authoritative` |
| `ping IP` var, `ping ad` yox | **DNS problemi**, şəbəkə yox | `dig`, `/etc/resolv.conf` |

> Sessiya 05-dən xatırla: əvvəl **`ping 192.168.0.1`**, sonra **`ping 93.184.216.34`**, sonra **`ping example.com`**. Sonuncu pozulursa, problem **DNS-dədir**, şəbəkədə yox.

---

## 🛠️ DevOps-da DNS harada?

| Yer | Burada nə edir |
|---|---|
| **AWS Route 53** | domen üçün authoritative DNS (A, CNAME, alias qeydləri) |
| **Kubernetes CoreDNS** | `svc.cluster.local` adlarını həll edir |
| **Terraform** | DNS qeydi (`aws_route53_record`) kodla yazılır, TTL orada təyin olunur |
| **Load balancer** | LB adı (`CNAME`) arxasında IP-lər dəyişir |
| **Sertifikat (Let's Encrypt)** | domen sahibliyi `TXT` qeydi ilə doğrulanır |
| **E-poçt** | `MX`, SPF/DKIM `TXT` qeydləri |

> "**Həmişə DNS-dir**" DevOps zarafatıdır, çünki çox vaxt doğrudur. Nəsə "çatmır" deyirsə, **əvvəl DNS-i yoxla**: `dig` və `getent`.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Bu `dig` çıxışını sətir-sətir izah et: [yapışdır]. `status`, `flags`, `TTL`, `SERVER` hissələrini ayrıca de."*
- *"`dig` düzgün IP verir, amma `curl` səhv serverə gedir. Səbəbləri sıra ilə say (hosts, cache, nsswitch, IPv6, proxy)."*
- *"`app.example.com`-u yeni IP-yə köçürürəm, TTL 86400. Sıfır downtime üçün addım-addım plan yaz."*

</div>

<div class="ai-tip">💡 AI-yə <b>fakt</b> ver: əvvəl <code>dig +trace</code>, <code>dig @8.8.8.8</code> və <code>getent hosts</code> nəticələrini topla, sonra yapışdır. Faktla verilən sual daha dəqiq cavab alır.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **DNS** | ad → IP (telefon kitabçası), UDP/53 |
| **Hierarchy** | root → TLD → authoritative, hər biri **növbətini** göstərir |
| **Recursive resolver** | zənciri gəzir və **cache edir** (`8.8.8.8`) |
| **Authoritative** | domenin **həqiqi** cavabı |
| **Record** | `A`, `AAAA`, `CNAME`, `MX`, `TXT`, `NS`, `PTR` |
| **TTL** | cache ömrü: köçməzdən **əvvəl** endir |
| **`dig` / `getent`** | birbaşa DNS / tətbiqin gördüyü |
| **Status** | `NXDOMAIN` yoxdur, `SERVFAIL` alınmadı, `timeout` çatmır |

**Növbəti dərs:** Sessiya 08 — **HTTP, HTTPS və sorğunun həyat dövrü**.

---

## ✍️ Tapşırıqlar

1. **Telefon kitabçası:** DNS-i öz sözlərinlə izah et. IP dəyişəndə niyə ad dəyişmir?
2. **Hierarchy:** `www.example.com`-un həllində root, TLD və authoritative **hər biri nə cavab verir**?
3. **Recursive vs authoritative:** fərqi izah et. `8.8.8.8` hansıdır?
4. **Record tipləri:** hər birinə bir nümunə yaz: `A`, `AAAA`, `CNAME`, `MX`, `TXT`, `NS`, `PTR`.
5. **TTL:** TTL `3600` olan qeydi dəyişdin. **Ən pis halda** neçə dəqiqə köhnə IP görünə bilər? Niyə?
6. **`dig` çıxışı:** `ANSWER SECTION` sətrində hər sütunun mənasını yaz. `Query time: 0 msec` nə deməkdir?
7. **Lab 7b:** `dig` və `getent hosts` niyə fərqli cavab verdi? `/etc/hosts`-u necə geri qaytardın?
8. **Diaqnostika:** `NXDOMAIN`, `SERVFAIL` və `timeout` üçün **hər biri üçün ilk yoxlayacağın addımı** yaz. Ev tapşırığı: DNS köçürmə planı (5 addım: TTL endir, gözlə, dəyiş, yoxla, TTL qaldır).

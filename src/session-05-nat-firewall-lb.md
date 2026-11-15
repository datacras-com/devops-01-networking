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

# 🛡️ NAT, Firewall, Load Balancer

## Trafik şəbəkəyə necə daxil olur, çıxır və paylanır?

```
  Daxili şəbəkə 192.168.0.0/24                         İnternet

  192.168.0.15 ──┐
  192.168.0.16 ──┼──► [Firewall] ──► [NAT router] ══► 203.0.113.10 ══► 93.184.216.34
  192.168.0.17 ──┘      qayda         ünvanı dəyişir     (bir açıq IP)       (sayt)
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 05 · Chapter 2 — Transport və Təhlükəsizlik

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Niyə NAT var?** Private IP internetdə işləmir, açıq IPv4 çatışmır
- **NAT necə işləyir:** SNAT/masquerade, DNAT/port forwarding, NAT cədvəli
- **Firewall:** stateless/stateful, qayda sırası, "default deny"
- **`DROP` və `REJECT`:** Sessiya 04-dəki `timeout` və `refused` buradan gəlir
- **Load balancer:** L4 və L7, alqoritmlər, health check
- **Bütöv mənzərə:** bir sorğu firewall, NAT və load balancer-dən necə keçir
- **Canlı lab:** açıq IP-ni tap, `ufw` ilə `timeout`/`refused` yarat, `nginx` ilə yük böl

> Burada **niyə lazımdır** və **şəbəkə arxitekturasında harada durur** öyrənirik. `ufw`/`iptables` əmrləri **Sessiya 09**-da, CIDR və routing **Sessiya 06**-dadır.

---

## 🧩 Problem: private IP internetdə işləmir

Sessiya 02-dən xatırla: `192.168.0.0/16` **private** aralıqdır. İnternet routerləri bu ünvanları **marşrutlaşdırmır**.

| | Nə var | Nəticə |
|---|---|---|
| **Daxili cihazlar** | `192.168.0.15`, `.16`, `.17`, ... | internetdə **tanınmır** |
| **Açıq IPv4** | **məhduddur** və pulludur | hər cihaza ayrıca almaq **mümkün deyil** |
| **Həll** | **NAT**: router **bir açıq IP** ilə hamının adından çıxır | hamı **bir ünvan** kimi görünür |

> Cavab paketi də geri qayıtmalıdır. Sayt `192.168.0.15`-ə cavab göndərə bilməz, çünki bu ünvan ona görünmür. Cavabın **hansı daxili cihaza** gedəcəyini router **yadda saxlayır**.

---

## 🔁 NAT necə işləyir? (SNAT / masquerade)

Router paket çıxanda **mənbə ünvanını** dəyişir, cavab gələndə **geri çevirir**:

```
  Giden:  192.168.0.15:41234 ──► 93.184.216.34:443
             router yenidən yazır ▼
          203.0.113.10:50001   ──► 93.184.216.34:443        (internetə çıxan)

  Gələn:  93.184.216.34:443 ──► 203.0.113.10:50001
             router cədvələ baxır ▼
          93.184.216.34:443 ──► 192.168.0.15:41234          (daxili cihaza)
```

- **Cədvəl** (conntrack) bu cütləri saxlayır: `192.168.0.15:41234 ⇄ 203.0.113.10:50001`
- Port da dəyişdiyi üçün buna **PAT** (Port Address Translation) də deyilir
- **Masquerade** = açıq IP dəyişəndə (DHCP) avtomatik götürən SNAT forması
- `203.0.113.0/24` sənədlərdə nümunə kimi ayrılmış açıq aralıqdır

---

## 🚪 Daxildən-xaricə yox, xaricdən-daxilə (DNAT)

NAT cədvəlində **yalnız daxildən başlamış** bağlantılar var. Xaricdən gələn **yeni** sorğunun cədvəldə yeri yoxdur, router onu **ataraq** qoyur.

| Növ | Nəyi dəyişir | Nə üçün |
|---|---|---|
| **SNAT / masquerade** | **mənbə** ünvan | daxili cihazlar internetə çıxsın |
| **DNAT / port forwarding** | **təyinat** ünvan | xaricdən daxili servisə çatmaq |

```
  Xaricdən:  203.0.113.10:8080  ──►  router (DNAT qaydası)  ──►  192.168.0.21:80
```

> `203.0.113.10:8080 → 192.168.0.21:80` qaydası **daxili serveri xaricə açır**. Hər belə qayda **hücum səthidir**, ona görə yalnız lazım olanı aç.

---

## ☁️ Bulud və Kubernetes-də NAT

| Yer | Qarşılığı | Nə edir |
|---|---|---|
| **AWS** | **NAT Gateway** | private subnet-dəki serverlər internetə **çıxır**, **girə bilmir** |
| **AWS** | **Internet Gateway** | public subnet-lə internet arasında **iki tərəfli** yol |
| **Kubernetes** | `Service` (NodePort/LoadBalancer) | xarici portu pod-a **DNAT** ilə yönləndirir |
| **Ev / ofis** | router | **SNAT** + istəyə görə port forwarding |

> **Private subnet** = açıq IP-si yoxdur, internetə yalnız **NAT Gateway** ilə çıxır. Sənin servisin buradan paket yeniləməsi çəkə bilər, amma internetdən **birbaşa əlçatan deyil**.

---

## 🧱 Firewall — keçid qaydaları

Firewall hər paketə **qaydalar siyahısı** ilə baxır və **keçir** və ya **bağlayır**.

| Anlayış | Mənası |
|---|---|
| **Stateless** | hər paketə **ayrıca** baxır, əvvəlki paketi **xatırlamır** |
| **Stateful** | **bağlantını izləyir**: icazə verilmiş bağlantının cavabı **avtomatik** keçir |
| **Default deny** | **açıq-aydın icazə verilməyən hər şey bağlıdır** |
| **Qayda sırası** | yuxarıdan aşağı, **ilk uyğun gələn qayda** qalib gəlir |

> Stateful firewall-da `192.168.0.15 → 93.184.216.34:443` çıxışına icazə verəndə **cavab paketlərini** ayrıca açmırsan. Bağlantı cədvəli bunu bilir.

---

## 📋 Qaydanın quruluşu

Hər qayda **kim → kimə → hansı port/protokol → nə et** cavabını verir:

| # | Mənbə | Təyinat | Protokol:port | Əməl |
|---|---|---|---|---|
| 1 | `192.168.0.0/24` | `192.168.0.21` | `tcp:22` | **ALLOW** (SSH yalnız daxildən) |
| 2 | `istənilən` | `192.168.0.21` | `tcp:443` | **ALLOW** (HTTPS hamıya) |
| 3 | `istənilən` | `192.168.0.21` | `tcp:5432` | **DENY** (verilənlər bazası bağlı) |
| 4 | `istənilən` | `istənilən` | `hamısı` | **DENY** (default deny) |

- Sıra vacibdir: 1-ci qayda `192.168.0.0/24`-ü **dərhal** buraxır, qalanlar baxılmır
- `tcp` və `udp` **ayrıca** açılır (Sessiya 04)
- Port **və mənbə** daraldıqca qayda **təhlükəsiz** olur: `0.0.0.0/0` üçün SSH açmaq **pis vərdişdir**

---

## 🔍 DROP və REJECT — Sessiya 04-ə körpü

Firewall paketi bağlayanda iki cür davrana bilər:

| Əməl | Nə edir | Client nə görür |
|---|---|---|
| **DROP** | paketi **səssizcə atır** | **`timeout`**, heç bir cavab yoxdur |
| **REJECT** | **RST/ICMP** qaytarır | **`refused`**, dərhal xəta |

```
  nc -vz 192.168.0.21 9000      # DROP   →  gözləyir ... Connection timed out
  nc -vz 192.168.0.21 9000      # REJECT →  Connection refused (dərhal)
```

> **Xaricə açılan firewall adətən `DROP` işlədir**: hücumçu portun **mövcud olub-olmadığını** bilməsin. Ona görə `timeout` görəndə ilk şübhəli **firewall**-dır.

---

## 🧱 Firewall harada yaşayır?

| Səviyyə | Alət | Bir nümunə |
|---|---|---|
| **Host** | `ufw`, `iptables`, `nftables` | serverin özündə `ufw allow 22/tcp` |
| **Şəbəkə cihazı** | router/firewall cihazı | ofis perimetri |
| **Bulud** | **Security Group** (stateful), **NACL** (stateless) | AWS instance və subnet |
| **Kubernetes** | **NetworkPolicy** | pod-lar arası qayda |

- Bir sorğu **bir neçə qatdan** keçir: **hər biri** bağlaya bilər
- `timeout` olanda **hamısını sıra ilə** yoxla: host firewall, Security Group, NACL, NetworkPolicy
- Host-da firewall əmrləri **Sessiya 09**-da var, burada qatları **bir yerdə** görürük

---

## ⚖️ Load balancer — yükü paylamaq

Bir server bütün sorğuları **götürə bilmir** və ya **dayana** bilər. Load balancer sorğunu **bir neçə serverə** paylayır:

```
                          ┌──► 192.168.0.21:8080  [OK] sağlam
  client ──► 192.168.0.10 ─┼──► 192.168.0.22:8080  [OK] sağlam
        (Load Balancer)   └──► 192.168.0.23:8080  [X]  cavab vermir (çıxarılıb)
```

- **Health check:** LB serverləri **yoxlayır**, xəstəni **siyahıdan çıxarır**
- **Üstünlük:** daha çox yük, **dayanıqlılıq** (bir server düşsə, sistem işləyir), **yeniləmə** zamanı kəsintisiz
- LB-nin **özü də** tək nöqtə ola bilər, ona görə adətən **iki LB** qurulur

---

## 🔬 L4 və L7 load balancer

| | L4 (transport) | L7 (tətbiq) |
|---|---|---|
| **Nəyə baxır** | IP və **port** (TCP/UDP) | **HTTP**: host, yol, başlıq |
| **Qərar** | "hansı serverə bağlantı açım?" | "`/api` bir yerə, `/` başqa yerə" |
| **Sürət** | **daha sürətli** | daha çox imkan, bir az yavaş |
| **TLS** | adətən **keçirir** | adətən **özü açır** (termination) |
| **AWS** | **NLB** | **ALB** |

| Alqoritm | Necə seçir |
|---|---|
| **Round robin** | növbə ilə: 1, 2, 3, 1, 2, 3 |
| **Least connections** | **ən az bağlantısı olan** |
| **IP hash** | eyni client həmişə **eyni serverə** (sticky) |

---

## 🗺️ Bütöv mənzərə: bir sorğunun yolu

```
  client (İnternet)
     │  https://app.datacras.com → 203.0.113.10:443
     ▼
  ① Firewall / Security Group     443-ə icazə var?            [OK]
     ▼
  ② NAT (DNAT)                    203.0.113.10:443 → 192.168.0.10:443
     ▼
  ③ Load Balancer 192.168.0.10    sağlam server seç           → 192.168.0.22
     ▼
  ④ Server 192.168.0.22:8080      cavab verir
     ▲
  Cavab eyni yolla geri qayıdır (stateful firewall və NAT cədvəli xatırlayır)
```

> Bu 4 addımın **hər birində** bağlantı pozula bilər. Simptom → addım: `timeout` (①), yanlış server/IP (②), bəzi sorğular xətalıdır (③), `refused` və ya 5xx (④).

---

## 🧪 Lab 5a — açıq və private IP-ni gör

```bash
$ ip -br a | grep -v lo           # daxili (private) ünvan
enp0s3  UP   192.168.0.15/24

$ curl -s https://ifconfig.me ; echo   # internetin gördüyü ünvan
203.0.113.10
```

| Müşahidə | Nə göstərir |
|---|---|
| **İki fərqli IP** | cihazın **private** IP-si və **NAT-dan sonrakı açıq** IP |
| Eyni şəbəkədəki digər cihazda eyni `curl` | **eyni açıq IP** çıxır (hamı bir ünvan kimi görünür) |

> Nəticə **nümunədir**, sənin açıq IP-n fərqli olacaq. Bəzən daxili cihazda `ifconfig.me` ünvanı provayderin **CGNAT** ünvanı ola bilər, bu normaldır.

---

## 🧪 Lab 5b — `DROP` və `REJECT` yarat

**Server VM-də** (`192.168.0.21`), `nc -l 9000` işləsin. **Client-dən** yoxla:

```bash
$ nc -vz 192.168.0.21 9000
Connection to 192.168.0.21 9000 port [tcp] succeeded!         # firewall yoxdur
```

Serverdə `ufw` ilə port 9000-i **bağla** və yenidən yoxla:

```bash
$ sudo ufw enable
$ sudo ufw deny 9000/tcp       # DROP kimi davranır
$ nc -vz -w 5 192.168.0.21 9000        # → timed out
$ sudo ufw reject 9000/tcp     # REJECT
$ nc -vz -w 5 192.168.0.21 9000        # → Connection refused
```

> `ufw deny` → **`timeout`**, `ufw reject` → **`refused`**. Lab bitəndə `sudo ufw delete reject 9000/tcp` və `sudo ufw disable` et. **SSH-da işləyirsənsə**, `enable`-dən **əvvəl** `sudo ufw allow 22/tcp` yaz, yoxsa özünü bağlaya bilərsən.

---

## 🧪 Lab 5c — `nginx` ilə yükü böl

İki sadə backend və bir `nginx` LB (`192.168.0.10`):

```bash
# backend 1 və 2 (iki terminal)
$ python3 -m http.server 8001 --directory /tmp/b1
$ python3 -m http.server 8002 --directory /tmp/b2
```

```nginx
# /etc/nginx/conf.d/lb.conf
upstream backend { server 127.0.0.1:8001; server 127.0.0.1:8002; }
server { listen 80; location / { proxy_pass http://backend; } }
```

```bash
$ for i in 1 2 3 4; do curl -s localhost | head -c 20; echo; done   # cavablar növbə ilə dəyişir
```

> Backend 2-ni dayandır və yenə `curl` et: **`nginx`** pis serveri **sıradan çıxarır** (passive health check). Bu, load balancer-in **dayanıqlılıq** vədidir.

---

## 🪜 Problemlər: simptom → səbəb

| Simptom | Ehtimal olunan səbəb | Yoxla |
|---|---|---|
| Daxili cihaz internetə **çıxmır** | NAT yoxdur, **default gateway** yanlış | `ip r`, router NAT qaydası |
| `timeout`, `ping` var | **firewall DROP** (host, SG, NACL) | qaydaları qat-qat yoxla |
| `refused` | firewall **REJECT** və ya **servis yoxdur** | `ss -tln`, qayda |
| Xaricdən servis **əlçatmazdır** | **DNAT / port forward** yoxdur | NAT qaydası, SG |
| Bəzən işləyir, bəzən yox | **bir backend** xəstədir | LB health check, backend log |
| Hər sorğu **eyni serverə** gedir | **IP hash / sticky** | LB alqoritmi |

---

## 🛠️ DevOps-da bu anlayışlar harada?

| Alət | Burada nə edir |
|---|---|
| **AWS Security Group** | stateful firewall, qayda: mənbə + port |
| **AWS NAT Gateway** | private subnet-dən internetə çıxış |
| **AWS ALB / NLB** | L7 / L4 load balancer |
| **Kubernetes Service** | pod-lara daxili LB və DNAT |
| **Ingress** | L7 giriş nöqtəsi (host/yol qaydası) |
| **NetworkPolicy** | pod-lararası firewall |
| **`nginx` / `HAProxy`** | proqram LB və reverse proxy |

> Terraform ilə Security Group yazanda **hər qayda** bu dərsdəki 4 sütundur: mənbə, təyinat, protokol:port, əməl.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"SNAT, DNAT və masquerade fərqini `192.168.0.15` cihazı üzərindən izah et. NAT cədvəlinin necə dəyişdiyini göstər."*
- *"Stateful və stateless firewall fərqi nədir? AWS Security Group və NACL hansıdır, niyə?"*
- *"L4 və L7 load balancer arasında necə seçim edim? Hər biri üçün 2 real nümunə ver."*

</div>

<div class="ai-tip">💡 AI cavabını <b>öz VM-ində yoxla</b>: <code>ufw</code> qaydası yaz, <code>nc</code> ilə <code>timeout</code> və <code>refused</code> yarat, sonra cavabla tutuşdur.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **NAT** | private IP-lər **bir açıq IP** ilə internetə çıxır |
| **SNAT** | **mənbə** ünvanı dəyişir (çıxış) |
| **DNAT** | **təyinat** ünvanı dəyişir (port forwarding) |
| **Firewall** | qayda: mənbə, təyinat, protokol:port, əməl |
| **Stateful** | bağlantını izləyir, cavab **avtomatik** keçir |
| **Default deny** | icazə verilməyən hər şey **bağlıdır** |
| **DROP / REJECT** | `timeout` / `refused` |
| **Load balancer** | yükü paylayır, **health check** ilə xəstəni çıxarır |
| **L4 / L7** | port ilə / HTTP məzmunu ilə qərar |

**Növbəti dərs:** Sessiya 06 — **Subnet, CIDR və routing**.

---

## ✍️ Tapşırıqlar

1. **NAT:** `192.168.0.15` internetə çıxanda router **hansı ünvanı** dəyişir? Cavab paketi geri **hansı cihaza** necə qayıdır?
2. **SNAT vs DNAT:** Hər birinə **bir real nümunə** ver. Hansı daxili servisi xaricə **açır**?
3. **Firewall:** `192.168.0.0/24`-dən SSH, hamıdan HTTPS icazə verən **4 qaydalı** cədvəl yaz (default deny daxil).
4. **DROP/REJECT:** Hansı `timeout`, hansı `refused` verir? Client-in **ilk nəyi yoxlayacağını** izah et.
5. **Lab 5a:** `ip -br a` və `curl ifconfig.me` nəticələrini müqayisə et. **Niyə fərqlidir**?
6. **Lab 5b:** `ufw deny` və `ufw reject` ilə **iki fərqli xəta** yarat. Sonra qaydaları **sil**.
7. **Lab 5c:** `nginx` ilə iki backend arasında yükü böl. Birini dayandır: **nə dəyişir**?
8. **Diaqnostika:** Sorğu `timeout` verir. **4 yoxlama yeri** (host firewall, SG, NACL, NetworkPolicy) sıra ilə **nəyi yoxlayarsan**?

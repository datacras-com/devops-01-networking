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

# 🧱 Linux Firewall: ufw, iptables, nftables

## Qaydanı özün yaz: Linux-da paketi kim buraxır, kim atır?

```
  paket gəlir ──►  [ netfilter  (kernel) ]  ──►  proqram (sshd, nginx ...)
                          ▲
            qaydaları buraya yazan "pultlar":
            ufw  ·  iptables  ·  nftables  ·  firewalld
```

DataCras DevOps & Platform Engineering · Networking Modulu · Sessiya 09 · Chapter 4 — Tətbiq qatı və Təhlükəsizlik

---

## 🎯 Bu dərsdə nə öyrənəcəksən?

- **Linux-da firewall harada işləyir:** kernel `netfilter`, alətlər onun "pultu"dur
- **`ufw`:** ən asan yol, `allow`, `deny`, `reject`, `delete`
- **`iptables`:** cədvəl (table), zəncir (chain), qayda, sıra
- **`ESTABLISHED,RELATED`** qaydası niyə vacibdir və **default DROP** necə qurulur
- **`nftables`:** yeni nəsil və `iptables` ilə fərqi
- **Özünü kilidləmə:** SSH-ı kəsmədən firewall dəyişmək
- **Problem həlli:** port açıqdır, amma çatmır. Haradan baxaq?

> Sessiya 05-də firewall-un **nə** olduğunu öyrəndin (stateful, default deny, `DROP` və `REJECT`). Bu dərs **Linux-da əlinlə necə yazılır** sualının cavabıdır.

---

## 🧩 Problem: qayda anlayışı var, bəs Linux-da hara yazım?

Sessiya 05-də qaydanın 4 hissəsini bilirik: **mənbə, təyinat, protokol:port, əməl**.

Linux-da bu qaydaları saxlayan və paketə tətbiq edən şey **ayrıca proqram deyil**. O, **kernelin** içindədir.

| Sual | Cavab |
|---|---|
| Paketə kim baxır? | kernelin içindəki **`netfilter`** |
| Qaydanı kim yazır? | sənin əmrin: `ufw`, `iptables`, `nft` |
| Servis lazımdır? | **yox**, qayda kernelə yazılan kimi işləyir |

> **Analogiya:** `netfilter` binanın **giriş turniketidir**. `ufw` və `iptables` isə turniketə **qayda yazan pultlardır**. Turniket bir dənədir, pult müxtəlifdir.

---

## 🗺️ Alətlər xəritəsi

| Alət | Nədir | Harada rast gəlinir |
|---|---|---|
| `netfilter` | kernelin firewall mexanizmi | **hər Linux-da** |
| `ufw` | sadə pult (qısa əmrlər) | Ubuntu, Debian |
| `iptables` | klassik pult, detallı | köhnə və çox mühit |
| `nftables` (`nft`) | yeni nəsil pult | yeni distributivlər |
| `firewalld` | zonalarla pult | RHEL, Rocky, CentOS |

> **Vacib:** hamısı **eyni** kernel mexanizminə yazır. Müasir Ubuntu-da `iptables` əmri çox vaxt arxada `nftables` ilə işləyir. **Bir serverdə iki pultu qarışdırma**: qaydalar bir-birini pozar.

---

## 🟢 ufw: ən asan başlanğıc

`ufw` (Uncomplicated Firewall) = "mürəkkəb olmayan firewall". Əvvəl **vəziyyətə** baxaq:

```bash
$ sudo ufw status verbose
Status: inactive
```

- `inactive` = firewall **söndürülüb**, bütün trafik keçir
- Qayda yazmaq olar, amma `enable` deyənə qədər **işləmir**

```bash
$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```

> **Xəbərdarlıq:** SSH ilə işləyirsənsə, `enable`-dən **əvvəl** `sudo ufw allow 22/tcp` yaz. Əks halda **özünü bağlaya** bilərsən (aşağıda ayrıca slayd var).

---

## 📜 ufw: default siyasət

`ufw` qayda yazmazdan əvvəl **iki ümumi qərar** verir: gələn və çıxan trafik üçün.

```bash
$ sudo ufw default deny incoming     # kənardan gələn: hamısı bağlı
$ sudo ufw default allow outgoing    # içəridən çıxan: hamısı açıq
```

| Qərar | Mənası |
|---|---|
| `deny incoming` | icazə verilməyən **gələn** hər şey bağlıdır (**default deny**, Sessiya 05) |
| `allow outgoing` | server internetə, DNS-ə, `apt`-a **çıxa bilir** |

> Bu iki sətir ən yaxşı başlanğıcdır: **gələni bağla, çıxanı burax**, sonra yalnız lazım olanı **aç**.

---

## ✅ ufw: allow, deny, reject

```bash
$ sudo ufw allow 22/tcp       # 22/tcp-ə hamıdan icazə
$ sudo ufw allow 443/tcp      # HTTPS
$ sudo ufw deny 5432/tcp      # DROP: səssiz at  (client: timeout)
$ sudo ufw reject 9000/tcp    # REJECT: "yox" de (client: refused)
```

| Əmr | Client nə görür | Sessiya 05 |
|---|---|---|
| `allow` | bağlantı **açılır** | ACCEPT |
| `deny` | **timeout** (cavab yoxdur) | DROP |
| `reject` | **refused** (dərhal rədd) | REJECT |

> `22/tcp` yazanda **yalnız TCP** açılır. Yalnız `22` yazsan, **həm TCP, həm UDP** açılar (Sessiya 04). Lazım olmayanı açma.

---

## 🎯 ufw: yalnız müəyyən şəbəkədən icazə

"SSH yalnız ofis şəbəkəsindən" (`192.168.0.0/24`) qaydası:

```bash
$ sudo ufw allow from 192.168.0.0/24 to any port 22 proto tcp
Rule added
```

Oxu: **`192.168.0.0/24`-dən → bu serverin istənilən (`any`) ünvanına → port 22, tcp**.

| Hissə | Mənası |
|---|---|
| `from 192.168.0.0/24` | **mənbə** (Sessiya 02: CIDR) |
| `to any` | **təyinat**: bu serverin hər ünvanı |
| `port 22 proto tcp` | **protokol:port** |

> Sessiya 05-dəki 4 hissəli qayda burada **sözlə** yazılıb. Yalnız tək IP üçün: `from 192.168.0.15`.

---

## 🔎 ufw status çıxışını oxuyaq

```bash
$ sudo ufw status verbose
Status: active
Default: deny (incoming), allow (outgoing), disabled (routed)   # <- default-lar
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    192.168.0.0/24          # <- 1
443/tcp                    ALLOW IN    Anywhere                # <- 2
5432/tcp                   DENY IN     Anywhere                # <- 3
```

| Sütun | Mənası |
|---|---|
| `To` | server tərəfdə **hansı port** |
| `Action` | `ALLOW IN` = **gələn** trafiki burax |
| `From` | **kimdən** (`Anywhere` = hamıdan) |

> Çıxış **nümunədir**. `Default:` sətri ilk baxılacaq yerdir: gələn trafik üçün **deny** olduğuna əmin ol.

---

## 🔢 ufw: nömrələ, əlavə et, sil

```bash
$ sudo ufw status numbered
     To                         Action      From
[ 1] 22/tcp                     ALLOW IN    192.168.0.0/24
[ 2] 443/tcp                    ALLOW IN    Anywhere
[ 3] 5432/tcp                   DENY IN     Anywhere

$ sudo ufw delete 3                        # nömrə ilə sil
$ sudo ufw delete allow 443/tcp            # qaydanın özü ilə sil
$ sudo ufw insert 1 deny from 192.168.0.99 # 1-ci yerə əlavə et
```

> Qaydalar **yuxarıdan aşağı** yoxlanır, **ilk uyğun gələn qalib gəlir** (Sessiya 05). Ona görə **`deny`** qaydası `allow`-dan **yuxarıda** olmalıdır, yoxsa heç vaxt işləməz. `delete`-dən sonra **nömrələr dəyişir**, əvvəl yenidən `numbered` bax.

---

## 🛡️ ufw: faydalı əlavələr

```bash
$ sudo ufw limit 22/tcp        # 30 saniyədə 6+ cəhd edəni müvəqqəti blokla
$ sudo ufw disable             # firewall-u söndür (qaydalar qalır)
$ sudo ufw reset               # HAMISINI sil və söndür
$ sudo ufw logging on          # bloklananları log-a yaz
```

| Əmr | Nə vaxt |
|---|---|
| `limit` | SSH-a **parol sındırma** cəhdlərinə qarşı |
| `disable` | test zamanı "firewall problemdir?" yoxlaması |
| `reset` | sıfırdan başlamaq (**diqqət**: SSH qaydası da silinir) |

> `ufw` rahatdır, amma **sadə** ssenarilər üçündür. NAT, `FORWARD`, ətraflı idarəetmə üçün **`iptables`** və ya **`nftables`** lazımdır. İndi arxa tərəfə baxaq.

---

## 🧭 iptables: paketin yolu

Paket Linux-a gələndə **yoldakı məntəqələrdən** keçir. Hər məntəqənin adı **chain** (zəncir) dir:

```
  A) Mənə gələn:
     gələn ──► PREROUTING ──► marşrut qərarı ──► INPUT ──► yerli proqram
               (nat: DNAT)    "mənə aiddir?"               (sshd, nginx)

  B) Məndən keçən (router/VPN/Docker):
     gələn ──► PREROUTING ──► marşrut qərarı ──► FORWARD ──► POSTROUTING ──► çıxış

  C) Məndən çıxan:
     yerli proqram ──► OUTPUT ──► POSTROUTING ──► çıxış   (nat: SNAT/masquerade)
```

> **Analogiya:** hava limanı. Paket **PREROUTING**-də qeydiyyatdan keçir, sonra **marşrut qərarı**: bu **mənim** paketimdir (`INPUT`), yoxsa **tranzitdir** (`FORWARD`)?

---

## 📦 Cədvəllər və zəncirlər

`iptables`-da **iki əsas cədvəl** (table) var. Cədvəl = qaydaların **qrupu**:

| Cədvəl | Nə üçün | Zəncirləri |
|---|---|---|
| `filter` (default) | **burax / at** qərarı | `INPUT`, `OUTPUT`, `FORWARD` |
| `nat` | **ünvanı dəyiş** (Sessiya 05) | `PREROUTING`, `POSTROUTING`, `OUTPUT` |

| Zəncir | Hansı paketə baxır |
|---|---|
| `INPUT` | **bu serverə** gələn |
| `OUTPUT` | **bu serverdən** çıxan |
| `FORWARD` | serverdən **keçib** başqasına gedən (router/VPN/Docker) |

> Adi veb server üçün demək olar ki, **yalnız `filter` cədvəli, `INPUT` zənciri** lazımdır. `FORWARD` və `nat` server **router kimi** işləyəndə (VPN, Docker) önəmli olur.

---

## 🧱 iptables: bir qaydanın quruluşu

```bash
sudo iptables -A INPUT -p tcp -s 192.168.0.0/24 --dport 22 -j ACCEPT
```

| Hissə | Mənası |
|---|---|
| `-A INPUT` | **A**ppend: `INPUT` zəncirinin **sonuna** əlavə et |
| `-p tcp` | protokol: TCP |
| `-s 192.168.0.0/24` | **s**ource: mənbə |
| `--dport 22` | **d**estination **port**: təyinat portu |
| `-j ACCEPT` | **j**ump: nə et (`ACCEPT`, `DROP`, `REJECT`) |

> Cümlə kimi oxu: "**INPUT**-a gələn, **tcp**, mənbəyi **`192.168.0.0/24`**, portu **22** olan paketi **burax**". Əmr çıxış vermir: **səssiz** işləyir. Yoxlamaq üçün `-L` lazımdır.

---

## 👀 iptables: qaydalara baxmaq

```bash
$ sudo iptables -L INPUT -n -v --line-numbers
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)              # <- default policy
num  pkts bytes target  prot opt in  out  source           destination
1     412 30K   ACCEPT  tcp  --  *   *    192.168.0.0/24   0.0.0.0/0   tcp dpt:22
2       0    0  DROP    tcp  --  *   *    0.0.0.0/0        0.0.0.0/0   tcp dpt:5432
```

| Bayraq / sütun | Mənası |
|---|---|
| `-n` | ünvanları **ad əvəzinə rəqəmlə** göstər (sürətli) |
| `-v`, `pkts` | neçə paket **bu qaydaya uyğun gəldi** |
| `--line-numbers`, `num` | qayda **nömrəsi** (silmək üçün) |
| `policy ACCEPT` | heç bir qayda uyğun gəlməsə **nə olur** |

> Çıxış **nümunədir**. `pkts` sütunu qiymətlidir: qayda **işləyirmi** görürsən (`0` = heç vaxt uyğun gəlməyib).

---

## 🔀 -A və -I: sıra niyə vacibdir?

`-A` qaydanı **sona** əlavə edir. `-I` **istədiyin yerə** (default: başa) qoyur.

```bash
$ sudo iptables -A INPUT -p tcp --dport 22 -j DROP        # 1-ci qayda (səhv)
$ sudo iptables -A INPUT -p tcp -s 192.168.0.15 --dport 22 -j ACCEPT   # 2-ci

# 192.168.0.15 SSH ilə girə BİLMƏZ: 1-ci qayda ilk uyğun gəlir (DROP)
$ sudo iptables -I INPUT 1 -p tcp -s 192.168.0.15 --dport 22 -j ACCEPT
# -I ... 1  : ACCEPT qaydasını 1-ci yerə qoyur, indi işləyir
```

```
  Sıra:   1. ACCEPT 192.168.0.15 -> 22     2. DROP hamı -> 22
          ilk uyğun gələn qalib: .15 buraxılır, qalanları atılır
```

> **Qayda:** **daha dar/istisna** qayda **yuxarıda**, **geniş** qayda **aşağıda**. Bu, `ufw`-dakı eyni məntiqdir.

---

## 🤝 ESTABLISHED,RELATED: niyə bu qayda şərtdir?

**Problem.** Sən serverə SSH ilə girmisən. Server **cavab paketləri** göndərir. `INPUT` zəncirini **hər şeyi atacaq** şəkildə qursan, cavab trafiki də **atılar**.

**Həll.** Stateful firewall (Sessiya 05) bağlantını **xatırlayır**:

```bash
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

| Hal | Mənası |
|---|---|
| `NEW` | bağlantının **ilk** paketi |
| `ESTABLISHED` | artıq **açılmış** bağlantının paketi |
| `RELATED` | açıq bağlantı ilə **əlaqəli** (məs. ICMP xətası) |

> Bu qayda olmasa, **mövcud SSH sessiyan dondurulur**. Ona görə hər `INPUT` siyasətinin **ilk qaydası** budur.

---

## 🚧 Default policy DROP: düzgün sıra

**Default policy** = heç bir qayda uyğun gəlməyəndə nə olsun. Sessiya 05-dəki **default deny** budur.

```bash
# 1) ƏVVƏL icazələr
$ sudo iptables -A INPUT -i lo -j ACCEPT                  # lo = özü ilə danışıq
$ sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
$ sudo iptables -A INPUT -p tcp -s 192.168.0.0/24 --dport 22 -j ACCEPT
$ sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
# 2) ƏN SONDA default policy
$ sudo iptables -P INPUT DROP
```

> **Xəbərdarlıq:** `-P INPUT DROP` **ən sonda** yazılır. Əvvəl yazsan, SSH dərhal kəsilə bilər. Və **`iptables -F`** (hamısını sil) **policy-ni sıfırlamır**: policy `DROP` qalıb qaydalar silinsə, **hər şey bağlanır**.

---

## 🗑️ iptables: qaydanı silmək

```bash
$ sudo iptables -L INPUT -n --line-numbers      # 1) nömrəyə bax
$ sudo iptables -D INPUT 2                      # 2) nömrə ilə sil
$ sudo iptables -D INPUT -p tcp --dport 443 -j ACCEPT   # və ya qaydanın özü ilə
$ sudo iptables -S INPUT                        # əmr formatında göstər
-P INPUT DROP
-A INPUT -i lo -j ACCEPT
```

| Əmr | Nə edir |
|---|---|
| `-D INPUT 2` | 2-ci qaydanı sil (sonra nömrələr **sürüşür**) |
| `-D INPUT <qayda>` | `-A` ilə yazdığın qaydanı **eynilə** təkrarla, `-A` əvəzinə `-D` |
| `-S` | qaydaları əmr kimi göstər (kopyalamaq üçün yaxşı) |

> `-F` (flush) bütün qaydaları silir. **SSH-da və policy `DROP`-da heç vaxt işlətmə** (əvvəlcə `-P INPUT ACCEPT`).

---

## 🔁 iptables: NAT nümunələri (Sessiya 05 əmrlə)

```bash
# SNAT/masquerade: 192.168.0.0/24 internetə eth0-dan çıxsın
$ sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
$ sudo sysctl -w net.ipv4.ip_forward=1       # kernel paketi "ötürə" bilsin

# DNAT: xaricdən 8080 -> daxili 192.168.0.21:80
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 \
    -j DNAT --to-destination 192.168.0.21:80
$ sudo iptables -A FORWARD -d 192.168.0.21 -p tcp --dport 80 -j ACCEPT
```

| Əmr | Hansı zəncir | Sessiya 05 |
|---|---|---|
| `MASQUERADE` | `POSTROUTING` (**çıxışda**) | SNAT |
| `DNAT` | `PREROUTING` (**girişdə**) | port forwarding |

> `ip_forward=1` olmasa, Linux **router kimi** paketi ötürmür. NAT qaydası yazsan da işləməz.

---

## 💾 Qaydalar reboot-da itir!

`iptables` qaydaları **yalnız yaddaşdadır**. Server yenidən başlayanda **hamısı silinir**.

| Əmr | Nə edir |
|---|---|
| `sudo iptables-save > /root/fw.bak` | hazırkı qaydaları **fayla yaz** (ehtiyat) |
| `sudo iptables-restore < /root/fw.bak` | fayldan **geri yüklə** |
| `sudo apt install iptables-persistent` | Debian/Ubuntu: açılışda avtomatik yükləmə |
| `sudo netfilter-persistent save` | qaydaları `/etc/iptables/rules.v4`-ə **daimi** yaz |

> `ufw enable` etdikdə qaydalar **özü daimi** olur. `iptables`-da isə `save` etməsən, test qaydaların **bir reboot-da yoxa çıxır**. Bəzən bu **yaxşıdır** (səhv qayda reboot ilə sönür), amma **sərt** qaydaya güvənmə.

---

## 🆕 nftables: yeni nəsil

`nftables` = `iptables`-ın **varisi**. Eyni kernel `netfilter`, **yeni pult**.

```bash
$ sudo nft list ruleset                    # bütün qaydalara bax
table inet filter {
  chain input {
    type filter hook input priority 0; policy drop;
    iif lo accept
    ct state established,related accept
    ip saddr 192.168.0.0/24 tcp dport 22 accept
    tcp dport { 80, 443 } accept
  }
}
```

- `table inet filter` = **IPv4 və IPv6** bir cədvəldə
- `hook input` = `INPUT` zənciri, `policy drop` = default DROP
- `{ 80, 443 }` = **set**: bir sətirdə bir neçə port

---

## ⚖️ iptables və nftables: müqayisə

| Mövzu | `iptables` | `nftables` |
|---|---|---|
| Əmr | `iptables`, `ip6tables` (ayrı) | **`nft`** (hamısı bir) |
| IPv4/IPv6 | ayrı qaydalar | `inet` ilə **birlikdə** |
| Çox port | hər port ayrı qayda | `{ 80, 443 }` **set** |
| Qaydaları yükləmə | **bir-bir** | fayl, **atomik** |
| Vəziyyət | köhnə, hələ geniş | **yeni**, tövsiyə olunan |

```bash
$ iptables-translate -A INPUT -p tcp --dport 22 -j ACCEPT
nft 'add rule ip filter INPUT tcp dport 22 counter accept'
```

> Yeni server üçün `ufw` və ya `nftables`. **Köhnə** mühitdə `iptables`-ı oxuya bilməlisən. `iptables-translate` köhnə qaydanı `nft`-ə **çevirir**.

---

## 😱 Özünü kilidləmək: ssenari

SSH ilə uzaq serverdəsən və firewall-u sərtləşdirirsən:

```
  1. Sən SSH ilə 192.168.0.21-ə qoşulmusan
  2. sudo iptables -P INPUT DROP          <- ESTABLISHED qaydası olmadan
  3. Cavab paketləri atılır: terminal DONUR
  4. Yenidən qoşulmaq da mümkün deyil (22 açıq deyil)
  5. Yeganə yol: cloud konsolu / fiziki giriş
```

| Səhv | Niyə baş verir |
|---|---|
| `ESTABLISHED` qaydası yoxdur | cavablar atılır |
| `allow 22` yoxdur | yeni giriş yoxdur |
| `-P DROP` əvvəl yazıldı | qayda əlavə etməyə vaxt qalmır |

> **Qayda №1:** uzaq maşında firewall dəyişəndə **ikinci SSH sessiyasını açıq saxla** və **avtomatik geri qaytarma** qur (növbəti slayd).

---

## 🛟 Təhlükəsiz tətbiq: avtomatik geri qaytarma

**Plan:** qaydanı yaz, **5 dəqiqə sonra** köhnə qaydaları **özü qaytaran** vaxtölçən qur. SSH işləsə, vaxtölçəni **ləğv et**.

```bash
# 1) Ehtiyat al
$ sudo iptables-save | sudo tee /root/fw.bak > /dev/null
# 2) 5 dəqiqə sonra geri qaytar (at ilə)
$ echo "iptables-restore < /root/fw.bak" | sudo at now + 5 minutes
# 3) İndi qaydanı dəyiş ... yeni SSH sessiyası ilə yoxla
# 4) İşləyirsə: planlanmış geri qaytarmanı ləğv et
$ sudo atq            # siyahı (iş nömrəsi)
$ sudo atrm 1         # nömrə 1-i ləğv et
```

> `at` yoxdursa: `sudo apt install at`. Alternativ: `(sleep 300; iptables-restore < /root/fw.bak) &` (ləğv: `kill`). Kilidlənsən, **5 dəqiqə gözlə**, firewall özü qayıdır.

---

## 🐳 Docker və firewall toqquşması

**Problem.** `ufw deny 5432`, amma `docker run -p 5432:5432 ...` portu **xaricə açır**. Niyə?

```
  Docker -p  ──► nat PREROUTING (DNAT) ──► FORWARD ──► konteyner 172.17.0.2
  ufw qaydaları ──────────► INPUT zənciri (bu yola BAXMIR)
```

- Docker **öz iptables qaydalarını** (`DOCKER` zənciri) yazır
- Trafik `INPUT`-dan **keçmir**, `FORWARD`-dan keçir, `ufw`-nun `INPUT` qaydası görmür
- **Həll:** portu yalnız lokala bağla: `docker run -p 127.0.0.1:5432:5432 ...`

> `ss -tlnp` ilə yoxla: `127.0.0.1:5432` = yalnız **lokal**, `0.0.0.0:5432` = **hamıya açıq**.

---

## 🪜 Problem həlli nərdivanı: port açıqdır, amma çatmır

Client-də `timeout` və ya `refused`. **Yuxarıdan aşağı** yoxla (Sessiya 04):

| Addım | Əmr (serverdə) | Nəyi göstərir |
|---|---|---|
| 1. Servis **dinləyir**? | `ss -tln` | `LISTEN` və **hansı ünvanda** (`0.0.0.0` / `127.0.0.1`) |
| 2. Host firewall? | `sudo ufw status` və ya `sudo iptables -L -n -v` | `DROP`/`REJECT` qaydası, `pkts` artır? |
| 3. Cloud firewall? | **Security Group**, NACL | Sessiya 05: AWS qatı |
| 4. Başqa qat? | Docker, NetworkPolicy | `-p` və Kubernetes |

> `refused` = adətən **servis dinləmir** və ya **REJECT** (addım 1-2). `timeout` = adətən **DROP** (addım 2-3). **Hər addımı** ardıcıl yoxla, atlama.

---

## ⚠️ Tipik səhvlər

| Səhv | Nəticə | Düzgün |
|---|---|---|
| `ufw enable` SSH icazəsiz | SSH kəsilir | əvvəl `ufw allow 22/tcp` |
| `-P INPUT DROP` əvvəl | SSH donur | policy **sonda**, `ESTABLISHED` ilə |
| `iptables -F` policy `DROP` ikən | hər şey bağlanır | əvvəl `-P INPUT ACCEPT` |
| `DROP` `ACCEPT`-dən yuxarıda | icazə **işləmir** | dar qayda **yuxarıda** |
| `iptables` + `ufw` qarışıq | qaydalar toqquşur | **bir** alət seç |
| `save` etməmək | reboot-da qayda itir | `netfilter-persistent save` |

> Hər dəfə dəyişiklikdən **sonra** yenidən **yeni SSH sessiyası** aç və yoxla. Köhnə sessiya `ESTABLISHED` qaydasına görə **işləyə bilər**, yeni giriş isə **bağlıdır**.

---

## 🧪 Lab 9a — ufw ilə təhlükəsiz başlanğıc

**Test VM-də** işlə (SSH ilə girməmisənsə, ən təhlükəsizi). Servis üçün: `python3 -m http.server 8080 &`

```bash
$ sudo ufw status                      # 1) Status: inactive gözlənilir
$ sudo ufw allow 22/tcp                # 2) SSH icazəsi (ƏVVƏL!)
$ sudo ufw enable                      # 3) y yaz: "active and enabled"
$ sudo ufw status numbered             # 4) 22/tcp ALLOW görünür
$ sudo ufw deny 8080/tcp               # 5) client-dən: nc -vz -w 3 192.168.0.21 8080
                                       #    -> timed out
$ sudo ufw reject 8080/tcp             # 6) yenə nc -> Connection refused
```

> 5-ci addımda **ilk `deny` qaydasını** silmədən `reject` əlavə etsən, ilk qayda qalib gəlir. Əvvəl `sudo ufw delete deny 8080/tcp` et, sonra yoxla. Lab bitəndə: `sudo ufw delete reject 8080/tcp`.

---

## 🧪 Lab 9b — iptables: DROP, REJECT, say

Servis `8080`-də işləyir. **Əvvəl ufw-nu söndür**: `sudo ufw disable`.

```bash
$ sudo iptables -I INPUT -p tcp --dport 8080 -j DROP
$ curl --connect-timeout 3 192.168.0.21:8080     # client-dən -> timed out
$ sudo iptables -L INPUT -n -v                   # pkts artdı (qayda işləyir)
$ sudo iptables -D INPUT -p tcp --dport 8080 -j DROP
$ sudo iptables -I INPUT -p tcp --dport 8080 -j REJECT --reject-with tcp-reset
$ curl --connect-timeout 3 192.168.0.21:8080     # -> Connection refused
$ sudo iptables -D INPUT -p tcp --dport 8080 -j REJECT --reject-with tcp-reset
```

| Addım | Gözlənilən nəticə |
|---|---|
| `DROP` + `curl` | 3 san. gözləyib **timeout** |
| `-L -n -v` | `pkts` sütunu **0-dan böyük** |
| `REJECT` + `curl` | **dərhal** `refused` |

---

## 🧪 Lab 9c — ehtiyat al və geri qaytar

**Təhlükəsiz:** ancaq **test VM-də**, SSH deyil, konsoldan işlə. Məqsəd: `save` və `restore` əmrini görmək.

```bash
$ sudo iptables-save | sudo tee /root/fw.bak > /dev/null   # 1) ehtiyat
$ sudo iptables -A INPUT -p tcp --dport 8080 -j DROP       # 2) test qaydası
$ sudo iptables -L INPUT -n --line-numbers                 # 3) 1 qayda görünür
$ sudo iptables-restore < /root/fw.bak                     # 4) geri qaytar
$ sudo iptables -L INPUT -n --line-numbers                 # 5) qayda yoxdur
```

| Addım | Gözlənilən nəticə |
|---|---|
| 3 | `dpt:8080` qaydası siyahıda |
| 5 | qayda **yoxa çıxıb**, ehtiyat işlədi |

> Bu, "özünü kilidləmə"dən **qorunma** təcrübəsidir. İstəsən 2-ci addımın əvəzinə `at now + 1 minutes` ilə **avtomatik** versiyanı sına.

---

## 🧪 Lab 9d — Docker `-p` və ufw (istəyə görə)

Docker qurulubsa. `ufw` aktivdir və `deny 5432` var.

```bash
$ docker run -d --name pg1 -p 5432:5432 -e POSTGRES_PASSWORD=x postgres
$ ss -tlnp | grep 5432                     # 0.0.0.0:5432 = hamıya açıq
$ nc -vz 192.168.0.21 5432                 # client-dən -> uğurlu (ufw yan keçildi!)
$ docker rm -f pg1
$ docker run -d --name pg2 -p 127.0.0.1:5432:5432 -e POSTGRES_PASSWORD=x postgres
$ ss -tlnp | grep 5432                     # 127.0.0.1:5432 = yalnız lokal
$ nc -vz 192.168.0.21 5432                 # client-dən -> artıq çatmır
$ docker rm -f pg2
```

> Gözlənilən: `pg1` ilə kənardan **çatır**, `pg2` ilə **çatmır**. Nəticə nümunədir, Docker versiyasından asılı ola bilər.

---

## 🤖 Bu mövzu üçün AI prompt-lar

<div class="ai-box">

### 🤖 AI ilə öyrən

- *"Ubuntu serveri üçün `ufw` qaydaları yaz: SSH yalnız `192.168.0.0/24`-dən, 443 hamıya, qalan hər şey qadağan, SSH-a rate limit. Hər əmri izah et."*
- *"Bu `iptables -S` çıxışımı yoxla: [yapışdır]. Sıra səhvi, artıq qayda, özümü kilidləmə riski varmı?"*
- *"Bu `iptables` qaydalarını `nftables`-ə çevir və hər sətri izah et: [qaydalar]."*

</div>

<div class="ai-tip">💡 AI-nin verdiyi firewall əmrini <b>uzaq maşında birbaşa işlətmə</b>. Əvvəl test VM-də sına, <code>iptables-save</code> ehtiyatı və geri qaytarma planı hazır olsun.</div>

---

## 📝 Xülasə

| Mövzu | Yadda saxla |
|---|---|
| **netfilter** | kernelin firewall-u, `ufw`/`iptables`/`nft` onun pultlarıdır |
| **ufw** | `default deny incoming`, `allow`, `deny` (timeout), `reject` (refused) |
| **Sıra** | yuxarıdan aşağı, ilk uyğun gələn qalib. `-I` başa, `-A` sona |
| **Zəncirlər** | `INPUT` (mənə), `OUTPUT` (məndən), `FORWARD` (keçən) |
| **ESTABLISHED,RELATED** | olmadan mövcud SSH **donur** |
| **Policy DROP** | həmişə **ən sonda**, `-F` onu sıfırlamır |
| **Reboot** | `iptables-save`, `netfilter-persistent save` |
| **Docker** | `-p` ufw-nu yan keçir: `127.0.0.1:port` |
| **Problem həlli** | `ss -tln` → host firewall → Security Group |

**Növbəti dərs:** Sessiya 10 — **`tcpdump`**: paketləri canlı görmək.

---

## ✍️ Tapşırıqlar

1. **netfilter:** `ufw`, `iptables` və `nft` arasındakı əlaqəni 2 cümlə ilə izah et.
2. **ufw:** SSH yalnız `192.168.0.0/24`-dən, HTTPS hamıdan, qalan gələn trafik bağlı olan qaydaları yaz.
3. **Sıra:** `DROP hamı->22` ilə `ACCEPT 192.168.0.15->22` qaydalarının hansı sıra ilə **işləmədiyini** izah et. Düzəlt.
4. **ESTABLISHED:** Bu qayda olmadan `-P INPUT DROP` nə ilə nəticələnir? **Niyə**?
5. **Lab 9b:** `DROP` və `REJECT` qaydaları ilə `timeout` və `refused` yarat. `pkts` sütunu nə göstərir?
6. **Təhlükəsizlik:** SSH ilə firewall dəyişərkən **hansı 2 tədbiri** görərsən?
7. **Docker:** `ufw deny 5432` olduğu halda `-p 5432:5432` niyə **açıq qalır**?
8. **Problem həlli:** Port açıqdır, `nc` `timeout` verir. **Dörd yoxlama addımını** sıra ilə yaz.

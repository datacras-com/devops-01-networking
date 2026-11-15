# DevOps Networking Fundamentals

DataCras DevOps & Platform Engineering bootcamp-ının **Networking** modulu. Linux modulundan ayrıdır (`devops-01-linux`), onun davamı deyil. Şəbəkə mövzuları (DNS, CIDR, firewall, `tcpdump`, HTTP) Linux kursundan buraya köçürülüb. Məqsəd tələbənin **əsas şəbəkə anlayışlarını** sıfırdan öyrənməsidir.

## Dərslər

| # | Mövzu | Vəziyyət |
|---|---|---|
| 01 | Şəbəkə nədir? Cihazlar, LAN/WAN, paket, client-server, gecikmə, diaqnostika nərdivanı | ✅ hazırdır |
| 02 | Ünvanlar: MAC, IP (private/public), subnet mask, default gateway, ARP, DHCP | ✅ hazırdır |
| 03 | OSI və TCP/IP modeli: qatlar, encapsulation, "problem hansı qatdadır?" | ✅ hazırdır |
| 04 | Transport: TCP/UDP, port, handshake | ✅ hazırdır |
| 05 | NAT, firewall, load balancer | ✅ hazırdır |
| 06 | Subnet, CIDR və routing | ✅ hazırdır |
| 07 | DNS: adı IP-yə çevirmək | ✅ hazırdır |
| 08 | HTTP, HTTPS və sorğunun həyat dövrü | ✅ hazırdır |
| 09 | Linux firewall: `ufw`, `iptables`, `nftables` | ✅ hazırdır |
| 10 | `tcpdump` ilə trafik analizi | ✅ hazırdır |
| 11 | Diaqnostika: `ping` → `traceroute`/`mtr` → `ss` → `curl` → `dig` | ✅ hazırdır |
| 12 | VPN və tunnel | planlaşdırılıb |

Sessiya 06–10 əvvəl Linux kursunda (S35, S36, S37, S40, S41) idi. Networking modulunda daha izahlı yenidən yazılıb və Linux kursundan çıxarılıb.

## Slaydları yenidən yığmaq

Slaydlar Marp ilə `src/*.md`-dən `.html`-ə çevrilir:

```bash
marp --no-stdin --html src/session-01-what-is-network.md -o session-01-what-is-network.html
```

`--no-stdin` skript və CI-da Marp-ın giriş gözləyib asılmaması üçündür.

## Qeyd

Əmr çıxışları **nümunədir** (Ubuntu 24.04, `192.168.0.0/24` şəbəkəsi). Tələbənin IP və interfeys adları fərqli ola bilər. Dərsdən əvvəl çıxışları öz VM-də yoxlayın.

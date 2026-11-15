# DevOps Networking Fundamentals

DataCras DevOps & Platform Engineering bootcamp-ının **Networking** modulu. Linux modulundan ayrıdır (`devops-01-linux`), onun davamı deyil. Məqsəd tələbənin **əsas şəbəkə anlayışlarını** sıfırdan öyrənməsidir.

## Dərslər

| # | Mövzu | Vəziyyət |
|---|---|---|
| 01 | Şəbəkə nədir? Cihazlar, LAN/WAN, paket, client-server, gecikmə, diaqnostika nərdivanı | ✅ hazırdır |
| 02 | Ünvanlar: MAC, IP (private/public), subnet mask, default gateway, ARP, DHCP | planlaşdırılıb |
| 03 | OSI və TCP/IP modeli: qatlar, encapsulation, "problem hansı qatdadır?" | planlaşdırılıb |
| 04 | Transport: TCP/UDP, port, handshake | planlaşdırılıb |
| 05 | NAT, firewall, load balancer | planlaşdırılıb |
| 06 | Diaqnostika: `ping` → `traceroute`/`mtr` → `ss` → `curl` → `dig` → `tcpdump` | planlaşdırılıb |
| 07 | VPN və tunnel | planlaşdırılıb |

Linux kursunda artıq olan mövzulara (DNS S36, `tcpdump` S37, CIDR/routing S40, firewall S41) təkrar etmirik, onlara körpü qururuq.

## Slaydları yenidən yığmaq

Slaydlar Marp ilə `src/*.md`-dən `.html`-ə çevrilir:

```bash
marp --no-stdin --html src/session-01-what-is-network.md -o session-01-what-is-network.html
```

`--no-stdin` skript və CI-da Marp-ın giriş gözləyib asılmaması üçündür.

## Qeyd

Əmr çıxışları **nümunədir** (Ubuntu 24.04, VirtualBox NAT). Tələbənin IP və interfeys adları fərqli ola bilər. Dərsdən əvvəl çıxışları öz VM-də yoxlayın.

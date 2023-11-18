# Jarkom-Modul-3-B14-2023
praktikum jaringan komputer 2023

## Kontributor
| Nama | NRP |
|---------------------------|------------|
|Gabrielle Immanuel Osvaldo Kurniawan | 5025211135 |
|Muhammad Arkan Karindra Darvesh | 5025211236 |

## TOPOLOGI
<img width="733" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/0b826349-96ee-4ece-9957-6af27eea63c2">

berikut adalah konfigurasi dari masing masing node :


AURA (DHCP RELAY)
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp

# Static config for eth1
auto eth1
iface eth1 inet static
	address 192.185.1.1
	netmask 255.255.255.0

# Static config for eth2
auto eth2
iface eth2 inet static
	address 192.185.2.1
	netmask 255.255.255.0

# Static config for eth3
auto eth3
iface eth3 inet static
	address 192.185.3.1
	netmask 255.255.255.0

# Static config for eth4
auto eth4
iface eth4 inet static
	address 192.185.4.1
	netmask 255.255.255.0
```
Himmel
```
auto eth0
iface eth0 inet static
	address 192.185.1.2
	netmask 255.255.255.0
	gateway 192.185.1.1
```
Helter
```
auto eth0
iface eth0 inet static
	address 192.185.1.3
	netmask 255.255.255.0
	gateway 192.185.1.1
```
Denken
```
auto eth0
iface eth0 inet static
	address 192.185.2.2
	netmask 255.255.255.0
	gateway 192.185.2.1
```
Eisen
```
auto eth0
iface eth0 inet static
	address 192.185.2.3
	netmask 255.255.255.0
	gateway 192.185.2.1
```
Revolte
```
auto eth0
iface eth0 inet dhcp
```
Ritcher
```
auto eth0
iface eth0 inet dhcp
```
Lawine
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 3a:ff:68:ce:d5:cf
```
Linie
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 76:a0:85:61:16:3e
```
Lugner
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 1e:9d:8e:94:86:46;
```
Sein
```
auto eth0
iface eth0 inet dhcp
```
Stark
```
auto eth0
iface eth0 inet dhcp
```
Frieren
```
auto eth0
iface eth0 inet dhcp
hwaddress ether c2:2c:f2:13:c0:48
```
Flemme
```
auto eth0
iface eth0 inet dhcp
hwaddress ether c6:30:18:d2:fd:9a
```
Fern
```
auto eth0
iface eth0 inet dhcp
hwaddress ether a2:e5:59:6d:aa:80
```

## Soal 0
melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP mengarah pada worker yang memiliki IP 192.185.x.1.
### Jawab
Jalankan script berikut untuk setup DNS :
```
#!bin/bash

cp named.conf.options /etc/bind/named.conf.options

echo '
zone "granz.channel.b14.com" {
    type master;
    file "/etc/bind/granz/granz.channel.b14.com";
};' >> /etc/bind/named.conf.local

mkdir -p /etc/bind/granz

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     channel.b14.com. root.channel.b14.com. (
                        2023131101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      granz.channel.b14.com.
@               IN      A       192.185.3.1       ; IP Frieren
www             IN      CNAME   granz.channel.b14.com.
' > /etc/bind/granz/granz.channel.b14.com

echo '
zone "riegel.canyon.b14.com" {
    type master;
    file "/etc/bind/riegel/riegel.canyon.b14.com";
};' >> /etc/bind/named.conf.local

mkdir -p /etc/bind/riegel
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     canyon.b14.com. root.canyon.b14.com. (
                        2023131101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.b14.com.
@               IN      A       192.185.4.1       ; IP Lawine
www             IN      CNAME   riegel.canyon.b14.com.
' >  /etc/bind/riegel/riegel.canyon.b14.com

service bind9 restart
```
### Hasil
Ketika sudah berhasil untuk testing bisa menggunakan ```host -t A``` untuk domain riegel dan channel

<img width="201" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/0bd5632a-2105-470c-9ac0-d1193c4db86d">


## Soal 1
>Lakukan konfigurasi sesuai dengan peta yang sudah diberikan
### Jawab
Konfigurasi yang dimaksud adalah membuat topologi dengan config yang sudah ada di tiap node. Setelah topologi selesai dapat menjalankan file ```~/.bashrc``` sebagai berikut :

DHCP server
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
apt-get install dnsutils -y
```
DHCP relay
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.185.0.0/16
apt-get update
apt-get install isc-dhcp-server -y
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```
DNS server
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update && apt-get install bind9 -y
```
Database
```
echo nameserver 192.185.1.2 > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y
service mysql start
```
LoadBalancer
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start
```
Worker PHP
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install php7.3 -y
apt-get install php7.3-fpm -y
apt-get install libapache2-mod-php7.3 -y
apt-get install htop -y
service nginx start
systemctl enable php7.3-fpm
service php7.3-fpm start
php -v
```
Worker laravel
```
apt-get update
apt-get install htop -y
```
Client
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

## Soal 2
>Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
### Jawab
menambahkan range pada setup DHCP sebagai berikut
```
echo 'subnet 192.185.1.0 netmask 255.255.255.0 {
}

subnet 192.185.2.0 netmask 255.255.255.0 {
}

subnet 192.185.3.0 netmask 255.255.255.0 {
    range 192.185.3.16 192.185.3.32;
    range 192.185.3.64 192.185.3.80;
    option routers 192.185.3.0;
}' > /etc/dhcp/dhcpd.conf
```

## Soal 3
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
### Jawab
Lakukan hal yang sama pada subnet ```192.185.3.0``` ke subnet ```192.185.4.0``` sebagai berikut 
```
echo 'subnet 192.185.1.0 netmask 255.255.255.0 {
}

subnet 192.185.2.0 netmask 255.255.255.0 {
}

subnet 192.185.3.0 netmask 255.255.255.0 {
    range 192.185.3.16 192.185.3.32;
    range 192.185.3.64 192.185.3.80;
    option routers 192.185.3.0;
}

subnet 192.185.4.0 netmask 255.255.255.0 {
    range 192.185.4.12 192.185.4.20;
    range 192.185.4.160 192.185.4.168;
    option routers 192.185.4.0;
} ' > /etc/dhcp/dhcpd.conf
```

## Soal 4
> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
### Jawab
Dapat dilakukan dengan ping ke google pada client. sebelumnya perlu jalankan relay dengan script sebagai berikut :
```
cp sysctl.conf /etc/sysctl.conf
cp isc-dhcp-relay /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```
Berikut adalah konten file sysctl.conf dan isc-dhcp-relay

isc-dhcp-relay
```
cp sysctl.conf /etc/sysctl.conf
cp isc-dhcp-relay /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
root@Aura:~# cat sysctl.conf 
#
# /etc/sysctl.conf - Configuration file for setting system variables
# See /etc/sysctl.d/ for additional system variables.
# See sysctl.conf (5) for information.
#

#kernel.domainname = example.com

# Uncomment the following to stop low-level messages on console
#kernel.printk = 3 4 1 3

##############################################################3
# Functions previously found in netbase
#

# Uncomment the next two lines to enable Spoof protection (reverse-path filter)
# Turn on Source Address Verification in all interfaces to
# prevent some spoofing attacks
#net.ipv4.conf.default.rp_filter=1
#net.ipv4.conf.all.rp_filter=1

# Uncomment the next line to enable TCP/IP SYN cookies
# See http://lwn.net/Articles/277146/
# Note: This may impact IPv6 TCP sessions too
#net.ipv4.tcp_syncookies=1

# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1

# Uncomment the next line to enable packet forwarding for IPv6
#  Enabling this option disables Stateless Address Autoconfiguration
#  based on Router Advertisements for this host
#net.ipv6.conf.all.forwarding=1


###################################################################
# Additional settings - these settings can improve the network
# security of the host and prevent against some network attacks
# including spoofing attacks and man in the middle attacks through
# redirection. Some network environments, however, require that these
# settings are disabled so review and enable them as needed.
#
# Do not accept ICMP redirects (prevent MITM attacks)
#net.ipv4.conf.all.accept_redirects = 0
#net.ipv6.conf.all.accept_redirects = 0
# _or_
# Accept ICMP redirects only for gateways listed in our default
# gateway list (enabled by default)
# net.ipv4.conf.all.secure_redirects = 1
#
# Do not send ICMP redirects (we are not a router)
#net.ipv4.conf.all.send_redirects = 0
#
# Do not accept IP source route packets (we are not a router)
#net.ipv4.conf.all.accept_source_route = 0
#net.ipv6.conf.all.accept_source_route = 0
#
# Log Martian Packets
#net.ipv4.conf.all.log_martians = 1
#

###################################################################
# Magic system request Key
# 0=disable, 1=enable all, >1 bitmask of sysrq functions
# See https://www.kernel.org/doc/html/latest/admin-guide/sysrq.html
# for what other values do
#kernel.sysrq=438

root@Aura:~# cat isc-dhcp-relay 
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.10.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
sysctl.conf
```
#
# /etc/sysctl.conf - Configuration file for setting system variables
# See /etc/sysctl.d/ for additional system variables.
# See sysctl.conf (5) for information.
#

#kernel.domainname = example.com

# Uncomment the following to stop low-level messages on console
#kernel.printk = 3 4 1 3

##############################################################3
# Functions previously found in netbase
#

# Uncomment the next two lines to enable Spoof protection (reverse-path filter)
# Turn on Source Address Verification in all interfaces to
# prevent some spoofing attacks
#net.ipv4.conf.default.rp_filter=1
#net.ipv4.conf.all.rp_filter=1

# Uncomment the next line to enable TCP/IP SYN cookies
# See http://lwn.net/Articles/277146/
# Note: This may impact IPv6 TCP sessions too
#net.ipv4.tcp_syncookies=1

# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1

# Uncomment the next line to enable packet forwarding for IPv6
#  Enabling this option disables Stateless Address Autoconfiguration
#  based on Router Advertisements for this host
#net.ipv6.conf.all.forwarding=1


###################################################################
# Additional settings - these settings can improve the network
# security of the host and prevent against some network attacks
# including spoofing attacks and man in the middle attacks through
# redirection. Some network environments, however, require that these
# settings are disabled so review and enable them as needed.
#
# Do not accept ICMP redirects (prevent MITM attacks)
#net.ipv4.conf.all.accept_redirects = 0
#net.ipv6.conf.all.accept_redirects = 0
# _or_
# Accept ICMP redirects only for gateways listed in our default
# gateway list (enabled by default)
# net.ipv4.conf.all.secure_redirects = 1
#
# Do not send ICMP redirects (we are not a router)
#net.ipv4.conf.all.send_redirects = 0
#
# Do not accept IP source route packets (we are not a router)
#net.ipv4.conf.all.accept_source_route = 0
#net.ipv6.conf.all.accept_source_route = 0
#
# Log Martian Packets
#net.ipv4.conf.all.log_martians = 1
#

###################################################################
# Magic system request Key
# 0=disable, 1=enable all, >1 bitmask of sysrq functions
# See https://www.kernel.org/doc/html/latest/admin-guide/sysrq.html
# for what other values do
#kernel.sysrq=438
```

## Soal 5
> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
### Jawab
Jalankan script berikut pada server DHCP
```
cp dhcpd.conf /etc/dhcp/dhcpd.conf
cp isc-dhcp-server /etc/default/isc-dhcp-server

service isc-dhcp-server restart
service isc-dhcp-server status
```
Berikut adalah konten file dhcpd.conf dan isc-dhcp-server

isc-dhcp-server
```
subnet 192.185.1.0 netmask 255.255.255.0 {

}

subnet 192.185.2.0 netmask 255.255.255.0 {

}

# eth3
subnet 192.185.3.0 netmask 255.255.255.0 {
    range 192.185.3.16 192.185.3.32;
    range 192.185.3.64 192.185.3.80;
    option routers 192.185.3.35;
    option broadcast-address 192.185.3.255;
    option domain-name-servers 192.185.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

# eth4
subnet 192.185.4.0 netmask 255.255.255.0 {
    range 192.185.4.12 192.185.4.20;
    range 192.185.4.160 192.185.4.168;
    option routers 192.185.4.35;
    option broadcast-address 192.185.4.255;
    option domain-name-servers 192.185.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}

# Switch 3
host Lawine {
    hardware ethernet 3a:ff:68:ce:d5:cf;
    fixed-address 192.185.3.1;
}

host Linie {
    hardware ethernet 76:a0:85:61:16:3e;
    fixed-address 192.185.3.2;
}

host Lugner {
    hardware ethernet 1e:9d:8e:94:86:46;
    fixed-address 192.185.3.3;
}

# Switch 4
host Frieren {
    hardware ethernet be:6a:9c:c4:ae:30;
    fixed-address 192.185.4.1;
}

host Flamme {
    hardware ethernet c6:30:18:d2:fd:9a;
    fixed-address 192.185.4.2;
}

host Fern {
    hardware ethernet a2:e5:59:6d:aa:80;
    fixed-address 192.185.4.3;
}
```
dhcpd.conf
```
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="eth0"
INTERFACESv6=""
```

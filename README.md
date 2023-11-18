# Jarkom-Modul-3-B14-2023
praktikum jaringan komputer 2023

## Kontributor
| Nama | NRP |
|---------------------------|------------|
|Gabrielle Immanuel Osvaldo Kurniawan | 5025211135 |
|Muhammad Arkan Karindra Darvesh | 5025211236 |

## Kendala
- dalam pengerjaan seringkali terjadi error yang tidak ada keterangan sehingga debug memakan waktu yang sangat lama

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
service isc-dhcp-relay restart
```
Berikut adalah konten file sysctl.conf dan isc-dhcp-relay

isc-dhcp-relay

setup secara manual dengan input :
1. 192.185.1.1
2. eth1 eth2 eth3 eth4
3. [enter]

Berikut adalah konten file sysctl.conf
```
# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1
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
INTERFACESv4="eth0"
```

### Hasil
Berikut tampilan jika leash berhasil

<img width="338" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/ca99728a-13ec-4c4f-b10d-f0fecb19061b">

## Soal 6
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.
### Jawaban
Jalankan pada 3 worker di linie,lugner dan lawine
```
service nginx start

mkdir -p /var/www/granz.channel.b14.com

cp granz.channel.b14.com /etc/nginx/sites-available/granz.channel.b14.com
rm /etc/nginx/sites-enabled/granz.channel.b14.com
ln -s /etc/nginx/sites-available/granz.channel.b14.com /etc/nginx/sites-enabled

apt-get update
apt-get install git -y
git -c http.sslVerify=false clone https://github.com/Osvaldo-Kurniawan/granz.channel.b14.com.git /var/www/granz.channel.b14.com

rm -rf /etc/nginx/sites-enabled/default

service php7.3-fpm start
service nginx restart
```
### Hasil
Lakukan uji pada client dengan ```lynx 192.185.3.x```. Berikut tampilan pada setup yang sudah berhasil

<img width="676" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/1bccabdf-e693-4bec-8a5a-e4575fb0cfbb">

## Soal 7
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

### Jawaban
Pada heiter arahkan dns mengarah ke Load balancer dengan script berikut :
```
#!bin/bash

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.b14.com. root.granz.channel.b14.com. (
                        2023131101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      granz.channel.b14.com.
@               IN      A       192.185.2.2       ; IP Eisen
www             IN      CNAME   granz.channel.b14.com.
' > /etc/bind/granz/granz.channel.b14.com

service bind9 restart
```

pada eisen setup loadbalancer round robin dengan script berikut :
```
service nginx start

cp lb-roundrobin /etc/nginx/sites-available/lb-granz

rm -f /etc/nginx/sites-enabled/lb-granz
ln -s /etc/nginx/sites-available/lb-granz /etc/nginx/sites-enabled/

service nginx restart
```
adapun konten file lb-roundrobin sebagai berikut
```
upstream worker_round_robin {
  server 192.185.3.1;
  server 192.185.3.2;
  server 192.185.3.3;
}

server {
    listen 80;
    server_name granz.channel.b14.com www.granz.channel.b14.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker_round_robin;
    }
    
}
```
### Hasil
Untuk testing lakukan pada revolt (client) dengan mengetikkan ```ab -n 1000 -c 100 http://www.granz.channel.b14.com/```, maka hasil yang diperoleh adalah sebagai berikut :
```
root@Revolte:~# ab -n 1000 -c 100 http://www.granz.channel.b14.com/
This is ApacheBench, Version 2.3 <$Revision: 1843412 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.granz.channel.b14.com (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx/1.14.2
Server Hostname:        www.granz.channel.b14.com
Server Port:            80

Document Path:          /
Document Length:        627 bytes

Concurrency Level:      100
Time taken for tests:   1.925 seconds
Complete requests:      1000
Failed requests:        333
   (Connect: 0, Receive: 0, Length: 333, Exceptions: 0)
Total transferred:      763667 bytes
HTML transferred:       626667 bytes
Requests per second:    519.59 [#/sec] (mean)
Time per request:       192.461 [ms] (mean)
Time per request:       1.925 [ms] (mean, across all concurrent requests)
Transfer rate:          387.49 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0   17   6.1     17      31
Processing:     1   88 134.3     56    1031
Waiting:        0   88 134.0     56    1031
Total:          1  105 134.8     76    1057

Percentage of the requests served within a certain time (ms)
  50%     76
  66%     84
  75%     91
  80%     95
  90%    147
  95%    284
  98%    377
  99%   1040
 100%   1057 (longest request)
```

## Soal 8
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis

### Jawaban
lakukan percobaan menggunakan script load balancer yang disediakan dengan menambahkan pada bagian upworker. Kemudian saat melakukan running di client dengan command ```ab -n 200 -c 10 http://www.granz.channel.b14.com/``` jalankan htop pada masing masing worker. Hasil dapat dilihat pada grimoire di link dibawah :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## Soal 9
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.
### jawaban
lakukan percobaan menggunakan mengcomment node worker di load balancer yang disediakan pada bagian upworker. Kemudian saat melakukan running di client dengan command ```ab -n 100 -c 10 http://www.granz.channel.b14.com/``` jalankan htop pada masing masing worker. Hasil dapat dilihat pada grimoire di link dibawah :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## Soal 10
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/
### Jawaban
jalankan 
```
mkdir /etc/nginx/rahasisakita
htpasswd -c /etc/nginx/rahasisakita/htpasswd netics
```
masukin password ajkb14, kemudian jalankan script :
```
service nginx start

cp lb-roundrobin-protected /etc/nginx/sites-available/lb-granz

rm -f /etc/nginx/sites-enabled/lb-granz
ln -s /etc/nginx/sites-available/lb-granz /etc/nginx/sites-enabled/

service nginx restart
```
Isi dari lb-roundrobin-protected adalah sebagai berikut :
```
upstream worker_round_robin {
  server 192.185.3.1;
  server 192.185.3.2;
  server 192.185.3.3;
}

server {
    listen 80;
    server_name granz.channel.b14.com www.granz.channel.b14.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker_round_robin;
    }
    
    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
}
```
### Hasil
<img width="401" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/ee82a817-aefc-4885-bc48-50d1bbc5dcd0">


Ketika mencoba mengakses ```lynx granz.channel.b14.com``` dari revolt akan tampil sebagai berikut :

![Screenshot (1052)](https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/e461104d-6c2a-46c4-9f4f-8d109439e35d)

![image](https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/6833414d-77d4-45e2-93ce-e8038d4ccc82)


## Soal 11
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. hint: (proxy_pass)
### jawaban
selesaikan menggunakan proxy pass sebagai berikut :
```
service nginx start

cp lb-its /etc/nginx/sites-available/lb-granz

rm -f /etc/nginx/sites-enabled/lb-granz
ln -s /etc/nginx/sites-available/lb-granz /etc/nginx/sites-enabled/

service nginx restart
```
Adapun isi dari file lb-its adalah :
```
upstream worker-its {
    server 192.185.3.1;
    server 192.185.3.2;
    server 192.185.3.3;
}

server {
    listen 80;
    server_name granz.channel.b14.com www.granz.channel.b14.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker-its;
    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
}
```
### Hasil
Ketika mencoba command ```lynx granz.channel.b14.com/its``` akan diarahkan ke halaman berikut :

<img width="538" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/933b1b0c-ffd1-4896-8359-e3038c118c93">

## Soal 12
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. hint: (fixed in dulu clinetnya)
### Jawaban
Ubah setting dhcp.conf pada server, tampahkan fixed addres untuk client. tambahkan configurasi berikut di dhcp.conf himmel :
```
# RICHTER FIXED ADDRESS
host Richter {
    hardware ethernet 96:05:97:2a:29:65;
    fixed-address 192.185.3.69;
}
```
Kemudian pada eisen tambahkan ip yang diizinkan mengakses sebagai berikut :
```
upstream worker_round_robin {
  server 192.185.3.1;
  server 192.185.3.2;
  server 192.185.3.3;
}

server {
    listen 80;
    server_name granz.channel.b14.com www.granz.channel.b14.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker_round_robin;
        allow 192.185.3.69;
        allow 192.185.3.70;
        allow 192.185.4.167;
        allow 192.185.4.168;
        deny all;
    }
    
}
```
### hasil
Coba ```lynx``` pada node yang sudah diizinkan yaitu richter kemudian coba juga pada revolt yang belum diizinkan :
- Node Revolt
<img width="498" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/2a7b5fe6-afb1-4e04-92d4-001fb667588b">

- Node Richter
<img width="500" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/df5c286f-9846-433f-9589-eae5983298d9">

## No 13
> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.
### jawaban
jalankan script berikut pada node denken
```
cp 50-server.cnf /etc/mysql/mariadb.conf.d/50-server.cnf
cp my.cnf /etc/mysql/my.cnf

service mysql restart

#!/bin/bash

# Prompt for the MySQL root password
read -sp 'Enter MySQL root password: ' MYSQL_ROOT_PASSWORD
echo

# Execute MySQL commands
mysql -u root -p"$MYSQL_ROOT_PASSWORD" <<EOF
DROP USER IF EXISTS 'kelompokb14'@'%';
DROP USER IF EXISTS 'kelompokb14'@'localhost';
CREATE USER 'kelompokb14'@'%' IDENTIFIED BY 'passwordb14';
CREATE USER 'kelompokb14'@'localhost' IDENTIFIED BY 'passwordb14';
DROP DATABASE IF EXISTS dbkelompokb14;
CREATE DATABASE dbkelompokb14;
GRANT ALL PRIVILEGES ON dbkelompokb14.* TO 'kelompokb14'@'%';
GRANT ALL PRIVILEGES ON dbkelompokb14.* TO 'kelompokb14'@'localhost';
FLUSH PRIVILEGES;
EOF

echo "dbkelompokb14 created"
```
Adapun konten file 
- 50-server.cnf
```
#
# These groups are read by MariaDB server.
# Use it for options that only the server (but not clients) should see
#
# See the examples of server my.cnf files in /usr/share/mysql

# this is read by the standalone daemon and embedded servers
[server]

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Basic Settings
#
user                    = mysql
pid-file                = /run/mysqld/mysqld.pid
socket                  = /run/mysqld/mysqld.sock
#port                   = 3306
basedir                 = /usr
datadir                 = /var/lib/mysql
tmpdir                  = /tmp
lc-messages-dir         = /usr/share/mysql
#skip-external-locking

# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0

#
# * Fine Tuning
#
#key_buffer_size        = 16M
#max_allowed_packet     = 16M
#thread_stack           = 192K
#thread_cache_size      = 8
# This replaces the startup script and checks MyISAM tables if needed
# the first time they are touched
#myisam_recover_options = BACKUP
#max_connections        = 100
#table_cache            = 64
#thread_concurrency     = 10

#
# * Query Cache Configuration
#
#query_cache_limit      = 1M
query_cache_size        = 16M

#
# * Logging and Replication
#
# Both location gets rotated by the cronjob.
# Be aware that this log type is a performance killer.
# As of 5.1 you can enable the log at runtime!
#general_log_file       = /var/log/mysql/mysql.log
#general_log            = 1
#
# Error log - should be very few entries.
#
log_error = /var/log/mysql/error.log
#
# Enable the slow query log to see queries with especially long duration
#slow_query_log_file    = /var/log/mysql/mariadb-slow.log
#long_query_time        = 10
#log_slow_rate_limit    = 1000
#log_slow_verbosity     = query_plan
#log-queries-not-using-indexes
#
# The following can be used as easy to replay backup logs or for replication.
# note: if you are setting up a replication slave, see README.Debian about
#       other settings you may need to change.
#server-id              = 1
#log_bin                = /var/log/mysql/mysql-bin.log
expire_logs_days        = 10
#max_binlog_size        = 100M
#binlog_do_db           = include_database_name
#binlog_ignore_db       = exclude_database_name

#
# * Security Features
#
# Read the manual, too, if you want chroot!
#chroot = /var/lib/mysql/
#
# For generating SSL certificates you can use for example the GUI tool "tinyca".
#
#ssl-ca = /etc/mysql/cacert.pem
#ssl-cert = /etc/mysql/server-cert.pem
#ssl-key = /etc/mysql/server-key.pem
#
# Accept only connections using the latest and most secure TLS protocol version.
# ..when MariaDB is compiled with OpenSSL:
#ssl-cipher = TLSv1.2
# ..when MariaDB is compiled with YaSSL (default in Debian):
#ssl = on

#
# * Character sets
#
# MySQL/MariaDB default is Latin1, but in Debian we rather default to the full
# utf8 4-byte character set. See also client.cnf
#
character-set-server  = utf8mb4
collation-server      = utf8mb4_general_ci

#
# * InnoDB
#
# InnoDB is enabled by default with a 10MB datafile in /var/lib/mysql/.
# Read the manual for more InnoDB related options. There are many!

#
# * Unix socket authentication plugin is built-in since 10.0.22-6
#
# Needed so the root database user can authenticate without a password but
# only when running as the unix root user.
#
# Also available for other users if required.
# See https://mariadb.com/kb/en/unix_socket-authentication-plugin/

# this is only for embedded server
[embedded]

# This group is only read by MariaDB servers, not by MySQL.
# If you use the same .cnf file for MySQL and MariaDB,
# you can put MariaDB-only options here
[mariadb]

# This group is only read by MariaDB-10.3 servers.
# If you use the same .cnf file for MariaDB of different versions,
# use this group for options that older servers don't understand
[mariadb-10.3]
```
- my.cnf
```
[client-server]

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address
```
saat dijalankan masukan password ajkb14. Pada worker laravel lakukan konfigurasi sebagai berikut :
```
apt-get update

apt-get install mariadb-client -y

apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2

curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg

sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update

apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

apt-get install git -y

apt-get install lynx
```
### hasil
untuk melakukan cek jalankan command berikut di worker laravel: ```mariadb --host=192.185.2.1 --port=3306 --user=kelompokb14 --password=passwordb14 dbkelompokb14 -e "SHOW DATABASES;"```

<img width="500" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/e9e7fbba-95e2-41ff-8246-7ab04b00972d">

## No 14
> Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer
### Jawaban
Jalankan konfigurasi menggunakan script berikut :
```
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update

cp .env.example .env
rm /var/www/laravel-praktikum-jarkom/public/storage

echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.185.2.1
DB_PORT=3306
DB_DATABASE=dbkelompokb14
DB_USERNAME=kelompokb14
DB_PASSWORD=passwordb14

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage

echo 'server {
        listen 8002 default_server;

        root /var/www/laravel-praktikum-jarkom/public;

        index index.php;

        server_name _;

        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }

        error_log /var/log/nginx/riegel_error.log;
        access_log /var/log/nginx/riegel_access.log;

}' > /etc/nginx/sites-available/riegel

rm /etc/nginx/sites-enabled/riegel
ln -s /etc/nginx/sites-available/riegel /etc/nginx/sites-enabled/riegel

unlink /etc/nginx/sites-enabled/default
service php8.0-fpm start
service php8.0-fpm restart
service nginx restart
```
kemudian pada loadbalancer juga jalankan :
```
service nginx start

cp lb-laravel /etc/nginx/sites-available/lb-laravel

rm -f /etc/nginx/sites-enabled/lb-laravel
ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled/

service nginx restart
```
isi dari lb-laravel adalah 
```
upstream laravel {
        server 192.185.4.1:8001;
        server 192.185.4.2:8002;
        server 192.185.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.b14.com www.riegel.canyon.b14.com;

        location / {
                proxy_pass http://laravel;
        }

}
```
### Hasil
Untuk testing pada worker laravel jalankan ```lynx localhost:8002```. Dan pada Sein juga dapat dijalankan ```lynx http://192.185.4.2:8002```

<img width="493" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/ef41d1d4-b9f1-43b5-be89-b018bf99a13d">


<img width="497" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/926dcf42-d4bc-4edc-adfe-0d49854c7da5">

## Soal 15
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register
### jawaban
jalankan script untuk membuat file register.json
```
echo '
{
  "username": "kelompokb14",
  "password": "passwordb14",
}' > register.json
```
### hasil 
Jalankan ```ab -n 100 -c 10 -p register.json -T application/json http://192.185.4.2:8002/api/auth/register```. Hasil dapat dilihat pada grimoire di link :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## Soal 16
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/login
### jawaban
jalankan script untuk membuat file login.json
```
echo '
{
  "username": "kelompokb14",
  "password": "passwordb14",
}' > login.json
```
### hasil
Jalankan ```ab -n 100 -c 10 -p login.json -T application/json http://192.185.4.2:8002/api/auth/login``` pada sein. Hasil dapat dilihat pada grimoire di link :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## Soal 17
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
GET /me
### jawaban
Pada sein jalankan script :
```
curl -X POST -H "Content-Type: application/json" -d @login.json http://192.185.4.2:8002/api/auth/login > token.txt
token=$(cat token.txt | jq -r '.token')
```
### Hasil
Untuk testing jalankan command ```ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.185.4.2:8002/api/me```.Hasil dapat dilihat pada grimoire di link :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## Soal 18
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.
### jawaban
Pada heiter pastikan sudah mengarah ke loadbalancer sebagai berikut 
```
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
@               IN      A       192.185.2.2       ; IP Eisen Load Balancer
www             IN      CNAME   riegel.canyon.b14.com.
' >  /etc/bind/riegel/riegel.canyon.b14.com

service bind9 restart
```
### Hasil
Jalankan ```ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.b14.com/api/auth/login``` pada sein

<img width="444" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/222d635e-6128-4336-af9f-7ddc221989af">

## No 19
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.
### Jawaban
Dicoba menggunakan 3 konfigurasi berikut :
- Konfigurasi 1
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 10
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
pm.process_idle_timeout = 5s
```
- Konfigurasi 2
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 30
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10
pm.process_idle_timeout = 8s
```
- Konfigurasi 3
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s
```
### Hasil
Lakukan testing dengan ```ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.b14.com/api/auth/login```. Hasil dapat dilihat pada grimoire di link :
```
https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing
```

## No 20

### jawaban
Jalankan pada loadbalancer untuk menerapkan least connection laravel dengan script :
```
service nginx start

cp lb-laravel-leastconnection /etc/nginx/sites-available/lb-laravel

rm -f /etc/nginx/sites-enabled/lb-laravel
ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled/

service nginx restart
```
Adapun konten dari lb-laravel-leastconnection
```
upstream laravel {
        least_conn;
        server 192.185.4.1:8001;
        server 192.185.4.2:8002;
        server 192.185.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.b14.com www.riegel.canyon.b14.com;

        location / {
                proxy_pass http://laravel;
        }

}
```
### hasil
jalankan ```ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.b14.com/api/auth/login``` pada node sein sehingga diperoleh hasil :

<img width="459" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/17ccce29-4d4b-484a-9555-5626eabfc0f5">


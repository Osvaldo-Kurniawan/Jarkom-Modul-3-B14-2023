# Jarkom-Modul-3-B14-2023
praktikum jaringan komputer 2023

## Kontributor
| Nama | NRP |
|---------------------------|------------|
|Gabrielle Immanuel Osvaldo Kurniawan | 5025211135 |
|Muhammad Arkan Karindra Darvesh | 5025211236 |

## Topologi
<img width="562" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/724a74a9-6723-4bb2-8e04-7cb6cdf2d5b0">

Konfigurasi topologi :
1. **Aura (DHCP Relay)**
   ```
    auto eth0
    iface eth0 inet dhcp
    
    auto eth1
    iface eth1 inet static
      address 192.185.1.4
      netmask 255.255.255.0
    
    auto eth2
    iface eth2 inet static
      address 192.185.2.4
      netmask 255.255.255.0
    
    auto eth3
    iface eth3 inet static
      address 192.185.3.4
      netmask 255.255.255.0
    
    auto eth4
    iface eth4 inet static
      address 192.185.4.4
      netmask 255.255.255.0
   ```
3. **Himmel (DHCP Server)**
   ```
   auto eth0
   iface eth0 inet static
      address 192.185.1.1
      netmask 255.255.255.0
      gateway 192.185.1.4
   ```
5. **Heiter (DNS Server)**
   ```
   auto eth0
   iface eth0 inet static
      address 192.185.1.2
      netmask 255.255.255.0
      gateway 192.185.1.4
   ```
7. **Denken (Database Server)**
   ```
   auto eth0
   iface eth0 inet static
      address 192.185.2.1
      netmask 255.255.255.0
      gateway 192.185.2.4
   ```
9. **Eisen (Load Balancer)**
   ```
   auto eth0
   iface eth0 inet static
      address 192.185.2.2
      netmask 255.255.255.0
      gateway 192.185.2.4
   ```
11. **Frieren (Laravel Worker)**
   ```
   auto eth0
   iface eth0 inet static
      address 192.185.3.1
      netmask 255.255.255.0
      gateway 192.185.3.4
   ```
13. **Flamme (Laravel Worker)**
   ```
    auto eth0
    iface eth0 inet static
      address 192.185.3.2
      netmask 255.255.255.0
      gateway 192.185.3.4
   ```
15. **Fern (Laravel Worker)**
   ```
    auto eth0
    iface eth0 inet static
      address 192.185.3.3
      netmask 255.255.255.0
      gateway 192.185.3.4
   ```
17. **Lawine (PHP Worker)**
   ```
    auto eth0
    iface eth0 inet static
      address 192.185.4.1
      netmask 255.255.255.0
      gateway 192.185.4.4
   ```
19. **Linie (PHP Worker)**
   ```
    auto eth0
    iface eth0 inet static
      address 192.185.4.2
      netmask 255.255.255.0
      gateway 192.185.4.4
   ```
21. **Lugner (PHP Worker)**
   ```
    auto eth0
    iface eth0 inet static
      address 192.185.4.1
      netmask 255.255.255.0
      gateway 192.185.4.4
   ```
23. **Revolte, Richter, Sein, dan Stark (Client)**
   ```
   auto eth0
   iface eth0 inet dhcp
   ```

### Instalasi awal tiap node
setiap node, kita inisiasi pada `.bashrc` menggunakan `nano`

- DNS Server
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y
  cd ~
  ```
- DHCP Server
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install isc-dhcp-server -y
  cd ~
  ```
- DHCP Relay
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.185.0.0/16
  apt-get update
  apt-get install isc-dhcp-relay -y
  service isc-dhcp-relay start
  cd ~
  ```
- Database Server
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo 'nameserver 192.185.1.2
  nameserver 192.168.122.1
  ' > /etc/resolv.conf
  apt-get update
  apt-get install mariadb-server -y
  service mysql start
  cd ~
  ```
- Load Balancer
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo 'nameserver 192.185.1.2
  nameserver 192.168.122.1
  ' > /etc/resolv.conf
  apt-get update
  apt-get install apache2-utils -y
  apt-get install nginx -y
  apt-get install lynx -y
  touch /etc/nginx/sites-available/lb-php
  
  echo 'upstream worker {
          server 192.185.3.1;
          server 192.185.3.2;
          server 192.185.3.3;
  }
  
  server {
          listen 80;
          server_name granz.channel.b14.com;
  
          location / {
                  proxy_pass http://worker;
          }
  }' > /etc/nginx/sites-available/lb-php
  
  ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
  rm -rf /etc/nginx/sites-enabled/default
  
  service nginx restart
  nginx -t
  cd ~
  ```
- Worker PHP
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo 'nameserver 192.185.1.2
  nameserver 192.168.122.1
  ' > /etc/resolv.conf
  
  apt-get update && apt install wget unzip nginx php7.3 php7.3-fpm -y
  
  mkdir /var/www/granz
  wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz/file.zip
  
  unzip /var/www/granz/file.zip -d /var/www/granz
  
  mv /var/www/granz/modul-3/* /var/www/granz
  rm -rf /var/www/granz/file.zip
  
  touch /etc/nginx/sites-available/granz.channel.b14
  
  echo ' server {
  
          listen 80;
  
          root /var/www/granz;
  
          index index.php index.html index.htm;
          server_name _;
  
          location / {
                          try_files $uri $uri/ /index.php?$query_string;
          }
  
          # pass PHP scripts to FastCGI server
          location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
          }
  
   location ~ /\.ht {
                          deny all;
          }
  
          error_log /var/log/nginx/jarkom_error.log;
          access_log /var/log/nginx/jarkom_access.log;
   }' > /etc/nginx/sites-available/granz.channel.b14
  
  ln -s /etc/nginx/sites-available/granz.channel.b14 /etc/nginx/sites-enabled
  rm -rf /etc/nginx/sites-enabled/default
  
  service php7.3-fpm start
  service php7.3-fpm restart
  service nginx restart
  nginx -t
  cd ~
  ```
- Worker Laravel
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo 'nameserver 192.185.1.2
  nameserver 192.168.122.1
  ' > /etc/resolv.conf
  apt-get update
  apt-get install lynx -y
  apt-get install mariadb-client -y
  # Test connection from worker to database
  # mariadb --host=192.185.2.1 --port=3306   --user=kelompokb14 --password=passwordb14 dbkelompokb14 -e "SHOW DATABASES;"
  apt-get install -y lsb-release ca-certificates a   apt-transport-https software-properties-common gnupg2
  curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
  sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
  apt-get update
  apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
  apt-get install nginx -y
  
  service nginx start
  service php8.0-fpm start
  cd ~
  ```
- Client
  ```
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022
  
  # You may uncomment the following lines if you want `ls' to be colorized:
  # export LS_OPTIONS='--color=auto'
  # eval "`dircolors`"
  # alias ls='ls $LS_OPTIONS'
  # alias ll='ls $LS_OPTIONS -l'
  # alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  # alias rm='rm -i'
  # alias cp='cp -i'
  # alias mv='mv -i'
  echo nameserver 192.185.1.2 > /etc/resolv.conf
  apt update
  apt install lynx -y
  apt install htop -y
  apt install apache2-utils -y
  apt-get install jq -y
  cd ~
  ```

## No 1
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan
Konfigurasikan topologi dan keseluruhan instalasi pada tiap node kemudian jangan lupa untuk konfigurasi DNS server sebagai berikut :
```
echo 'zone "riegel.canyon.b14.com" {
        type master;
        file "/etc/bind/jarkom/riegel.canyon.b14.com";
};

zone "granz.channel.b14.com" {
        type master;
        file "/etc/bind/jarkom/granz.channel.b14.com";
};' > /etc/bind/named.conf.local

echo '
options {
        directory "/var/cache/bind";
        forwarders {
                192.168.122.1;
        };
        allow-query{any;};
        auth-nxdomain no;       # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

mkdir -p /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.b14.com
cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.b14.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.b14.com. root.riegel.canyon.b14.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.b14.com.
@       IN      A       192.185.3.1     ; IP Freiren
www     IN      CNAME   riegel.canyon.b14.com.' > /etc/bind/jarkom/riegel.canyon.b14.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.b14.com. root.granz.channel.b14.com. (
                        2023111402      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.b14.com.
@       IN      A       192.185.4.1     ; IP Lawine
www     IN      CNAME   granz.channel.b14.com.' > /etc/bind/jarkom/granz.channel.b14.com

service bind9 restart
```

Testing dengan ping pada node static yang lain 


<img width="425" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/9034d0a9-4da4-4b85-bcab-802c144fc091">


<img width="369" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/3ffb473a-9051-46b4-8bff-4a56fcc95037">

## No 2 - 5
Kemudian, karena masih banyak spell yang harus dikumpulkan, bantulah para petualang untuk memenuhi kriteria berikut.:
- Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
- Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80(2)
- Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)
- Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)
- Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)

Jalankan konfigurasi dhcp server sebagai berikut
```
echo ' INTERFACES="eth0"
' > /etc/default/isc-dhcp-server

echo 'default-lease-time 600;
max-lease-time 7200;

authoritative;

#eth1
subnet 192.185.1.0 netmask 255.255.255.0 {
}

#eth2
subnet 192.185.2.0 netmask 255.255.255.0 {
}

#eth3
subnet 192.185.3.0 netmask 255.255.255.0 {
    range 192.185.3.16 192.185.3.32;
    range 192.185.3.64 192.185.3.80;
    option routers 192.185.3.4;
    option broadcast-address 192.185.3.255;
    option domain-name-servers 192.185.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

#eth4
subnet 192.185.4.0 netmask 255.255.255.0 {
    range 192.185.4.12 192.185.4.20;
    range 192.185.4.160 192.185.4.168;
    option routers 192.185.4.4;
    option broadcast-address 192.185.4.255;
    option domain-name-servers 192.185.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
service isc-dhcp-server status
```
Jika sukses outputnya akan seperti berikut :
<img width="286" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/3f364903-f786-4a9e-b05c-06bb5eaab57b">


Kemudian juga jalankan file config berikut pada aura
```
echo 'SERVERS="192.185.1.1"
INTERFACES="eth3 eth4"
OPTIONS="" ' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart
```
Jika sukses outputnya akan seperti berikut :
<img width="188" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/eb508c8b-8c4a-43c4-b2ca-e2297afdeaa4">

Testing dengan ip a di node client untuk melihat apakah sudah mendapatkan ip dan ping google
<img width="450" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/6c65067f-84f5-4070-b18b-3fb3cda674d2">

## No 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.
Webserver yang digunakan adalah nginx dan sudah terkonfigurasi saat instalasi pada bagian :
```
mkdir /var/www/granz
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz/file.zip
  
unzip /var/www/granz/file.zip -d /var/www/granz
  
mv /var/www/granz/modul-3/* /var/www/granz
rm -rf /var/www/granz/file.zip
  
touch /etc/nginx/sites-available/granz.channel.b14
  
echo ' server {
  
        listen 80;
  
        root /var/www/granz;
  
        index index.php index.html index.htm;
        server_name _;
  
        location / {
                          try_files $uri $uri/ /index.php?$query_string;
        }
  
        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        }
        location ~ /\.ht {
                  deny all;
        }
  
        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
   }' > /etc/nginx/sites-available/granz.channel.b14
  
  ln -s /etc/nginx/sites-available/granz.channel.b14 /etc/nginx/sites-enabled
  rm -rf /etc/nginx/sites-enabled/default
  
  service php7.3-fpm start
  service php7.3-fpm restart
  service nginx restart
  nginx -t
```
Ketika dicoba diakses akan tampil sebagai berikut :
<img width="332" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/53fca572-1187-4e23-be7e-c387c678a247">

## No 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

Konfigurasi loadbalancing
```
echo '
upstream backend  {
        server 192.185.3.1 weight=640; #IP Lawine
        server 192.185.3.2 weight=200; #IP Linie
        server 192.185.3.3 weight=25; #IP Lugner
}

server {
        listen 80;
        server_name granz.channel.b14.com www.granz.channel.b14.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;

        }
        error_log /var/log/nginx/lb_error.log;
        access_log /var/log/nginx/lb_access.log;

}' > /etc/nginx/sites-available/lb-eisen
unlink /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/lb-eisen /etc/nginx/sites-enabled
service nginx restart

ab -V

mkdir /root/benchmark
cd /root/benchmark
```

Command untuk testing adalah
```
ab -n 1000 -c 100 -g out.data http://granz.channel.b14.com/
```

Hasilnya 
<img width="313" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/7b4c6bdf-4ae0-46dc-8c85-3d06da36a2a0">

## No 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
-Nama Algoritma Load Balancer
-Report hasil testing pada Apache Benchmark
-Grafik request per second untuk masing masing algoritma. 
-Analisis

Yang perlu dilakukan adalah setup algoritma load balancer menjadi 
- round robin
- least connection
- ip hash
- generic hash

Hasil dapat dilihat pada grimoire (https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing)

## No 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. 

dengan konfigurasi round robin yang sebelumnya digunakan yaitu :
```
apt-get update && apt-get install apache2-utils -y
apt-get install nginx php php-fpm -y
apt-get install htop -y

echo '
upstream backend  {
        server 192.185.3.1 weight=640; #IP Lawine
        server 192.185.3.2 weight=200; #IP Linie
        server 192.185.3.3 weight=25; #IP Lugner
}

server {
        listen 80;
        server_name granz.channel.b14.com www.granz.channel.b14.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;

        }
        error_log /var/log/nginx/lb_error.log;
        access_log /var/log/nginx/lb_access.log;

}' > /etc/nginx/sites-available/lb-eisen
unlink /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/lb-eisen /etc/nginx/sites-enabled
service nginx restart

ab -V

mkdir /root/benchmark
cd /root/benchmark
ab -n 200 -c 10 -g out.data http://granz.channel.b14.com/
```
Kemudian pada seting load balancer workernya dikurangi bertahap. Hasil dapat dilihat pada grimoire (https://docs.google.com/document/d/1EA9mcTm42bwJX5Y7Rk2cGimlB5DWhZvF_hf0izhEZeQ/edit?usp=sharing)

## No 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

Buat file dengan command berikut
```
mkdir /etc/nginx/rahasisakita
htpasswd -c /etc/nginx/rahasisakita/htpasswd netics
```

Lalu, masukkan passwordnya ``ajkb14``

Jika sudah memasukkan ``password`` dan ``re-type password``. Sekarang bisa dicoba dengan menambahkan command berikut pada setup nginx.

```sh
auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
```

Hasilnya akan mebatasi pengguna yang mengakses url ``http://www.granz.channel.b14.com/`` akan unauthorized sebagai berikut 
<img width="320" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/e4eeae4f-6266-473f-a7fb-c4baa33ccbe6">

<img width="331" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/e5a9c42e-a26c-4bd2-8e65-a5da5659b5fc">

<img width="77" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/15831fe2-c7c8-4b74-a948-adeb0546687d">

<img width="397" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/4bbcccea-41da-4174-99c9-9973213b9ad6">

## No 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. hint: (proxy_pass)

Ubah konfigurasi lb-php sebagai berikut :
```
echo 'upstream worker {
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
        proxy_pass http://worker;
    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/lb_php
```

Maksudnya adalah ketika kita melakukan akses pada endpoint yang mengandung ``/its`` akan diarahkan oleh ``proxy_pass`` menuju ``https://www.its.ac.id``. Jadi ketika melakukan testing pada client ``Revolte`` dengan menggunakan perintah berikut 

```
lynx www.granz.channel.b14.com/its
```

Hasilnya sebagai berikut :
<img width="840" alt="image" src="https://github.com/Osvaldo-Kurniawan/Jarkom-Modul-3-B14-2023/assets/108170210/66b6098d-8d28-449b-9ec4-116e7a537d8d">

## No 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

edit konfigurasi load balancer sebagai berikut :
```
echo 'upstream worker {
    server 192.185.4.1;
    server 192.185.4.2;
    server 192.185.4.3;
}

server {
    listen 80;
    server_name granz.channel.a09.com www.granz.channel.a09.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        allow 192.185.3.69;
        allow 192.185.3.70;
        allow 192.185.4.167;
        allow 192.185.4.168;
        deny all;
        proxy_pass http://worker;
    }

    location /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/lb_php
```

# Jarkom-Modul-3-E19-2023
Laporan Resmi Praktikum Modul 3 Kelompok E19
| Nama               |  NRP       | 
|--------------------|-------------|
| M. Armand Giovani | 5025211054  |
| Afiq Fawwaz Haidar | 5025211246  |

## Initial Config

### Aura

```sh
echo '
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.46.0.0/16

auto eth1s
iface eth1 inet static
	address 10.46.4.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.46.1.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.46.2.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.46.3.0
	netmask 255.255.255.0

' > /etc/network/interfaces

echo "nameserver 192.168.122.1" > /etc/resolv.conf

apt-get update
apt install isc-dhcp-relay -y

```

### Heiter

```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.1.2
	netmask 255.255.255.0
	gateway 10.46.1.0
' > /etc/network/interfaces

echo "nameserver 192.168.122.1" > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

service bind9 start

```

### Himmel

```sh

echo '
auto eth0
iface eth0 inet static
	address 10.46.1.1
	netmask 255.255.255.0
	gateway 10.46.1.0
' > /etc/network/interfaces

echo "nameserver 192.168.122.1" > /etc/resolv.conf

apt-get update
apt install isc-dhcp-server -y

```

### Client

```sh

echo '
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces

```

## Soal 1
_Register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1._

### Heiter

```sh
echo '
zone "riegel.canyon.e19.com" {
    type master;
    file "/etc/bind/m3/riegel.canyon.e19.com";
};

zone "riegel.canyon.e19.com" {
    type master;
    file "/etc/bind/m3/riegel.canyon.e19.com";
};
' > /etc/bind/named.conf.local

mkdir -p /etc/bind/m3

cp /etc/bind/db.local /etc/bind/m3/riegel.canyon.e19.com
cp /etc/bind/db.local /etc/bind/m3/granz.channel.e19.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.e19.com. root.riegel.canyon.e19.com. (
                        2023111501      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.e19.com.
@       IN      A       10.46.4.1     ; 
www     IN      CNAME   riegel.canyon.e19.com.
' > /etc/bind/m3/riegel.canyon.e19.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.e19.com. root.granz.channel.e19.com. (
                        2023111501      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.e19.com.
@       IN      A       10.46.3.1     ; 
www     IN      CNAME   granz.channel.e19.com.
' > /etc/bind/m3/granz.channel.e19.com

echo '
options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      allow-query{any;};
      auth-nxdomain no;  
      listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

service bind9 restart
```

---
## Soal 2
_Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80_

### Himmel

```sh

echo '
subnet 10.46.1.0 netmask 255.255.255.0 {
}

subnet 10.46.2.0 netmask 255.255.255.0 {
}

subnet 10.46.3.0 netmask 255.255.255.0 {
    range 10.46.3.16 10.46.3.32;
    range 10.46.3.64 10.46.3.80;
    option routers 10.46.3.0;
}

' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```

---
## Soal 3
_Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168_

### Himmel

```sh

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo '
subnet 10.46.1.0 netmask 255.255.255.0 {
}

subnet 10.46.2.0 netmask 255.255.255.0 {
}

subnet 10.46.3.0 netmask 255.255.255.0 {
    range 10.46.3.16 10.46.3.32;
    range 10.46.3.64 10.46.3.80;
    option routers 10.46.3.0;
}

subnet 10.46.4.0 netmask 255.255.255.0 {
    Range 10.46.4.12 10.46.4.20;
    range 10.46.4.160 10.46.4.168;
    option routers 10.46.4.0;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```

---
## Soal 4
_Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut_

### Himmel

```sh

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo '
subnet 10.46.1.0 netmask 255.255.255.0 {
}

subnet 10.46.2.0 netmask 255.255.255.0 {
}

subnet 10.46.3.0 netmask 255.255.255.0 {
    range 10.46.3.16 10.46.3.32;
    range 10.46.3.64 10.46.3.80;
    option routers 10.46.3.0;
    option broadcast-address 10.46.3.255;
    option domain-name-servers 10.46.1.2;
}

subnet 10.46.4.0 netmask 255.255.255.0 {
    Range 10.46.4.12 10.46.4.20;
    range 10.46.4.160 10.46.4.168;
    option routers 10.46.4.0;
    option broadcast-address 10.46.4.255;
    option domain-name-servers 10.46.1.2;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```

### Aura

```sh

echo '
SERVERS="10.46.1.2"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart

```

### Client

```sh

echo "nameserver 10.46.1.2" > /etc/resolv.conf

apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y

```

---
## Soal 5
_Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit_

---
## Soal 6
_Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3._

---
## Soal 7
_Lakukan testing dengan 1000 request dan 100 request/second_

---
## Soal 8
_Menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer_

---
## Soal 9
_Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire_

---
## Soal 10
_Tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/_

---
## Soal 11
_Buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id._

---
## Soal 12
_LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168._

---
## Soal 13
_Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern._

---
## Soal 14
_Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer_

---
## Soal 15
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/register_

---
## Soal 16
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/login_

---
## Soal 17
_Testing sebanyak 100 request dengan 10 request/second. GET /me_

---
## Soal 18
_Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern._

---
## Soal 19
_Lakukan testing sebanyak 100 request dengan 10 request/second dengan menaikan pm.max_children, pm.start_servers,  pm.min_spare_servers, pm.max_spare_servers_

---
## Soal 20
_Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second_

---

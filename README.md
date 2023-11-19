# Jarkom-Modul-3-E19-2023
Laporan Resmi Praktikum Modul 3 Kelompok E19
| Nama               |  NRP       | 
|--------------------|-------------|
| M. Armand Giovani | 5025211054  |
| Afiq Fawwaz Haidar | 5025211246  |

## Link Grimoire

https://docs.google.com/document/d/1i5b5GL8LXc_h71Uv4_Qp682wmWis0SpB5pHmxt3zqpw/edit?usp=sharing

## Initial Config

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/21950933-a607-4479-8463-cfddac6f4b55)

### Aura

```sh
echo '
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.46.0.0/16

auto eth1
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

### Eisen

```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.2.2
	netmask 255.255.255.0
	gateway 10.46.2.0
' > /etc/network/interfaces
```

### Danken

```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.2.1
	netmask 255.255.255.0
	gateway 10.46.2.0
' > /etc/network/interfaces
```

### Lawine
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.3.1
	netmask 255.255.255.0
	gateway 10.46.3.0
' > /etc/network/interfaces
```

### Linie
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.3.2
	netmask 255.255.255.0
	gateway 10.46.3.0
' > /etc/network/interfaces
```

### Lugner
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.3.3
	netmask 255.255.255.0
	gateway 10.46.3.0
' > /etc/network/interfaces
```
### Frieren
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.4.1
	netmask 255.255.255.0
	gateway 10.46.4.0
' > /etc/network/interfaces
```

### Flamme
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.4.2
	netmask 255.255.255.0
	gateway 10.46.4.0
' > /etc/network/interfaces
```

### Fern
```sh
echo '
auto eth0
iface eth0 inet static
	address 10.46.4.3
	netmask 255.255.255.0
	gateway 10.46.4.0
' > /etc/network/interfaces
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

zone "granz.canyon.e19.com" {
    type master;
    file "/etc/bind/m3/granz.canyon.e19.com";
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

## Penjelasan 
-   `zone "riegel.canyon.e19.com"` dan `zone "granz.canyon.e19.com"` digunakan untuk membuat domain baru dengan nama `riegel.canyon.e19.com` dan `granz.canyon.e19.com`
-   `type master` digunakan untuk membuat domain baru dengan tipe master
-   `file "/etc/bind/m3/riegel.canyon.e19.com"` dan `file "/etc/bind/m3/granz.canyon.e19.com"` digunakan untuk membuat domain baru dengan file konfigurasi `/etc/bind/m3/riegel.canyon.e19.com` dan `/etc/bind/m3/granz.canyon.e19.com`
-   `mkdir -p /etc/bind/m3` digunakan untuk membuat direktori baru bernama `/etc/bind/m3`
-  `cp /etc/bind/db.local /etc/bind/m3/riegel.canyon.e19.com` dan `cp /etc/bind/db.local /etc/bind/m3/granz.channel.e19.com` digunakan untuk mengcopy file konfigurasi `/etc/bind/db.local` ke `/etc/bind/m3/riegel.canyon.e19.com` dan `/etc/bind/m3/granz.channel.e19.com`
-  options digunakan untuk mengatur opsi-opsi dari bind9 seperti `forwarders` digunakan untuk mengatur DNS server yang akan digunakan untuk melakukan query
-  `allow-query{any;};` digunakan untuk mengatur siapa saja yang dapat melakukan query

## Testing
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/bf7bf07d-3c6b-4e1d-bc19-e9f9ae85c8a1)

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
## Testing

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/8f9fded9-b0bf-4095-b548-a0d5975309f3)

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
## Testing

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/6c647a6f-fd1f-4798-9194-b8124ab3eae7)

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

## Penjelasan 
-   `option domain-name-servers 10.46.1.2;` digunakan untuk mengatur DNS server yang akan digunakan oleh client yaitu `heiter` dengan IP `10.46.1.2`
### Aura

```sh

echo '
SERVERS="10.46.1.1"
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
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/09f8a656-7059-4256-a117-a0d95fe2f9f6)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/4dab7462-b1ee-4320-88fb-0d2921d03d4e)

---
## Soal 5
_Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit_

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
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.46.4.0 netmask 255.255.255.0 {
    Range 10.46.4.12 10.46.4.20;
    range 10.46.4.160 10.46.4.168;
    option routers 10.46.4.0;
    option broadcast-address 10.46.4.255;
    option domain-name-servers 10.46.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```

## Penjelasan
-   `default-lease-time 180` & `default-lease-time 180` merupakan waktu peminjaman alamat IP yang dialokasikan untuk client yang melalui Switch3 dan Switch4
-   `max-lease-time 5760` merupakan waktu maksimal peminjaman alamat IP yang dialokasikan untuk client yang melalui Switch3 dan Switch4


![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/1710fa10-640c-4374-90c4-a42258d72cd4)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/35d551b7-b3db-4e84-b9e4-a1bc41c7b761)

---
## Soal 6
_Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3._

### PHP Worker

```sh

echo "nameserver 10.46.1.2" > /etc/resolv.conf

apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql

wget --no-check-certificate -O '/var/www/granz.channel.e19.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'

unzip -o /var/www/granz.channel.e19.com -d /var/www/
rm /var/www/granz.channel.e19.com
mv /var/www/modul-3 /var/www/granz.channel.e19.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.e19.com
ln -s /etc/nginx/sites-available/granz.channel.e19.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo '
server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.e19.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock; 
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
' > /etc/nginx/sites-available/granz.channel.e19.com

service nginx start
service php7.3-fpm start

lynx localhost

```
## Penjelasan
-   `apt-get install php7.3-fpm php7.3-common php7.3-mysql` digunakan untuk menginstall php7.3
-   `wget --no-check-certificate -O '/var/www/granz.channel.e19.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'` digunakan untuk mendownload file zip dari link yang diberikan
-   `unzip -o /var/www/granz.channel.e19.com -d /var/www/` digunakan untuk mengunzip file zip yang telah didownload
-   `rm /var/www/granz.channel.e19.com` digunakan untuk menghapus file zip yang telah didownload
-   `mv /var/www/modul-3 /var/www/granz.channel.e19.com` digunakan untuk mengubah nama folder modul-3 menjadi granz.channel.e19.com
-   `cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.e19.com` digunakan untuk mengcopy file konfigurasi `/etc/nginx/sites-available/default` ke `/etc/nginx/sites-available/granz.channel.e19.com`
-   `ln -s /etc/nginx/sites-available/granz.channel.e19.com /etc/nginx/sites-enabled/` digunakan untuk membuat symlink dari `/etc/nginx/sites-available/granz.channel.e19.com` ke `/etc/nginx/sites-enabled/`
-   `rm /etc/nginx/sites-enabled/default` digunakan untuk menghapus symlink dari `/etc/nginx/sites-enabled/default`
-   `fastcgi_pass unix:/run/php/php7.3-fpm.sock;` digunakan untuk mengatur fastcgi_pass ke unix:/run/php/php7.3-fpm.sock

## Testing

![6](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/8e6ba9ab-6738-4ded-9ff2-1a4530e7858b)

---
## Soal 7
_Lakukan testing dengan 1000 request dan 100 request/second_

### Heiter

```sh
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
@       IN      A       10.46.2.2     ; 
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
@       IN      A       10.46.2.2     ; 
www     IN      CNAME   granz.channel.e19.com.
' > /etc/bind/m3/granz.channel.e19.com
```

## Penjelasan 
-   `10.46.2.2` Ip load balancer Eissen 

### Eisen

```sh

echo "nameserver 10.46.1.2" > /etc/resolv.conf

apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb
ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/
unlink /etc/nginx/sites-enabled/default

echo '
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

server {
    listen 80;
    server_name granz.channel.e19.com www.granz.channel.e19.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
}
' > /etc/nginx/sites-available/lb

service nginx restart

```
## Penjelasan
-  `upstream worker` digunakan untuk membuat upstream worker dengan nama worker
- Konfigurasi nginx untuk load balancer

### Client

```sh
ab -n 1000 -c 100 http://www.granz.channel.e19.com/
#atau
ab -n 1000 -c 100 http://10.46.2.2:80/
```
## Testing

![7](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/273bfc89-c264-4ff6-ba7f-f2fd7c29e45c)

---
## Soal 8
_Menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer_

### Eisen | Ganti setiap testing algo

```sh

# Round Robin (default)
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

# Least Connection
upstream worker {
    least_conn;
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

# IP Hash
upstream worker {
    ip_hash;
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

# Generic Hash
upstream worker {
    hash $request_uri consistent;
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

```

-  `least_conn` digunakan untuk mengatur algoritma load balancing menjadi least connection
-  `ip_hash` digunakan untuk mengatur algoritma load balancing menjadi ip hash
-  `hash $request_uri consistent` digunakan untuk mengatur algoritma load balancing menjadi generic hash


### PHP Worker

```sh
htop
```

- `htop` digunakan untuk melihat proses php-fpm

### Client
```sh
ab -n 200 -c 10 http://www.granz.channel.e19.com/
#atau
ab -n 200 -c 10 http://10.46.2.2:80/
```

## Testing

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/4034e7f2-6240-4359-add8-65cc353ef326)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/6c2e5871-0ee1-4071-9478-06a576a5fca1)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/5bc96d48-79d5-4e94-9e5b-b461b0a23297)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/82e80320-9348-436c-a81b-66b2f0e21b12)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/e2666473-c046-4077-bde2-e36f96ee2cc8)

---
## Soal 9
_Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire_

### Eisen | Ganti setiap testing jumlah worker

```sh

# 3 Worker
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

# 2 Worker
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
}

# 1 Worker
upstream worker {
    server 10.46.3.1;
}

```
-   `server 3 worker` digunakan untuk mengatur jumlah worker menjadi 3 worker
-   `server 2 worker` digunakan untuk mengatur jumlah worker menjadi 2 worker
-   `server 1 worker` digunakan untuk mengatur jumlah worker menjadi 1 worker

### PHP Worker

```sh
htop
```

-   `htop` digunakan untuk melihat proses php-fpm

### Client
```sh
ab -n 200 -c 10 http://www.granz.channel.e19.com/
#atau
ab -n 200 -c 10 http://10.46.2.2:80/
```

## Testing
- 3 Worker <br></br>
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/8e1dfeb2-6ff8-4d2c-884d-c529e78849d2)

- 2 Worker <br></br>
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/3b1c6c3e-99b9-4a76-aa40-fba3f600df6a)

- 1 Worker <br></br>
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/b91687bc-e965-46c2-a4a3-94b83ecda00b)
![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/100523471/d707004c-7f7a-4aa7-9dba-cd3a8b879e0b)

---
## Soal 10
_Tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/_

### Eisen

```sh
mkdir /etc/nginx/rahasisakita
echo 'ajke19' | htpasswd -ic /etc/nginx/rahasisakita/.htpasswd netics

echo '
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

server {
    listen 80;
    server_name granz.channel.e19.com www.granz.channel.e19.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;

        auth_basic "Administrators Area";
        auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;

    }

     location ~ /\.ht {
        deny all;
    }

}
' > /etc/nginx/sites-available/lb

service nginx restart

```
-  `mkdir /etc/nginx/rahasisakita` digunakan untuk membuat direktori baru bernama `/etc/nginx/rahasisakita`
-   `auth_basic "Administrators Area";` digunakan untuk mengatur pesan yang akan ditampilkan saat melakukan autentikasi
-   `auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;` digunakan untuk mengatur lokasi file `.htpasswd` yang digunakan untuk autentikasi

### Client
```sh
lynx www.granz.channel.e19.com
```

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/d7166e93-98a9-4f32-81a5-fd0d9a5db76c)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/92cf2bca-79ab-4506-be76-544a470dd5c9)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/1d55671f-1036-48b4-9f3b-91a395eb566d)


## Testing

---
## Soal 11
_Buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id._

### Eisen

```sh
echo '
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

server {
    listen 80;
    server_name granz.channel.e19.com www.granz.channel.e19.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;

        auth_basic "Administrators Area";
        auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;

    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

     location ~ /\.ht {
        deny all;
    }

}
' > /etc/nginx/sites-available/lb

service nginx restart

```
-  `location ~ /its` digunakan untuk mengatur proxy pass ke `https://www.its.ac.id` jika request mengandung `/its`
-   `proxy_set_header Host www.its.ac.id;` digunakan untuk mengatur header host menjadi `www.its.ac.id`
-   `proxy_set_header X-Real-IP $remote_addr;` digunakan untuk mengatur header X-Real-IP menjadi `$remote_addr`
-   `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;` digunakan untuk mengatur header X-Forwarded-For menjadi `$proxy_add_x_forwarded_for`
-   `proxy_set_header X-Forwarded-Proto $scheme;` digunakan untuk mengatur header X-Forwarded-Proto menjadi `$scheme`

### Client
```sh
lynx www.granz.channel.e19.com/its
#atau
lynx 10.46.2.2:80/its
```

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/73dab35c-5064-4906-aec3-997d7d995a45)


## Testing

---
## Soal 12
_LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168._

### Eisen

```sh
echo '
upstream worker {
    server 10.46.3.1;
    server 10.46.3.2;
    server 10.46.3.3;
}

server {
    listen 80;
    server_name granz.channel.e19.com www.granz.channel.e19.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;

	allow 10.46.3.69;
        allow 10.46.3.70;
        allow 10.46.4.167;
        allow 10.46.4.168;
        deny all;

        auth_basic "Administrators Area";
        auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;

    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

     location ~ /\.ht {
        deny all;
    }

}
' > /etc/nginx/sites-available/lb

service nginx restart

```
Disini kami hanya mengizinkan beberapa IP saja sesuai dengan ketentual soal dan kamu menolak seluruh IP selain yang telah ditentukan soal. Untuk melakukan testingnya. Bisa dilakukan dengan membuka client yang mendapatkan IP 10.46.3.69, 10.46.3.70, 10.46.4.167, dan 10.46.4.168. 

Client dengan IP selain diatas.

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/b2c977e7-f5d5-4280-a74b-f9b7e8c3ccdf)

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/8c426787-6465-4cb1-9be8-1ba05601ec2c)

---
## Soal 13
_Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern._

### Danken

```sh

apt-get update
apt-get install mariadb-server -y

echo '
# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf

nano /etc/mysql/mariadb.conf.d/50-server.cnf
# Lalu ganti bind-address menjadi 0.0.0.0

service mysql start

# Lalu buka MariaDB
mysql -u root -p

# saat keluar 'Enter password:', tekan enter saja langsung

# Masukan command MySQL
CREATE USER 'kelompoke19'@'%' IDENTIFIED BY 'passworde19';
CREATE USER 'kelompoke19'@'localhost' IDENTIFIED BY 'passworde19';
CREATE DATABASE dbkelompoke19;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoke19'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoke19'@'localhost';
FLUSH PRIVILEGES;

```
-   setup mariadb server dengan `apt-get install mariadb-server -y` jangan lupa ganti bind-address menjadi 0.0.0.0

### Laravel Worker

```sh
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y

mariadb --host=10.46.2.1 --port=3306 --user=kelompoke19 --password=passworde19 -e "SHOW DATABASES;"

```
-  `mariadb --host=host=10.46.2.1 --port=3306 --user=kelompoke19 --password=passworde19 -e "SHOW DATABASES;"` digunakan untuk mengecek apakah koneksi ke database berhasil atau tidak

## Testing

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/d4105bb3-4832-46ab-a9ae-b73f1f8396ae)


---
## Soal 14
_Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer_

### Laravel Worker

```sh
apt-get install ca-certificates ntpdate
ntpdate-debian
apt-get install -y lsb-release ca-certificates a   apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

service nginx start
service php8.0-fpm start

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer

apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update

cd /var/www/laravel-praktikum-jarkom && cp .env.example .env

echo '
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.46.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoke19
DB_USERNAME=kelompoke19
DB_PASSWORD=passworde19

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
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
' > /var/www/laravel-praktikum-jarkom/.env

cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage

echo '
server {
    listen 8001 # Sesuaikan dengan worker, 8001 -> Frieren | 8002 -> Flamme | 8003 -> Fern;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}
' > /etc/nginx/sites-available/default

service nginx restart
service php8.0-fpm restart

lynx localhost:8001  # Sesuaikan dengan worker, 8001 -> Frieren | 8002 -> Flamme | 8003 -> Fern;

```
-   Lakukan install composer dan php8.0 dengan `apt-get install composer php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y`
-   Lakukan install git dengan `apt-get install git -y` dan clone repository dengan `cd /var/www && git clone
-   Lakukan install laravel dengan `cd /var/www/laravel-praktikum-jarkom && composer update`
-   lakukan konfigurasi pada masing-masing worker dengan `cd /var/www/laravel-praktikum-jarkom && cp .env.example .env`
-   Setelah berhasil menjalankan semuanya dan tidak mendapatkan error. Sekarang lakukan konfigurasi nginx sebagai berikut pada masing-masing worker dimana port nya adalah sebagai berikut `8001 -> Frieren | 8002 -> Flamme | 8003 -> Fern;`
-   Setelah itu lakukan restart nginx dan php8.0-fpm dengan `service nginx restart` dan `service php8.0-fpm restart`

## Testing

![image](https://github.com/AfiqHaidar/Jarkom-Modul-3-E19-2023/assets/70834506/5dca6e4e-4dad-491c-83f2-3ddff07e91c4)


---
## Soal 15
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/register_

### Client
```sh
echo '{"username": "kelompoke19", "password": "passworde19"}' > register.json && ab -n 100 -c 10 -p register.json -T application/json http://10.46.4.1:8001/api/auth/register
```
-   lakukan testing dengan `ab -n 100 -c 10 -p register.json -T application/json http://10.46.4.1:8001/api/auth/register`

---
## Soal 16
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/login_

### Client
```sh
echo '{"username": "kelompoke19", "password": "passworde19"}' > login.json && ab -n 100 -c 10 -p login.json -T application/json http://10.46.4.1:8001/api/auth/login
```
-   lakukan testing dengan `ab -n 100 -c 10 -p login.json -T application/json http://10.46.4.2:8002/api/auth/login`

---
## Soal 17
_Testing sebanyak 100 request dengan 10 request/second. GET /me_

### Client
```sh
curl -X POST -H "Content-Type: application/json" -d @login.json http://10.46.4.1:8001/api/auth/login > login_output.txt

token=$(cat login_output.txt | jq -r '.token')

ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.46.4.1:8001/api/me

```
-   `curl -X POST -H "Content-Type: application/json" -d @login.json http://10.46.4.1:8001/api/auth/login > login_output.txt` digunakan untuk melakukan login dan menyimpan outputnya ke `login_output.txt`
-   `token=$(cat login_output.txt | jq -r '.token')` digunakan untuk mengambil token dari `login_output.txt`

---
## Soal 18
_Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern._

### Eisen

```sh

cp /etc/nginx/sites-available/lb /etc/nginx/sites-available/lr

echo '
upstream worker {
    server 10.46.4.1:8001;
    server 10.46.4.2:8002;
    server 10.46.4.3:8003;
}

server {
    listen 81;
    server_name riegel.canyon.e19.com www.riegel.canyon.e19.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/lr

ln -s /etc/nginx/sites-available/lr /etc/nginx/sites-enabled/lr

service nginx restart

```

## Note
-   Hati-hati port tabrakan dengan load balancer dari php worker

### Client
```sh

echo '{"username": "kelompoke19", "password": "passworde19"}' > login.json && ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.e19.com/api/auth/login

```

---
## Soal 19
_Lakukan testing sebanyak 100 request dengan 10 request/second dengan menaikan pm.max_children, pm.start_servers,  pm.min_spare_servers, pm.max_spare_servers_


pm.max_children Menentukan jumlah maksimum pekerja PHP (proses anak) yang dapat berjalan secara bersamaan. Nilai ini sebaiknya disesuaikan dengan kapasitas sumber daya server. Jika terlalu rendah, server mungkin tidak dapat menangani banyak permintaan secara bersamaan, sementara jika terlalu tinggi, dapat menyebabkan kelebihan beban dan kekurangan sumber daya.

pm.start_servers Menentukan jumlah pekerja PHP yang akan dimulai secara otomatis ketika PHP-FPM pertama kali dijalankan atau direstart. Ini membantu dalam mengoptimalkan performa pada saat server pertama kali dimulai.

pm.min_spare_servers Menentukan jumlah minimum pekerja PHP yang tetap berjalan saat server berjalan. Ini membantu menjaga agar server tetap responsif terhadap permintaan bahkan saat lalu lintas rendah.

pm.max_spare_servers Menentukan jumlah maksimum pekerja PHP yang dapat berjalan tetapi tidak menangani permintaan. Jumlah ini disesuaikan dengan kebutuhan untuk menangani lonjakan lalu lintas tanpa menambahkan terlalu banyak sumber daya ketika beban rendah


### Laravel Worker

```sh

# Base Case
echo '
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
pm.max_children = 5 
pm.start_servers = 2 
pm.min_spare_servers = 1 
pm.max_spare_servers = 3
' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

# Lakukan Testing Base Case pada Client!

# Case 1
echo '
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
pm.max_children = 15 
pm.start_servers = 12 
pm.min_spare_servers = 2 
pm.max_spare_servers = 13
' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

# Lakukan Testing Case 1 pada Client!

# Case 2
echo '
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
pm.max_children = 25 
pm.start_servers = 22 
pm.min_spare_servers = 3 
pm.max_spare_servers = 23
' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

# Lakukan Testing Case 2 pada Client!

# Case 3
echo '
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
pm.start_servers = 25 
pm.min_spare_servers = 4 
pm.max_spare_servers = 25
' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

# Lakukan Testing Case 3 pada Client!
```

### Client

```sh

ab -n 100 -c 10 http://10.46.2.2:81/

```

---
## Soal 20
_Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second_

### Eisen

```sh

echo '
upstream worker {
    least_conn;
    server 10.46.4.1:8001;
    server 10.46.4.2:8002;
    server 10.46.4.3:8003;
}

server {
    listen 81;
    server_name riegel.canyon.e19.com www.riegel.canyon.e19.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/lr

service nginx restart

```

### Client

```sh

ab -n 100 -c 10 http://10.46.2.2:81/

```

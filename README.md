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

### Client

```sh
ab -n 1000 -c 100 http://www.granz.channel.e19.com/
#atau
ab -n 1000 -c 100 http://10.46.2.2:80/
```

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

### PHP Worker

```sh
htop
```

### Client
```sh
ab -n 200 -c 10 http://www.granz.channel.e19.com/
#atau
ab -n 200 -c 10 http://10.46.2.2:80/
```

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

### PHP Worker

```sh
htop
```

### Client
```sh
ab -n 200 -c 10 http://www.granz.channel.e19.com/
#atau
ab -n 200 -c 10 http://10.46.2.2:80/
```

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

### Client
```sh
lynx www.granz.channel.e19.com/its
#atau
lynx 10.46.2.2:80/its
```

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

### Laravel Worker

```

apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y

mariadb --host=10.46.2.1 --port=3306 --user=kelompoke19 --password=passworde19 -e "SHOW DATABASES;"

```

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

---
## Soal 15
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/register_

### Client
```sh
echo '{"username": "kelompoke19", "password": "passworde19"}' > register.json && ab -n 100 -c 10 -p register.json -T application/json http://10.46.4.1:8001/api/auth/register
```

---
## Soal 16
_Testing sebanyak 100 request dengan 10 request/second. POST /auth/login_

### Client
```sh
echo '{"username": "kelompoke19", "password": "passworde19"}' > login.json && ab -n 100 -c 10 -p login.json -T application/json http://10.46.4.1:8001/api/auth/login
```

---
## Soal 17
_Testing sebanyak 100 request dengan 10 request/second. GET /me_

### Client
```sh
curl -X POST -H "Content-Type: application/json" -d @login.json http://10.46.4.1:8001/api/auth/login > login_output.txt

token=$(cat login_output.txt | jq -r '.token')

ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.46.4.1:8001/api/me

```

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

### Client
```sh

echo '{"username": "kelompoke19", "password": "passworde19"}' > login.json && ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.e19.com/api/auth/login

```

---
## Soal 19
_Lakukan testing sebanyak 100 request dengan 10 request/second dengan menaikan pm.max_children, pm.start_servers,  pm.min_spare_servers, pm.max_spare_servers_

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

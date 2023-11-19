# JARKOM-MODUL-3-D11-2023

|Nama|NRP|
|----|---|
|Gracetriana Survinta Septinaputri|5025211199|
|Muhammad Rifqi Fadhilah|5025211228|

- [Soal 0](#soal-0)
  - [Configuration](#configuration) 
- [Soal 1](#soal-1)
- [Soal 2](#soal-2)
- [Soal 3](#soal-3)
- [Soal 4](#soal-4)
- [Soal 5](#soal-5)
- [Soal 6](#soal-6)
- [Soal 7](#soal-7)
- [Soal 8](#soal-8)
- [Soal 9](#soal-9)
- [Soal 10](#soal-10)
- [Soal 11](#soal-11)
- [Soal 12](#soal-12)
- [Soal 13](#soal-13)
- [Soal 14](#soal-14)
- [Soal 15](#soal-15)
- [Soal 16](#soal-16)
- [Soal 17](#soal-17)
- [Soal 18](#soal-18)
- [Soal 19](#soal-19)
- [Soal 20](#soal-20)

## SOAL 0
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP mengarah pada worker yang memiliki IP [prefix IP].x.1.

### Topologi
![Screenshot 2023-11-19 181505](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/757a2c93-8961-4a49-ae13-1de91df889a9)

### Configuration
Set up konfigurasi dalam `root/.bashrc` pada DNS Server Heiter untuk register domain `riegel.canyon.d11.com` dan `granz.channel.d11.com`

- Heiter (DNS Server)

```
apt-get update
 apt-get install bind9 -y
echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf

echo ‘zone "canyon.d11.com" {
	type master;
	file "/etc/bind/jarkom/canyon.d11.com";
};

zone "channel.d11.com" {
	type master;
	file "/etc/bind/jarkom/channel.d11.com";
};’ > /etc/bind/named.conf.local

mkdir -p /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/canyon.d11.com
cp /etc/bind/db.local /etc/bind/jarkom/channel.d11.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     canyon.d11.com. root.canyon.d11.com. (
                        2023110101    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      canyon.d11.com.
@               IN      A       10.27.2.3 ; IP Eisen
riegel             IN      A   10.27.4.1 ; IP Frieren' > /etc/bind/jarkom/canyon.d11.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     channel.d11.com. root.channel.d11.com. (
                        2023110101    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      channel.d11.com.
@               IN      A       10.27.2.3 ; IP Eisen
granz             IN      A   10.27.3.1 ; IP Lawine' > /etc/bind/jarkom/channel.d11.com


echo ‘options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

      // dnssec-validation auto;        
	allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};’ > /etc/bind/named.conf.options


service bind9 restart
```

Lakukan test dengan ping kedua domain tersebut di client

```
ping riegel.canyon.d11.com
ping granz.channel.d11.com
```
### Hasil 
![Screenshot 2023-11-19 193007](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/e6a78994-7da0-41fb-809e-82eef6c38860)

## SOAL 1

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

- AURA (Router)

```auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.27.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.27.2.0
	netmask 255.255.255.0


auto eth3
iface eth3 inet static
	address 10.27.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.27.4.0
	netmask 255.255.255.0
```
Tambahkan command pada `.bashrc` Aura 
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.27.0.0/16
```
- HIMMEL

```
auto eth0
iface eth0 inet static
	address 10.27.1.2
	netmask 255.255.255.0
	gateway 10.27.1.0
```
- HEITER
  
```
auto eth0
iface eth0 inet static
	address 10.27.1.3
	netmask 255.255.255.0
	gateway 10.27.1.0
```
- DENKEN

```
auto eth0
iface eth0 inet static
	address 10.27.2.2
	netmask 255.255.255.0
	gateway 10.27.2.0
```
- EISEN

```
auto eth0
iface eth0 inet static
	address 10.27.2.3
	netmask 255.255.255.0
	gateway 10.27.2.0
```
- FRIEREN

```
auto eth0
iface eth0 inet static
	address 10.27.4.1
	netmask 255.255.255.0
	gateway 10.27.4.0

echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether 4a:cf:f2:53:ce:a6’ > /etc/network/interfaces
```
- FLAMME

```
auto eth0
iface eth0 inet static
	address 10.27.4.2
	netmask 255.255.255.0
	gateway 10.27.4.0

echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether 5a:00:4c:af:44:14’ > /etc/network/interfaces
```
- FERN

```
auto eth0
iface eth0 inet static
	address 10.27.4.3
	netmask 255.255.255.0
	gateway 10.27.4.0

echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether a2:09:8b:59:c9:52’ > /etc/network/interfaces
```
- LAWINE

```
auto eth0
iface eth0 inet static
	address 10.27.3.1
	netmask 255.255.255.0
	gateway 10.27.3.0

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether c2:87:25:2d:5d:e0’ > /etc/network/interfaces
```
- LINIE

```

auto eth0
iface eth0 inet static
	address 10.27.3.2
	netmask 255.255.255.0
	gateway 10.27.3.0

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether da:e9:42:3c:56:96’ > /etc/network/interfaces
```
- LUGNER

```
auto eth0
iface eth0 inet static
	address 10.27.3.3
	netmask 255.255.255.0
	gateway 10.27.3.0

echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf

echo ‘auto eth0
iface eth0 inet dhcp
hwaddress ether 7e:6f:8b:a6:9c:4d’ > /etc/network/interfaces
```

- CLIENT (REVOLTE, RICHTER, SEIN, STARK)

```
auto eth0
iface eth0 inet dhcp
```

Pada node yang memiliki IP statis pada awalnya konfigurasi IP menggunakan `auto eth0 iface eth0 inet static` kemudian dirubah menggunakan DHCP untuk fixed address (setelah nomor 5) dengan menambahkan command berikut pada DHCP servernya.

```
echo ‘host Lawine {
    hardware ethernet c2:87:25:2d:5d:e0;
    fixed-address 10.27.3.1;
}
host Linie {
    hardware ethernet da:e9:42:3c:56:96;
    fixed-address 10.27.3.2;
}
host Lugner {
    hardware ethernet 7e:6f:8b:a6:9c:4d;
    fixed-address 10.27.3.3;
}
host Frieren {
    hardware ethernet 4a:cf:f2:53:ce:a6;
    fixed-address 10.27.4.1;
}
host Flamme {
    hardware ethernet 5a:00:4c:af:44:14;
    fixed-address 10.27.4.2;
}
host Fern {
    hardware ethernet a2:09:8b:59:c9:52;
    fixed-address 10.27.4.3;
}’ >> /etc/dhcp/dhcpd.conf
```

Tes di salah satu node untuk ping ke google.com

### Hasil
![Screenshot 2023-11-19 202216](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/ca66eb7a-e9d6-42dd-86ac-96b53e9a34d7)

## SOAL 2

Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

Pada DHCP Server (HIMMEL) dilakukan set up menggunakan command di bawah ini

```
apt-get update
apt-get install isc-dhcp-server -y

echo ‘INTERFACESv4="eth0"
INTERFACESv6=""’ > /etc/default/isc-dhcp-server

echo ‘subnet 10.27.1.0 netmask 255.255.255.0 {
}

subnet 10.27.2.0 netmask 255.255.255.0 {
}

subnet 10.27.3.0 netmask 255.255.255.0 {
    range 10.27.3.16 10.27.3.32;
    range 10.27.3.64 10.27.3.80;
    option routers 10.27.3.0;
    ...
}

```

## SOAL 3

Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 

Set up pada DHCP server HIMMEL untuk Switch4

```
echo ‘subnet 10.27.1.0 netmask 255.255.255.0 {
}

subnet 10.27.2.0 netmask 255.255.255.0 {
}

subnet 10.27.3.0 netmask 255.255.255.0 {
    range 10.27.3.16 10.27.3.32;
    range 10.27.3.64 10.27.3.80;
    option routers 10.27.3.0;
    ...
}

subnet 10.27.4.0 netmask 255.255.255.0 {
    range 10.27.4.12 10.27.4.20;
    range 10.27.4.160 10.27.4.168;
    option routers 10.27.4.0;
    ...
}’>  /etc/dhcp/dhcpd.conf
```

## SOAL 4

Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

Pada set up nomor 2 dan 3, perlu ditambahkan command ` option broadcast-address 10.27.4.255;` dan `option domain-name-servers 10.27.1.3;` untuk menyambungkan ke internet melalui DNS

```
...
subnet 10.27.3.0 netmask 255.255.255.0 {
    range 10.27.3.16 10.27.3.32;
    range 10.27.3.64 10.27.3.80;
    option routers 10.27.3.0;
    option broadcast-address 10.27.3.255;
    option domain-name-servers 10.27.1.3;
    ...
}

subnet 10.27.4.0 netmask 255.255.255.0 {
    range 10.27.4.12 10.27.4.20;
    range 10.27.4.160 10.27.4.168;
    option routers 10.27.4.0;
    option broadcast-address 10.27.4.255;
    option domain-name-servers 10.27.1.3;
    ...
}’>  /etc/dhcp/dhcpd.conf
```

## SOAL 5

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Karena waktu yang dipinjamkan ke Switch3 adalah `3 menit` dan Switch4 adalah `12 menit` maka pada set up DHCP server tadi perlu ditambahkan `default-lease-time 180;` pada subnet Switch3 dan `default-lease-time 720;` pada subnet Switch4. Dengan waktu maksimal untuk peminjaman alamat IP adalah 96 menit maka ditambahkan juga `max-lease-time 5760;` untuk kedua Switch.

```
echo ‘subnet 10.27.1.0 netmask 255.255.255.0 {
}

subnet 10.27.2.0 netmask 255.255.255.0 {
}

subnet 10.27.3.0 netmask 255.255.255.0 {
    range 10.27.3.16 10.27.3.32;
    range 10.27.3.64 10.27.3.80;
    option routers 10.27.3.0;
    option broadcast-address 10.27.3.255;
    option domain-name-servers 10.27.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.27.4.0 netmask 255.255.255.0 {
    range 10.27.4.12 10.27.4.20;
    range 10.27.4.160 10.27.4.168;
    option routers 10.27.4.0;
    option broadcast-address 10.27.4.255;
    option domain-name-servers 10.27.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}’>  /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Testing nomor 2-5
Tes untuk nomor 2-5

Dengan melakukan command `ip a` kemudian restart node dan `ip a` lagi untuk cek apakah IP sudah berubah-ubah sesuai range. Berikut adalah hasil `ip a` pada node Revolte

- `ip a` pertama
![Screenshot 2023-11-19 212529](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/62e8d08d-015f-4f52-9a3b-f433e6296354)

- `ip a` setelah restart node
![Screenshot 2023-11-19 212714](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/08975861-3b1c-433f-a16e-91237c2994af)

### Hasil
Saat run node, terlihat bahwa DHCP berjalan
![Screenshot 2023-11-19 212658](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/1c7b3735-1510-41d2-89ce-d7779e0c743f)

## SOAL 6

Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3

Pada setiap PHP worker, set up `.bashrc` terlebih dahulu untuk install dependenciesnya

- LAWINE, LINIE, LUGNER (PHP WORKER)

```
apt-get update
apt-get install nginx -y
apt-get install php php-fpm -y
apt-get install wget
apt-get install unzip
```
Kemudian, tambahkan script untuk mendownload file yang diperlukan menggunakan `wget` dan script untuk konfigurasi `nginx` seperti di bawah ini

```
mkdir -p /var/www/html/granz.channel.d11.com
mkdir -p /var/www/jarkom

echo ‘server {

listen 80;

root /var/www/html/granz.channel.d11.com;

index index.php index.html index.htm;
server_name granz.channel.d11.com;

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
}' > /etc/nginx/sites-available/jarkom


wget –no-check-certificate “https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1” -O /var/www/jarkom/granz.channel.d11.com.zip

unzip /var/www/jarkom/granz.channel.d11.com.zip -d /var/www/jarkom
cp -r /var/www/jarkom/modul-3/* /var/www/html/granz.channel.d11.com
rm -r /var/www/html/granz.channel.d11.com/modul-3

ln -sf /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled

rm -r /etc/nginx/sites-enabled/default

service nginx reload
service nginx restart
service php7.3-fpm start
service php7.3-fpm status
```

Tidak lupa untuk set up install dependencies pada client
- REVOLTE, RICHTER, SEIN, STARK (CLIENT)

```
apt-get update
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install nginx -y
```

### Testing
Untuk testing dapat menggunakan lynx pada client dengan command sebagai berikut
```
lynx granz.channel.d11.com
```

### Hasil

![Screenshot (138)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/6ba87450-4575-4fc9-9375-75111a8ef8c4)

## SOAL 7

Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. 

Pertama, lakukan set up dan konfigurasi nginx pada Load Balancer yaitu `Eisen` yang disimpan pada `root/.bashrc`

- EISEN

```
echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf
echo ‘nameserver 10.27.1.3’ >> /etc/resolv.conf
apt-get update
apt install nginx -y
service nginx start
echo ‘upstream myweb {
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	server 10.27.3.3 weight=1;
}

server {
	listen 80;
	server_name granz.channel.d11.com;
		location / {
                proxy_pass http://myweb;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }
	error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}’ > /etc/nginx/sites-available/lb-jarkom

unlink /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled/default
service nginx restart
```

### Hasil
Lakukan testing pada client dengan command
```
ab -n 1000 -c 100 http://granz.channel.d11.com
```
Muncul hasil seperti ini

![Screenshot (139)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/3fba0810-498d-44a0-9e0f-cc6369f63a50)

> Requests per second = 1074.92 [#/sec]

## SOAL 8

Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
> Nama Algoritma Load Balancer

> Report hasil testing pada Apache Benchmark

> Grafik request per second untuk masing masing algoritma.

> Analisis

Untuk konfigurasi kurang lebih sama dengan [SOAL 7](#soal-7), hanya melakukan set up pada LB Worker EISEN terlebih dahulu sesuai algoritma yang digunakan. Kemudian, testing di node client dengan command

```
ab -n 200 -c 10 http://granz.channel.d11.com/
```

Jangan lupa untuk command `htop` di tiap worker PHP untuk menunjukkan informasi proses yang berjalan.

### Hasil
- Round Robin

Set up untuk bagian `upstream myweb` pada `/etc/nginx/sites-available/lb-jarkom` eisen seperti di bawah ini

```
upstream myweb {
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	server 10.27.3.3 weight=1;
}
```

![Screenshot (140)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/705d9709-7ebf-4f8d-9c2e-03eb969cac6c)
> Requests per second 1907.98 [#/sec]

- Least Connection

Ubah pada `/etc/nginx/sites-available/lb-jarkom` sesuai dengan algoritma ini

```
upstream myweb {
	least_conn;
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	server 10.27.3.3 weight=1;
}
```

![Screenshot (143)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/f58407bf-53bb-4bbb-aabd-e72c5913f058)

![Screenshot (144)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/93064d77-8560-4c6f-b501-2e72d7d2cf1b)

> Requests per second : 2877.99 [#/sec]

- IP Hash

Set up `/etc/nginx/sites-available/lb-jarkom` sesuai algoritma IP Hash

```
upstream myweb {
	ip_hash;
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	server 10.27.3.3 weight=1;
}
```
![Screenshot (148)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/87fa1907-94b9-4edb-bae9-87bf403950a8)

> Requests per second : 2916.51 [#/sec]

![Screenshot (147)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/204b3a1e-98a7-4de7-a522-38996e6f3c03)

- Generic Hash

Set up `/etc/nginx/sites-available/lb-jarkom` sesuai algoritma Generic Hash

```
upstream myweb {
	hash $request_uri consistent;
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	server 10.27.3.3 weight=1;
}
```

![Screenshot (154)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/c5138e5d-a50a-420c-9c9f-ce33d02cff57)
> Requests per second : 3613.76 [#/sec]

![Screenshot (153)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/c32a9aab-c68c-4b5b-b638-49a2ce2c0b94)

### Grafik

![Requests per second vs  Algoritma](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/f0e7c6ec-f25c-424c-acbb-f9bdb74ccf14)

## SOAL 9

Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. 

Set up `/etc/nginx/sites-available/lb-jarkom` dari nomor sebelumnya kembali ke algoritma round robin. Testing menggunakan ApacheBench dengan command

`ab -n 100 -c 10 http://granz.channel.d11.com/`

- 3 worker
  
![Screenshot (164)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/fa33a422-f6ae-4524-9909-100e7d88209c)
> Requests per second : 810.96 [#/sec]

![Screenshot (163)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/a26f67d4-6fc1-4a87-b2aa-253968d65e42)

- 2 worker
  
Untuk 2 worker, lakukan comment untuk salah satu server pada `upstream myweb` di `/etc/nginx/sites-available/lb-jarkom`

```
echo ‘upstream myweb {
	server 10.27.3.1 weight=4;
	server 10.27.3.2 weight=2;
	# server 10.27.3.3 weight=1;
}
```

![Screenshot (162)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/3d3ca081-18cc-495d-8561-fd40a9eba961)
> Requests per second : 649.27 [#/sec]

![Screenshot (161)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/3e4b6252-6c81-4ae1-a67b-27fac29786f5)

- 1 worker
  
Untuk 1 worker, lakukan comment untuk 2 server pada `upstream myweb` di `/etc/nginx/sites-available/lb-jarkom`

```
echo ‘upstream myweb {
	server 10.27.3.1 weight=4;
	# server 10.27.3.2 weight=2;
	# server 10.27.3.3 weight=1;
}
```

![Screenshot (160)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/d9fd5695-808a-4785-a253-6f325272308b)
> Requests per second : 711.76 [#/sec]

![Screenshot (159)](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/60c84ba2-5d02-4404-a113-24abadb2031b)


### Grafik

![Requests per second vs  Worker](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/d5e2534d-7a39-428b-aaae-177a13c6d5b6)

### Laporan Grimoire

Laporan Grimoire yang berisi analisis bisa diakses di link [berikut](https://docs.google.com/document/d/1NGMJFZIG9Cl242Gs3bXUiULFMXhgzZhfRmfS_TW5l4A/edit?usp=sharing).
**NOTE : Data hasil yang ada di nomor 8 dan 9 lapres ini dengan data di laporan grimoire dibuat oleh anggota yang berbeda sehingga terdapat perbedaan hasil data**

## SOAL 10

Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

ubah konfigurasi pada nginx menjadi seperti ini:

```
upstream myweb {
        hash $request_uri consistent;
        server 10.27.3.1 weight=4;
        server 10.27.3.2 weight=2;
        server 10.27.3.3 weight=1;
}

server {
        listen 80;
        server_name granz.channel.d11.com;
                location / {
                proxy_pass http://myweb;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
		auth_basic "Administrator's Area";
                auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }
        error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

setelah melakukan konfigurasi jangan lupa buat direktori rahasia kita dan tambahkan password seperti yang di perintahkan soal

```
mkdir /etc/nginx/rahasiakita
htpasswd -b -c /etc/nginx/rahasiakita/.htpasswd netics ajkd11
```
### Hasil

![10](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/130858750/2b7eec36-4dcd-4e49-b134-05af3c58efdc)


## SOAL 11

Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

untuk melakukan hal tersebut kita perlu konfigutasi tambahan di nginx kita seperti ini:

```
       location /its {
                proxy_pass https://www.its.ac.id;

                allow all;
        }
```

### Hasil

![11](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/130858750/f37c02e9-89cb-4094-8868-d46446e27568)

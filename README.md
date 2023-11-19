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

service bind9 restart
```

Lakukan test dengan ping kedua domain tersebut di client

```
ping riegel.canyon.d11.com
ping granz.channel.d11.com
```
### Hasil 
![Screenshot 2023-11-19 193007](https://github.com/gracetrianaa/JARKOM-MODUL-3-D11-2023/assets/90684914/e6a78994-7da0-41fb-809e-82eef6c38860)


 

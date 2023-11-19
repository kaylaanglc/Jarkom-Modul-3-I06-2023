# Jarkom-Modul-3-I06-2023

### **Group Members**

| **Name**                  | **NRP**    |
| ------------------------- | ---------- |
| Ahmad Danindra Nugroho    | 5025211259 |
| Kayla Angelica Priambudi  | 5025211262 |

## Topology
<img width="923" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/ca3ecd36-440c-43e3-8c48-e05ea83ffae5">

## Network Configurations
- **Aura (Router - DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.231.1.195
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.231.2.195
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.231.3.195
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.231.4.195
	netmask 255.255.255.0

```
- **Himmel (DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.231.1.1
	netmask 255.255.255.0
	gateway 192.231.1.0

```
- **Heiter (DNS Server)**
```
auto eth0
iface eth0 inet static
	address 192.231.1.2
	netmask 255.255.255.0
	gateway 192.231.1.0
```
- **Denken (Database Server)**
```
auto eth0
iface eth0 inet static
	address 192.231.2.1
	netmask 255.255.255.0
	gateway 192.231.2.0
```
- **Eisen (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 192.231.2.2
	netmask 255.255.255.0
	gateway 192.231.2.0
```
- **Frieren (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.4.3
	netmask 255.255.255.0
	gateway 192.231.4.0

hwaddress ether 9e:d5:2c:df:39:32
```
- **Flamme (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.4.2
	netmask 255.255.255.0
	gateway 192.231.4.0

hwaddress ether da:cc:d9:29:59:e8
```
- **Fern (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.4.1
	netmask 255.255.255.0
	gateway 192.231.4.0

hwaddress ether 8a:31:c6:e6:a2:7b
```
- **Lawine (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.3.3
	netmask 255.255.255.0
	gateway 192.231.3.0

hwaddress ether 8e:c1:16:c2:6d:4b
```
- **Linie (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.3.2
	netmask 255.255.255.0
	gateway 192.231.3.0

hwaddress ether 7a:e7:1c:d2:ae:44
```
- **Lugner (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.231.3.1
	netmask 255.255.255.0
	gateway 192.231.3.0

hwaddress ether 8a:02:fd:bb:05:be
```
- **Revolte, Richter, Sein, dan Stark (Client)**
```
auto eth0
iface eth0 inet dhcp
```
## Question #1
>Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1. Lakukan konfigurasi sesuai dengan peta yang sudah diberikan. (1)

### Solution
Run the following commands on Heiter (DNS Server):
```sh
echo 'zone "riegel.canyon.i06.com" {
    type master;
    file "/etc/bind/sites/riegel.canyon.i06.com";
};

zone "granz.channel.i06.com" {
    type master;
    file "/etc/bind/sites/granz.channel.i06.com";
};

zone "1.231.192.in-addr.arpa" {
    type master;
    file "/etc/bind/sites/1.231.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/riegel.canyon.i06.com
cp /etc/bind/db.local /etc/bind/sites/granz.channel.i06.com
cp /etc/bind/db.local /etc/bind/sites/1.231.192.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.i06.com. root.riegel.canyon.i06.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.i06.com.
@       IN      A       192.231.4.1     ; IP Fern
www     IN      CNAME   riegel.canyon.i06.com.' > /etc/bind/sites/riegel.canyon.i06.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.i06.com. root.granz.channel.i06.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.i06.com.
@       IN      A       192.231.3.1     ; IP Lugner
www     IN      CNAME   granz.channel.i06.com.' > /etc/bind/sites/granz.channel.i06.com

echo 'options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 start
```
### Result

## Question #6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

```
wget -O '/var/www/granz.channel.i06.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.i06.com -d /var/www/
rm /var/www/granz.channel.i06.com
mv /var/www/modul-3 /var/www/granz.channel.i06.com
```
### Script

Configurate `nginx`
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.i06.com
ln -s /etc/nginx/sites-available/granz.channel.i06.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.i06.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.i06.com

service nginx restart
```
### Result

## Question 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Configure `Load Balancing` on the `Eisen` node as follows.
Reopen the DNS Server Node and point the domain to the IP `Load Balancer Eise`

### Script
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.i06.com. root.riegel.canyon.i06.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.i06.com.
@       IN      A       192.231.2.2    ; IP LB Eiken
www     IN      CNAME   riegel.canyon.i06.com.' > /etc/bind/sites/riegel.canyon.i06.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.i06.com. root.granz.channel.i06.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.i06.com.
@       IN      A       192.231.2.2    ; IP LB Eiken
www     IN      CNAME   granz.channel.i06.com.' > /etc/bind/sites/granz.channel.i06.com
```
Then return to the Eisen node and configure nginx as follows
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
    server 192.231.3.1;
    server 192.231.3.2;
    server 192.231.3.3;
}

server {
    listen 80;
    server_name granz.channel.i06.com www.granz.channel.i06.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```
After setup, run the following command on the Revolte client

```
ab -n 1000 -c 100 http://www.granz.channel.i06.com/
```

### Hasil


## NO 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut: 1. Nama Algoritma Load Balancer; 2. Report hasil testing pada Apache Benchmark; 3.Grafik request per second untuk masing masing algoritma.

### Script
Run the following command on the Revolte client
```
ab -n 200 -c 10 http://www.granz.channel.i06.com/ 
```

### Hasil


## NO 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

### Script
Run the following command on the Revolte client
```
ab -n 100 -c 10 http://www.granz.channel.i06.com/ 
```

### Hasil

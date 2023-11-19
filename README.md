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
### Results
<img width="472" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/527404d2-74c8-4bef-9e09-c1af9e2d2f36">

## Question #2
>Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

### Solution
Run the following commands on Himmel (DHCP Server):
```sh
echo 'subnet 192.231.1.0 netmask 255.255.255.0 {
}

subnet 192.231.2.0 netmask 255.255.255.0 {
}

subnet 192.231.3.0 netmask 255.255.255.0 {
    range 192.231.3.16 192.231.3.32;
    range 192.231.3.64 192.231.3.80;
    option routers 192.231.3.0;
}' > /etc/dhcp/dhcpd.conf
```

## Question #3 
>Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

### Solution
Continuing from the previous script, add the following:
```sh
echo 'subnet 192.231.1.0 netmask 255.255.255.0 {
}

subnet 192.231.2.0 netmask 255.255.255.0 {
}

subnet 192.231.3.0 netmask 255.255.255.0 {
    range 192.231.3.16 192.231.3.32;
    range 192.231.3.64 192.231.3.80;
    option routers 192.231.3.0;
}

subnet 192.231.4.0 netmask 255.255.255.0 {
    range 192.231.4.12 192.231.4.20;
    range 192.231.4.160 192.231.4.168;
    option routers 192.231.4.0;
} ' > /etc/dhcp/dhcpd.conf
```

## Question #4 
>Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

### Solution
Continuing from the previous script, add configurations ``option broadcast-address`` and ``option domain-name-server`` so that the DNS server can be utilized:
```sh 
subnet 192.231.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.231.3.255;
    option domain-name-servers 192.231.1.2;
    ...
}

subnet 192.231.4.0 netmask 255.255.255.0 {
    option broadcast-address 192.231.4.255;
    option domain-name-servers 192.231.1.2;
} ' > /etc/dhcp/dhcpd.conf
```
service isc-dhcp-server start

Next, run the following commands on Aura (Router - DHCP Relay):
```sh
echo '# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.231.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay start 
```
Next, uncomment ``net.ipv4.ip_forward=1`` in file ``/etc/sysctl.conf``.
Finally, restart all client nodes.

### Results for Questions 2-4
<img width="702" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/33a9026a-9e09-4125-8759-7d7c708eec2b">
<img width="699" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/30f2d64e-c72d-46cb-b549-4777b3a00e66">

## Question #5
>Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit.

### Solution
We need to use the``default-lease-time`` and ``max-lease-time`` functions, where the unit is in seconds.

Since ``switch3`` can lease an IP for ``3 minutes`` and ``Switch4`` can lease an IP for ``12 minutes,`` on ``Switch3``, it requires ``180 seconds``, and on ``Switch4``, it requires ``720 seconds``. The ``max-lease-time`` is ``96 minutes``, which becomes ``5760 seconds``.

Next, we need to add some new configurations to set the leasing time on switch3 and switch4 according to the given rules. We can execute the following command on Himmel (DHCP Server).
```sh
echo 'subnet 192.231.1.0 netmask 255.255.255.0 {
}

subnet 192.231.2.0 netmask 255.255.255.0 {
}

subnet 192.231.3.0 netmask 255.255.255.0 {
    range 192.231.3.16 192.231.3.32;
    range 192.231.3.64 192.231.3.80;
    option routers 192.231.3.0;
    option broadcast-address 192.231.3.255;
    option domain-name-servers 192.231.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.231.4.0 netmask 255.255.255.0 {
    range 192.231.4.12 192.231.4.20;
    range 192.231.4.160 192.231.4.168;
    option routers 192.231.4.0;
    option broadcast-address 192.231.4.255;
    option domain-name-servers 192.231.1.2;
    default-lease-time 720;
    max-lease-time 5760;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
### Results
<img width="405" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/6a5e58df-ac25-4685-b05b-505818c11722">
<img width="420" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/96d7114a-33f2-43f5-acf1-ebbed3c01bbb">

## Question #6
>Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

### Solution
Run the following on all PHP Workers:
```
wget -O '/var/www/granz.channel.i06.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.i06.com -d /var/www/
rm /var/www/granz.channel.i06.com
mv /var/www/modul-3 /var/www/granz.channel.i06.com
```

Configurate `nginx` on all PHP Workers:
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
### Results
<img width="534" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/236dbd61-67f9-43a0-9e4c-e8a87384e90c">

## Question 7
>Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

### Solution
Reopen the DNS Server Node and point the domain to the IP `Load Balancer Eise`:

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
Next, configure `Load Balancing` on the `Eisen` node as follows:
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

### Results
<img width="481" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/0bfc3cec-59f4-453d-8f6f-2eca79d72d7f">


## Question #8
>Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut: 1. Nama Algoritma Load Balancer; 2. Report hasil testing pada Apache Benchmark; 3.Grafik request per second untuk masing masing algoritma.

### Solution
Run the following command on the Revolte client:
```
ab -n 200 -c 10 http://www.granz.channel.i06.com/ 
```
### Results
- **Round Robin**
<img width="1512" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/b6a0be27-e013-4ec3-b37d-536c4ec3477e">

- **Least Connection**
<img width="1512" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/1bf7bc73-2718-4cf1-b501-de7e7b910c96">

- **IP Hash**
<img width="1512" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/bf29ccee-5fa1-4b8c-b388-1990dc640032">

- **Generic Hash**
<img width="1512" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/09fe8591-135a-45ad-9e84-119d698acb13">

## Question #9
>Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

### Solution
Run the following command on the Revolte client
```
ab -n 100 -c 10 http://www.granz.channel.i06.com/ 
```

### Results
- **3 Workers**
<img width="487" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/6afc4a36-76c3-4d46-bcfa-799f9dbf534b">

- **2 Workers**
<img width="481" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/0cfcc1cf-557e-446e-81d4-2cd29bd1e4f3">

- **1 Worker**
<img width="481" alt="image" src="https://github.com/kaylaanglc/Jarkom-Modul-3-I06-2023/assets/116704203/174a96e6-9af3-4f6d-9156-1f1a1ae6854a">


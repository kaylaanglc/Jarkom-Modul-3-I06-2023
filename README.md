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



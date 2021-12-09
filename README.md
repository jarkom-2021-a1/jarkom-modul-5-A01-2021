# jarkom-modul-5-A01-2021

# Anggota kelompok A1 :
- 05111940000083 Fajar Satria
- 05111940000124 Gerry Sihaj
- 05111940000130 Adrian

# Persiapan
## Gambar Subnetting VLSM
xxx

## Gambar Tree
xxx

Subnet | Jumlah IP | Netmask | NID | Subnetmask
--- | --- | --- | --- | ---
A1  	|2        |/30    | 192.169.0.0     | 255.255.255.252     
A2	    |2        |/30    | 192.169.0.4     | 255.255.255.252
A3	    |201      |/24    | 192.169.1.0     | 255.255.255.0
A4	    |301      |/23    | 192.169.2.0     | 255.255.254.0
A5	    |101      |/25    | 192.169.0.128   | 255.255.255.128
A6	    |701      |/22    | 192.169.4.0     | 255.255.252.0
A7	    |4        |/29    | 192.169.0.16    | 255.255.255.248
A8	    |4        |/29    | 192.169.0.24    | 255.255.255.248
Total	|1316     |/21    | -               | -

## Setting ip pada gns3 :
Pada ```Edit Network Connection``` setting ip seperti berikut : 
Pada Client :
1. FOOSHA (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
        address 192.169.0.1
        netmask 255.255.255.252

auto eth2
iface eth2 inet static
        address 192.169.0.5
        netmask 255.255.255.252
```
2. WATER7 (DHCP Relay)
```
auto eth0
iface eth0 inet static
        address 192.169.0.2
        netmask 255.255.255.252
        gateway 192.169.0.1

auto eth1
iface eth1 inet static
        address 192.169.0.17
        netmask 255.255.255.248

auto eth2
iface eth2 inet static
        address 192.169.0.129
        netmask 255.255.255.128

auto eth3
iface eth3 inet static
        address 192.169.4.1
        netmask 255.255.252.0
```
3. GUANHAO (DHCP Relay)
```
auto eth0
iface eth0 inet static
        address 192.169.0.6
        netmask 255.255.255.252
        gateway 192.169.0.5

auto eth1
iface eth1 inet static
        address 192.169.0.25
        netmask 255.255.255.248

auto eth2
iface eth2 inet static
        address 192.169.2.1
        netmask 255.255.254.0

auto eth3
iface eth3 inet static
        address 192.169.1.1
        netmask 255.255.255.0
```
4. DORIKI (DNS Server)
```
auto eth0
iface eth0 inet static
       address 192.169.0.18
       netmask 255.255.255.248
       gateway 192.169.0.17
```
5. JIPANGU (DHCP Server)
```
auto eth0
iface eth0 inet static
       address 192.169.0.19
       netmask 255.255.255.248
       gateway 192.169.0.17
```
6. JORGE (Web Server)
```
auto eth0
iface eth0 inet static
       address 192.169.0.26
       netmask 255.255.255.248
       gateway 192.169.0.25
```
7. MAINGATE (Web Server)
```
auto eth0
iface eth0 inet static
       address 192.169.0.27
       netmask 255.255.255.248
       gateway 192.169.0.25
```
8. BLUENO (sebagai Client)
```
auto eth0
iface eth0 inet dhcp
```
9. CIPHER (sebagai Client)
```
auto eth0
iface eth0 inet dhcp
```
10. ELENA (sebagai Client)
```
auto eth0
iface eth0 inet dhcp
```
11. FUKUROU (sebagai Client)
```
auto eth0
iface eth0 inet dhcp
```

## Routing :
- Foosha
```
route add -net 192.169.0.16 netmask 255.255.255.248 gw 192.169.0.2
route add -net 192.169.0.128 netmask 255.255.255.128 gw 192.169.0.2
route add -net 192.169.4.0 netmask 255.255.252.0 gw 192.169.0.2
route add -net 192.169.0.24 netmask 255.255.255.248 gw 192.169.0.6
route add -net 192.169.2.0 netmask 255.255.254.0 gw 192.169.0.6
route add -net 192.169.1.0 netmask 255.255.255.0 gw 192.169.0.6
```

## DHCP Server :
- Jipangu
Langkah - langkah :
1. Install isc-dhcp-server ```apt-get install isc-dhcp-server -y```
2. Edit ```/etc/default/isc-dhcp-server``` seperti berikut :
```
INTERFACES="eth0"
```
3. Edit ```/etc/dhcp/dhcpd.conf``` seperti berikut :
```
subnet 192.169.0.16 netmask 255.255.255.248 {
}
subnet 192.169.0.128 netmask 255.255.255.128 {
    range 192.169.0.130 192.169.0.254;
    option routers 192.169.0.129;
    option broadcast-address 192.169.0.255;
    option domain-name-servers 192.169.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.169.4.0 netmask 255.255.252.0 {
    range 192.169.4.2 192.169.4.254;
    option routers 192.169.4.1;
    option broadcast-address 192.169.4.255;
    option domain-name-servers 192.169.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.169.2.0 netmask 255.255.254.0 {
    range 192.169.2.2 192.169.2.254;
    option routers 192.169.2.1;
    option broadcast-address 192.169.2.255;
    option domain-name-servers 192.169.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.169.1.0 netmask 255.255.255.0 {
    range 192.169.1.2 192.169.1.254;
    option routers 192.169.1.1;
    option broadcast-address 192.169.1.255;
    option domain-name-servers 192.169.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
```
4. Restart isc-dhcp-server ```service isc-dhcp-server restart```

## DHCP Relay
- Foosha
Langkah - langkah :
1. Install isc-dhcp-relay ```apt-get install isc-dhcp-relay -y```
2. Edit ```/etc/default/isc-dhcp-relay```  seperti berikut :
```
SERVERS="192.169.0.19"
INTERFACES="eth1 eth2"
```
3. Restart isc-dhcp-relay ```service isc-dhcp-relay restart```

- Water7
Langkah - langkah :
1. Install isc-dhcp-relay ```apt-get install isc-dhcp-relay -y```
2. Edit ```/etc/default/isc-dhcp-relay```  seperti berikut :
```
SERVERS="192.169.0.19"
INTERFACES="eth0 eth1 eth2 eth3"
```
3. Restart isc-dhcp-relay ```service isc-dhcp-relay restart```

- Guanhao
Langkah - langkah :
1. Install isc-dhcp-relay ```apt-get install isc-dhcp-relay -y```
2. Edit ```/etc/default/isc-dhcp-relay```  seperti berikut :
```
SERVERS="192.169.0.19"
INTERFACES="eth0 eth1 eth2 eth3"
```
3. Restart isc-dhcp-relay ```service isc-dhcp-relay restart```

## DNS Server
- Doriki
Langkah - langkah :
1. Install bind9 ```apt-get install bind9 -y```
2. Edit ```/etc/bind/named.conf.options``` seperti berikut :
```
forwarders{
    192.168.122.1;
};
// dnssec-validation auto;
allow-query{any;};
```
3. Restart bind9 ```service bind9 restart```

# Soal 1
# Soal 2
# Soal 3
# Soal 4
# Soal 5
# Soal 6

# Kendala
1. xxx
2. xxx
# jarkom-modul-5-A01-2021

# Anggota kelompok A1 :
- 05111940000083 Fajar Satria
- 05111940000124 Gerry Sihaj
- 05111940000130 Adrian

# Persiapan
## Gambar Subnetting VLSM
![vlsmfix](https://user-images.githubusercontent.com/65168221/145680923-f8497b3f-9f8b-4dae-b9e0-7845b40becf0.png)

## Gambar Tree
![treeFIx](https://user-images.githubusercontent.com/65168221/145680929-3982f854-bbd4-49d2-82e1-ecc5de5c51df.png)

## IP Table
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
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

## Jawaban
**FOOSHA**
```
iptables -t nat -A POSTROUTING -s 192.169.0.0/16 -o eth0 -j SNAT --to-source (ip eth0)
```

Catatan: ip eth0 didapatkan dengan menjalankan command `ip a` pada FOOSHA.

**BLUENO**

![image](https://user-images.githubusercontent.com/72863287/145680731-50bfa237-50a7-4490-82ad-890ee7872a5a.png)

# Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

## Jawaban
**FOOSHA**
```
iptables -A FORWARD -p tcp --dport 80 -d 192.169.0.16/29 -i eth0 -j DROP
```

**JIPANGU dan DORIKI**

Install aplikasi netcat: `apt-get install netcat`.

**Testing**
- Pada JIPANGU dan DORIKI ketikkan: `nc -l -p 80`.

  ![image](https://user-images.githubusercontent.com/72863287/145680854-1f18141a-9d47-4bac-b17a-c3d7bd956a19.png)
  ![image](https://user-images.githubusercontent.com/72863287/145680885-299d9f1f-e1d0-4e48-aae4-133e48c8dd01.png)

- Pada FOOSHA ketikkan: `nmap -p 80 192.186.0.18` atau `nmap -p 80 192.186.0.19`.

  ![image](https://user-images.githubusercontent.com/72863287/145680862-a802c405-81fa-485e-ac6e-c49c86a04316.png)

# Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

## Jawaban
**JIPANGU dan DORIKI**
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

**Testing**

Lakukan ping ke JIPANGU atau DORIKI dari 4 client secara bersamaan.

![image](https://user-images.githubusercontent.com/72863287/145680962-1bce30ed-a475-4f03-bc17-ab8386ce1b79.png)
![image](https://user-images.githubusercontent.com/72863287/145681033-b4d9925a-677e-4b79-b4d6-2724c0a81166.png)
![image](https://user-images.githubusercontent.com/72863287/145681180-82bf4221-c116-4f90-91d5-e8c1dbda4278.png)
![image](https://user-images.githubusercontent.com/72863287/145681197-99175e32-8bf5-4e2c-be26-940c2ef57e8f.png)


# Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

## Jawaban
**DORIKI**
```
iptables -A INPUT -s 192.169.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.169.0.128/25 -j REJECT
iptables -A INPUT -s 192.169.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.169.4.0/22 -j REJECT
```

**Testing**
- Pada BLUENO ubah waktu: `date -s "3 DEC 2021 13:00:00"` dan ping DORIKI.

![image](https://user-images.githubusercontent.com/72863287/145681250-f8e7be07-9190-42c3-8b3f-7841700b6365.png)

- Pada CIPHER ubah waktu: `date -s "8 DEC 2021 13:00:00"` dan ping DORIKI.

![image](https://user-images.githubusercontent.com/72863287/145681297-a17ec188-026e-49b8-a878-bf8d543c7c39.png)


# Soal 5
Akses dari subnet Elena dan Fukurou hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

## Jawaban
**DORIKI**
```
iptables -A INPUT -s 192.169.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
iptables -A INPUT -s 192.169.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

**Testing**
- Pada ELENA ubah waktu: `date -s "8 DEC 2021 13:00:00"` dan ping DORIKI.

![image](https://user-images.githubusercontent.com/72863287/145681337-a082c817-905c-455c-a9c7-fbc2fd5cf039.png)

- Pada FUKUROU ubah waktu: `date -s "8 DEC 2021 18:00:00"` dan ping DORIKI.

![image](https://user-images.githubusercontent.com/72863287/145681375-48de4684-47ca-447c-bcc9-eeb50df75555.png)


# Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

## Jawaban
**GUANHAO**
```
iptables -t nat -A PREROUTING -p tcp -d 192.169.0.18 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.169.0.26:80
iptables -t nat -A PREROUTING -p tcp -d 192.169.0.18 --dport 80 -j DNAT --to-destination 192.169.0.27:80
iptables -t nat -A POSTROUTING -p tcp -d 192.169.0.26 --dport 80 -j SNAT --to-source 192.169.0.18:80
iptables -t nat -A POSTROUTING -p tcp -d 192.169.0.27 --dport 80 -j SNAT --to-source 192.169.0.18:80
```

**Testing**
- Pada JORGE, MAINGATE, ELENA, dan FUKUROU install aplikasi netcat: `apt-get install netcat`.
- Pada JORGE dan MAINGATE ketikkan: `nc -l -p 80`.
- Pada ELENA dan FUKUROU ketikkan: `nc 192.169.0.18 80`
- Ketikkan sembarang kata pada ELENA atau FUKUROU, nanti akan muncul pada JORGE atau MAINGATE.

![image](https://user-images.githubusercontent.com/72863287/145681535-294901a6-41db-4aaa-b198-0814fa84af37.png)

![image](https://user-images.githubusercontent.com/72863287/145681552-73798535-0826-43ce-a7d7-fa5c9792c67b.png)

![image](https://user-images.githubusercontent.com/72863287/145681562-b5019564-6d42-4347-a538-597d33e56d95.png)

![image](https://user-images.githubusercontent.com/72863287/145681587-bb7c8136-381f-48d1-801d-5e13af0efc7f.png)

# Kendala
1. Agak bingung saat tidak menggunakan masquerade

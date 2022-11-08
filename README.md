# Jarkom-Modul-3-D12-2022
Praktikum Jaringan Komputer Modul 2 (DHCP &amp; Proxy) 2022.

## Anggota Kelompok:

| Nama                           | NRP        | Nomor Yang dikerjakan |
| ------------------------------ | ---------- | --------------------- |
| Hafizh Mufid Darussalam        | 5025201093 | -               |
| Januar Evan Zuriel Banjarnahor | 5025201210 | - 		      |
| Alexander 			 | 5025201247 | -                 |

### Topologi

<img src="https://github.com/godlixe/Jarkom-Modul-3-D12-2022/blob/main/SS%20Modul%202/topologi.png?raw=true" width="600" height="400">

### Konfigurasi

- Ostania
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.21.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.21.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.21.3.1
	netmask 255.255.255.0
```

- SSS
```
auto eth0
iface eth0 inet static
	address 10.21.1.2
	netmask 255.255.255.0
	gateway 10.21.1.1
```

- Garden
```
auto eth0
iface eth0 inet static
	address 10.21.1.3
	netmask 255.255.255.0
	gateway 10.21.1.1
```

- WISE
```
auto eth0
iface eth0 inet static
	address 10.21.2.2
	netmask 255.255.255.0
	gateway 10.21.2.1
```

- Berlint
```
auto eth0
iface eth0 inet static
	address 10.21.2.3
	netmask 255.255.255.0
	gateway 10.21.2.1
```

- Westalis
```
auto eth0
iface eth0 inet static
	address 10.21.2.4
	netmask 255.255.255.0
	gateway 10.21.2.1
```

- Eden
```
auto eth0
iface eth0 inet static
	address 10.21.3.2
	netmask 255.255.255.0
	gateway 10.21.3.1
```

- NewstonCastle
```
auto eth0
iface eth0 inet static
	address 10.21.3.3
	netmask 255.255.255.0
	gateway 10.21.3.1
```

- KemonoPark
```
auto eth0
iface eth0 inet static
	address 10.21.3.4
	netmask 255.255.255.0
	gateway 10.21.3.1
```

### Soal 1

Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server (1)

### Jawaban

Karena WISE merupakan DNS Server, maka diinstall bind9 pada wise menggunakan perintah
```
apt-get update
apt-get install bind9 -y
```

Karena Westalis merupakan DHCP Server, maka diinstall isc-dhcp-server dengan menggunakan perintah
```
apt-get update
apt-get install isc-dhcp-server -y
```

Tidak lupa juga untuk menambahkan interface yang digunakan DHCP server pada file /etc/default/isc-dhcp-server sebagai berikut :
```
INTERFACES="eth0"
```

Dan karena Berlint merupakan proxy server, maka diinstall squid proxy dengan menggunakan perintah
```
apt-get update
apt-get install squid
```

### Soal 2

Ostania sebagai DHCP Relay (2)

### Jawaban

Karena Ostania merupakan DHCP Relay, maka diinstall isc-dhcp-relay pada wise menggunakan perintah

```
apt-get update
apt-get install isc-dhcp-relay -y
```

Setelah itu, konfigurasi pada file /etc/default/isc-dhcp-relay perlu ditambahkan pengaturan sebagai berikut :

```
SERVERS"10.21.2.4"
INTERFACES="eth1 eth2 eth3"
OPTIONS=""
```
Tidak lupa untuk merestart service isc-dhcp-relay

### Soal 3, 4, 5, dan 6

Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu:
1. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
2. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155 (3)
3. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85 (4)
4. Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut. (5)
5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit. (6)


### Jawaban

Pertama, dilakukan setup DHCP server dengan menambahkan `INTERFACES="eth0"` pada file /etc/default/isc-dhcp-server. Setelah itu, kita akan mengkonfigurasi tiap subnet pada DHCP server. Konfigurasi yang dibuat adalah sebagai berikut :

- Switch1
```
subnet 10.21.1.0 netmask 255.255.255.0 {
    range 10.21.1.50 10.21.1.88;
    range 10.21.1.120 10.21.1.155;
    option routers 10.21.1.1;
    option broadcast-address 10.21.1.255;
    option domain-name-servers 10.21.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}
```

- Switch3
```
subnet 10.21.3.0 netmask 255.255.255.0 {
    range 10.21.3.10 10.21.3.30;
    range 10.21.1.60 10.21.1.85;
    option routers 10.21.3.1;
    option broadcast-address 10.21.3.255;
    option domain-name-servers 10.21.2.2;
    default-lease-time 600;
    max-lease-time 6900;
```

- Switch2
```
subnet 10.21.2.0 netmask 255.255.255.0 {
	option routers 10.21.2.2;
}
```

- /etc/bind/named.conf.options
```
options {
	directory "/var/cache/bind";
		
	forwarders {
		192.168.122.1;
	};
	allow-query{any;};
	
	auth-nxdomain no;
	listen-on-v6 { any; };
}
```

Tidak lupa juga untuk memodifikasi file /etc/network/interfaces pada client SSS dan Eden agar mendapatkan lease IP dari DHCP server. Konfigurasi untuk kedua client sama dengan isi sebagai berikut :

```
auto eth0
iface eth0 inet dhcp
```

Setelah itu, client harus direstart dan setelah mendapat lease, hasilnya adalah sebagai berikut :

Hasil lease pada SSS :

<img src="https://github.com/godlixe/Jarkom-Modul-3-D12-2022/blob/main/SS%20Modul%202/SSS_lease.png?raw=true" width="600" height="200">

Hasil lease pada Garden :

<img src="https://github.com/godlixe/Jarkom-Modul-3-D12-2022/blob/main/SS%20Modul%202/Garden_lease.png?raw=true" width="600" height="200">


### Soal 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13 (7)

### Jawaban

Karena menggunakan fixed address, host Eden harus disetting pada file konfigurasi /etc/dhcp/dhcpd.conf di Westalis. Sebelum itu, hardware address Eden harus disimpan agar dhcp server dapat mengenali Eden dan memberikan fixed address. Konfigurasi adalah sebagai berikut :

```
host Eden {
	hardware ethernet ea:d9:c2:94:47:c6;
	fixed-address 10.21.3.13;
}
```
Setelah itu, konfigurasi pada file /etc/network/interfaces di Eden harus dikonfigurasi sebagai berikut :

```
auto eth0
iface eth0 inet dhcp
hwaddress ether ea:d9:c2:94:47:c6
```

Setelah itu, Eden harus direstart agar mendapat lease.

Hasil lease pada Eden :

<img src="https://github.com/godlixe/Jarkom-Modul-3-D12-2022/blob/main/SS%20Modul%202/Eden_lease.png?raw=true" width="600" height="200">

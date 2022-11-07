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

# Jarkom-Modul-3-D06-2022
# Laporan Resmi Praktikum Modul 3 Jarkom Kelompok D06
Dokumen laporan resmi praktikum modul 3 Jarkom Kelompok D06 Jaringan Komputer D Tahun 2022/2023

### Anggota Kelompok:
Nama Lengkap                | NRP
--------------------------- | -------------
Fian Awamiry Maulana        | 5025201035 
Rere Arga Dewanata          | 5025201078 
Muhamad Ridho Pratama       | 5025201186

## Daftar Isi  
- [Jarkom-Modul-3-D06-2022](#jarkom-modul-3-d06-2022)
- [Laporan Resmi Praktikum Modul 3 Jarkom Kelompok D06](#laporan-resmi-praktikum-modul-3-jarkom-kelompok-d06)
    - [Anggota Kelompok:](#anggota-kelompok)
  - [Daftar Isi](#daftar-isi)
  - [Konfigurasi Awal](#konfigurasi-awal)
    - [Konfigurasi Tiap-tiap node](#konfigurasi-tiap-tiap-node)
      - [Ostania](#ostania)
      - [WISE](#wise)
      - [Berlint](#berlint)
      - [Westalis](#westalis)
      - [SSS, Garden, Eden, NewstonCastle, KemonoPark](#sss-garden-eden-newstoncastle-kemonopark)
  - [Soal 1](#soal-1)
    - [Jawab](#jawab)
      - [Ostania](#ostania-1)
      - [WISE](#wise-1)
      - [Westalis](#westalis-1)
      - [Berlint](#berlint-1)
  - [Soal 2](#soal-2)
    - [Jawab](#jawab-1)
  - [Soal 3](#soal-3)
    - [Jawab](#jawab-2)
    - [Jawab](#jawab-3)
      - [Westalis](#westalis-2)
  - [Soal 4](#soal-4)
    - [Jawab](#jawab-4)
  - [Soal 5](#soal-5)
    - [Jawab](#jawab-5)
  - [Soal 6](#soal-6)
    - [Jawab](#jawab-6)
  - [Soal 7](#soal-7)
    - [Jawab](#jawab-7)
  - [Soal 8](#soal-8)
    - [Jawab](#jawab-8)
  - [Soal 9](#soal-9)
    - [Jawab](#jawab-9)
  - [Soal 10](#soal-10)
    - [Jawab](#jawab-10)
  - [Soal 11](#soal-11)
    - [Jawab](#jawab-11)
  - [Soal 12](#soal-12)
    - [Jawab](#jawab-12)
## Konfigurasi Awal

  {Gambar Topologi}
  Loid bersama Franky berencana membuat peta tersebut.

  ### Konfigurasi Tiap-tiap node

  Berikut adalah konfigurasi tiap node di GNS3:

  #### Ostania
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
      address 10.18.1.1
      netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
      address 10.18.2.1
      netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
      address 10.18.3.1
      netmask 255.255.255.0
  ```

  #### WISE
  ```
  auto eth0
  iface eth0 inet static
      address 10.18.2.2
      netmask 255.255.255.0
      gateway 10.18.2.1
  ```

  #### Berlint
  ```
  auto eth0
  iface eth0 inet static
      address 10.18.2.3
      netmask 255.255.255.0
      gateway 10.18.2.1
  ```

  #### Westalis
  ```
  auto eth0
  iface eth0 inet static
      address 10.18.2.4
      netmask 255.255.255.0
      gateway 10.18.2.1
  ```

  #### SSS, Garden, Eden, NewstonCastle, KemonoPark
  ```
  auto eth0
  iface eth0 inet dhcp
  ```


## Soal 1   

Loid bersama Franky berencana membuat peta tersebut dengan kriteria sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server

### Jawab

#### Ostania

1. Menjalankan command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.18.0.0/16` agar bisa konek ke luar (internet).

2. Pada WISE, Berlint, dan Westalis, menjalankan command `echo nameserver 192.168.122.1 > /etc/resolv.conf` agar dapat terhubung ke luar.

#### WISE

3. Pada WISE, jalankan command `apt-get update` dan `apt-get install bind9 -y` untuk meng-install bind9

#### Westalis

4. Pada Westalis, install isc-dhcp-server dengan menjalankan command `apt-get update` lalu `apt-get install isc-dhcp-server -y`
5. Lalu, setting variabel `INTERFACES` pada file `/etc/default/isc-dhcp-server` dengan menambahkan `eth0`
{screenshot file /etc/default/isc-dhcp-server}

#### Berlint

6. Pada Berlint, install `squid` dengan menjalankan command `apt-get update` lalu `apt-get install squid -y`


## Soal 2

Ostania sebagai DHCP Relay

### Jawab

1. Pada node `Ostania`, install isc-dhcp-relay dengan menjalankan command `apt-get update` lalu `apt-get install isc-dhcp-relay -y`
2. Edit file `/etc/default/isc-dhcp-relay` dengan menambahkan `SERVERS="IP Westalis"`, serta tambahkan juga pada `INTERFACES="eth1 eth2 eth3"`
{screenshot file /etc/default/isc-dhcp-relay pada Ostania}
3. Jalankan command `service isc-dhcp-relay restart`

## Soal 3

Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu:

1. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
   
### Jawab

Pastikan konfigurasi pada setiap node client (SSS, Garden, Eden, NewstonCastle, KemonoPark) di GNS3 menggunakan `iface eth0 inet dhcp`

2. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

### Jawab

#### Westalis

1. Pada `Westalis`, dit file `/etc/dhcp/dhcpd.conf` dengan menambahkan:
   ```
   subnet 10.18.1.0 netmask 255.255.255.0 {
      range 10.18.1.50 10.18.1.88;
      range 10.18.1.120 10.18.1.155;
      option routers 10.18.1.1;
      option broadcast-address 10.18.1.255;
      option domain-name-servers 10.18.2.2;
      default-lease-time 360;
      max-lease-time 7200;
   }
   ```

  {screenshot file /etc/dhcp/dhcpd.conf di Westalis}

2. Jalankan command `service isc-dhcp-server restart`

## Soal 4

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85 (4)

### Jawab

1. Pada `Westalis`, edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan:
   ```
   subnet 10.18.3.0 netmask 255.255.255.0 {
      range 10.18.3.10 10.18.3.30;
      range 10.18.3.60 10.18.3.85;
      option routers 10.18.3.1;
      option broadcast-address 10.18.3.255;
      option domain-name-servers 10.18.2.2;
      default-lease-time 360;
      max-lease-time 7200;
   }
   ```

   {screenshot file /etc/dhcp/dhcpd.conf di Westalis}

2. Jalankan command `service isc-dhcp-server restart`

## Soal 5  
Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.  

### Jawab  
1. Pada WISE, edit file `/etc/bind/named.conf.options` dengan menambahkan

```
    forwarders {
        "192.168.122.1";
    };

    allow-query{any;};
```

dan mengkomen bagian

```
    // dnssec-validation auto;
```  

   {screenshot file /etc/dhcp/dhcpd.conf.options di Westalis}

2. kemudian jalankan command `service bind9 restart`  
3. Pada Westalis, edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan baris `option domain-name-servers "10.18.2.2"` pada `subnet 10.18.1.0` dan `subnet 10.18.3.0`  
## Soal 6  
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.  

### Jawab  
Pada Westalis, edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan baris ini pada `subnet 10.18.1.0`

```
        default-lease-time 300;
        max-lease-time 6900;
```


dan menambahkan baris ini pada `subnet 10.18.3.0`

```
        default-lease-time 600;
        max-lease-time 6900;
```  

{screenshot file /etc/dhcp/dhcpd.conf di Westalis}

## Soal 7  
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13  

### Jawab  
1. Pada Westalis, edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan baris ini

```
    host Eden {
        hardware ethernet "hardware address Eden";
        fixed-address 10.18.3.13;
    }
```  

{screenshot file /etc/dhcp/dhcpd.conf di Westalis}

2. Lalu jalankan command `service isc-dhcp-server restart`  
3. Kemudian tambahkan `hwaddress ether "hardware address Eden"` pada `/etc/network/interfaces` agar hwaddress tidak berubah-ubah ketika project direstart atau diexport

{screenshot file /etc/network/interfaces di Eden}

**Pada Proxy Server di Berlint, Loid berencana untuk mengatur bagaimana Client dapat mengakses internet. Artinya setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Adapun kriteria pengaturannya adalah sebagai berikut:**      

## Soal 8  
Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)  

### Jawab 

## Soal 9  
Adapun pada hari dan jam kerja sesuai nomor (8), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)  

### Jawab  


## Soal 10  
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)  

### Jawab  


## Soal 11  
Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)  

### Jawab  


## Soal 12  
Setelah diterapkan, ternyata peraturan nomor (11) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur  

### Jawab  




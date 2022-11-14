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
  - [Persiapan Soal 8 - 12](#persiapan-soal-8---12)
    - [Jawab](#jawab-8)
  - [Soal 8](#soal-8)
    - [Jawab](#jawab-9)
      - [Percobaan pada klien](#percobaan-pada-klien)
  - [Soal 9](#soal-9)
    - [Jawab](#jawab-10)
      - [Pengaturan di WISE](#pengaturan-di-wise)
      - [Pengaturan di Berlint](#pengaturan-di-berlint)
      - [Akses dengan klien](#akses-dengan-klien)
  - [Soal 10](#soal-10)
    - [Jawab](#jawab-11)
  - [Soal 11](#soal-11)
    - [Jawab](#jawab-12)
      - [Perbandingan kecepatan](#perbandingan-kecepatan)
  - [Soal 12](#soal-12)
    - [Jawab](#jawab-13)
## Konfigurasi Awal

  ![Gambar Topologi](https://user-images.githubusercontent.com/70679432/201629687-d3f723e0-f585-4c25-b47a-52c0d1a14943.jpg)  
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
![Soal1_Westalis_Screenshot isc-dhcp-server](https://user-images.githubusercontent.com/70679432/201629845-8ae4962e-ac3e-479a-90b8-5fc7de462bfe.jpg)  

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
      default-lease-time 300;
      max-lease-time 6900;
   }
   ```

  ![Soal3_Westalis_SS dhcpd conf](https://user-images.githubusercontent.com/70679432/201629960-e29ca118-fdbc-4a5d-b5e2-2ea3354e6a93.jpg)

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
      default-lease-time 600;
      max-lease-time 6900;
   }
   ```
  ![Soal4_Westalis_SS dhcpd conf](https://user-images.githubusercontent.com/70679432/201630364-9c2474df-e55a-4ecf-84ea-60115a625425.jpg)  
  
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

   ![Soal5_WISE_SS named conf options](https://user-images.githubusercontent.com/70679432/201630457-5eb50174-63f3-4140-b788-e3ae4d2ab736.jpg)  

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

![Soal6_Westalis_SS dhcpd conf](https://user-images.githubusercontent.com/70679432/201630530-7c98cf6e-6426-4270-8513-40facf056236.jpg)
d2ab736.jpg)  

## Soal 7  
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13  

### Jawab
1. Di Eden, cek hardware address-nya terlebih dahulu dengan menjalankan command `ip a` lalu salin hasilnya.
2. Pada Westalis, edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan baris ini

```
    host Eden {
        hardware ethernet "hardware address Eden";
        fixed-address 10.18.3.13;
    }
```  

![Soal7_Westalis_SS dhcpd conf](https://user-images.githubusercontent.com/70679432/201630567-19dbad2c-2323-4389-930c-253e933573c3.jpg)  

3. Lalu jalankan command `service isc-dhcp-server restart`  
4. Kemudian, di Eden, tambahkan `hwaddress ether <hardware address Eden>` pada `/etc/network/interfaces` agar hwaddress tidak berubah-ubah ketika project di-restart atau di-export

![Soal7_Eden_SS interface](https://user-images.githubusercontent.com/70679432/201630614-a02f3506-c384-4422-947c-95fb695d1a0e.jpg)

## Persiapan Soal 8 - 12

Pada Proxy Server di Berlint, Loid berencana untuk mengatur bagaimana Client dapat mengakses internet. Artinya setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Adapun kriteria pengaturannya adalah sebagai berikut:

### Jawab

  Pada setiap client yang ingin dijadikan proxy client, update variabel `http_proxy` dan `https_proxy` menjadi mengarah ke Berlint dengan port 8080
    ```
    export http_proxy="http://10.18.2.3:8080"
    export https_proxy="10.18.2.3:8080"
    ```
  

## Soal 8

Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)

### Jawab
1. Pada Berlint, ubah file default squid.conf di `/etc/squid/squid.conf` menjadi seperti berikut:
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager
   
   # --- Masukkan allow/deny di sini ---


   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```
2. Masih di squid.conf, tambahkan acl entry dan access entry untuk memenuhi kriteria soal
   ```
   acl working_time time MTWHF 08:00-17:00
   http_access allow !working_time
   ```

   sehingga isi file squid.conf menjadi seperti ini:
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   acl working_time time MTWHF 08:00-17:00

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager
   
   # --- Masukkan allow/deny di sini ---
   http_access allow !working_time
   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```
3. Restart squid dengan `service squid restart`

#### Percobaan pada klien

- Berikut adalah tampilan saat mengakses **http://example.com** di hari dan jam kerja
   ![Soal8_SSS_Akses example ketika jam kerja](https://user-images.githubusercontent.com/70679432/201637116-37679a16-5a09-4b57-81e8-088844071b04.jpg)  
- Berikut adalah tampilan saat mengakses **http://example.com** di luar hari dan jam kerja
   ![Soal8_SSS_Akses example diluar jam kerja](https://user-images.githubusercontent.com/70679432/201637138-85f557a7-a9b0-48a6-b20f-6a2ca8c25cef.jpg)

## Soal 9  
Adapun pada hari dan jam kerja sesuai nomor (8), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)  

### Jawab

#### Pengaturan di WISE

1. Pertama, buat IP tujuan untuk dua domain tersebut terlebih dahulu pada WISE. Karena IP tujuan dibebaskan, di sini kami mengarahkan dua domain tersebut ke IP WISE
2. Buat direktori `/etc/bind/wise`
3. Copy file `/etc/bind/db.local` menjadi file `/etc/bind/wise/loid-work.com` dan `/etc/bind/wise/franky-work.com`
4. Edit kedua file tersebut
   - **file `/etc/bind/wise/loid-work.com`**:
      ```
      ;
      ; BIND data file for local loopback interface
      ;
      $TTL    604800
      @       IN      SOA     loid-work.com. root.loid-work.com. (
                              2022110701     ; Serial
                              604800         ; Refresh
                              86400          ; Retry
                              2419200        ; Expire
                              604800 )       ; Negative Cache TTL
      ;
      @       IN      NS      loid-work.com.
      @       IN      A       10.18.2.2 ; IP WISE
      @       IN      AAAA    ::1
      ```

   - **file `/etc/bind/wise/franky-work.com`**:
      ```
      ;
      ; BIND data file for local loopback interface
      ;
      $TTL    604800
      @       IN      SOA     franky-work.com. root.franky-work.com. (
                              2022110701     ; Serial
                              604800         ; Refresh
                              86400          ; Retry
                              2419200        ; Expire
                              604800 )       ; Negative Cache TTL
      ;
      @       IN      NS      franky-work.com.
      @       IN      A       10.18.2.2 ; IP WISE
      @       IN      AAAA    ::1
      ```
5. Tambahkan kode berikut ke file `/etc/bind/named.conf.local`:
   ```
   zone "loid-work.com" {
      type master;
      file "/etc/bind/wise/loid-work.com";
   };

   zone "franky-work.com" {
      type master;
      file "/etc/bind/wise/franky-work.com";
   };
   ```
6. Restart bind9 dengan `service bind9 restart`

#### Pengaturan di Berlint

1. Tambahkan kode berikut ke file `/etc/squid/squid.conf`
   ```
   acl allowed_domain dstdomain loid-work.com franky-work.com
   http_access allow allowed_domain working_time
   ```

   sehingga file `/etc/squid/squid.conf` menjadi seperti berikut
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   acl working_time time MTWHF 08:00-17:00
   acl allowed_domain dstdomain loid-work.com franky-work.com

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager
   
   # --- Masukkan allow/deny di sini ---
   http_access allow allowed_domain working_time
   http_access allow !working_time
   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```
2. Restart squid dengan `service squid restart`

#### Akses dengan klien

1. Dengan menggunakan salah satu client, ini adalah tampilan saat mengakses loid-work.com dan franky-work.com pada jam kerja:
   - loid-work.com
      ![Soal9_SSS_ping loid-work](https://user-images.githubusercontent.com/70679432/201641498-e9227ea3-dcd4-4b94-ae45-3a5882a4fa0a.jpg)
      ![Soal9_SSS_lynx loid-work](https://user-images.githubusercontent.com/70679432/201641531-792c0742-6430-4203-9efb-dd8670d9ab5b.jpg)
      
   - franky-work.com
     ![Soal9_SSS_ping franky-work](https://user-images.githubusercontent.com/70679432/201641586-b3f4e3a7-55ac-454a-9589-481bfacac560.jpg)
     ![Soal9_SSS_lynx franky-work](https://user-images.githubusercontent.com/70679432/201641615-06dd9005-33ec-4d6d-844e-dec9c3254f31.jpg)
     
## Soal 10  
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Jawab  

1. Pada Berlint, tambahkan kode berikut ke `/etc/squid/squid.conf`
   ```
   http_access deny !SSL_ports !working_time
   ```

   sehingga file `/etc/squid/squid.conf` menjadi:
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   acl working_time time MTWHF 08:00-17:00
   acl allowed_domain dstdomain loid-work.com franky-work.com

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager
   
   # --- Masukkan allow/deny di sini ---
   http_access allow allowed_domain working_time
   http_access allow SSL_ports !working_time
   http_access deny !SSL_ports !working_time
   http_access allow !working_time
   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```
2. Restart squid dengan `service squid restart`

3. Dengan menggunakan client, saat akses internet dibuka,
   - Berikut adalah tampilan saat mengakses website dengan protokol http:
      {screenshot lynx akses http://example.com}
   - Berikut adalah tampilan saat mengakses website dengan protokol https:
      {screenshot `curl https://example.com -k`}

## Soal 11  

Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)  

### Jawab  

1. Pada Berlint, tambahkan kode berikut ke `/etc/squid/squid.conf`
   ```
   acl only128kusers src 10.18.1.0/24 10.18.3.13

   delay_pools 1
   delay_class 1 3
   delay_access 1 allow only128kusers
   delay_access 1 deny all
   delay_parameters 1 none none 16000/16000
   ```

   sehingga file `/etc/squid/squid.conf` menjadi:
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   acl working_time time MTWHF 08:00-17:00
   acl allowed_domain dstdomain loid-work.com franky-work.com
   acl only128kusers src 10.18.1.0/24 10.18.3.13

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager
   
   # --- Masukkan allow/deny di sini ---
   http_access allow allowed_domain working_time
   http_access allow SSL_ports !working_time
   http_access deny !SSL_ports !working_time
   http_access allow !working_time

   delay_pools 1
   delay_class 1 3
   delay_access 1 allow only128kusers
   delay_access 1 deny all
   delay_parameters 1 none none 16000/16000
   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```
2. Restart squid dengan `service squid restart`

#### Perbandingan kecepatan

Pada client, kita tes kecepatan internetnya:

- Berikut adalah tampilan kecepatan internet saat mengunduh file sebesar 100MB saat tidak menggunakan proxy
   {screenshot donlot pake wget}

- Berikut adalah tampilan kecepatan internet saat mengunduh file sebesar 100MB saat menggunakan proxy
   {screenshot donlot pake wget}

## Soal 12

Setelah diterapkan, ternyata peraturan nomor (11) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur  

### Jawab

1. Pada Berlint, ubah file `/etc/squid/squid.conf` menjadi seperti berikut
   ```
   visible_hostname Berlint
   http_port 8080

   acl SSL_ports port 443
   acl Safe_ports port 80          # http
   acl Safe_ports port 21          # ftp
   acl Safe_ports port 443         # https
   acl Safe_ports port 70          # gopher
   acl Safe_ports port 210         # wais
   acl Safe_ports port 1025-65535  # unregistered ports
   acl Safe_ports port 280         # http-mgmt
   acl Safe_ports port 488         # gss-http
   acl Safe_ports port 591         # filemaker
   acl Safe_ports port 777         # multiling http
   acl CONNECT method CONNECT

   acl working_time time MTWHF 08:00-17:00
   acl allowed_domain dstdomain loid-work.com franky-work.com
   acl only128kusers src 10.18.1.0/24 10.18.3.13
   acl free_time time SA

   http_access deny !Safe_ports
   http_access deny CONNECT !SSL_ports
   http_access allow localhost manager
   http_access deny manager

   # --- Masukkan allow/deny di sini ---
   http_access allow allowed_domain working_time
   http_access allow SSL_ports !working_time
   http_access deny !SSL_ports !working_time
   http_access allow !working_time

   delay_pools 1
   delay_class 1 3
   delay_access 1 allow free_time only128kusers
   delay_access 1 deny all
   delay_parameters 1 none none 16000/16000
   # -----------------------------------

   http_access allow localhost
   http_access deny all
   ```

2. Restart squid dengan `service squid restart`

Pada client, kita tes kecepatan internetnya dengan menggunakan proxy:

- Berikut adalah tampilan kecepatan internet saat mengunduh file sebesar 100MB pada hari kerja
   {screenshot donlot pake wget}

- Berikut adalah tampilan kecepatan internet saat mengunduh file sebesar 100MB pada hari libur
   {screenshot donlot pake wget}

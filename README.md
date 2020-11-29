# Jarkom_Modul3_Lapres_T21

praktikan :

Achsan Noorsalam (05311840000021)

I Komang Aditya Mahadiharja (05311840000042)

# soal 1-6 :

Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya, (1) yaitu untuk **membuat topologi jaringan** demi kelancaran TA-nya dengan kriteria sebagai berikut:

@photosoal

Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan **SURABAYA** sebagai router, **MALANG** sebagai DNS Server, **TUBAN** sebagai DHCP server, serta **MOJOKERTO** sebagai Proxy server, dan UML lainnya sebagai client. 

Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri. 
Karena TUBAN jauh dari client, maka perlu adanya perantara agar bisa saling terhubung. (2) **SURABAYA** ditunjuk sebagai perantara **(DHCP Relay)** antara DHCP Server dan client.

Kriteria lain yang diminta Bu Meguri pada topologi jaringan tersebut adalah:

- Seluruh client **TIDAK DIPERBOLEHKAN** menggunakan konfigurasi IP Statis.

- (3) Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

- (4) Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

- (5) Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

- (6) Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan (6) client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

langkah-langkah

1. membuat file topologi.sh seperti pada berikut 

@phototopologi

lalu dijalankan dengan `bash topologi.sh`

2. lalu pada uml SURABAYA buka `nano /etc/sysctl.conf` dan hapus # pada kalimat net.ipv4.ip_forward=1 dan jalankan `sysctl -p`

3. konfigurasi interface dengan menggunakan `nano /etc/network/interfaces` pada semua uml kecuali uml client

@photosurabaya

@photomalang

@photomojokerto

@phototuban

4. lalu restart dengan `service networking restart`

5. lalu jalankan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada uml SURABAYA

6. lalu jalankan `apt-get update` pada UML SURABAYA , TUBAN , MOJOKERTO

7. setelah itu jalankan `apt-get install isc-dhcp-relay` pada uml SURABAYA lalu pada configurasi pertama masukkan `IP TUBAN` dan `ETH1 ETH2 ETH3` pada konfigurasi ke 2 
  ps. kosongkan kolom ke 3

@relay1

@relay2

@relay3

8. lalu buat dchp server pada uml TUBAN dengan mengetikkan `apt-get install isc-dhcp-server`

9. lalu buka `nano /etc/default/isc-dhcp-server` dan ubah pada kalimat paling bawah menjadi `INTERFACEsv4="eth0`

@interfaces

10.lalu buka `nano /etc/dhcp/dhcpd.conf` dan masukkan sebagai berikut 

@dhcp.conf

11. restart dhcp server dengan `service isc-dhcp-server restart`

12 lalu buka uml GRESIK, SIDOARJO, MADIUN, BANYUWANGI dan ubah `nano /etc/network/interfaces` menjadi seperti berikut :

`auto eth0

iface eth0 inet dhcp`

# soal 7-9

Bu Meguri adalah dosbing yang suka overthinking. Ia tidak ingin jaringan lokalnya terhubung ke internet secara langsung. Sehingga ia memberi tugas tambahan pada Anri untuk membuatkan Proxy sebagai penghubung jaringan lokal ke internet. Ada beberapa ketentuan yang harus dipenuhi dalam pembuatan Proxy ini.

Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:

-User : userta_yyy

-Password : inipassw0rdta_yyy

-Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

Anri sudah menjadwal pengerjaan TA-nya (8) setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut. Jadwal bimbingan dengan Bu Meguri adalah (9) setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA, (10) setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.

## langkah langkah

1. pada uml MOJOKERTO install squid dengan `apt-get install squid` dan cek status squid dengan `service squid status`

2. lalu install `apt-get install apache2-utils`

3. buat username dan password dengan `htpasswd -c /etc/squid/passwd userta_t21`(username yang diinginkan) setelah itu masukkan password (dalam soal ini `inipassw0rdta_t21`)

4. lalu buka konfigurasi squid dengan `nano /etc/squid/squid.conf` dan tulis seperti berikut,

@konfigurasisquid

5. lalu buka `nano /etc/squid/squid.conf` dan tulis waktu yang diinginkan sebagai berikut 

@acl

6. lalu restart squid dengan `service squid restart`

7. lalu untuk menjalankankannya buka setting pada perangkat anda pada bagian proxy dan ubah sebagai berikut,

@settingproxy

8. lalu buka browser baru dalam keadaan **incognito/private** dan coba mengakses monta.if.its.ac.id

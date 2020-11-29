# Jarkom_Modul3_Lapres_T21

soal :
Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya, (1) yaitu untuk membuat topologi jaringan demi kelancaran TA-nya dengan kriteria sebagai berikut:

@photosoal

Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan SURABAYA sebagai router, MALANG sebagai DNS Server, TUBAN sebagai DHCP server, serta MOJOKERTO sebagai Proxy server, dan UML lainnya sebagai client. 
Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri. 
Karena TUBAN jauh dari client, maka perlu adanya perantara agar bisa saling terhubung. (2) SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.

Kriteria lain yang diminta Bu Meguri pada topologi jaringan tersebut adalah:
Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
(3) Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
(4) Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
(5) Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
(6) Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan (6) client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.


1. membuat file topologi.sh seperti pada berikut 

@phototopologi

lalu dijalankan dengan `bash topologi.sh`

2. lalu pada uml SURABAYA buka `nano /etc/sysctl.conf` dan hapus # pada kalimat net.ipv4.ip_forward=1 dan jalankan `sysctl -p`

3. konfigurasi interface pada semua uml kecuali uml client

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

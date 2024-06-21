# FP-TKA

**Anggota kelompok C2**:
* Wira Samudra Siregar (5027231041)
* I Dewa Made Satya Raditya (5027231051)
* M. Syahmi Ash Shidqi (5027231085)

## Permasalahan
Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.
Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python: sentiment-analysis.py dengan spesifikasi yang telah diberikan. Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka. 
Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat.

## Rancangan Arsitektur dan Estimasi Harga
![image](https://github.com/wscregar/FP-TKA/assets/145766477/1efb985c-ea3d-4e38-87ff-d2d250551298)

![image](https://github.com/wscregar/FP-TKA/assets/145766477/3370b9bc-a204-4110-a560-f80ef6b05ba3)

## Implementasi Arsitektur Cloud

Worker1

Backend

Database


## Langkah-langkah awal

Membuat 2 Droplets, 1 untuk application dan 1 untuk Back-end di Digital Ocean

Membuat Load-Balancer dan disambungkan ke 1 application Droplets

Terakhir, membuat Database MongoDB

### Konfigurasi

Untuk droplet application:

`sudo apt-get install apache2`

masukkan `index.html` beserta `styles.css` di Resources yang diberikan

Gunakan command `systemctl restart apache2` untuk restart service apache2

Akses web menggunakan IP yang terbuat didroplets

## Hasil dari Implementasi


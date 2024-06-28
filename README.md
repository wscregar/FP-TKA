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
* Arsitektur

![image](https://github.com/wscregar/FP-TKA/assets/145766477/4dbf9be3-4ae5-42fd-833a-73cf65a73e76)

* Estimasi Harga

![image](https://github.com/wscregar/FP-TKA/assets/145766477/d7d92f0c-474d-49db-adfd-5188bb7eec4a)


## Implementasi Arsitektur Cloud

A. Setup
* Load Balancer

* Worker 1

* Worker 2

## Langkah-langkah awal

Membuat 2 Droplets, 1 untuk application dan 1 untuk Back-end di Digital Ocean

Membuat Load-Balancer dan disambungkan ke 1 application Droplets

Terakhir, membuat Database MongoDB

### Konfigurasi

Untuk droplet application:

- `sudo apt-get install apache2`

- masukkan `index.html` beserta `styles.css` di Resources yang diberikan

- Gunakan command `systemctl restart apache2` untuk restart service apache2

- Akses web menggunakan IP yang terbuat didroplets

# Hasil dari Implementasi
### Hasil Endpoin API 

![Gambar WhatsApp 2024-06-21 pukul 21 09 58_8925ba1f](https://github.com/wscregar/FP-TKA/assets/145766477/542cc8d5-8281-4f49-a8b1-bb29a5861bab)

### Hasil Locust 
![Gambar WhatsApp 2024-06-21 pukul 21 08 14_b9d86110](https://github.com/wscregar/FP-TKA/assets/145766477/f6e5f60c-aa77-4a8b-9834-6bdc9b0a5f60)

![Gambar WhatsApp 2024-06-21 pukul 21 08 44_ee4f62ab](https://github.com/wscregar/FP-TKA/assets/145766477/270c4c8e-d84f-4e78-94e6-4e6ba737e123)

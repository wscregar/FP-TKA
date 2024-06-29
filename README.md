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

![image](https://github.com/wscregar/FP-TKA/assets/145766477/c1459c3d-5f26-41c1-8143-6582d958502d)



# Implementasi Arsitektur Cloud

A. Setup
* Load Balancer
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/6f64c0cb-8c14-495a-b1a9-dd5779fda8de)


* Worker 1
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/b36d7b6d-37e8-4725-92aa-5faa0c290640)

* Worker 2
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/9d12bb01-d4c6-440a-b461-3b0b67bc5f2d)

#### Set up Worker 1 dan 2 (sebagai BackEnd dan FrontEnd)

- Mengambil Resource `git clone https://github.com/fuaddary/fp-tka.git`
- Masuk ke dalam direktori BE `cd fp-tka/Resources/BE`
- Set up database
```
sudo apt-get install gnupg curl

curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \ sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \ --dearmor

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update

sudo apt-get install -y mongodb-org

echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections

sudo systemctl start mongod
```

- Setup Python environment dan istall dependenciesnya
```
 sudo apt install -y python3-venv
 python3 -m venv venv
 source venv/bin/activate
 pip install flask flask-cors textblob pymongo
```

- Konfigurasikan index.html beserta styles.css
-  Nyalakan menggunakan command `source venv/bin/activate` dan `gunicorn -b 0.0.0.0:5000 -w 5 -k gevent --timeout 60 --graceful-timeout 60 sentiment_analysis:app`


# Hasil dari Implementasi
### Hasil Endpoin POST

![Screenshot_2024-06-29_193450 1](https://github.com/wscregar/FP-TKA/assets/145766477/c3ec641f-9194-4bf6-acf4-127d1bb86cc3)

![Gambar WhatsApp 2024-06-29 pukul 21 12 19_e7374095](https://github.com/wscregar/FP-TKA/assets/145766477/145ffb55-c9aa-4e69-b80d-dceaf20b1ea5)


### Hasil Sentiment Analysis

![Screenshot_2024-06-29_193227 1](https://github.com/wscregar/FP-TKA/assets/145766477/6b91894e-6d45-46cf-880f-6452f128bdf3)

![Screenshot_2024-06-29_193144 1](https://github.com/wscregar/FP-TKA/assets/145766477/5aa72273-3397-43bb-a991-30a8fa58cffd)

### Hasil Locust 

# Kesimpulan 


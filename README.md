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

## Konfigurasi

* Load Balancer
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/6f64c0cb-8c14-495a-b1a9-dd5779fda8de)


1. Update package list and install nginx, git, and python3-venv `sudo apt update` dan `sudo apt install -y nginx git python3-venv`
   
2. Set ulimit `ulimit -n 100000`

3. Mengambil Resource `git clone https://github.com/fuaddary/fp-tka.git`

4. Memindahkan default nginx configuration `sudo unlink /etc/nginx/sites-available/default`

5. Membuat konfigurasi file nginx baru untuk app `cat <<EOL | sudo tee /etc/nginx/sites-available/app`
```
 upstream backend_servers {
     # VM1
     server 68.183.231.98:5000;
     # VM2
     server 165.22.241.204:5000;
 }

 server {
     listen 80;
     server_name 159.223.64.29;  # Ganti dengan IP LB

     location / {
         # Aktifkan penggunaan cache
         proxy_cache my_cache;
         proxy_cache_valid 200 302 10m;
         proxy_cache_valid 404 1m;

         # Konfigurasi caching tambahan
         proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
         proxy_cache_lock on;
         proxy_cache_lock_timeout 5s;

         # Pengaturan proxy_pass dan header lainnya
         proxy_pass http://backend_servers;
         proxy_set_header Host \$host;
         proxy_set_header X-Real-IP \$remote_addr;
         proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
         proxy_set_header X-Forwarded-Proto \$scheme;

         # Header tambahan untuk mengidentifikasi status cache
         add_header X-Cached \$upstream_cache_status;
     }
 }
 EOL
```

6. Mengaktifkan konfig baru `sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled`

7. Mengedit nginx.conf file `cat <<EOL | sudo tee /etc/nginx/nginx.conf`
```
 user www-data;
 worker_processes 2;
 error_log /var/log/nginx/error.log warn;
 pid /var/run/nginx.pid;
 worker_rlimit_nofile 100000;

 events {
     worker_connections 4096;
 }

 http {
     include /etc/nginx/mime.types;
     default_type application/octet-stream;

     log_format main '\$remote_addr - \$remote_user [\$time_local] '
                     '"\$request" \$status \$body_bytes_sent '
                     '"\$http_referer" "\$http_user_agent" '
                     '"\$http_x_forwarded_for"';

     access_log /var/log/nginx/access.log main;

     sendfile on;
     tcp_nopush on;
     tcp_nodelay on;
     keepalive_timeout 65;
     types_hash_max_size 2048;

     # Konfigurasi cache
     proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m;
     proxy_temp_path /var/cache/nginx/temp;
     proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
     proxy_cache_lock on;
     proxy_cache_lock_timeout 5s;

     include /etc/nginx/conf.d/*.conf;
     include /etc/nginx/sites-enabled/*;
 }
 EOL
```

8. Restart nginx `sudo systemctl restart nginx`

10. masuk ke dalam direktori test `cd fp-tka/Resources/Test`

11. menjalankan locustfile.py dengan locust dengan python virtual environment

`python3 -m venv venv`
 
`source venv/bin/activate`
 
`pip install locust`
 
`locust -f locustfile.py`  



* Worker 1
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/b36d7b6d-37e8-4725-92aa-5faa0c290640)

* Worker 2
  ![image](https://github.com/wscregar/FP-TKA/assets/163504787/9d12bb01-d4c6-440a-b461-3b0b67bc5f2d)
  


#### Set up Worker 1 dan 2 (sebagai BackEnd dan FrontEnd)

1. Mengambil Resource `git clone https://github.com/fuaddary/fp-tka.git`
  
2. Masuk ke dalam direktori BE `cd fp-tka/Resources/BE`
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

3. Setup Python environment dan istall dependenciesnya
```
 sudo apt install -y python3-venv
 python3 -m venv venv
 source venv/bin/activate
 pip install flask flask-cors textblob pymongo
```

4. Konfigurasikan index.html beserta styles.css pada `/var/www/html`
  
5. Nyalakan menggunakan command `source venv/bin/activate` dan `gunicorn -b 0.0.0.0:5000 -w 5 -k gevent --timeout 60 --graceful-timeout 60 sentiment_analysis:app`


# Hasil dari Implementasi
### Hasil Endpoin POST

![Screenshot_2024-06-29_193450 1](https://github.com/wscregar/FP-TKA/assets/145766477/c3ec641f-9194-4bf6-acf4-127d1bb86cc3)

![Gambar WhatsApp 2024-06-29 pukul 21 12 19_e7374095](https://github.com/wscregar/FP-TKA/assets/145766477/145ffb55-c9aa-4e69-b80d-dceaf20b1ea5)


### Hasil Sentiment Analysis

![Screenshot_2024-06-29_193227 1](https://github.com/wscregar/FP-TKA/assets/145766477/6b91894e-6d45-46cf-880f-6452f128bdf3)

![Screenshot_2024-06-29_193144 1](https://github.com/wscregar/FP-TKA/assets/145766477/5aa72273-3397-43bb-a991-30a8fa58cffd)

### Hasil Locust 
1. Jumlah Request per seconds (RPS) maksimum yang dapat ditangani oleh server dengan durasi waktu load testing 60 detik
   ![Screenshot_2024-06-29_224858 1](https://github.com/wscregar/FP-TKA/assets/145766477/1091e3cf-1cd3-4bf8-9503-427cc78e3006)

2. Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 50 dan durasi waktu load testing 60 detik
   ![Screenshot_2024-06-29_224858 1](https://github.com/wscregar/FP-TKA/assets/145766477/d017e106-d421-4992-a191-741e4beee0c9)

3. Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 100 dan durasi waktu load testing 60 detik
   ![Screenshot_2024-06-29_230930 1](https://github.com/wscregar/FP-TKA/assets/145766477/5b4386a6-655e-42bc-9c83-81fdca6afa12)

4. Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 200 dan durasi waktu load testing 60 detik
   ![Screenshot_2024-06-29_231245 1](https://github.com/wscregar/FP-TKA/assets/145766477/30c62469-e2b8-4adf-862e-15f53708efe1)

5. Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 500 dan durasi waktu load testing 60 detik
   ![Screenshot_2024-06-29_231459 1](https://github.com/wscregar/FP-TKA/assets/145766477/a3183a61-a904-49f4-8bf5-2d1c248a1ce4)

# Kesimpulan 
Dari semua pengujian yang telah kami uji coba dengan spawn rate yang telah di tetapkan hasil failuresnya tetap 100% dan kami telah mencoba untuk memakai arsitektur yang berbeda tetapi tetap saja hasil failuresnya berada di 100%. Jadi kami belum berhasil memenuhi permintaan soal 

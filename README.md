# LAPORAN PRAKTIKUM PERTEMUAN 04
Nama:Theresia A.T Dasilva
Nim: 24231014
## DOCKER, CONTAINER, DAN NGINX

### A. Tujuan Praktikum

Praktikum ini bertujuan untuk memahami penggunaan Docker dalam menjalankan aplikasi berbasis container, mengenal perbedaan image dan container, serta mempelajari cara menjalankan web server Nginx menggunakan Docker.

### B. Langkah 1 – Pemeriksaan Sistem

**Sistem Operasi Host:** Ubuntu 24.04

**Versi Docker:**
(Isi berdasarkan hasil perintah `docker --version`)

**Kernel Host:**
(Isi berdasarkan hasil perintah `uname -r`)

Docker merupakan platform yang digunakan untuk menjalankan aplikasi dalam lingkungan container sehingga aplikasi dapat berjalan secara terisolasi.

### C. Langkah 3 – Pemeriksaan Sistem Operasi

**1. Pemeriksaan pada Host Ubuntu**

Perintah:
```bash
cat /etc/os-release
uname -r
```

**2. Pemeriksaan pada Container Alpine**

Perintah:
```bash
docker run --rm alpine:latest sh -c 'cat /etc/os-release && uname -r'
```

Hasil:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42c9be66-f4cf-4a0e-b64e-42b6ca757808" />


**Analisis:**

Ubuntu merupakan sistem operasi host yang digunakan untuk menjalankan Docker, sedangkan Alpine merupakan sistem operasi ringan yang digunakan di dalam container. Keduanya memiliki informasi distribusi yang berbeda, tetapi menggunakan kernel host yang sama.

### D. Langkah 5 – Menjalankan Web Server Nginx

Pada langkah ini dilakukan pengujian web server Nginx menggunakan Docker dengan port forwarding 8080.

Perintah:
```bash
docker run -d --name web -p 8080:80 nginx
```

Keterangan:
- `docker run`: Membuat dan menjalankan container.
- `-d`: Menjalankan container di background.
- `--name web`: Memberikan nama container web.
- `-p 8080:80`: Menghubungkan port 8080 host ke port 80 container.
- `nginx`: Menggunakan image Nginx.

**Hasil Pengujian:**

Halaman Nginx diakses melalui fitur Traffic / Ports pada Killercoda dengan memilih port 8080.

**Bukti Screenshot:**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/be5a1b59-b51e-415f-b272-13c998a8bdc1" />


### E. Langkah 7 – Pengujian Stop, Start, Remove, dan Run

Pengujian dilakukan untuk mengetahui perubahan kondisi container ketika dihentikan, dijalankan kembali, dihapus, dan dibuat ulang.

| Perintah | Hasil Pengujian |
|---|---|
| `docker stop web` | Menghentikan container sehingga halaman Nginx tidak dapat diakses. |
| `docker start web` | Menjalankan kembali container sehingga halaman Nginx dapat diakses kembali. |
| `docker rm web` | Menghapus container setelah dihentikan. |
| `docker run` | Membuat container baru berdasarkan image yang digunakan. |

**Analisis:**

Ketika container dihentikan menggunakan perintah stop, layanan Nginx tidak dapat diakses sementara. Setelah container dijalankan kembali menggunakan start, layanan dapat digunakan lagi.

Ketika container dihapus menggunakan rm, container tersebut tidak dapat dijalankan kembali dengan start. Untuk menjalankan layanan kembali, diperlukan pembuatan container baru menggunakan run.

Apabila perubahan halaman sebelumnya hanya disimpan di dalam writable layer container, perubahan tersebut tidak otomatis terbawa ke container baru.

### F. Langkah 8 – Perbandingan Ukuran Image Docker

Pemeriksaan dilakukan menggunakan perintah:

```bash
docker images
```

Tabel perbandingan:

| No. | Nama Image | Ukuran |
|---|---|---|
| 1 | nginx | (Isi hasil aktual) |
| 2 | nginx:alpine | (Isi hasil aktual) |
| 3 | nginx:1.26 | (Isi hasil aktual) |

**Analisis:**

Image Nginx dengan varian Alpine umumnya memiliki ukuran lebih kecil karena menggunakan basis Alpine Linux yang ringan. Sementara itu, image Nginx biasa dan versi 1.26 memiliki ukuran yang berbeda sesuai komponen dan dependensi yang disertakan.

Jawaban pertanyaan evaluasi 1–5
1. Apa yang dibuktikan dari kesamaan uname -r pada Alpine dan Ubuntu?

Kesamaan hasil uname -r membuktikan bahwa container tidak memiliki kernel sendiri. Container Alpine menggunakan kernel dari sistem operasi host Ubuntu. Berbeda dengan Virtual Machine (VM), yang memiliki sistem operasi dan kernel sendiri.

2. Apa perbedaan image dan container?

Image adalah cetakan atau paket aplikasi yang bersifat tetap, sedangkan container adalah image yang sedang dijalankan.

Contohnya, image nginx dapat digunakan untuk membuat beberapa container, seperti web, web-alpine, dan web-lama. Ketiganya dapat berjalan secara bersamaan menggunakan image dan versi yang berbeda.

3. Apa arti -p 8081:80?

Angka 8081 adalah port host, sedangkan angka 80 adalah port container. Ketiga container Nginx dapat menggunakan port 80 secara bersamaan karena masing-masing container memiliki lingkungan jaringan sendiri. Host mengaksesnya melalui port yang berbeda, yaitu 8080, 8081, dan 8082.

4. Mengapa perubahan halaman hilang setelah rm dan run, tetapi tidak setelah stop dan start?

Perintah stop hanya menghentikan container sementara sehingga perubahan halaman masih tersimpan. Ketika menggunakan start, container yang sama dijalankan kembali dan perubahan tetap ada.

Sebaliknya, rm menghapus container beserta perubahan pada writable layer-nya. Ketika menjalankan run kembali, Docker membuat container baru dari image asli sehingga halaman kembali menjadi halaman default Nginx.

5. Apa keuntungan Docker dibandingkan instalasi Nginx menggunakan apt?

Instalasi Nginx menggunakan apt membutuhkan proses pemasangan dan konfigurasi pada sistem operasi. Dengan Docker, Nginx dapat dijalankan langsung dari image yang sudah tersedia tanpa memasang Nginx secara manual di setiap server.

Keuntungan Docker adalah proses deployment lebih cepat, lingkungan aplikasi lebih konsisten, mudah dipindahkan, dan memudahkan tim menjalankan aplikasi yang sama di banyak server cloud.
### H. Kesimpulan

Berdasarkan praktikum yang telah dilakukan, dapat disimpulkan bahwa Docker mempermudah proses menjalankan aplikasi menggunakan container. Melalui praktikum ini, dapat dipahami cara memeriksa sistem operasi, menjalankan container Alpine dan Nginx, mengatur port forwarding, serta melakukan pengelolaan container menggunakan perintah Docker.

Selain itu, praktikum ini memberikan pemahaman mengenai perbedaan ukuran image Docker dan pentingnya penggunaan image yang sesuai dengan kebutuhan aplikasi.

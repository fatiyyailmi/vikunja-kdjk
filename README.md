<h1 align="center"><img src="https://vikunja.cloud/images/vikunja-logo.svg"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:

## Sekilas Tentang
[`^ kembali ke atas ^`](#)

Vikunja adalah aplikasi to-do open-source yang bisa di-self-host, yang dirancang untuk membantu individu maupun tim dalam mengelola tugas dan proyek secara fleksibel juga kolaboratif. Aplikasi ini menawarkan berbagai tampilan yang memudahkan pengaturan pekerjaan, sekaligus menjaga privasi dan kendali penuh atas data pengguna.

Vikunja memiliki dua komponen utama:
- **Frontend (Web UI)** — antarmuka berbasis browser untuk mengelola tugas.
- **Backend (API Server)** — menyimpan data dan menangani proses aplikasi.


## Instalasi
[`^ kembali ke atas ^`](#)

Sebelum instalasi, pastikan sudah menginstal:
- **Linux server** (misalnya Ubuntu 22.04 LTS)
- **Docker** & **Docker Compose**
- **Git**
- **Domain / localhost** untuk akses aplikasi
- **AWS EC2 Instance untuk hosting online**

#### Spesifikasi Minimum
- RAM: 1 GB
- Storage: 10 GB
- Internet: Koneksi stabil untuk update & akses web
- Port: 3456 (default Vikunja)

## Instalasi di Server
[`^ kembali ke atas ^`](#)
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose git -y
sudo systemctl enable --now docker
```

##  Maintenance (opsional)
[`^ kembali ke atas ^`](#)

Setting tambahan untuk maintenance secara periodik, misalnya:
- buat backup database tiap pekan
- hapus direktori sampah tiap hari
- dll


## Otomatisasi (opsional)
[`^ kembali ke atas ^`](#)

Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.


## Cara Pemakaian
[`^ kembali ke atas ^`](#)

Cara pemakaian Vikunja cukup mudah, dengan interface yang cukup praktis dan mudah dipahami. Berikut langkah-langkahnya.
1. Sebelum memulai, user akan diminta untuk Login atau Sign up.
<img width="2879" height="1536" alt="Screenshot 2025-10-16 234344" src="https://github.com/user-attachments/assets/015eb62b-da11-400c-b728-a3ba76c6d3fd" />

2. Setelah login berhasil, tampilan yang pertama keluar adalah dashboard. Dashboard memiliki lima menu utama yang terletak pada sidebar, yaitu **Overview, Upcoming, Projects, Labels, dan juga Teams**. Pada menu **Overview**, disajikan projek-projek terakhir yang dilihat dan juga tugas-tugas (tasks) yang sedang berjalan. Selain itu, user juga bisa langsung menambahkan task baru.
<img width="2872" height="1510" alt="image" src="https://github.com/user-attachments/assets/386e405f-dc03-4790-8111-3950077f4282" />

3. Untuk membuat task baru, user bisa tinggal mengetikkan judul task-nya dan klik tombol **Add**.
<img width="2182" height="240" alt="image" src="https://github.com/user-attachments/assets/f3d1e5f0-191c-483c-a385-ecbaad1d1f70" />

4. Ketika task tersebut diklik, akan muncul tampilan untuk menambahkan detail dari task tersebut. Pada bagian kanan, terdapat fitur-fitur yang dapat dipilih untuk mengorganisir task yang telah dibuat, seperti warna, label, prioritas, progress, tanggal mulai dan selesai.
<img width="2879" height="1523" alt="image" src="https://github.com/user-attachments/assets/27a3f02e-23ce-48e4-b538-9fe33ae79337" />
Berikut adalah contoh task yang telah ditambahkan beberapa detail: due date, progress, start date, end date, reminder, color.
<img width="1507" height="970" alt="image" src="https://github.com/user-attachments/assets/bb788ffb-db3e-4af4-b280-42b3d3788af2" />


6. Menu **Upcoming** akan menampilkan tasks terdekat sesuai *due date* yang telah ditentukan. Informasi-informasi mengenai kategori, label, dan progres yang telah ditambahkan sebelumnya juga akan terlihat disini. Ketika suatu task sudah selesai dikerjakan, user bisa menandainya dengan cara mencentang kotak checkbox yang terdapat pada sebelah kiri task.
<img width="2879" height="1231" alt="image" src="https://github.com/user-attachments/assets/1ae965ec-24d2-458a-b121-c386d5021668" />
Task yang telah dibuat juga dapat ditampilkan berdasarkan rentang waktu tertentu, dengan mengklik tombol select a date range.
<img width="1951" height="863" alt="image" src="https://github.com/user-attachments/assets/e95e6235-b8ae-4fb9-a5ca-8481edd8a84d" />


8. Menu **Projects** akan menampilkan projek-projek yang telat dibuat. Satu projek bisa berisi banyak task di dalamnya. Untuk membuat projek baru, user bisa tinggal mengetikkan judul projeknya dan klik tombol **Add** .
<img width="2845" height="1526" alt="image" src="https://github.com/user-attachments/assets/0846942b-40e2-4acd-987a-89d7b3b43ae2" />
Ketika suatu projek di klik, user bisa melihat task-task dalam projek tersebut dalam 4 jenis tampilan, yaitu List, Gantt, Table, dan juga Kanban. Masing-masing jenis akan menampilkan 

**List**
<img width="2080" height="500" alt="image" src="https://github.com/user-attachments/assets/e2e7f184-c4c6-4446-8aea-8484d712ebba" />
**Gantt**
<img width="2289" height="946" alt="image" src="https://github.com/user-attachments/assets/2336b04f-e2c5-4d70-a049-5002fd16167e" />
**Table**
<img width="2322" height="501" alt="image" src="https://github.com/user-attachments/assets/284338e3-b583-47f9-98f8-a306782170b8" />
**Kanban**
<img width="2333" height="991" alt="image" src="https://github.com/user-attachments/assets/fb9b027a-6d1d-4a47-9267-b15ed1cc91be" />

7. Menu **Labels** akan menampilkan label-label yang telah dibuat oleh user. Label digunakan untuk mengelompokkan task-task sesuai kategori tertentu.
<img width="2879" height="1346" alt="image" src="https://github.com/user-attachments/assets/7a4c501f-4421-44c1-ac1b-4b663a5b6ab8" />
<img width="1504" height="788" alt="image" src="https://github.com/user-attachments/assets/eab68801-4065-428b-841c-f9f1f749bfd7" />

8. Menu **Teams** digunakan ketika user ingin berkolaborasi dengan user lainnya dalam sebuah tim. User bisa langsung menembahkan anggota tim dengan mengetikkan username user lain.
<img width="2853" height="871" alt="image" src="https://github.com/user-attachments/assets/93d1ee8b-b5d1-40d5-ad25-fb20d7f00bc1" />
<img width="2879" height="1502" alt="image" src="https://github.com/user-attachments/assets/5a7a5fb9-bbb3-4dd4-8a2a-e989d85dbd1b" />

9. Jika mengklik bagian **Profile**, terdapat bagian **Settings** dengan beberapa menu lain didalamnya.
<img width="2849" height="1526" alt="image" src="https://github.com/user-attachments/assets/70d7a5cd-4157-4d6c-9f8f-508c1db31bea" />
Di sini terdapat fitur penting, yaitu:

**Export your Vikunja Data**, merupakan fitur untuk mengunduh salinan semua data Vikunja, termasuk projek, task, dan semua yang terkait dengannya.
<img width="2857" height="1031" alt="Screenshot 2025-10-17 012456" src="https://github.com/user-attachments/assets/afc8acda-3c85-457c-841f-3e1ca8bae394" />

**Import form other devices**, merupakan fitur untuk mengimpor data dari sebuah file dari 
<img width="2879" height="1031" alt="image" src="https://github.com/user-attachments/assets/ccbc4ae1-e852-49df-8c11-d6cc65e3b074" />





## Pembahasan
[`^ kembali ke atas ^`](#)

- Pendapat anda tentang aplikasi web ini
    - kelebihan
- kekurangan
  - Kekurangan Dari Segi Fitur
    1. Belum ada fitur real-time update, seperti dua orang edit list/ tugas secara bersamaan
       - Solusi: refresh project team secara berkala agar ter-update dengan baik
    2. Tampilan tanggal di bagian Gantt horizontal (kurang enak dipandang)
       <img width="921" height="327" alt="image" src="https://github.com/user-attachments/assets/ba330f9e-de63-43cf-aca9-4fe2dda76afd" />
       - Solusi: buat seperti bentuk kalender biasa (ada row dan column) 
    3. Belum adanya sinkronisasi langsung dengan Google Calendar
       - Solusi: Cek berkala task melalui aplikasi
    4. Belum dapat invite teman otomatis melalui Email
       <img width="995" height="321" alt="image" src="https://github.com/user-attachments/assets/91fb13ee-d40f-4e2e-8606-5c9b93a5a1ed" />
       - Solusi: invite team members langsung melalui aplikasinya
    5. Belum adanya fitur persentase progress untuk tiap task
       - Solusi: melihat/ menganalisis persentase progress secara manual
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi
[`^ kembali ke atas ^`](#)

- https://vikunja.io/

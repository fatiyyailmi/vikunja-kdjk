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

- Tampilan aplikasi web
- Fungsi-fungsi utama
- Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot


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

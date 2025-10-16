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
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi
[`^ kembali ke atas ^`](#)

- https://vikunja.io/

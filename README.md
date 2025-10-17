<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6b3a36d-b9a3-4339-bdee-459ecc369122" /><h1 align="center"><img src="https://vikunja.cloud/images/vikunja-logo.svg"></h1>

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

# Tutorial Instalasi Vikunja di AWS

Panduan instalasi Vikunja di VM AWS menggunakan Docker Compose.

## Requirements

### Hardware & VM
- VM AWS EC2 (minimal t2.small)
- OS: Ubuntu 20.04/22.04 LTS
- IP Publik yang sudah di-assign
- Security Group dengan port terbuka:
  - Port 22 (SSH)
  - Port 80 (HTTP)
  - Port 443 (HTTPS)

### Software
- Docker Engine (versi 20.10+)
- Docker Compose (versi 2.0+)

### Akun & Kredensial
- AWS Key Pair (.pem file)
- DuckDNS Account dan Token
- Email SMTP (Gmail App Password)

## Tahapan Instalasi

### 1. Koneksi ke VM AWS

#### Set permission untuk SSH key

**Linux/Mac:**
```bash
chmod 400 /path/to/your-key.pem
```

**Windows (WSL):**
```bash
chmod 400 your-key.pem
```

#### SSH ke EC2

**Untuk Ubuntu:**
```bash
ssh -i "your-key.pem" ubuntu@your-ec2-public-ip
```

Contoh:
```bash
ssh -i "vikunja-key.pem" ubuntu@54.123.45.67
```

### 2. Update Sistem

```bash
sudo apt update && sudo apt upgrade -y
```

### 3. Install Docker Engine

```bash
# Tambahkan Docker GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Tambahkan Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
#Install Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

### 4. Setup Docker Permission

```bash
# Tambahkan user ke grup docker
sudo usermod -aG docker $USER

# Aktifkan perubahan
newgrp docker

# Test tanpa sudo
docker ps
```

### 5. Buat Direktori Project

```bash
# Buat direktori project
mkdir -p ~/vikunja
cd ~/vikunja

# Buat subdirectory
mkdir -p files duckdns
```

### 6. Buat File docker-compose.yml

```bash
nano docker-compose.yml
```

Copy paste konfigurasi berikut:

```yaml
version: "3.7"

services:

  vikunja:
    image: vikunja/vikunja:latest
    container_name: vikunja
    restart: unless-stopped
    environment:
      VIKUNJA_DATABASE_HOST: db
      VIKUNJA_DATABASE_PASSWORD: rahasia_db               # GANTI
      VIKUNJA_DATABASE_TYPE: mysql
      VIKUNJA_DATABASE_USER: vikunja
      VIKUNJA_DATABASE_DATABASE: vikunja
      VIKUNJA_SERVICE_JWTSECRET: gantidengankunciacakpanjang # GANTI
      VIKUNJA_SERVICE_PUBLICURL: https://vikunja.contoh.com # GANTI
      VIKUNJA_MAILER_ENABLED: "true"
      VIKUNJA_MAILER_HOST: smtp.gmail.com
      VIKUNJA_MAILER_PORT: 587
      VIKUNJA_MAILER_USERNAME: emailanda@gmail.com          # GANTI
      VIKUNJA_MAILER_PASSWORD: apppassword_email            # GANTI
      VIKUNJA_MAILER_FROMEMAIL: emailanda@gmail.com         # GANTI
    volumes:
      - ./files:/app/vikunja/files
    depends_on:
      db:
        condition: service_healthy
    networks:
      - vikunja_net
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.vikunja-secure.rule=Host(`vikunjaanda.duckdns.org`)" # GANTI
      - "traefik.http.routers.vikunja-secure.entrypoints=websecure"
      - "traefik.http.routers.vikunja-secure.tls=true"
      - "traefik.http.routers.vikunja-secure.tls.certResolver=myresolver"
      - "traefik.http.services.vikunja-secure.loadbalancer.server.port=3456"

  db:
    image: mariadb:10
    container_name: vikunja_db
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: rahasia_db_root                    # GANTI
      MYSQL_DATABASE: vikunja
      MYSQL_USER: vikunja
      MYSQL_PASSWORD: rahasia_db                             # GANTI (sama dengan VIKUNJA_DATABASE_PASSWORD)
    volumes:
      - db_data:/var/lib/mysql
    healthcheck:
      test: [ "CMD", "mariadb", "-u$$MYSQL_USER", "-p$$MYSQL_PASSWORD", "-e", "SELECT 1" ]
      interval: 5s
      timeout: 3s
      retries: 5
    networks:
      - vikunja_net

  traefik:
    image: traefik:v2.10
    container_name: traefik
    restart: unless-stopped
    command:
      - "--api.insecure=false"
      - "--providers.docker=true"
      - "--providers.docker.exposedByDefault=false"
      - "--providers.docker.network=vikunja_net"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
      - "--entrypoints.web.http.redirections.entrypoint.to=websecure"
      - "--entrypoints.web.http.redirections.entrypoint.scheme=https"
      - "--certificatesresolvers.myresolver.acme.httpchallenge=true"
      - "--certificatesresolvers.myresolver.acme.httpchallenge.entrypoint=web"
      - "--certificatesresolvers.myresolver.acme.email=emailanda@gmail.com" # GANTI
      - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
      - "--log.level=INFO"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      - "letsencrypt:/letsencrypt"
    networks:
      - vikunja_net

  duckdns:
    image: linuxserver/duckdns
    container_name: duckdns
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Jakarta
      - SUBDOMAINS=vikunjaanda              # GANTI (tanpa .duckdns.org)
      - TOKEN=token-duckdns-anda           # GANTI
      - LOG_FILE=true
    volumes:
      - ./duckdns:/config
    restart: unless-stopped
    networks:
      - vikunja_net

volumes:
  db_data:
  letsencrypt:

networks:
  vikunja_net:
    driver: bridge
```

Simpan file: `Ctrl + X`, lalu `Y`, lalu `Enter`

### 7. Konfigurasi Yang Harus Diganti

- `VIKUNJA_DATABASE_PASSWORD`
- `MYSQL_ROOT_PASSWORD`
- `MYSQL_PASSWORD` (harus sama dengan VIKUNJA_DATABASE_PASSWORD)
- `VIKUNJA_SERVICE_JWTSECRET`
- `VIKUNJA_SERVICE_PUBLICURL`
- `VIKUNJA_MAILER_USERNAME`
- `VIKUNJA_MAILER_PASSWORD`
- `VIKUNJA_MAILER_FROMEMAIL`
- `traefik.http.routers.vikunja-secure.rule=Host(...)`
- `certificatesresolvers.myresolver.acme.email`
- `SUBDOMAINS` di DuckDNS
- `TOKEN` DuckDNS


### 8. Pull Docker Images

```bash
docker compose pull
```

### 9. Start Services

```bash
docker compose up -d
```

### 10. Akses Vikunja

Buka browser dan akses: `https://vikunjakdjk.duckdns.org`


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


7. Menu **Projects** akan menampilkan projek-projek yang telat dibuat. Satu projek bisa berisi banyak task di dalamnya. Untuk membuat projek baru, user bisa tinggal mengetikkan judul projeknya dan klik tombol **Add** .
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

8. Menu **Labels** akan menampilkan label-label yang telah dibuat oleh user. Label digunakan untuk mengelompokkan task-task sesuai kategori tertentu.
<img width="2879" height="1346" alt="image" src="https://github.com/user-attachments/assets/7a4c501f-4421-44c1-ac1b-4b663a5b6ab8" />
<img width="1504" height="788" alt="image" src="https://github.com/user-attachments/assets/eab68801-4065-428b-841c-f9f1f749bfd7" />

9. Menu **Teams** digunakan ketika user ingin berkolaborasi dengan user lainnya dalam sebuah tim. User bisa langsung menembahkan anggota tim dengan mengetikkan username user lain.
<img width="2853" height="871" alt="image" src="https://github.com/user-attachments/assets/93d1ee8b-b5d1-40d5-ad25-fb20d7f00bc1" />
<img width="2849" height="1517" alt="image" src="https://github.com/user-attachments/assets/6f8d760f-4de1-4830-bbe1-7e24b472632e" />

10. Jika mengklik bagian **Profile**, terdapat bagian **Settings** dengan beberapa menu lain didalamnya.
<img width="2849" height="1526" alt="image" src="https://github.com/user-attachments/assets/70d7a5cd-4157-4d6c-9f8f-508c1db31bea" />
Di sini terdapat fitur penting, yaitu:

a. **Export your Vikunja Data**, merupakan fitur untuk mengunduh salinan semua data Vikunja, termasuk projek, task, dan semua yang terkait dengannya.
<img width="2857" height="1031" alt="Screenshot 2025-10-17 012456" src="https://github.com/user-attachments/assets/afc8acda-3c85-457c-841f-3e1ca8bae394" />

b. **Import form other devices**, merupakan fitur untuk mengimpor data dari sebuah file dari 
<img width="2879" height="1031" alt="image" src="https://github.com/user-attachments/assets/ccbc4ae1-e852-49df-8c11-d6cc65e3b074" />





## Pembahasan
[`^ kembali ke atas ^`](#)

- Pendapat anda tentang aplikasi web ini
- Kelebihan
  1. Pengguna dapat melakukan export data dimana mereka dapat mengunduh arsip ZIP yang mencakup seluruh entitas data dan memungkinkan pengguna untuk melakukan backup data secara offline.
  2. Fleksibilitas migrasi data dimana memungkinkan pengguna untuk dapat memindahkan data mereka dari satu instance server ke instance server lainnya.
  3. Dapat melakukan Kolaborasi Tim sehingga pengguna dapat dengan mudah berbagi proyek dan menetapkan task assignment dengan pengguna lain dalam suatu grup.
  4. Visualisasi multiple view yang menyesuaikan preferensi pengguna dimana mereka dapat memilih jenis tampilan mereka apakah ingin berbentuk List Item, Kanban Board, Gantt Chart, atau tampilan dalam bentuk tabel.
  5. Memberikan integrasi lintas platform yang dilengkapi dengan protol calDAV dimana pengguna dapat melakukan sinkronisasi data task mereka dengan kalener handphone, khususnya bagi pengguna Apple yang memungkinkan terkoneksi langsung dengan iCalendar.

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
  
| **Aspek / Fitur** | **Vikunja** | **Trello** |
|--------------------|-------------|-------------|
| **Jenis Aplikasi** | Aplikasi open-source yang dapat di-*self-host* secara gratis dan fleksibel. | Aplikasi berbasis cloud (SaaS) yang tidak dapat di-*self-host*. |
| **Lisensi** | Gratis untuk di-host sendiri, bersifat open-source dengan lisensi AGPLv3. | Menyediakan fitur dasar gratis, namun fitur lanjutan tersedia pada versi berbayar (Trello Premium/Enterprise). |
| **Hosting & Kontrol Data** | Dapat di-host di server pribadi (Linux, Docker, AWS, dll) dengan kontrol penuh terhadap data pengguna. | Data disimpan di server milik Atlassian (cloud Trello), sehingga pengguna tidak memiliki kontrol langsung terhadap datanya. |
| **Fitur Tampilan / UI** | Menyediakan tampilan *List View*, *Kanban Board*, *Table View*, *Gantt Chart*, serta fitur *Filter*. | Fokus pada tampilan *Kanban Board*. Tampilan lain seperti *Table*, *Dashboard*, *Timeline*, dan *Map* hanya tersedia di versi premium. |
| **Struktur Proyek & Tugas** | Mendukung alur kerja hierarkis: *Projects → Lists → Tasks → Descriptions*. | Tidak memiliki struktur hierarkis bawaan, hanya *Board → List → Card*. |
| **Kolaborasi** | Mendukung kolaborasi dengan banyak pengguna dalam satu server. | Mendukung kolaborasi antar pengguna dalam organisasi, baik bersifat publik maupun privat. |
| **Integrasi & Ekosistem** | Dapat melakukan impor data dari satu instance server ke server lain | Tidak mendukung impor data eksternal secara langsung, namun memiliki banyak integrasi seperti Slack, Google Drive, dan lainnya. |
| **Kemudahan Instalasi & Penggunaan** | Memerlukan instalasi manual menggunakan Docker dan Docker Compose, sedikit teknis namun sangat fleksibel. | Tidak memerlukan instalasi, cukup membuat akun dan langsung dapat digunakan. |
| **Keamanan & Privasi** | Data pribadi disimpan di server sendiri, sehingga privasi lebih terjamin. | Bergantung pada sistem keamanan dan kebijakan privasi Atlassian Cloud. |
| **Backup & Pemeliharaan** | Dikelola oleh pengguna sendiri, baik secara manual maupun otomatis menggunakan Docker volume. | Seluruh pemeliharaan dilakukan oleh Trello; pengguna tidak memiliki akses langsung ke data mentah. |
| **Kinerja & Skalabilitas** | Ringan dan dapat disesuaikan dengan kapasitas server. Cocok untuk proyek pribadi atau skala kecil. | Stabil untuk proyek kecil hingga menengah, namun fitur kompleks memerlukan *Power-Ups* tambahan. |
| **Cocok Untuk** | Pengguna yang menginginkan privasi tinggi, kontrol penuh atas data, dan fleksibilitas untuk *self-host* di server pribadi atau AWS. | Pengguna yang mengutamakan kemudahan penggunaan, tampilan visual menarik, dan tidak ingin repot melakukan konfigurasi teknis. |

---

## Referensi
[`^ kembali ke atas ^`](#)

- https://vikunja.io/
- https://www.youtube.com/watch?v=4ZDYJeeG_mg (Instalasi dan Overview mengenai Vikunja)

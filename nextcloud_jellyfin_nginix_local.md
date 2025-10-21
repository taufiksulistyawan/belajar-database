Sangat bagus 👍 — mencoba **di lokal dulu** adalah langkah paling aman dan efisien sebelum Anda upload ke domain publik.
Dengan begitu, Anda bisa **uji semua fungsi (Nextcloud, Jellyfin, integrasi web)** tanpa risiko keamanan atau gangguan jaringan.

Berikut panduan lengkap untuk **setup lokal di Ubuntu 22.04** (tanpa domain, cukup via IP lokal seperti `192.168.1.x`).

---

## 🧭 Tujuan

Menjalankan sistem lengkap di **jaringan lokal**, terdiri dari:

* **Nextcloud** → penyimpanan & tampilan PPT
* **Jellyfin** → streaming video
* **Website lokal** → menampilkan embed dari keduanya
* Semua dijalankan lewat **Docker Compose** (mudah start/stop/reset)

---

## ⚙️ 1. Persiapan Dasar

Pastikan server Anda sudah up-to-date dan Docker terinstal.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
sudo systemctl enable docker
sudo systemctl start docker
```

Cek versi:

```bash
docker --version
docker compose version
```

---

## 📁 2. Siapkan Folder Proyek

Buat folder untuk semua layanan:

```bash
mkdir -p /srv/selfhosted/{nextcloud,jellyfin,nginx}
cd /srv/selfhosted
```

---

## 🧩 3. Buat File `docker-compose.yml`

Buat file:

```bash
nano docker-compose.yml
```

Isi dengan konfigurasi berikut 👇

```yaml
version: "3.8"

services:
  # Nextcloud untuk PPT, dokumen, file
  nextcloud:
    image: nextcloud
    container_name: nextcloud
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - ./nextcloud/html:/var/www/html
      - ./nextcloud/data:/var/www/html/data

  # Jellyfin untuk video streaming
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    network_mode: "host"
    restart: unless-stopped
    volumes:
      - ./jellyfin/config:/config
      - ./jellyfin/media:/media

  # Nginx (opsional untuk website lokal)
  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - ./nginx/html:/usr/share/nginx/html
      - ./nginx/conf.d:/etc/nginx/conf.d
```

Simpan (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

## ▶️ 4. Jalankan Semua Layanan

```bash
sudo docker compose up -d
```

Cek apakah semua berjalan:

```bash
sudo docker ps
```

---

## 🌐 5. Akses di Browser

Pastikan Anda berada di jaringan yang sama, lalu buka di laptop/PC lokal Anda:

| Layanan   | Alamat                  | Fungsi                  |
| --------- | ----------------------- | ----------------------- |
| Nextcloud | `http://IP_SERVER:8080` | Cloud storage PPT       |
| Jellyfin  | `http://IP_SERVER:8096` | Streaming video         |
| Website   | `http://IP_SERVER`      | Web lokal (static test) |

> Ganti `IP_SERVER` dengan IP Ubuntu Anda, contoh `http://192.168.1.10:8080`

---

## 🧱 6. Tes Nextcloud (PPT)

1. Buka `http://192.168.1.10:8080`
2. Buat akun admin.
3. Upload satu file `.pptx` atau `.pdf`.
4. Klik ikon "share" → aktifkan “public link”.
5. Salin link preview (mis. `/s/abc123/preview`).

Contoh embed ke HTML:

```html
<iframe src="http://192.168.1.10:8080/s/abc123/preview" width="800" height="600"></iframe>
```

---

## 🎬 7. Tes Jellyfin (Video)

1. Buka `http://192.168.1.10:8096`
2. Buat akun admin.
3. Tambahkan folder `/srv/selfhosted/jellyfin/media`
4. Upload file `.mp4` ke folder tersebut:

   ```bash
   cp video.mp4 /srv/selfhosted/jellyfin/media/
   ```
5. Akses lewat web Jellyfin → klik video untuk streaming.

Embed ke HTML:

```html
<video controls width="800">
  <source src="http://192.168.1.10:8096/media/video.mp4" type="video/mp4">
</video>
```

---

## 🧪 8. Buat Website Lokal (opsional)

Buat file sederhana:

```bash
nano /srv/selfhosted/nginx/html/index.html
```

Isi contoh:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Demo Lokal</title>
</head>
<body>
  <h2>Presentasi PPT</h2>
  <iframe src="http://192.168.1.10:8080/s/abc123/preview" width="800" height="600"></iframe>

  <h2>Video Tutorial</h2>
  <video controls width="800">
    <source src="http://192.168.1.10:8096/media/video.mp4" type="video/mp4">
  </video>
</body>
</html>
```

Lalu akses di browser:
👉 `http://192.168.1.10`

---

## 🔒 9. Tips Tambahan (Lokal)

* Jalankan `sudo ufw allow 8080,8096,80/tcp` agar port bisa diakses dari jaringan.
* Kalau ingin akses dari laptop lain → pastikan berada di **jaringan WiFi/LAN yang sama**.
* Untuk stop semua layanan:

  ```bash
  sudo docker compose down
  ```
* Untuk restart:

  ```bash
  sudo docker compose up -d
  ```

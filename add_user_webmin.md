**Dua Cara** menambahkan user di Webmin: lewat **Webmin GUI** atau langsung via **Linux terminal** (nanti otomatis bisa login ke Webmin juga kalau user masuk grup sudo).

---

## 🔹 1. Tambah user lewat Webmin GUI

Kalau kamu sudah bisa login ke Webmin (misalnya dengan root), ikuti langkah ini:

1. Login ke Webmin → menu **System → Users and Groups**.

2. Klik **Create a new user**.

3. Isi:

   * **Username** → nama user baru.
   * **Password** → pilih `Normal password` lalu ketikkan password.
   * **Shell** → pilih `/bin/bash` (supaya bisa dipakai SSH juga).
   * **Primary group** → misalnya `users`.
   * **Secondary group** → tambahkan `sudo` kalau user ini butuh akses administrator.

4. Klik **Create** → user baru langsung aktif.

---

## 🔹 2. Tambah user via Linux Terminal

Kalau kamu lebih nyaman pakai terminal, caranya:

1. Buat user baru

   ```bash
   sudo adduser namauser
   ```

   → isikan password + data user.

2. Tambahkan ke grup `sudo` (opsional, kalau butuh hak admin)

   ```bash
   sudo usermod -aG sudo namauser
   ```

3. Cek grup user

   ```bash
   groups namauser
   ```

   Kalau ada `sudo`, berarti sudah benar.

4. Login Webmin pakai user baru itu:

   * Username: `namauser`
   * Password: (yang tadi dibuat)

---

## 🔹 3. Khusus user Webmin (bukan Linux)

Kalau kamu **tidak ingin bikin user Linux**, tapi hanya akses Webmin saja:

1. Di Webmin → **Webmin → Webmin Users**.
2. Klik **Create a new Webmin user**.
3. Isi username + password.
4. Pilih modul apa yang boleh diakses (misalnya hanya Database, Apache, dll).
5. Simpan → selesai.

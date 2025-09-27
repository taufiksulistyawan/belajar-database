**Dua Cara** pasang Webmin: pakai repo resmi Webmin atau langsung download paket `.deb`.

---

## 🔹 Cara 1 – Tambah repo resmi Webmin (lebih disarankan)

1. **Install dependensi**

   ```bash
   sudo apt update
   sudo apt install wget apt-transport-https software-properties-common gnupg -y
   ```

2. **Import GPG key Webmin**

   ```bash
   wget -qO - http://www.webmin.com/jcameron-key.asc | sudo apt-key add -
   ```

3. **Tambahkan repository Webmin**

   ```bash
   echo "deb http://download.webmin.com/download/repository sarge contrib" | sudo tee /etc/apt/sources.list.d/webmin.list
   ```

4. **Update dan install Webmin**

   ```bash
   sudo apt update
   sudo apt install webmin -y
   ```

5. **Cek status**

   ```bash
   sudo systemctl status webmin
   ```

---

## 🔹 Cara 2 – Install manual via file `.deb`

Kalau repo susah dipakai, bisa download langsung:

1. **Download Webmin terbaru**

   ```bash
   wget http://prdownloads.sourceforge.net/webadmin/webmin_2.110_all.deb
   ```

   (versi bisa dicek di [https://www.webmin.com/download.html](https://www.webmin.com/download.html))

2. **Install paket**

   ```bash
   sudo apt install ./webmin_2.110_all.deb -y
   ```

3. **Cek status**

   ```bash
   sudo systemctl status webmin
   ```

---

## 🔹 Akses Webmin

Setelah jalan, buka di browser:

```
https://IP-SERVER:10000
```

Login pakai `root` atau user sudo.

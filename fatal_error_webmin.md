Install semua **Perl modules** yang biasanya dibutuhkan Webmin.
Ini aman karena modulnya standar di repo Ubuntu/Debian.

---

## 🔹 Perintah Install Sekaligus

Jalankan di server:

```bash
sudo apt update
sudo apt install -y \
  liblist-moreutils-perl \
  liblist-moreutils-xs-perl \
  libdigest-perl \
  libdigest-md5-perl \
  libdigest-sha-perl \
  libnet-ssleay-perl \
  libauthen-pam-perl \
  libio-pty-perl \
  libperl5.38 \
  libwww-perl \
  libencode-locale-perl \
  libhtml-parser-perl \
  libtimedate-perl
```

---

## 🔹 Restart Webmin

```bash
sudo systemctl restart webmin
```

---

## 🔹 Kenapa modul ini penting?

* `liblist-moreutils-perl` → dipakai Webmin untuk manipulasi list.
* `libdigest-perl`, `libdigest-md5-perl`, `libdigest-sha-perl` → hashing (password, session).
* `libnet-ssleay-perl` → SSL/TLS (https).
* `libauthen-pam-perl`, `libio-pty-perl` → otentikasi user.
* `libwww-perl`, `libhtml-parser-perl` → parsing web data.
* `libtimedate-perl` → konversi waktu.

---

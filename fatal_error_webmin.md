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



Fatal error variabel `$month` bisa jadi **kosong atau salah format**, lalu dikurangi `1` → hasilnya `-1`, makanya keluar error:

```
Month '-1' out of range 0..11
```

---

## 🔧 Cara patch manual

Edit file:

```bash
sudo nano /usr/share/webmin/webmin/os-eol-lib.pl
```

Cari blok yang kamu tunjukkan tadi, lalu ubah jadi seperti ini (tambahkan validasi bulan):

Kita bisa **patch langsung di blok yang kamu kirim** biar aman.

---

### 🔧 Versi setelah diedit

```perl
if (ref($eol_json) eq 'ARRAY' && @$eol_json) {
    my $os_version = $gconfig{'real_os_version'};
    # Extract the major and minor versions
    $os_version =~ m/^(?:stable\/)?(?<major>\d+)(?:\.(?<minor>\d+))?/;
    $os_version = $+{minor} ? "$+{major}.$+{minor}" : $+{major};
    return undef if (!$+{major});
    # Minor versions in cycle are allowed only for Ubuntu
    $os_version = $+{major} if ($os !~ /ubuntu/);
    my ($eol_json_this_os) =
        grep { $_->{'_os'} eq $os &&
               $_->{'cycle'} eq $os_version } @$eol_json;

    $eol_json_this_os->{'_os_name'}    = $gconfig{'real_os_type'};
    $eol_json_this_os->{'_os_version'} = $os_version;

    # Convert EOL date to a timestamp
    my ($year, $month, $day) = split('-', $eol_json_this_os->{'eol'});
    $month = 1 if (!$month || $month < 1 || $month > 12);   # FIX
    $day   = 1 if (!$day   || $day < 1   || $day > 31);     # FIX
    $eol_json_this_os->{'_eol_timestamp'} =
        timelocal(0, 0, 0, $day, $month - 1, $year);

    # Convert EOL extended date to a timestamp
    if ($eol_json_this_os->{'extendedSupport'}) {
        my ($year, $month, $day) = split('-', $eol_json_this_os->{'extendedSupport'});
        $month = 1 if (!$month || $month < 1 || $month > 12);   # FIX
        $day   = 1 if (!$day   || $day < 1   || $day > 31);     # FIX
        $eol_json_this_os->{'_ext_eol_timestamp'} =
            timelocal(0, 0, 0, $day, $month - 1, $year);
    }

    return $eol_json_this_os if ($eol_json_this_os);
}
return undef;
}
```

---

## 🔄 Setelah edit

1. Simpan (`CTRL+O`, Enter, `CTRL+X`).
2. Restart Webmin:

```bash
sudo systemctl restart webmin
```

3. Coba buka lagi Webmin → menu **System Status** harusnya sudah jalan tanpa error.

---

⚡ Jadi intinya: patch ini **memaksa bulan minimal = 1 (Januari)** kalau parsing gagal.
Daripada crash, lebih aman fallback.

👉 Mau saya bikinkan **patch otomatis (pakai `sed`)** biar kamu nggak perlu edit manual lagi?

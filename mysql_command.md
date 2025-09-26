### 🔹 Membuat database

```sql
CREATE DATABASE sekolah;
```

👉 Membuat sebuah database baru dengan nama `sekolah`.
Database ini ibarat **lemari besar** tempat menyimpan tabel-tabel.

---

### 🔹 Memilih database yang dipakai

```sql
USE sekolah;
```

👉 Memilih database `sekolah` supaya perintah berikutnya dijalankan di dalam database itu.

---

### 🔹 Membuat tabel

```sql
CREATE TABLE siswa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    kelas VARCHAR(10),
    umur INT
);
```

👉 Membuat tabel `siswa` dengan kolom:

* `id` → angka unik otomatis (sebagai **primary key** / identitas).
* `nama` → teks maksimal 100 karakter.
* `kelas` → teks maksimal 10 karakter.
* `umur` → angka (integer).

---

### 🔹 Menambahkan data

```sql
INSERT INTO siswa (nama, kelas, umur) 
VALUES ('Budi', '10A', 16), ('Ani', '10B', 15);
```

👉 Memasukkan data ke tabel `siswa`.

* `Budi` di kelas `10A`, umur `16`.
* `Ani` di kelas `10B`, umur `15`.

---

### 🔹 Melihat data

```sql
SELECT * FROM siswa;
```

👉 Menampilkan semua data (`*` artinya semua kolom) dari tabel `siswa`.

---

### 🔹 Mengubah data

```sql
UPDATE siswa SET kelas = '11A' WHERE nama = 'Budi';
```

👉 Mengubah data siswa yang bernama **Budi**, kelasnya jadi `11A`.

* `WHERE` itu syarat, kalau nggak pakai → semua data bisa ikut berubah. ⚠️

---

### 🔹 Menghapus data

```sql
DELETE FROM siswa WHERE nama = 'Ani';
```

👉 Menghapus baris data yang punya nama `Ani`.

---

### 🔹 Membuat tabel relasi (contoh nilai)

```sql
CREATE TABLE nilai (
    id INT AUTO_INCREMENT PRIMARY KEY,
    siswa_id INT,
    mata_pelajaran VARCHAR(50),
    nilai INT,
    FOREIGN KEY (siswa_id) REFERENCES siswa(id)
);
```

👉 Tabel `nilai` ini menyimpan:

* `siswa_id` → menghubungkan ke `id` di tabel `siswa`.
* `mata_pelajaran` → nama pelajaran.
* `nilai` → angka nilai siswa.

`FOREIGN KEY` artinya data ini harus sesuai dengan data di tabel lain (relasi antar tabel).

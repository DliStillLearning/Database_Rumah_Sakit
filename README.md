Siap 🔥
Ini versi **final paling clean + lengkap + ada struktur tabel (DESC) + hasil query**.
Sudah siap langsung kamu pakai untuk `README.md`.

---

# 🏥 Database Rumah Sakit

## 👥 Anggota Kelompok

1. **Aulia Bilqis Zahrani (6)**
2. **Muhamad Adli Irawan (19)**
3. **Rafa Zulfahmi (28)**

---

# 📖 Deskripsi

Database `rumah_sakit` adalah sistem basis data sederhana untuk mengelola pelayanan rumah sakit yang mencakup:

* Data Pasien
* Data Dokter
* Data Obat
* Data Transaksi
* Laporan Transaksi (View)

---

# 1️⃣ Membuat Database

```sql
CREATE DATABASE rumah_sakit;
USE rumah_sakit;
```

---

# 2️⃣ Struktur Tabel

## 👤 Tabel: pasien

```sql
CREATE TABLE pasien(
    id_pasien INT PRIMARY KEY,
    nama_pasien VARCHAR(100),
    umur INT,
    jenis_kelamin VARCHAR(20)
);
```

### 🔎 Struktur (DESC pasien)

```sql
DESC pasien;
```

| Field         | Type         | Null | Key | Default | Extra |
| ------------- | ------------ | ---- | --- | ------- | ----- |
| id_pasien     | int(11)      | NO   | PRI | NULL    |       |
| nama_pasien   | varchar(100) | YES  |     | NULL    |       |
| umur          | int(11)      | YES  |     | NULL    |       |
| jenis_kelamin | varchar(20)  | YES  |     | NULL    |       |

---

## 🩺 Tabel: dokter

```sql
CREATE TABLE dokter(
    id_dokter INT PRIMARY KEY,
    nama_dokter VARCHAR(100),
    spesialis VARCHAR(50)
);
```

### 🔎 Struktur (DESC dokter)

```sql
DESC dokter;
```

| Field       | Type         | Null | Key | Default | Extra |
| ----------- | ------------ | ---- | --- | ------- | ----- |
| id_dokter   | int(11)      | NO   | PRI | NULL    |       |
| nama_dokter | varchar(100) | YES  |     | NULL    |       |
| spesialis   | varchar(50)  | YES  |     | NULL    |       |

---

## 💊 Tabel: obat

```sql
CREATE TABLE obat(
    id_obat INT PRIMARY KEY,
    nama_obat VARCHAR(100),
    harga INT
);
```

### 🔎 Struktur (DESC obat)

```sql
DESC obat;
```

| Field     | Type         | Null | Key | Default | Extra |
| --------- | ------------ | ---- | --- | ------- | ----- |
| id_obat   | int(11)      | NO   | PRI | NULL    |       |
| nama_obat | varchar(100) | YES  |     | NULL    |       |
| harga     | int(11)      | YES  |     | NULL    |       |

---

## 🧾 Tabel: transaksi

```sql
CREATE TABLE transaksi(
    id_transaksi INT PRIMARY KEY,
    id_pasien INT,
    id_dokter INT,
    id_obat INT,
    tanggal DATE,
    jumlah_obat INT,
    FOREIGN KEY (id_pasien) REFERENCES pasien(id_pasien),
    FOREIGN KEY (id_dokter) REFERENCES dokter(id_dokter),
    FOREIGN KEY (id_obat) REFERENCES obat(id_obat)
);
```

### 🔎 Struktur (DESC transaksi)

```sql
DESC transaksi;
```

| Field        | Type    | Null | Key | Default | Extra |
| ------------ | ------- | ---- | --- | ------- | ----- |
| id_transaksi | int(11) | NO   | PRI | NULL    |       |
| id_pasien    | int(11) | YES  | MUL | NULL    |       |
| id_dokter    | int(11) | YES  | MUL | NULL    |       |
| id_obat      | int(11) | YES  | MUL | NULL    |       |
| tanggal      | date    | YES  |     | NULL    |       |
| jumlah_obat  | int(11) | YES  |     | NULL    |       |

---

# 3️⃣ JOIN

## 🔗 INNER JOIN

```sql
SELECT pasien.nama_pasien,
       dokter.nama_dokter,
       obat.nama_obat,
       transaksi.jumlah_obat
FROM transaksi
INNER JOIN pasien ON transaksi.id_pasien = pasien.id_pasien
INNER JOIN dokter ON transaksi.id_dokter = dokter.id_dokter
INNER JOIN obat ON transaksi.id_obat = obat.id_obat;
```

### 📊 Hasil

| nama_pasien | nama_dokter | nama_obat   | jumlah_obat |
| ----------- | ----------- | ----------- | ----------- |
| Andi        | Dr. Ahmad   | Paracetamol | 2           |
| Siti        | Dr. Sinta   | Amoxicillin | 1           |
| Budi        | Dr. Hadi    | Vitamin C   | 5           |
| Rina        | Dr. Lina    | Ibuprofen   | 3           |
| Dewi        | Dr. Rudi    | Antasida    | 4           |

---

## 🔗 LEFT JOIN

```sql
SELECT pasien.nama_pasien,
       transaksi.id_transaksi
FROM pasien
LEFT JOIN transaksi
ON pasien.id_pasien = transaksi.id_pasien;
```

### 📊 Hasil

| nama_pasien | id_transaksi |
| ----------- | ------------ |
| Andi        | 1            |
| Siti        | 2            |
| Budi        | 3            |
| Rina        | 4            |
| Dewi        | 5            |

---

# 4️⃣ Aggregate Function

## 💰 Total Pendapatan

```sql
SELECT SUM(obat.harga * transaksi.jumlah_obat) AS total_pendapatan
FROM transaksi
INNER JOIN obat ON transaksi.id_obat = obat.id_obat;
```

| total_pendapatan |
| ---------------- |
| 72000            |

---

## 📊 Rata-rata Harga Obat

```sql
SELECT AVG(harga) AS rata_rata_harga FROM obat;
```

| rata_rata_harga |
| --------------- |
| 5800            |

---

## 👥 Jumlah Pasien

```sql
SELECT COUNT(*) AS jumlah_pasien FROM pasien;
```

| jumlah_pasien |
| ------------- |
| 5             |

---

# 5️⃣ View Laporan Transaksi

```sql
CREATE VIEW view_laporan_transaksi AS
SELECT pasien.nama_pasien,
       dokter.nama_dokter,
       obat.nama_obat,
       transaksi.jumlah_obat,
       (obat.harga * transaksi.jumlah_obat) AS total_bayar
FROM transaksi
INNER JOIN pasien ON transaksi.id_pasien = pasien.id_pasien
INNER JOIN dokter ON transaksi.id_dokter = dokter.id_dokter
INNER JOIN obat ON transaksi.id_obat = obat.id_obat;
```

```sql
SELECT * FROM view_laporan_transaksi;
```

### 📊 Hasil

| nama_pasien | nama_dokter | nama_obat   | jumlah_obat | total_bayar |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| Andi        | Dr. Ahmad   | Paracetamol | 2           | 10000       |
| Siti        | Dr. Sinta   | Amoxicillin | 1           | 10000       |
| Budi        | Dr. Hadi    | Vitamin C   | 5           | 15000       |
| Rina        | Dr. Lina    | Ibuprofen   | 3           | 21000       |
| Dewi        | Dr. Rudi    | Antasida    | 4           | 16000       |

---

# 🔗 Relasi Antar Tabel

* `transaksi.id_pasien` → `pasien.id_pasien`
* `transaksi.id_dokter` → `dokter.id_dokter`
* `transaksi.id_obat` → `obat.id_obat`

---

# 🎯 Kesimpulan

Database `rumah_sakit` terdiri dari:

* 4 tabel utama
* 1 view
* Relasi foreign key
* JOIN
* Aggregate function

Cocok untuk latihan:

* CRUD
* JOIN
* VIEW
* Laporan sederhana

---

Kalau mau, saya bisa tambahin **diagram ERD dalam bentuk ASCII biar README kamu makin kelihatan pro banget di GitHub** 🚀

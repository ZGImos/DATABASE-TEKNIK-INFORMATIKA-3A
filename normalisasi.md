# NORMALISASI BASIS DATA – Khilmy Firdaus Romadon

**Proyek** : Aplikasi E-Commerce
**Mata Kuliah** : Basis Data
**Tahap** : 1 – Analisis & Normalisasi

---

## Deskripsi Awal Tabel

**Detail Pembayaran (Payment)** pada sistem e-commerce.

### Atribut Awal

| Atribut             | Keterangan                       |
| ------------------- | -------------------------------- |
| `payment_id`        | Primary Key pembayaran           |
| `order_id`          | Foreign Key ke tabel Order       |
| `payment_method_id` | Foreign Key ke metode pembayaran |
| `payment_status`    | Status pembayaran                |
| `paid_at`           | Waktu pembayaran                 |
| `total_paid`        | Total dana dibayarkan            |
| `transaction_ref`   | Referensi transaksi dari gateway |

---

## Analisis Kebutuhan Sistem

Dalam sistem e-commerce, pembayaran memiliki karakteristik:

- Tidak selalu berhasil dalam satu kali proses
- Dapat mengalami gagal, pending, atau retry
- Memerlukan histori transaksi
- Berkaitan langsung dengan pesanan (Order)

Karena itu, data pembayaran **tidak dapat digabung langsung** dengan tabel Order.

---

## Proses Normalisasi

### First Normal Form (1NF)

- Semua atribut bersifat atomik
- Tidak ada data multivalue

**Memenuhi 1NF**

---

### Second Normal Form (2NF)

- Primary Key hanya satu (`payment_id`)
- Seluruh atribut bergantung penuh pada Primary Key

**Memenuhi 2NF**

---

### Third Normal Form (3NF)

- Tidak ada dependensi transitif
- Informasi metode pembayaran dipisah ke tabel `payment_method`

**Memenuhi 3NF**

---

## Keputusan Normalisasi

### Dijadikan Tabel Tersendiri

Alasan:

1. Pembayaran merupakan **entitas bisnis**, bukan sekadar proses
2. Memiliki siklus hidup dan status
3. Diperlukan untuk audit dan histori
4. Menghindari redundansi pada tabel Order
5. Mendukung pengembangan sistem pembayaran di masa depan

---

## Relasi Antar Tabel

- **Order (1) → (N) Payment**
- **Payment (N) → (1) Payment_Method**

---

## ERD (ASCII Diagram)

```
+------------+        +--------------+        +------------------+
|   ORDER    | 1    N |   PAYMENT    | N    1 | PAYMENT_METHOD   |
+------------+--------+--------------+--------+------------------+
| order_idPK |        | payment_idPK |        | method_idPK     |
| user_idFK  |        | order_idFK   |        | method_name     |
| order_date |        | method_idFK  |        | description     |
+------------+        | status       |        +------------------+
                      | paid_at      |
                      | total_paid   |
                      | trx_ref      |
                      +--------------+
```

---

## Perbandingan dengan Tabel Mahasiswa Lain

| Mahasiswa  | Tabel       | Konsistensi                       |     |
| ---------- | ----------- | --------------------------------- | --- |
| User       | Customer    | Relasi ke Order                   | ✔️  |
| Produk     | Product     | Tidak campur transaksi            | ✔️  |
| Order      | Pesanan     | Tidak menyimpan detail pembayaran | ✔️  |
| **Khilmy** | **Payment** | Entitas transaksi terpisah        | ✔️  |

Struktur Payment **selaras dan konsisten** dengan tabel mahasiswa lain.

---

## Kesimpulan

Tabel **Payment** wajibul kudu dijadikan tabel tersendiri karena:

- Memenuhi prinsip normalisasi hingga 3NF
- Mewakili entitas transaksi pembayaran
- Mendukung audit, histori, dan skalabilitas sistem

---

# Jarkom-Modul-4-2025-IT-20

**Laporan Resmi Praktikum Modul 4 — Komunikasi Data & Jaringan Komputer 2025**

## Daftar Anggota

| Nama                  | NRP        |
|-----------------------|------------|
| Zahra Khaalishah      | 5027241070 |
| Dimas Muhammad Putra  | 5027241076 |


# CIDR
## 1. Gambaran Umum Topologi Jaringan

Studi kasus ini mengimplementasikan tata kelola alamat IP untuk jaringan kompleks berdasarkan topologi "Middle Earth Network" dengan total kebutuhan **3094 IP Host**. Implementasi menggunakan dua metode utama untuk efisiensi dan perutean: **VLSM (Variable Length Subnet Masking)** dan **CIDR Aggregation**.

### Topologi Jaringan

Berikut adalah gambaran visual dari seluruh jaringan yang mencakup berbagai *host* dan *router* utama.

![](assets/topologiawal.png)

---

## 2. Pembagian IP

VLSM digunakan untuk mengalokasikan alamat IP secara efisien, dimulai dari *subnet* dengan kebutuhan IP terbesar hingga terkecil (Link R2R). Total keseluruhan alokasi *host* berada pada *Network ID* **192.221.0.0/20**.

### Tabel VLSM (Alokasi IP Awal)

| Nama Subnet | Rute | Jumlah IP (N+2) | Netmask (/M) | Network ID | Range IP | Keterangan |
| :---: | :--- | :---: | :---: | :---: | :---: | :--- |
| A10 | Numenor + Arthedain | 877 | /22 | 192.221.0.0 | 192.221.0.1 - 192.221.3.254 | Aggregasi Host Terbesar. |
| A6 | Anor + Simarils | 663 | /22 | 192.221.4.0 | 192.221.4.1 - 192.221.7.254 | Aggregasi Host Besar. |
| A14 | Erain + Khazad | 505 | /23 | 192.221.8.0 | 192.221.8.1 - 192.221.9.254 | Aggregasi Host. |
| A15 | Erain + Gothmog | 472 | /23 | 192.221.10.0 | 192.221.10.1 - 192.221.11.254 | Aggregasi Host. |
| A19 | Valmar + Lindon | 134 | /24 | 192.221.12.0 | 192.221.12.1 - 192.221.12.254 | Aggregasi Host. **(Koreksi: /24)** |
| A2 | Eregion + Morgul | 128 | /25 | 192.221.13.0 | 192.221.13.1 - 192.221.13.126 | Aggregasi Host. |
| A16 | Gudur + Edhil | 122 | /25 | 192.221.13.128 | 192.221.13.129 - 192.221.13.254 | Aggregasi Host. |
| A8 | Morgoth + Elrond | 61 | /26 | 192.221.14.0 | 192.221.14.1 - 192.221.14.62 | Aggregasi Host. |
| A18 | Valmar + Utumno | 36 | /27 | 192.221.14.64 | 192.221.14.65 - 192.221.14.94 | Aggregasi Host. |
| A20 | Valmar + Arnor | 30 | /27 | 192.221.14.96 | 192.221.14.97 - 192.221.14.126 | Aggregasi Host. |
| A17 | Gudur + Grond | 16 | /28 | 192.221.14.128 | 192.221.14.129 - 192.221.14.142 | Aggregasi Host Kecil. |
| A7 | Throne + Erebor | 5 | /29 | 192.221.14.144 | 192.221.14.145 - 192.221.14.150 | Subnet Link R2R. |
| A1 - A23 | Link R2R | 4 | /30 | (Berlanjut dari 192.221.14.152) | (2 IP Host per Link) | Link Router-to-Router. |

*Note: Tabel di atas menyajikan urutan VLSM logis untuk efisiensi.*

---

## 3. CIDR Aggregation (Supernetting)

CIDR Aggregation dilakukan untuk menggabungkan *subnet* yang berdekatan menjadi *supernet* yang lebih besar, tujuannya untuk **mengurangi ukuran tabel *routing***. Proses ini dilakukan secara bertahap dari penggabungan terkecil hingga terbesar (J1).

### 3.1. Bagan Pohon Agregasi (CIDR Tree)

Berikut adalah bagan pohon yang menunjukkan bagaimana *subnet* dikelompokkan dan diagregasi:

![](assets/treecidr.png)

### 3.2. Proses Penggabungan (Tahap I - IX)

Berikut adalah contoh tabel penggabungan hingga Tahap I - IX (lanjutan):

 ![tes](assets/penggabungan1.png)
 ![tes](assets/penggabungan2.png)
Hasil akhir dari CIDR Aggregation adalah **J1/14**, menunjukkan bahwa keseluruhan jaringan dapat diwakili oleh satu blok IP besar: **192.221.0.0/14**.

### 3.3. Visualisasi CIDR Aggregation Final

Berikut adalah visualisasi topologi dengan *loop* yang menggambarkan batas-batas *supernet* hasil agregasi CIDR, mulai dari B1 hingga agregasi final J1/14:

![Visualisasi CIDR Aggregation Final yang menunjukkan pengelompokan subnet ke dalam B1, C1, D1, E2, hingga J1/14.](assets/subneting.png)

---

## 4. Kesimpulan dan Manfaat

Implementasi gabungan VLSM dan CIDR Aggregation ini menghasilkan tata kelola IP yang:

1.  **Efisiensi IP Tinggi (VLSM):** Setiap *subnet* dialokasikan *Netmask* yang sesuai dengan kebutuhan *host* aktual (N+2), meminimalkan pemborosan alamat IP.
2.  **Skalabilitas & Perutean Efisien (CIDR Aggregation):** Dengan menggunakan CIDR, *router* eksternal ke jaringan utama hanya perlu menyimpan satu entri rute (**192.221.0.0/14**) untuk mencapai seluruh *subnet* internal, yang secara signifikan mengurangi ukuran tabel *routing* (Route Summarization).

---

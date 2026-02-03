# Matchamu

Tools untuk mengolah dan meng-GC SBR di Matchapro

## Tugas Kita
1. GC status
2. Isi latlong kalau kosong

## Permasalahan
- Data banyak dan duplikat
- Identitas desa dan/atau kecamatan kosong
- Latlong kosong

## Solusi
1. **matcha helper**: mengerjakan tugas kita
2. **ceksbr helper**: menyelesaikan permasalahan kita

---

## Alur Kerja Umum

1. Import sheet ke **ceksbr helper** → selesaikan permasalahan (preprocessing)
2. Ekspor sheet hasil preprocessing
3. Import ke **matcha helper**
4. Entri (disarankan paralel, speed ±4000 baris/24 jam per PC, 3 PC = 10.000 baris/hari)

---

## cekSBR Helper

### Required Columns
- `nama_usaha`, `nmdesa`, `nmkec`, `latitude`, `longitude`, `keberadaan_usaha`

### Fitur

#### 1. Fix Latlong
Memperbaiki latlong yang dipisah koma/invalid tapi masih berupa angka

#### 2. Isi dan Match Desa & Kecamatan
- Butuh DataFrame list desa dan kecamatan
- Namai DataFrame: `msls`
- Kolom: `nmdesa`, `nmkec`
- Akan di-join ke DataFrame SBR

#### 3. Detect Duplicate
- Mencari duplikat berdasar: `nama_usaha`, `nmdesa`, `nmkec`
- Duplikat dikelompokkan dengan nomor di kolom `flag_dup`

#### 4. Tandai Duplikat
- Dari setiap kelompok duplikat, pilih 1 sebagai utama (`keberadaan_usaha` = 1)
- Sisanya ditandai duplikat (`keberadaan_usaha` = 4)

#### 5. Isi Latlong Kosong
- Mengisi latlong berdasar: `nmdesa`, `nmkec`, `alamat`
- Kalau tidak ditemukan → return koordinat kantor desa
- **Ganti variabel**: `LAT_MIN`, `LAT_MAX`, `LON_MIN`, `LON_MAX` sesuai kabupatenmu

#### 6. Usage
Untuk mengisi latlong kosong di SBR

---

## Matcha Helper

### Required Columns
- `idsbr` (angka)
- `keberadaan_usaha` (0/1/3/4 sesuai Matchapro)
- `latitude`, `longitude` (pemisah desimal: titik, bukan koma)

### Langkah Kerja

1. **Import** → Run chunk "Import"
2. **Buka Chrome** → Run chunk "Buka Chrome & Redirect"
3. **Login SSO** → Run chunk "Login SSO BPS", sesuaikan variabel `sso` dan `pass`
4. **Import Spreadsheet** → Run chunk "import spreadsheet"
   - Ganti variabel `purekoord` dengan path file-mu
   - Support spreadsheet/Excel, pastikan sesuai required columns
5. **Fungsi Isi Otomatis** → Run chunk "Fungsi Isi Otomatis"
6. **Isi** → Run chunk "Isi"
7. **Isi Susulan** (opsional, kalau error) → Run chunk "Isi susulan"
   - Memisahkan latlong di luar range kabupaten
   - Memisahkan latlong non-angka
   - **Ganti variabel**: `LAT_MIN`, `LAT_MAX`, `LON_MIN`, `LON_MAX`
   - Baris yang dipisah disimpan di variabel `outlier` dan otomatis disave ke lokal

---

## Tips
- Pastikan latlong ada sebelum GC (re-GC tanpa latlong ribet)
- Entri paralel untuk speed maksimal
- Cek outlier setelah preprocessing

---

## Matcha Reverse

### Required Columns
- `idsbr` (angka)
- `keberadaan_usaha` (bebas latlong kosong, keberadaan_usaha full terisi)

### Langkah Kerja

1. **Import** → Run chunk "Import"
2. **Buka Chrome** → Run chunk "Buka Chrome n Redirect"
3. **Login SSO** → Run chunk "Login SSO BPS", sesuaikan variabel `sso` dan `pass`
4. **Import SBR** → Run chunk yang impor SBR clean
   - SBR harus clean (bebas latlong kosong, keberadaan_usaha full terisi)
5. **Fungsi** → Run chunk "Fungsi" (jalankan keduanya: Tandai dan Reverse)
6. **EKSEKUSI** → Run chunk "EKSEKUSI"

---

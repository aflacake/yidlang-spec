# Execution Gate - Formal Rules
Versi: 0.2
> Execution Gate bukan runtime.
> Ia adalah penjaga kelayakan.

Dokumen ini menetapkan **kapan program Yidlang boleh dieksekusi, ditolak, atau dihentikan,** berdasarkan **Decision Ledger**, bukan hasil evaluasi semata.

## 1. Definisi Normatif

Execution Gate adalah mekanisme statik–semantik yang:
- Membaca **Decision Ledger (final)**
- **Tidak** mengevaluasi ulang AST
- Menghasilkan satu **keputusan final** terhadap program

Keputusan ini **final** dan **tidak dapat dibatalkan.**

## 2. Status Akhir Program (EKSKLUSIF)
Program Yidlang tepat satu dari status berikut:
1. `EXECUTABLE`
2. `REJECTED`
3. `HALTED`

Tidak ada status keempat.

## 3. Syarat `EXECUTABLE`
Program **BOLEH DIEKSEKUSI** jika **SEMUA** terpenuhi:
1. Ledger **tidak terkunci dengan error**
2. Tidak ada `LEGITIMACY_VIOLATION`
3. Semua `inyen`:
   - Berstatus `resolved`
   - Tidak `halted`
   - Tidak `rejected`
4. Tidak ada `OPEN_THOUGHT`
5. Tidak ada nilai `safek`:
   - Kecuali disertai `emunah`
6. Jika `psak` ada:
   - Hanya satu
   - Berada di scope global
7. Tidak ada `halt` di scope global

Hasil:
- Status = `EXECUTABLE`

## 4. Syarat `HALTED`
Program **DIHENTIKAN DENGAN SADAR** jika:
- `halt` terjadi di scope global

Makna:
- Bukan kesalahan
- Bukan penolakan
- Pilihan sadar untuk tidak lanjut

Ledger:
- `INTENTIONAL_STOP`

Hasil:
- Status = `HALTED`

## 5. Syarat `REJECTED`
Program **DITOLAK** jika **salah satu benar**:
1. Ada LEGITIMACY_VIOLATION
2. Ada inyen berstatus rejected
3. Ada inyen berstatus halted
4. Ada OPEN_THOUGHT tersisa
5. Ada nilai safek tanpa emunah
6. `psak`:
   - Muncul di dalam `inyen`
   - atau lebih dari satu
7. Mutasi ledger setelah `FINAL_RULING`
   Makna:
   - Valid secara sintaks
   - Tidak layak dieksekusi

Hasil:
- Status = `REJECTED`

## 6. Urutan Evaluasi Gate (DETERMINISTIK)
Execution Gate **WAJIB** mengevaluasi dalam urutan berikut:
1. `HALTED` (global)
2. `LEGITIMACY_VIOLATION`
3. `inyen` status
4. `OPEN_THOUGHT`
5. `safek` tanpa `emunah`
6. `psak` validity

Urutan ini **tidak boleh diubah**.

## 7. Hubungan dengan psak
- `psak` **tidak mengeksekusi**
- psak **mengunci keputusan**
- Jika program `EXECUTABLE`:
   - psak menjadi **penanda final**
- Jika program `REJECTED`:
   - psak **tidak bermakna**

## 8. Diagnostic Wajib
Execution Gate **HARUS** menghasilkan satu diagnostic utama:
- Status akhir
- Alasan pertama yang menyebabkan status
- Referensi ledger entry
- Referensi komentar terdekat (jika ada)

## 9. Larangan Implementasi
Execution Gate **TIDAK BOLEH**:
- Mengubah ledger
- Mengevaluasi ulang ekspresi
- Menjalankan kode
- Mengabaikan urutan evaluasi

## 10. Penutup Normatif
> Tidak semua program ingin dijalankan.\
> Tidak semua yang ingin dijalankan pantas dijalankan.

Execution Gate menjaga batas itu.

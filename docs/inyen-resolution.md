# Inyen Resolution Rules

Versi: 0.2

> Inyen bukan fungsi./
> Ia adalah **perkara sadar** yang **wajib diselesaikan atau ditolak**.

Dokumen ini mengunci kapan inyen:
- Dianggap cukup
- Wajib ditutup
- Ditolak
- atau dihentikan dengan sadar

## 1. Definisi Normatif

Inyen adalah:
- Unit logika tertutup
- Scope keputusan sadar
- Tidak otomatis dieksekusi
- Tidak boleh bocor status
Setiap `inyen` **HARUS** berakhir dengan status resolusi eksplisit.

## 2. Status Resolusi Inyen (EKSKLUSIF)

Setiap inyen tepat satu dari:
- `resolved`
- `halted`
- `rejected`

Tidak ada status keempat.

## 3. Kapan Inyen WAJIB Ditutup
`inyen` **WAJIB** ditutup saat:
1. Traversal keluar dari scope `inyen`
2. `halt` dipanggil di dalam `inyen`
3. Execution Gate mengevaluasi kelayakan

Jika belum ditutup -> LEGITIMACY ERROR

## 4. Kriteria resolved
Sebuah `inyen` dianggap **resolved** jika **SEMUA** terpenuhi:
1. Minimal satu `zoger` dieksekusi
2. `trakht` -> `farshtey` terpenuhi untuk subject utama
3. Tidak ada `OPEN_THOUGHT` aktif
4. Tidak ada `LEGITIMACY_VIOLATION`
5. Nilai terakhir `zoger` tidak `safek` atau disertai `emunah`

Hasil:
- Nilai terakhir `zoger` menjadi hasil `inyen`

Ledger:
- `RESOLVE_MATTER (resolved)`

## 5. Kriteria halted
`inyen` menjadi **halted** jika:
- `halt` dieksekusi di dalam scope `inyen`

Makna:
- Penghentian sadar
- Bukan kegagalan

Ledger:
- `INTENTIONAL_STOP`
- `RESOLVE_MATTER (halted)`

Catatan:
- Nilai `inyen` = **undefined**
- Tidak boleh digunakan lanjut

## 6. Kriteria rejected
inyen **DITOLAK** jika salah satu benar:
- Tidak ada `zoger`
- Ada `OPEN_THOUGHT` yang belum ditutup
- Lompat tahap (`farshtey` tanpa `trakht`)
- Ekspresi utama menghasilkan `safek` tanpa `emunah`
- Terjadi `LEGITIMACY_VIOLATION`
- `psak` muncul di dalam `inyen`

Ledger:
- `RESOLVE_MATTER (rejected)`

Makna:
- Valid syntax
- Tidak layak digunakan

## 7. Aturan Pemakaian Nilai Inyen
Nilai `inyen` HANYA BOLEH dipakai jika:
- Status = `resolved`

Jika tidak:
- **STATIC LEGITIMACY ERROR**

## 8. Penutupan Implisit
Jika akhir scope tercapai dan:
- semua kriteria `resolved` terpenuhi
   -> penutupan **implisit diperbolehkan**

Jika tidak:
-> DITOLAK

## 9. Larangan Keras
Di dalam inyen:
- `psak` **DILARANG**
- Mengakses ledger luar DILARANG
- Mengubah `shteyger` global **DILARANG**

## 10. Diagnostic Normatif
Setiap penolakan inyen HARUS menghasilkan:
- Status (`rejected`/`halted`)
- Alasan spesifik
- Referensi komentar terdekat (jika ada)

## 11. Penutup Normatif
> Perkara yang tidak diselesaikan\
> bukan perkara yang gagal,\
> ia perkara yang tidak layak diteruskan.

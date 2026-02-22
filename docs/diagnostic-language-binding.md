# Diagnostic Language Binding
Versi: 0.2

> Diagnostic bukan error message.\
> Ia adalah jawaban bahasa terhadap kegagalan berpikir.

Dokumen ini menetapkan **bagaimana kondisi AST + Decision Ledger diterjemahkan menjadi pesan diagnostik Yidlang**, secara **konsisten**, **bermakna**, dan **tidak teknokratis**.

## 1. Prinsip Umum Diagnostik
- Diagnostik tidak netral
- Diagnostik tidak memaki
- Diagnostik tidak menyalahkan pengguna
- Diagnostik menjelaskan sebab, bukan sekadar akibat
- Diagnostik selalu dalam bahasa Yiddish Latin/YIVO

## 2. Struktur Diagnostik (WAJIB)
Setiap diagnostic **HARUS** memiliki:
1. **Judul (Kategori Kesadaran)**
2. **Isi Penjelasan**
3. **Sebab Normatif**
4. **Referensi Ledger**
5. **Referensi Konteks (Komentar)** (jika ada)

Format konseptual:
```
<Judul>:
<Penjelasan>

Sebab:
<alasan normatif>

Konteks:
<kutipan komentar atau lokasi>
```

## 3. Kategori Diagnostik (TERKUNCI)
Kategori **TIDAK BOLEH** ditambah sembarangan.

### 3.1 Unfarshteyn - **Tidak Dipahami**
Dipakai jika:
- Lompat tahap (`farshtey` tanpa `trakht`)
- Menggunakan nilai tanpa kesadaran awal

Makna:
> Program berjalan tanpa memahami apa yang sedang ia pegang.

### 3.2 Sofek — Keraguan Aktif
Dipakai jika:
- Nilai `safek` digunakan
- Tidak ada `emunah`

Makna:
> Keraguan diakui, tapi tidak ditanggung.

### 3.3 Zikhertkeyt - Keamanan Dilampaui
Dipakai jika:
- Melanggar aturan legitimasi
- Mutasi ledger ilegal
- Mengakses scope terlarang

Makna:
> Program melampaui batas yang dijaga.

### 3.4 Akhraies - Tanggung Jawab Tidak Diambil
Dipakai jika:
- `inyen` tidak diselesaikan
- `OPEN_THOUGHT` dibiarkan

Makna:
> Ada perkara yang ditinggalkan tanpa keputusan.

### 3.5 Shtil - Penghentian Sadar
Dipakai jika:
- Program `HALTED`

Makna:
> Program memilih diam.

### 3.6 Din — Putusan Final
Dipakai jika:
- `psak` sah
- Program `EXECUTABLE`

Makna:
> Keputusan telah dikunci.

## 4. Binding Ledger -> Diagnostic
Mapping **WAJIB** berikut:
| **Ledger Entry** | **Diagnostic** |
|---|---|
| `LEGITIMACY_VIOLATION` | `Zikhertkeyt` |
| `OPEN_THOUGHT` tersisa | `Akhraies` |
| `COMPARE` -> `safek` tanpa `emunah` | `Sofek` |
| Lompat tahap | `Unfarshteyn` |
| `INTENTIONAL_STOP` | `Shtil` |
| `FINAL_RULING` sah | `Din` |

## 5. Referensi Komentar
Jika diagnostic muncul:
- Compiler **HARUS** mencari komentar terdekat sebelumnya (`/>` atau `</ ... />`)
- Kutipan **boleh dipotong**
- Jika tidak ada -> konteks = lokasi kode

Makna:
> Komentar adalah suara program yang diingat kembali.

## 6. Prioritas Diagnostic
Jika lebih dari satu kondisi terjadi:
Urutan prioritas **TIDAK BOLEH DIUBAH**:
- `Shtil`
- `Zikhertkeyt`
- `Akhraies`
- `Sofek`
- `Unfarshteyn`
- `Din`

Hanya **SATU diagnostic utama** yang ditampilkan.

## 7. Larangan Bahasa
Diagnostic DILARANG:
- Menyebut istilah teknis (`stack`, `null`, `exception`)
- Mengutip grammar (`expected token`)
- Menyalahkan pengguna (“Anda salah”)

## Contoh Normatif
### Contoh 1 - Inyen Ditolak
```
Akhraies:
A case was opened
but never closed.

Because:
Inyen ends with an open mind.

Context:
/> A case was opened
```

### Contoh 2 - Keraguan Tanpa Emunah
```
Sofek:
Values ​​are used without the courage
to endure uncertainty.

Because:
safek is used without emunah.

Konteks:
(trakht proceed)
```

## 9. Penutup Normatif
> Bahasa yang baik\
> tidak hanya berkata “salah”,\
> tapi menjelaskan mengapa ia berhenti mendengarkan.

Diagnostic Language adalah **wajah Yidlang.**






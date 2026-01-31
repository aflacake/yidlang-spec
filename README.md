# Spesifikasi Yidlang
Versi: 0.2

## 1. Tujuan Utama
Yidlang dibuat untuk:
1. Memaksa kehati-hatian berpikir
   - Tidak cepat, tidak otomatis, tidak manja
   - Setiap aksi harus dipertimbangkan
2. Menolak abstraksi berlebihan
   - Tidak seperti Python atau JavaScript
   - Lebih dekat ke Assembly
3. Mencerminkan budaya berpikir Yahudi
   - Diskusi (dialog internal)
   - Keraguan produktif
   - Penundaan sebelum keputusan
   - Tanggung jawab moral atas aksi
4. Bukan bahasa untuk semua orang
   - Kategori: _advanced/difficult_
   - Cocok untuk:
      - Sistem eksperimental
      - VM/interpreter riset
      - Pendidikan how computers really think

## 2. Implementasi Teknis
C++ murni\
**C++17 atau C++20**, tanpa framework, STL secukupnya.

## 3. Identitas Bahasa
Yidlang **TIDAK**:
- Meniru Hebrew syntax
- Jadi bahasa religi

Yidlang **YA**:
- Reflektif
- Dialogis
- Sadar konsekuensi

Prinsip desain syntax:
- Kata kerja -> **aksi berpikir**
- Tidak langsung _“do”_, tapi _“consider”_
- Banyak perintah tidak langsung mengeksekusi

## 4. Format File
- `.yidl` -> **source utama**
- `.yid` -> **modul/fragmen/eksperimen**

## 5. Model Eksekusi
Yidlang **BUKAN**:
- _Purely imperative_
- _Purely functional_

Yidlang adalah:
**_Contemplative Execution Model_**

Artinya:
Ada perintah yang:
- Tidak langsung berjalan
- Bisa ditunda
- Bisa dibatalkan

Program = proses berpikir, bukan hanya hasil

## 6. Daftar Perintah
> **Di Yidlang, perbedaan utama dengan bahasa lain bukan “apa yang dilakukan komputer”, tapi “apa yang diakui oleh program sebagai telah dilakukan”.**

Ini kunci memahami.

Sengaja menghindari: `if`, `while`, `for`, `return`.

### 6.1 `trakht`
Makna Yiddish: _to think deeply_

```
trakht x
```

Fungsi:
- Menandai variabel/ekspresi untuk **dipertimbangkan**
- Belum dievaluasi

Berguna karena:
- Tidak ada bahasa lain yang **memisahkan berpikir dan eksekusi**
- Cocok untuk:
   - _Lazy evaluation_ sadar
   - Debugging logika

### 6.2 `farshtey`
Makna: _to understand_

```
farshtey x
```

Fungsi:
- Memaksa evaluasi hanya jika semua dependensi jelas
- Kalau tidak -> error (bukan runtime error biasa)

Ini **tidak ada** di bahasa lain:\
Biasanya bahasa **mengeksekusi meski programmer belum paham**

### 6.3 `shoyn`
Makna: _already_

```
shoyn x

/> Contoh lain:

zoger x = 10
shoyn x
```

`shoyn` mengecek status pengakuan tindakan, bukan status data.

Makna:
- `x` sudah diakui sebagai tindakan
- Bukan hanya “pernah dijalankan CPU”

| Bahasa lain | Yidlang |
| --- | --- |
| Fokus: nilai | Fokus: keputusan |
| `x == 10` | `x has been acknowledged` |
| _State_ data | _State_ kesadaran |

Kenapa ini baru & berguna?
- Mencegah aksi berulang tanpa sadar
- Cocok untuk:
   - Transaksi
   - _Side-effect_ berat
   - Operasi _irreversible_

> Dalam bahasa lain: “apakah ini terjadi?”
> Dalam Yidlang: “apakah kita sadar ini sudah terjadi?”

Bukan:
- Boolean flag
- Pengecekan variabel sudah diisi
- Sekadar `if executed_before`

Fungsi:
- Mengecek apakah suatu **aksi sudah dilakukan sebelumnya**
- Bukan sekadar boolean, tapi state kesadaran

Berguna:
- Idempotensi sadar
- Audit logika
- Anti double execution

### 6.4 `zoger`
Makna: _to say/declare aloud_

```
zoger x = 10

/> Contoh lain:

zoger balance = balance - 100
```

`zoger` adalah tindakan pengakuan eksplisit atas perubahan.

Makna:
- Programmer menyatakan secara sadar perubahan ini
- Dicatat dalam decision ledger internal

| Assignment biasa | `zoger` |
| --- | --- |
| Perubahan nilai | Pengakuan perubahan |
| Bisa implisit | Selalu eksplisit |
| Tidak tercatat | Tercatat sebagai keputusan |

Kenapa ini penting?\
Banyak bug terjadi bukan karena salah hitung, tapi karena tidak sadar telah mengubah sesuatu.

`zoger` memisahkan:
- Perhitungan
- Tanggung jawab

Ini tidak ada di bahasa lain secara eksplisit.

Bukan:
- Assignment biasa
- Deklarasi variabel ala C/`let`/`var`

Fungsi:
- Deklarasi **publik & sadar**
- Berbeda dengan assignment biasa

Insight:
- Semua assignment adalah tindakan moral
- `zoger` = _I acknowledge this change_

### 6.5 `halt`
Makna: _stop, but temporarily_

```
halt
```

Fungsi:
- Menghentikan alur **tanpa mengakhiri program**
- Bisa dilanjutkan dengan konteks utuh

- Tidak sama dengan:
- `break`
- `return`
- `exit`

Ini seperti **pause eksistensial**.

### 6.6 `ibertrakht`
Makna: _reconsider_

```
ibertrakht x
```

Fungsi:
- Membatalkan keputusan sebelumnya
- Rollback logika, bukan hanya data

Ini sangat unik:
- Bahasa lain rollback data
- Yidlang rollback keputusan

### 6.7 `oyshern`
Makna: _to exhaust_

```
oyshern possibilities
```

Fungsi:
- Memaksa semua kemungkinan dievaluasi
- Jika ada yang terlewat → error

Lebih ketat dari `switch exhaustive` di Rust.
 
Cocok untuk:
- Keamanan
- Logika kritis

### 6.8 `nisht`
Makna: _not/absence_

```
nisht x
```

Fungsi:
- Bukan boolean NOT
- Menyatakan **ketiadaan makna**, bukan `false`

Bedanya:
- `false` -> salah
- `nisht` -> tidak ada konsepnya

### 6.9 `shteyger`
Makna: _ladder/level_

```
shteyger 3
```

Fungsi:
- Menentukan level kesadaran eksekusi
- Level tinggi -> lambat tapi aman

Ini baru:\
Eksekusi sadar level, bukan optimization flag

### 6.10 `emunah`
Makna: `trust`

```
emunah x
```

Fungsi:

Mengizinkan eksekusi tanpa bukti lengkap

Dicatat sebagai faith-based execution

Cocok untuk:
- AI
- Heuristic
- Eksperimen

### 6.11 `psak`
Makna: _final ruling_

```
psak x
```

Fungsi:
- Mengunci status keputusan
- Setelah `psak`, `ibertrakht` ditolak

Aturan:
- Tidak boleh di dalam inyen
- Tidak menghasilkan nilai
- Hanya mengunci decision state

Kegunaan:
- Keputusan kritis
- Finalisasi proses berpikir

### 6.12 `inyen`
Makna: _matter/concern_

```
inyen name(param1, param2) {
    ...
}
```

Fungsi:
- Mendefinisikan urusan logika tertutup
- Tidak otomatis dieksekusi

Karakteristik:
- Tidak ada `return`
-  Nilai terakhir yang di- `zoger` = hasil
- Harus melalui `trakht` dan `farshtey` sebelum dipakai

Kegunaan:
- Modularitas tanpa lompat alur
- Function sebagai proses berpikir

### 6.13 Boolean Logic, Operator Logika, dan Short-Circuit ala Yidlang

#### _Logical State_ Resmi
| _State_ | Makna |
| `ken` | afirmatif |
| `safek` | ragu |
| `nisht` | ketiadaan makna |

#### Operator Logika
| Operator | Arti |
| `un` | AND sadar |
| `oder` |  OR sadar |
| `nit` | negasi |

#### _Short-Circuit_ Sadar
- `un` berhenti jika kiri = `nisht`
- `oder` berhenti jika kiri = `ken`
- Semua short-circuit **dicatat sebagai keputusan**

Catatan:\
Short-circuit di Yidlang bukan optimasi performa, melainkan keputusan etis untuk tidak melanjutkan pertimbangan.


### 6.1 Komentar
Komentar bukan sekadar catatan, tapi bagian dari dialog berpikir.

#### 1. Komentar satu baris: `/>`
```
/> trakht ini belum dievaluasi, sengaja ditahan
trakht x
```

Makna:
- Catatan reflektif
- Tidak memengaruhi eksekusi
- Bisa dibaca compiler untuk konteks pesan error

#### 2. Komentar multi-baris: `</ ... />`
```
</
Ini bukan sekadar loop.
Ini rangkaian kemungkinan
yang belum layak dijalankan.
Aku menahannya di sini.
/>
```

Karakteristik:
- Bebas format
- Tidak wajib teknis
- Tidak boleh digunakan untuk dokumentasi API formal
(itu nanti mekanisme lain)

#### 3. Komentar sebagai konteks error
Compiler/debugger boleh merujuk komentar terdekat:

```
Unfarshteyn:
Anda mengeksekusi sesuatu
yang sebelumnya Anda catat
sebagai belum layak.
```

## 7. Error Model
Yidlang error ≠ crash

Jenis error:
- Unfarshteyn -> tidak dipahami
- Zikhertkeyt -> melanggar kehati-hatian
- Sofek -> keraguan logis
- Akhraies -> tanggung jawab dilanggar

## 8. Penolakan Eksekusi Program
Program boleh menolak berjalan jika:
- Ada `trakht` tanpa `farshtey`
- Ada keputusan dibatalkan (`ibertrakht`) tapi tetap dipakai
- Ada `emunah` di level `shteyger` rendah
- Ada `oyshern` yang tidak lengkap

Penolakan **bukan error fatal**, tapi status:
```
Sofek:
Program ini belum selesai berpikir.
Eksekusi ditunda.
```

## 9. Memory & Decision Model
Yidlang **memisahkan memori menjadi dua lapisan** yang saling terkait tetapi **tidak identik**:
1. Memory Store -> menyimpan nilai
2. Decision Ledger -> menyimpan kesadaran, keputusan, dan status berpikir

> Dalam Yidlang, program bisa punya nilai yang benar, **namun tetap tidak layak dieksekusi**.

### 9.1 Prinsip Dasar
#### Prinsip 1 — Nilai ≠ Keputusan

Memori menyimpan apa,\
Ledger menyimpan mengapa dan apakah sudah disadari.

#### Prinsip 2 — Eksekusi Berbasis Keputusan

Tidak ada aksi kritis yang boleh dilakukan hanya karena:
- Nilai ada
- Ekspresi valid

Aksi **harus didukung status keputusan yang sah**.

### 9.2 _Memory Store_

Fungsi
- Menyimpan:
   - _Integer_
   - _String_
   - _Logical state_ (`ken/safek/nisht`)
   - hasil `inyen`
   
Karakteristik
- Bersifat **pasif**
- Tidak tahu:
   - Apakah nilai sudah dipahami
   - Apakah boleh dipakai
- Mirip memori tradisional (stack/heap), **tapi bukan penentu eksekusi**

Memory Store **tidak pernah menolak apa pun**.

### 9.3 _Decision Ledger_
#### Definisi
Decision Ledger adalah struktur pusat yang mencatat:
- Status berpikir
- Pengakuan
- Revisi
- Kepercayaan
- Finalisasi

Ledger **menentukan kelayakan eksekusi**, bukan Memory Store.

##### 9.3.1 Status yang Dicatat
Setiap identifier (variabel, hasil `inyen`, ekspresi) memiliki **_Decision Record_**.

Contoh status:

| Status | Asal |
| --- | --- |
| `Trakhted` | setelah `trakht` |
| `Understood` | setelah `farshtey` |
| `Acknowledged` | setelah `zoger` |
| `Reconsidered` | setelah `ibertrakht` |
| `Exhausted` | setelah `oyshern` |
| `FaithBased` | setelah `emunah` |
| `Finalized` | setelah `psak` |
| `Paused` | saat `halt` |
| `ShortCircuited` | akibat `un/oder` |

Satu identifier **bisa punya beberapa status**, kecuali yang saling eksklusif.

### 9.4 Alur Status Keputusan
#### 9.4.1 Alur Normal
```
trakht
  ↓
farshtey
  ↓
zoger
  ↓
(shoyn)
  ↓
psak (opsional)
```

Jika alur ini terputus -> status `Unfarshteyn` atau `Sofek`.

#### 9.4.2 Revisi
```
ibertrakht
  ↓
(status pengakuan dihapus)
```

- Nilai boleh tetap ada
- Tapi tidak sah untuk dipakai

### 9.5 Interaksi Perintah dengan Ledger
`trakht x`
- Ledger: `x -> Trakhted`

`farshtey x`
- Valid hanya jika:
   - `x` pernah `Trakhted`
- Ledger: `x -> Understood`

`zoger x = expr`
- Memory Store: simpan nilai
- Ledger:
   - Catat sebagai `Acknowledged`
   - Simpan _decision timestamp_

`shoyn x`
- Tidak mengubah nilai
- Mengakses _Ledger_:
   - Mengecek apakah `Acknowledged` ada

`ibertrakht x`
- Ledger:
   - Hapus Acknowledged
   - Hapus Finalized jika ada
- Nilai di Memory Store tidak dihapus

`oyshern x`
- Ledger:
   - Tandai `Exhausted`
- Jika belum lengkap:
   - Status global -> `Sofek`

`emunah x`
- Ledger:
   - Tandai `FaithBased`
- Risiko dicatat untuk evaluasi eksekusi

`psak x`
- Ledger:
   - Tandai `Finalized`
- Setelah ini:
   - `ibertrakht x`, ditolak
   - Status dikunci

### 9.6 Short-Circuit & Ledger
Short-circuit **selalu tercatat**.

Contoh:
```
ken oder expensive_check
```

Ledger mencatat:
- Operand kiri -> `Evaluated`
- Operand kanan -> `ShortCircuited`
- Alasan: sudah cukup untuk melanjutkan

Ini penting untuk:
- Audit logika
- Dialog debugging

### 9.7 Ledger & `inyen`
Saat `inyen` **didefinisikan**:
- Ledger mencatat **struktur**, bukan eksekusi

Saat `inyen` dipakai:
1. Hasil masuk _Memory Store_
2. Ledger mencatat:
   - Status berpikir
   - Status pengakuan (zoger)
   - Ketergantungan keputusan

`psak` **tidak boleh** berada di dalam `inyen`.

### 9.8 Program Menolak Berjalan

Program ditolak untuk eksekusi penuh jika:
- Ada identifier `Trakhted` tanpa `Understood`
- Ada `FaithBased` di `shteyger` rendah
- Ada `Exhausted` yang tidak lengkap
- Ada konflik status (mis. `Finalized` + `Reconsidered`)

Penolakan berbentuk **dialog**, contoh:
```
Sofek:
Keputusan ini belum selesai dipikirkan.
```

### 9.9 Perbedaan dengan Bahasa Lain
| Bahasa lain | Yidlang |
| --- | --- |
| Stack/Heap | Memory + Decision Ledger |
| Control flow | Decision flow |
| Error | Dialog |
| Boolean | Logical state |
| Optimization | Ethical short-circuit |


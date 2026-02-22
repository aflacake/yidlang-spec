# Minimal Conformance Test
Versi: 0.2

> Jika dua implementasi lulus tes ini,\
> maka **mereka berperilaku sama**.

Dokumen ini mendefinisikan **kasus minimum yang WAJIB menghasilkan hasil identik**\
di semua implementasi Yidlang.

Ini **bukan unit test runtime**,\	
melainkan **tes kesesuaian makna bahasa**.

## 1. Prinsip Umum Conformance
1. Semua kasus di sini WAJIB DIDUKUNG
2. Output yang diuji adalah:
   - **Status Execution Gate**
   - **Kategori Diagnostic**
3. Implementasi **TIDAK BOLEH**:
   - Menambah status baru
   - Mengubah alasan utama
4. Perbedaan teks detail diagnostic **boleh**,
kategori & sebab **HARUS SAMA**

## 2. Kategori Hasil yang Diuji
Setiap program tepat satu dari:
- `EXECUTABLE`
- `REJECTED`
- `HALTED`

Dengan diagnostic utama:
- `Din`
- `Akhraies`
- `Sofek`
- `Unfarshteyn`
- `Zikhertkeyt`
- `Shtil`

## 3. Kasus A - VALID & EXECUTABLE
### A1 - Program Minimal Sadar
```
/> Kita mulai dengan keyakinan penuh
zoger x = 5
trakht x
farshtey x
shoyn x
psak x
```

Harapan:
- Status: `EXECUTABLE`
- Diagnostic: `Din`
- Tidak ada `OPEN_THOUGHT`
- Tidak ada `safek`

### A2 - Inyen Terselesaikan
```
inyen decide() {
    trakht a
    zoger a = 10
    farshtey a
}
```
Harapan:
- Inyen: `resolved`
- Status program: `EXECUTABLE`
- Diagnostic: `Din`

## 4. Kasus B - VALID tapi REJECTED
### B1 - Inyen Tidak Ditutup
```
inyen stuck() {
    trakht a
    zoger a = 3
}
```
Harapan:
- Inyen: `rejected`
- Status: `REJECTED`
- Diagnostic: `Akhraies`

### B2 - Safek tanpa Emunah
```
zoger x = safek
trakht x
farshtey x
```

Harapan:
- Status: `REJECTED`
- Diagnostic: `Sofek`

### B3 - Lompat Tahap
```
farshtey y
```

Harapan:
- Status: `REJECTED`
- Diagnostic: `Unfarshteyn`

### B4 - Psak di Dalam Inyen
```
inyen wrong() {
    zoger x = 1
    psak x
}
```

Harapan:
- Status: `REJECTED`
- Diagnostic: `Zikhertkeyt`

## 5. Kasus C - VALID tapi HALTED
### C1 - Halt Global
```
/> Program memilih berhenti
halt
zoger x = 5
```

Harapan:
- Status: `HALTED`
- Diagnostic: `Shtil`
- Ledger tidak error

### C2 - Halt di Dalam Inyen
```
inyen pause() {
    zoger x = 1
    halt
}
```

Harapan:
- Inyen: `halted`
- Status: `REJECTED`
- Diagnostic: `Akhraies`

## 6. Kasus D - Short-Circuit Logika
### D1 - un Short-Circuit

```
zoger x = nisht un (5 / 0)
trakht x
farshtey x
```

Harapan:
- Tidak ada error pembagian
- Ledger mencatat `SHORT_CIRCUIT`
- Status: `EXECUTABLE`

### D2 - `oder` Short-Circuit
```
zoger y = ken oder (5 / 0)
trakht y
farshtey y
```

Harapan:
- Operand kanan tidak dievaluasi
- Status: `EXECUTABLE`

## 7. Kasus E - Komentar sebagai Konteks
```
/> Ini belum layak dijalankan
farshtey x
```

Harapan:
- Status: `REJECTED`
- Diagnostic: `Unfarshteyn`
- Diagnostic **merujuk komentar**

## 8. Aturan Lulus Conformance
Sebuah implementasi **LULUS** jika:
- Semua kasus di atas:
   - Menghasilkan status yang benar
   - Menghasilkan kategori diagnostic yang benar
- Tidak ada tambahan error
- Tidak ada kasus ambigu

## 9. Penutup Normatif
> Spesifikasi tanpa tes\
> hanyalah puisi.

Dengan dokumen ini:
- Spesifikasi tertutup
- Makna terkunci
- Implementasi bebas, tapi setia


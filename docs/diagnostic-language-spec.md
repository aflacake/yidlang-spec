# Diagnostic Language Spec

## Prinsip Dasar (WAJIB)
1. Diagnostic bukan error message
2. Diagnostic bukan exception
3. Diagnostic adalah dialog kesadaran
4. Bahasa diagnostic = Yiddish dasar (Latin/YIVO)
5. Tidak menjelaskan “cara memperbaiki” secara teknis
   -> hanya menyatakan keadaan

> Yidlang tidak menyalahkan, ia menyatakan.

## Struktur Diagnostic
Setiap diagnostic terdiri dari 3 lapis:
```
<Sheym>:
<Text>
[Context]
```

### 1.1 Sheym (Nama Keadaan)
Nama tetap, terbatas, dan bermakna etis.

### 1.2 `Text` (Pernyataan)
- Kalimat pendek
- Tidak emosional
- Tidak menggurui
- Tidak teknis berlebihan

### 1.3 `Context` (Opsional)
- Bisa merujuk `inyen`, `decision`, atau komentar terdekat
- Tidak wajib selalu ada

## Daftar Diagnostic Resmi
### 2.1 `Unfarshteyn` (Tidak Dipahami)
Dipakai ketika:
- Loncat status (`trakht` -> `psak`)
- `psak` tanpa kesadaran cukup
- Transisi sadar ilegal

Template:
```
Unfarshteyn:
Der gedank iz nisht genug farshteyt.
```

Contoh dengan konteks:
```
Unfarshteyn:
Du host gemakht a psak
on keyn farshteyn.

Kontekst:
decision: allow_entry
```

### 2.2 `Sofek` (Keraguan)
Dipakai ketika:
- Ekspresi menghasilkan `safek`
- Keputusan belum cukup kuat untuk dipercaya
- Ledger menunjukkan ambiguitas

Template:
```
Sofek:
Der matsev iz nisht klor.
```

### 2.3 `Akhraies` (Tanggung Jawab)
Dipakai ketika:
- Ada `inyen` terbuka
- Program diblokir secara etis
- Execution Gate = `blokirt`

Template:
```
Akhraies:
S'iz do a inyen
vos iz nokh nisht farendikt.
```


2.4 `Shtil` (Diam/Henti Sadar)
Dipakai ketika:
- Program berhenti dengan `halt`
- Tidak ada kesalahan
- Tidak ada penolakan

Template:
```
Shtil:
Der program hot zikh opgehaltn
mit a klor viln.
```

### 2.5 Emes (Dibenarkan / Diizinkan)
Dipakai ketika:
- Execution Gate = `muter`
- Program boleh lanjut ke runtime

Template:
```
Emes:
Der program darf vaytergayn.
```

> Emes **bukan sukses**, tapi **izin**.

## Hal yang SENGAJA TIDAK ADA

Tidak ada:
- Stack trace
- Line number default
- “Expected X but got Y”
- Saran teknis eksplisit

Kenapa?\
Karena itu bahasa mesin, bukan bahasa kesadaran.

_(Informasi teknis boleh ada di developer mode, tapi *bukan bagian bahasa.)_

## Integrasi dengan Komentar (`/>` dan `</ ... />`)
Compiler/debugger **BOLEH** mengutip komentar terdekat.

Contoh:
```
/> trakht ini belum layak dijalankan
psak decision
```

Diagnostic:
```
Unfarshteyn:
Du host gemakht a psak
af epes vos du aleyn host gezen
az s'iz nokh nisht laykht.

Tsitat:
"trakht ini belum layak dijalankan"
```

Komentar = suara programmer masa lalu.

## Mapping ke Sistem (C++)
### 5.1 Enum Diagnosti
```
enum class DiagnosticSheym {
    Unfarshteyn,
    Sofek,
    Akhraies,
    Shtil,
    Emes
};
```

### 5.2 Struktur Diagnostic
```
struct Diagnostic {
    DiagnosticSheym sheym;
    std::string text;
    std::optional<std::string> kontekst;
};
```

## Siapa yang Menghasilkan Apa
| Komponen                  | Diagnostic                  |
| --- | --- |
| Parser                    | Tidak                           |
| Grammar                   | Tidak                           |
| Static Legitimacy Checker | `Unfarshteyn`               |
| Decision Ledger           | `Sofek`                     |
| Execution Gate            | `Akhraies`, `Shtil`, `Emes` |

Tidak ada overlap.

## Prinsip Terakhir
Yidlang tidak berkata:\
“Programmu salah.”

Ia berkata:
- Belum dipahami
- Belum diselesaikan
- Belum layak
- Sudah cukup

Itu perbedaan filosofis yang bisa diimplementasikan.


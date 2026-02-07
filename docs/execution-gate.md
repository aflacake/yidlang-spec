# Execution Gate

## Posisi & Fungsi

Execution Gate berada setelah:
1. Grammar & AST (selesai)
2. Static Legitimacy Checker (selesai)
3. Decision Ledger terisi (selesai)

Dan **sebelum**:
- Runtime
- Evaluasi ekspresi lanjutan
- “hasil apa pun”

> Execution Gate tidak mengeksekusi. Ia memutuskan apakah eksekusi pantas terjadi.

## Pertanyaan Tunggal Execution Gate

> “Darf der program vaytergayn?”
> (Bolehkah program melanjutkan?)

Jawabannya **bukan boolean**, tapi **keputusan sadar**.

## Keadaan Global Program
Execution Gate membaca empat sumber kebenaran:
1. Hasil **Static Legitimacy Checker**
2. Status **Decision Ledger**
3. Apakah ada `halt`
4. Status semua `inyen`

## Status Akhir Program
Ini bukan error code, tapi putusan gerbang.

```
enum class ProgramMatsev {
    muter,        // diizinkan lanjut
    blokirt,      // diblokir (inyen terbuka)
    opgehaltn,    // dihentikan sadar (halt)
    opgeleynt     // ditolak (legitimacy)
};
```

## Aturan Keputusan
## 4.1 Prioritas (atas -> bawah)
1. Legitimacy ditolak -> `opgeleynt`
2. Ada `halt` -> `opgehaltn`
3. Ada `inyen` `oyf`/`ibertrakht` -> `blokirt`
4. Semua bersih -> `muter`

Tidak ada negosiasi.

## Model C++
```
class ExecutionGate {
public:
    ProgramMatsev evaluate(
        const StaticLegitimacyChecker& checker,
        const DecisionLedger& ledger
    ) const;
};
```

## Implementasi Inti
```
ProgramMatsev ExecutionGate::evaluate(
    const StaticLegitimacyChecker& checker,
    const DecisionLedger& ledger
) const {

    if (checker.isRejected()) {
        return ProgramMatsev::opgeleynt;
    }

    if (checker.isHalted()) {
        return ProgramMatsev::opgehaltn;
    }

    if (ledger.hasOpenInyen()) {
        return ProgramMatsev::blokirt;
    }

    return ProgramMatsev::muter;
}
```

## Konsekuensi Setiap Status
`opgeleynt`
- Program tidak layak
- Diagnostic: Unfarshteyn
- Tidak ada runtime

`blokirt`
- Program benar tapi belum selesai secara etis
- Diagnostic: Akhraies
- Menunggu resolusi inyen

`opgehaltn`
- Program berhenti atas kehendak sendiri
- Diagnostic: netral
- Bukan kegagalan

`muter`
- Program boleh melanjutkan
- Runtime dimulai
- Ledger tetap aktif

## Contoh Dialog Diagnostic
### Kasus `blokirt`
```
Akhraies:
Der program hot genug farshteyn,
ober s'iz nokh do a inyen vos iz oyf.
Keyn vaytergayn.
```

### Kasus `opgehaltn`
```
Shtil:
Der program hot zikh opgehaltn
mit a klor viln.
```

## Kenapa Execution Gate BUKAN Runtime Check
| Runtime Check         | Execution Gate           |
| --- | --- |
| Terjadi saat eksekusi | Terjadi sebelum eksekusi |
| Reaktif               | Preventif                |
| Error-oriented        | Responsibility-oriented  |
| Bisa diabaikan        | Tidak bisa dilewati      |

Execution Gate tidak bisa dimatikan, karena ia adalah Yidlang.

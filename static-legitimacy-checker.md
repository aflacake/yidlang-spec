# Static Legitimacy Checker

## Posisi dalam Arsitektur

Urutan pipeline Yidlang (FIX):
1. Parsing (Grammar EBNF)
2. AST terbentuk
3. Static Legitimacy Checker <- TAHAP INI
4. Decision Ledger initialization
5. Execution Gate
6. Runtime (jika lolos)

> Static Legitimacy Checker bekerja SEBELUM runtime, tapi berpikir seperti conscience, bukan compiler biasa.

## Tujuan Utama
Static Legitimacy Checker menjawab satu pertanyaan:
> “Apakah program ini layak dipertimbangkan untuk dijalankan?”

Bukan:
- Apakah benar
- Apakah efisien
- Apakah hasilnya sesuai harapan

## Hal yang DICEK
### 2.1 Transisi Kesadaran ILEGAL
Contoh ILEGAL:
```
zoger x = 5
psak x
```

`psak` tanpa:
- `trakht`
- `farshtey`
- `shoyn`

Checker:
> Unfarshteyn:
> Du host gemakht a psak on keyn farshteyn.

### 2.2 `inyen` yang Tidak Pernah Ditutup
```
inyen trust
...
// EOF
```

ILEGAL walau grammar valid.

Aturan:
- Semua inyen HARUS
   - `psak`
   - atau `nisht`
   - atau program `halt`
   
### 2.3 `inyen` Dipakai sebagai Nilai
```
zoger x = trust_context
```

ILEGAL

Karena:
- `inyen` bukan expression
- Tidak punya nilai

### 2.4 `halt` dalam Konteks Salah
```
inyen a() {
    halt
}
```

ILEGAL

Aturan:
- `halt` **hanya sah di level program**
- Bukan di dalam `inyen`

### 2.5 Loncat Status (Skip Consciousness)
```
trakht x
psak x
```

ILEGAL

Wajib:
```
trakht -> farshtey -> shoyn -> psak

```

## Bentuk Kesalahan
Static Legitimacy Checker **TIDAK**:
- Throw syntax error
- Throw type error
- Crash

Ia menghasilkan penolakan sadar.

Contoh pesan: 
```
Unfarshteyn:
Du host ibergegangen a shtepn fun trakht tsu psak.
Der gedank iz nisht genug reyf.
```

## Model Data (C++)
### 4.1 Status Tracker (per identifier)
```
struct ConsciousTrack {
    std::string name;
    EntitetTip tip;
    Matsev current;
};
```

### 4.2 Checker Class
```
class StaticLegitimacyChecker {
public:
    void registerInyen(const std::string& name);
    void registerZoger(const std::string& name);

    void onTrakht(const std::string& name);
    void onFarshtey(const std::string& name);
    void onShoyn(const std::string& name);
    void onIbertrakht(const std::string& name);
    void onPsak(const std::string& name);
    void onNisht(const std::string& name);

    void onHalt();

    void finalize(); // dipanggil di EOF

private:
    std::unordered_map<std::string, ConsciousTrack> tracks;
    bool halted = false;
};
```

## Contoh Aturan di Kode
### 5.1 Mencegah Loncat Status
```
void StaticLegitimacyChecker::onPsak(const std::string& name) {
    auto& t = tracks.at(name);

    if (t.current != Matsev::shoyn &&
        t.current != Matsev::emunah &&
        t.current != Matsev::oyshern) {

        throw LegitimacyRejection(
            "Unfarshteyn",
            "A psak on keyn shoyn iz nisht muter."
        );
    }
}
```

### 5.2 EOF Check (`finalize()`)
```
void StaticLegitimacyChecker::finalize() {
    if (halted) return;

    for (const auto& [_, t] : tracks) {
        if (t.tip == EntitetTip::inyen &&
            (t.current == Matsev::oyf ||
             t.current == Matsev::ibertrakht)) {

            throw LegitimacyRejection(
                "Akhraies",
                "S'iz do a inyen vos iz nokh oyf."
            );
        }
    }
}
```

## Hubungan dengan Decision Ledger
| Checker | Ledger |
| --- | --- |
| Menolak transisi ilegal | Menyimpan status sah |
| Bekerja statik          | Bekerja dinamis      |
| Satu lintasan AST       | Runtime-aware        |
| Tidak simpan sejarah    | Simpan sejarah       |

Checker = penjaga gerbang etika\
Ledger = catatan hidup

## Kenapa Ini BUKAN Type Checker
| Type Checker  | Static Legitimacy Checker |
| --- | --- |
| Fokus nilai   | Fokus niat                |
| Salah = error | Tidak layak = ditolak     |
| Boolean       | Kesadaran                 |
| Universal     | Yidlang-only              |

# Decision Ledger Model

## Prinsip Desain
- Ledger ≠ Memory
   - Memory -> nilai (`zoger`)
   - Ledger -> status kesadaran & kelayakan
- Tidak ada implicit state
- Semua istilah status = Yiddish (Latin/YIVO)
- Ledger memutuskan “boleh/tidak”, bukan “benar/salah”

## Enum Status Dasar
```
enum class Matsev {        // מצב — state
    trakht,               // dipikirkan
    farshtey,             // dipahami
    shoyn,                // disadari
    ibertrakht,           // dipertimbangkan ulang
    oyshern,              // dituntaskan (habis)
    emunah,               // dipercaya
    farendikt,            // diselesaikan
    opgeleynt,            // ditolak
    oyf,                  // terbuka (khusus inyen)
    blokirt               // menghalangi eksekusi
};
```

Catatan:
- `oyf` hanya valid untuk `inyen`
- `blokirt` bukan state permanen, tapi hasil evaluasi global

## Tipe Entitas Ledger
Ledger **tidak menyimpan nilai, hanya entitas sadar.**
```
enum class EntitetTip {
    zoger,      // nilai
    psak,       // keputusan
    inyen       // perkara
};
```

## Struktur Entri Ledger
```
struct LedgerEntry {
    std::string name;          // identifier
    EntitetTip tip;            // jenis entitas
    Matsev matsev;              // status sadar
    std::vector<Matsev> tarih;  // riwayat status
};
```

Makna:
- `tarih` = sejarah kesadaran (penting untuk dialog debugger)
- Tidak boleh dihapus
- Tidak boleh diubah langsung (HARUS lewat API)

## Decision Ledger
```
class DecisionLedger {
public:
    void openInyen(const std::string& name);
    void trakht(const std::string& name);
    void farshtey(const std::string& name);
    void shoyn(const std::string& name);
    void ibertrakht(const std::string& name);
    void oyshern(const std::string& name);
    void emunah(const std::string& name);
    void psak(const std::string& name);
    void nisht(const std::string& name);

    bool hasOpenInyen() const;
    bool isBlocked() const;

    const LedgerEntry& get(const std::string& name) const;

private:
    std::unordered_map<std::string, LedgerEntry> entries;
};
```

## Aturan Transisi Status
### 5.1 `inyen`

| Dari | Boleh ke |
| --- | --- |
| `oyf` | `ibertrakht`, `farendikt`, `opgeleynt` |
| `ibertrakht` | `farendikt`, `opgeleynt` |
| `farendikt` | Tidak |
| `opgeleynt` | Tidak |

5.2 `zoger`/`psak`

| Dari | Boleh ke |
| --- | --- |
| `trakht` | `farshtey` |
| `farshtey` | `shoyn` |
| `shoyn` | `emunah`, `oyshern` |
| `emunah` | `farendikt` |
| `oyshern` | `farendikt` |

Loncat status -> invalid program, bukan crash

## Implementasi Kunci
Membuka `inyen`

```
void DecisionLedger::openInyen(const std::string& name) {
    entries[name] = LedgerEntry{
        name,
        EntitetTip::inyen,
        Matsev::oyf,
        { Matsev::oyf }
    };
}
```

Menyelesaikan `inyen`
```
void DecisionLedger::psak(const std::string& name) {
    auto& e = entries.at(name);

    if (e.tip == EntitetTip::inyen &&
        (e.matsev == Matsev::oyf || e.matsev == Matsev::ibertrakht)) {
        e.matsev = Matsev::farendikt;
        e.tarih.push_back(Matsev::farendikt);
        return;
    }

    // psak untuk decision biasa (aturan lain)
}
```

Mengecek Blokir Global
```
bool DecisionLedger::hasOpenInyen() const {
    for (const auto& [_, e] : entries) {
        if (e.tip == EntitetTip::inyen &&
           (e.matsev == Matsev::oyf || e.matsev == Matsev::ibertrakht)) {
            return true;
        }
    }
    return false;
}

bool DecisionLedger::isBlocked() const {
    return hasOpenInyen();
}
```

## Hubungan dengan Eksekusi
> Decision Ledger adalah gerbang terakhir.

Decision Ledger adalah gerbang terakhir.
```
if (ledger.isBlocked()) {
    throw YidlangRejection(
        "Akhraies",
        "S'iz do a inyen vos iz nokh nisht farendikt."
    );
}
```

Bukan exception runtime\
Iya: Penolakan sadar

## Kenapa Model Ini KUAT
- Bisa diunit-test
- Bisa di-serialize (audit)
- Bisa dipakai debugger dialog
- Tidak tergantung syntax
- Tidak bisa “diakali” oleh programmer

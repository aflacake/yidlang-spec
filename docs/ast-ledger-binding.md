# AST
Versi: 0.2

> Ledger bukan log. Ledger adalah ingatan keputusan.

Dokumen ini mendefinisikan **bagaimana setiap node AST memengaruhi Decision Ledger**,
bukan bagaimana ia dieksekusi.

## 1. Prinsip Umum Binding
1. **Setiap mutasi ledger harus berasal dari node AST**
2. Tidak ada mutasi implisit
3. Satu node -> satu atau lebih **ledger entry**
4. Ledger **append-only**, kecuali status transisi eksplisit
5. Setelah `psak`, ledger **read-only**

## 2. Struktur Konseptual Ledger
Setiap entry minimal memiliki:
- `subject` -> identifier/expression/context
- `node_type`
- `action` -> contemplate, acknowledge, reconsider, trust, exhaust, rule
- `state_before`
- `state_after`
- `confidence_level`
- `position` (file, line)

> Struktur detail akan difinalkan di model data C++

## 3. Binding per AST Node
### 3.1 `ProgramNode`
- Membuka ledger baru
- Menetapkan konteks global

Ledger:
- `OPEN_PROGRAM`

### 3.2 `CommentNode`
- **Tidak membuat entry**
- Dapat ditautkan sebagai context_ref

### 3.3 `LevelNode` (`shteyger`)
Ledger:
- `SET_CAUTION_LEVEL`

Efek:
- Mengubah ambang toleransi `safek`
- Berlaku global hingga diubah

### 3.4 `DeclarationNode` (`zoger`)
Ledger:
- `DECLARE_VALUE`

Isi:
- Nilai hasil evaluasi
- Status awal: `declared`

Catatan:
- Jika ekspresi `safek`, confidence turun

### 3.5 `ContemplationNode` (`trakht`)
Ledger:
- `OPEN_THOUGHT`

Efek:
- Subject masuk status `contemplated`

Larangan:
- Duplikasi tanpa `ibertrakht`

### 3.6 `UnderstandingNode` (`farshtey`)
Ledger:
- `MARK_UNDERSTOOD`

Syarat:
- Harus ada `OPEN_THOUGHT`

Gagal jika:
- Lompat tahap

### 3.7 `AcknowledgmentNode` (`shoyn`)
Ledger:
- ACK_RESULT

Makna:
- Menyadari hasil
- Tidak mengunci

### 3.8 `ReconsiderationNode` (`ibertrakht`)

Ledger:
- `REOPEN_CONSIDERATION`

Efek:
- Confidence turun
- Status kembali `open`

### 3.9 `ExhaustionNode` (`oyshern`)

Ledger:
- `MARK_EXHAUSTED`

Makna:
- Semua kemungkinan telah dipikirkan

### 3.10 `TrustNode` (`emunah`)

Ledger:
- `TRUST_WITH_GAP`

Makna:
- Melanjutkan meski belum lengkap
- Dicatat sebagai assumption

### 3.11 `PauseNode` (`halt`)

Ledger:
- `INTENTIONAL_STOP`

Efek:
- Eksekusi berhenti
- Ledger tetap terbuka

### 3.12 `RulingNode` (`psak`)

Ledger:
- `FINAL_RULING`

Efek:
- Mengunci ledger
- Menandai keputusan final

Larangan:
- Tidak boleh di dalam `inyen`


## 4. Inyen Binding Khusus

### 4.1 InyenNode

Ledger:
- `OPEN_MATTER`

Scope:
- Ledger sub-scope
- Terisolasi dari global

### 4.2 InyenBodyNode
Ledger:
- Semua entry ditandai `scope=inyen:name`

Aturan:
- Harus menghasilkan minimal satu `DECLARE_VALUE`
- Harus ditutup eksplisit atau implisit

### 4.3 Resolusi Inyen

Saat penutupan:

Ledger:
- `RESOLVE_MATTER`

Isi:
- Nilai terakhir `zoger`
- Status resolusi:
   - `resolved`
   - `halted`
   - `rejected`

## 5. Expression Binding

### 5.1 Logical Expression (`un`/`oder`)

Ledger:
- `EVAL_LOGICAL`

Jika short-circuit:
- Tambahan entry `SHORT_CIRCUIT`

Isi:
- Operand yang dihentikan
- Alasan

### 5.2 Relational Expression

Ledger:
- `COMPARE`

Hasil:
- `ken`|`safek`|`nisht`

## 6. Binding Error → Ledger

Jika terjadi pelanggaran:
- Entry `LEGITIMACY_VIOLATION`
- Referensi node + komentar terdekat
- Tidak langsung crash

## 7. Penutup Normatif
> AST menentukan apa yang mungkin
> Ledger menentukan apa yang diakui

Tanpa ledger, AST hanyalah struktur.\
Tanpa AST, ledger hanyalah opini.

# Traversal
Versi: 0.2

> Traversal bukan sekadar urutan kunjungan.
> Traversal adalah urutan tanggung jawab.

Dokumen ini mendefinisikan bagaimana urutan traversal AST menyebabkan mutasi Decision Ledger.

## 1. Prinsip Umum Mutasi

1. Mutasi hanya **terjadi saat traversal**
2. Tidak ada mutasi di fase build AST
3. Setiap mutasi:
   - Memiliki sebab (node)
   - Memiliki urutan
4. Mutasi **tidak dapat dibatalkan**, hanya ditandai ulang
5. Ledger dibaca oleh Execution Gate, bukan traversal

## 2. Fase Traversal Global

Traversal dilakukan dalam empat fase kaku:

### 2.1 Phase I — Structural Pass

Tujuan:
- Memastikan AST lengkap
- Menolak node terlarang secara struktural

Ledger:
- TIDAK dimodifikasi

### 2.2 Phase II — Awareness Pass

Tujuan:
- Mencatat niat berpikir
- Membuka konteks

Ledger:
- `OPEN_PROGRAM`
- `OPEN_MATTER`
- `OPEN_THOUGHT`

Node aktif:
- `ProgramNode`
- `InyenNode`
- `ContemplationNode`

### 2.3 Phase III — Evaluation Pass

Tujuan:
- Evaluasi ekspresi
- Mutasi status dan nilai

Ledger:
- Semua mutasi utama (`DECLARE`, `COMPARE`, dll)

Node aktif:
- `DeclarationNode`
- `ExpressionNode`
- `UnderstandingNode`
- `AcknowledgmentNode`
- `ReconsiderationNode`
- `TrustNode`
- `ExhaustionNode`

### 2.4 Phase IV — Resolution Pass

Tujuan:
- Mengunci atau menghentikan

Ledger:
- `RESOLVE_MATTER`
- `FINAL_RULING`
- `INTENTIONAL_STOP`

Node aktif:
- `RulingNode`
- `PauseNode`

## 3. Aturan Traversal Umum
### 3.1 Urutan Statement
- Selalu top-down
- Tidak ada lompat
- `halt` menghentikan traversal Phase III & IV

### 3.2 Expression Traversal
- Left -> Right
- Tidak ada reorder
- Parent menunggu child selesai

## 4. Mutasi Spesifik per Node
### 4.1 `ProgramNode`
Traversal:
- Phase II

Ledger:
- `OPEN_PROGRAM` (sekali)

### 4.2 `InyenNode`

Traversal:
- Phase II -> buka scope
- Phase IV -> tutup scope

Ledger:
- `OPEN_MATTER`
- `RESOLVE_MATTER`

### 4.3 DeclarationNode

Traversal:
1. Traverse expression
2. Validasi hasil
3. Commit ke ledger

Ledger:
- `DECLARE_VALUE`

### 4.4 `ContemplationNode`

Traversal:
- Phase II

Ledger:
- `OPEN_THOUGHT`

Larangan:
- Duplikat tanpa `ibertrakht`

### 4.5 UnderstandingNode

Traversal:
- Phase III

Ledger:
- `MARK_UNDERSTOOD`

### 4.6 Logical Expression (`un`/`oder`)

Traversal:
1. Traverse left
2. Evaluasi short-circuit
3. Jika lanjut -> traverse right

Ledger:
- `EVAL_LOGICAL`
- Opsional `SHORT_CIRCUIT`

### 4.7 `ReconsiderationNode`

Traversal:
- Phase III

Ledger:
- `REOPEN_CONSIDERATION`

Efek:
- Menurunkan confidence

### 4.8 `ExhaustionNode`

Traversal:
- Phase III

Ledger:
- `MARK_EXHAUSTED`

### 4.9 `TrustNode`

Traversal:
- Phase III

Ledger:
- `TRUST_WITH_GAP`

### 4.10 `PauseNode` (`halt`)

Traversal:
- Phase IV (immediate)

Ledger:
- `INTENTIONAL_STOP`

Efek:
- Semua traversal berhenti

### 4.11 RulingNode (psak)

Traversal:
- Phase IV

Ledger:
- `FINAL_RULING`

Efek:
- Ledger terkunci

## 5. Short-Circuit Ledger Rules

Jika short-circuit terjadi:

Ledger wajib mencatat:
- Operator
- Operand dihentikan
- Nilai penentu
- Alasan sadar

Traversal:
- Operand kanan **tidak ditraverse**

## 6. Mutasi yang DILARANG

Traversal **TIDAK BOLEH**:
- Menghapus entry ledger
- Mengubah urutan entry
- Menulis setelah `FINAL_RULING`
- Menyisipkan mutasi tanpa node

## 7. Konsistensi & Determinisme

Jika AST sama:
- Traversal sama
- Ledger **identik**
- Execution Gate hasilnya sama

Ini **kontrak implementasi**.

## 8. Penutup Normatif
> Traversal adalah bentuk etika.
> Urutan yang dilanggar, keputusan yang cacat.


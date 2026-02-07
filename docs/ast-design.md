# AST Design
**Node Types & Traversal Order**
Versi: 0.2

> AST Yidlang bukan pohon instruksi, melainkan peta proses berpikir yang dibatasi legitimasi.

AST tidak menjamin eksekusi.
Ia hanya menjamin struktur kesadaran.

## 1. Prinsip Dasar AST Yidlang
1. AST dibangun penuh sebelum validasi legitimasi
2. Tidak ada node implisit
3. Tidak ada transformasi kontrol-alur tersembunyi
4. Urutan traversal selalu deterministik
5. Evaluasi ≠ Eksekusi

## 2. Root Node
### 2.1 `ProgramNode`
**Peran**: wadah semua pernyataan sadar
```
ProgramNode
 ├─ StatementNode*
```

Karakteristik:
- Tidak memiliki nilai
- Menyimpan konteks global
- Menjadi input awal:
   - Static Legitimacy Checker
   - Execution Gate

Traversal:
- Sequential
- Tidak boleh lompat

## 3. Statement Nodes
Semua statement adalah **unit sadar**, bukan aksi langsung.

### 3.1 `CommentNode`
- Disimpan di AST
- Tidak dievaluasi
- Bisa direferensikan diagnostik

```
CommentNode
 ├─ text
 ├─ position
```

Traversal:
- Dilewati evaluator
- Dibaca diagnostic engine

### 3.2 `LevelNode` (`shteyger`)
```
LevelNode
 ├─ level_value : integer
```

Efek:
- Mengubah ambang kehati-hatian global
- Dicatat di ledger

Traversal:
- Dievaluasi segera
- Tidak menghasilkan nilai

### 3.3 `DeclarationNode` (`zoger`)
```
DeclarationNode
 ├─ identifier
 ├─ ExpressionNode
```

Makna:
- Pengakuan nilai sadar
- Bisa gagal jika ekspresi belum sah

Traversal:
- Evaluasi ekspresi
- Catat nilai
- Catat status kesadaran

### 3.4 `ContemplationNode` (`trakht`)
```
ContemplationNode
 ├─ identifier
```

Makna:
- Membuka ruang pikir
- Wajib sebelum farshtey

Traversal:
- Tidak menghasilkan nilai
- Mengubah status identifier -> contemplated

### 3.5 `UnderstandingNode` (`farshtey`)
```
UnderstandingNode
 ├─ identifier
```

Syarat:
- Identifier harus sudah trakht

Traversal:
- Mengubah status -> understood
- Gagal jika loncat tahap

### 3.6 `AcknowledgmentNode` (`shoyn`)
```
AcknowledgmentNode
 ├─ identifier
```

Makna:
- Kesadaran atas hasil
- Tidak mengunci

### 3.7 `ReconsiderationNode` (`ibertrakht`)
```
ReconsiderationNode
 ├─ identifier
```

Makna:
- Menandai keraguan ulang
- Menurunkan legitimasi sementara

### 3.8 ExhaustionNode (oyshern)
```
ExhaustionNode
 ├─ identifier
```

Makna:
- Menyatakan semua kemungkinan telah dipertimbangkan

### 3.9 `TrustNode` (`emunah`)
```
TrustNode
 ├─ identifier
```

Makna:
- Mengizinkan kelanjutan meski tidak lengkap

### 3.10 `PauseNode` (`halt`)
```
PauseNode
```

Makna:
- Menghentikan eksekusi
- AST tetap valid

Traversal:
- Menyetop evaluator
- Bukan error

### 3.11 `RulingNode` (`psak`)
```
RulingNode
 ├─ identifier
```

Makna:
- Keputusan final
- Mengunci ledger

Aturan:
- Tidak boleh di dalam inyen
- Tidak menghasilkan nilai

## 4. Inyen Node (Unit Logika Sadar)
### 4.1 `InyenNode`
```
InyenNode
 ├─ name
 ├─ parameters*
 ├─ InyenBodyNode
```

Karakteristik:
- Tidak otomatis dieksekusi
- Membentuk ruang keputusan tertutup

### 4.2 `InyenBodyNode`
```
InyenBodyNode
 ├─ InyenStatementNode*
```

Aturan penting:
- **Wajib resolusi** (lihat spesifikasi resolusi inyen)
- Nilai terakhir `zoger` -> hasil

Traversal:
- Linear
- Tidak boleh `psak`

## 5. Expression Nodes
Ekspresi selalu **pure**, tapi **dicatat efeknya**.

### 5.1 `ExpressionNode` (abstrak)
Turunan:
- `LogicalExpressionNode`
- `RelationalExpressionNode`
- `ArithmeticExpressionNode`
- `LiteralNode`
- `IdentifierNode`
- `NegationNode`

### 5.2 `LogicalExpressionNode`
```
LogicalExpressionNode
 ├─ left
 ├─ operator (un | oder)
 ├─ right
```

Traversal:
1. Evaluasi kiri
2. Short-circuit sadar
3. Catat keputusan berhenti/lanjut

### 5.3 `RelationalExpressionNode`
Hasil:
- `ken`
- `safek`
- `nisht`

Tidak ada boolean cast implisit.

## 6. Traversal Order (Global)
### 6.1 Fase AST
1. Build Phase
   - Bangun AST lengkap
2. Legitimacy Scan
   - Validasi struktur
3. Evaluation Phase
   - Evaluasi node satu per satu
4. Execution Gate
   - Izinkan/Tolak

### 6.2 Aturan Traversal Umum
- Pre-order untuk statement
- Left-to-right untuk ekspresi
- Tidak ada backtracking implisit
- Semua keputusan dicatat

## 7. Hal yang Sengaja Tidak Ada di AST
- Control flow node (`IfNode`, `LoopNode`)
- Jump node
- Implicit cast
- Optimizer node

## 8. Penutup Normatif
> AST Yidlang tidak bertanya bagaimana cepat tapi apakah pantas dilanjutkan.

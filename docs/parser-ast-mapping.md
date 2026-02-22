# Parser → AST Mapping
Versi: 0.2

> Parser tidak memutuskan makna.
> Parser hanya memetakan bentuk ke struktur.

Dokumen ini menetapkan **bagaimana hasil parsing grammar EBNF dipetakan ke node AST**,\
serta **apa yang wajib ditolak sebelum masuk ke tahap legitimasi**.

## 1. Prinsip Umum Mapping
1. Parser **tidak membaca Decision Ledger**
2. Parser **tidak mengevaluasi ekspresi**
3. Parser **tidak menolak karena makna**
4. Parser **hanya menolak karena struktur**
5. Mapping **harus deterministik**

Jika grammar valid -> AST terbentuk.
Layak atau tidak -> **bukan urusan parser**.

## 2. Batas Tanggung Jawab Parser
Parser **BERTANGGUNG JAWAB** atas:
- Struktur AST
- Scope (`program`, `inyen`)
- Urutan statement
- Hirarki ekspresi

Parser **TIDAK BERTANGGUNG JAWAB** atas:
- `trakht` -> `farshtey` urutan
- `safek` tanpa `emunah`
- `inyen` resolved atau tidak
- `psak` legality

## 3. Mapping Program-Level
### 3.1 program
Grammar:
```
program = { statement } ;
```

AST:
```
ProgramNode
 └─ statements: [ StatementNode* ]
```

Catatan:
- Urutan **dipertahankan**
- Program kosong -> AST valid

## 4. Mapping Statement -> Node
| Grammar Rule | AST Node |
| --- | --- |
| `comment` | `CommentNode` |
| `declaration` (`zoger`) | `DeclarationNode` |
| `contemplation` (`trakht`) | `ContemplationNode` |
| `understanding` (`farshtey`) | `UnderstandingNode` |
| `acknowledgment` (`shoyn`) | `AcknowledgmentNode` |
| `reconsideration` (`ibertrakht`) | `ReconsiderationNode` |
| `exhaustion` (`oyshern`) | `ExhaustionNode` |
| `trust` (`emunah`) | `TrustNode` |
| `pause` (`halt`) | `PauseNode` |
| `level` (`shteyger`) | `LevelNode` |
| `ruling` (`psak`) | `RulingNode` |
| `inyen_definition` | `InyenNode` |
| `expression_statement` | `ExpressionStatementNode` |

Parser WAJIB membuat node meskipun:
- secara semantik akan ditolak nanti

## 5. Mapping Inyen
### 5.1 Struktur
Grammar:
```
inyen name(param1, ...) { ... }
```

AST:
```
InyenNode
 ├─ name: Identifier
 ├─ parameters: [ Identifier* ]
 └─ body: InyenBodyNode
      └─ statements: [ InyenStatementNode* ]
```
Catatan:
- Parser **membuka scope**
- Scope **ditutup oleh parser**, bukan traversal

### 5.2 Larangan Parser
Parser **HARUS MENOLAK** jika:
- Kurung `{}` tidak seimbang
- Parameter duplikat
- `inyen` bersarang (nested inyen)

Ini structural error, bukan legitimasi.

## 6. Mapping Expression
### 6.1 Ekspresi Umum
Grammar:
```
expression = logical_expression ;
```
AST:
```
ExpressionNode
 └─ LogicalExpressionNode
```
Parser **TIDAK BOLEH**:
- Melakukan short-circuit
- Menggabungkan operand

### 6.2 Logical Expression
Grammar:
```
logical_expression
 = relational_expression
 | logical_expression logical_operator relational_expression ;
```
AST (left-associative):
```
LogicalExprNode
 ├─ left: ExpressionNode
 ├─ operator: un | oder
 └─ right: ExpressionNode
```

### 6.3 Relational Expression
Grammar:
```
a > b
```
AST:
```
RelationalExprNode
 ├─ left: ArithmeticExprNode
 ├─ operator: >
 └─ right: ArithmeticExprNode
```

### 6.4 Arithmetic Expression
Parser membangun pohon sesuai precedence:
```
ArithmeticExprNode
 ├─ left
 ├─ operator (+ - * /)
 └─ right
```

### 6.5 Negation
Grammar:
```
nit x
```

AST:
```
NegationNode
 └─ operand
```

Parser TIDAK melakukan evaluasi boolean.

## 7. Identifier & Literal Mapping
###7.1 Identifier
Grammar:
```
identifier
```
AST:
```
IdentifierNode(name)
```

### 7.2 Literal
Grammar:
```
integer | string | logical_state
```

AST:
```
LiteralNode(type, value)
```
Nilai `ken | safek | nisht`:
- Disimpan sebagai literal logika
- Tidak di-cast

## 8. Error Kategori Parser (WAJIB)
Parser **HANYA BOLEH** menghasilkan error berikut:
1. **SyntaxError**
2. **UnexpectedToken**
3. **UnbalancedStructure**
4. **DuplicateParameter**
5. **NestedInyenError**

Parser DILARANG:
- Menyebut `safek`
- Menyebut `emunah`
- Menyebut legitimasi

## 9. Ambiguitas yang DILARANG
Parser **HARUS** menolak:
- Operator tanpa operand
- Expression tanpa penutup
- Statement kosong di `inyen`

## 10. Penutup Normatif
> Parser membentuk tulang.\
> Makna datang kemudian.

Parser yang terlalu pintar\
akan merusak bahasa ini.

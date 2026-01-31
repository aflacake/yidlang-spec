# Grammar EBNF
Versi: 0.2

## 1. Struktur Program
```
program
    = { statement } ;
```

> Program = rangkaian keputusan sadar
> Valid syntax ≠ layak dieksekusi

## 2. _Statement_
```
statement
    = comment
    | declaration
    | contemplation
    | understanding
    | acknowledgment
    | reconsideration
    | exhaustion
    | trust
    | pause
    | level
    | ruling
    | inyen_definition
    | expression_statement
    ;
```

## 3. Komentar
```
comment
    = single_comment
    | multi_comment ;

single_comment
    = "/>" , { character } , newline ;

multi_comment
    = "</" , newline ,
      { character | newline } ,
      "/>" ;
```

## 4. Deklarasi & Pengakuan

### 4.1 zoger — deklarasi sadar
```
declaration
    = "zoger" , identifier , "=" , expression ;
```

## 5. Kontemplasi & Pemahaman
```
contemplation
    = "trakht" , identifier ;

understanding
    = "farshtey" , identifier ;
```

## 6. Status & Keputusan
```
acknowledgment
    = "shoyn" , identifier ;

reconsideration
    = "ibertrakht" , identifier ;

exhaustion
    = "oyshern" , identifier ;

trust
    = "emunah" , identifier ;

pause
    = "halt" ;

level
    = "shteyger" , integer ;
```

## 7. Keputusan Final
```
ruling
    = "psak" , identifier ;
```
Aturan semantik (di luar EBNF):
- Tidak boleh di dalam inyen
- Tidak menghasilkan nilai
- Mengunci decision state

## 8. Unit Logika Sadar
```
inyen_definition
    = "inyen" , identifier ,
      "(" , [ parameter_list ] , ")" ,
      "{" , { inyen_statement } , "}" ;
```

```
parameter_list
    = identifier , { "," , identifier } ;
```

```
inyen_statement
    = comment
    | declaration
    | contemplation
    | understanding
    | acknowledgment
    | reconsideration
    | exhaustion
    | trust
    | pause
    | level
    | expression_statement ;
```

Tidak ada _return_\
Nilai terakhir yang di- `zoger` = hasil `inyen`

## 9. _Expression Statement_
```
expression_statement
    = expression ;
```

## 10. Ekspresi Lengkap (V0.2)

### 10.1 Hirarki Ekspresi	
```
expression
    = logical_expression ;

```

### 10.2 _Logical Expression_ (_Short-Circuit_ Sadar)
```
logical_expression
    = relational_expression
    | logical_expression , logical_operator , relational_expression ;
```

```
logical_operator
    = "un"     /> AND sadar
    | "oder" ; /> OR sadar
```

Short-circuit:
- `un` berhenti jika kiri = `nisht`
- `oder` berhenti jika kiri = `ken`
- Semua dicatat dalam decision ledger

### 10.3 _Relational Expression_
```
relational_expression
    = arithmetic_expression
    | arithmetic_expression , relation_operator , arithmetic_expression ;
```

```
relation_operator
    = "==" | "!=" | "<" | ">" | "<=" | ">=" ;
```

Hasil: ken | safek | nisht

### 10.4 _Arithmetic Expression_
```
arithmetic_expression
    = term
    | arithmetic_expression , ("+" | "-") , term ;
```

### 10.5 _Term_
```
term
    = factor
    | term , ("*" | "/") , factor ;
```

### 10.6 _Factor & Negation_
```
factor
    = literal
    | identifier
    | "(" , expression , ")"
    | negation ;

negation
    = "nit" , factor ;
```

## 11. _Literal & Logical State_
```
literal
    = integer
    | string
    | logical_state ;
```

```
logical_state
    = "ken"
    | "safek"
    | "nisht" ;
```

## 12. Simbol Dasar
```
identifier
    = letter , { letter | digit | "_" } ;

integer
    = digit , { digit } ;

string
    = '"' , { character } , '"' ;
```

## 13. Hal yang SENGAJA TIDAK ADA
- `if`/`else`
- `while`/`for`
- `return`
- _Implicit boolean cast_
- _Automatic control flow_

Semua alur terjadi melalui kesadaran & keputusan.












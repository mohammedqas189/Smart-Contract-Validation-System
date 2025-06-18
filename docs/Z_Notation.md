# V. Formal Specification (Z Notation)

## Basic Types

```z
[CONTRACT, ERROR, VERSION, REPORT]
ERROR ::= DeadCode | Overflow | AccessControl | Reentrancy
```

---

## ValidationSystem Schema

```z
ValidationSystem
  contracts: CONTRACT \u21a3 VERSION       -- stores uploaded contracts
  results: CONTRACT \u21a3 \u211a ERROR         -- holds error sets for validated contracts
  validated: \u211a CONTRACT               -- lists all analyzed contracts

where
  dom results \u2286 validated \u2227 validated \u2286 dom contracts
```

---

## AddContract Schema

```z
AddContract
  \u2206ValidationSystem
  newContract?: CONTRACT
  ver?: VERSION

where
  newContract? \u2209 dom contracts
  contracts'  = contracts \u222a {newContract? \u21a6 ver?}
  results'    = results
  validated'  = validated
```

---

## ParseContract Schema

```z
ParseContract
  \u2205ValidationSystem
  contract?: CONTRACT
  parsed!: BOOL

where
  contract? \u2208 dom contracts
  parsed!   = TRUE
```

---

## ValidateContract Schema

```z
ValidateContract
  \u2206ValidationSystem
  contract?: CONTRACT
  errors!: \u211a ERROR

where
  contract? \u2208 dom contracts
  results'   = results \u2295 {contract? \u21a6 errors!}
  validated' = validated \u222a {contract?}
  contracts' = contracts
```

---

## ViewValidationReport Schema

```z
ViewValidationReport
  \u2205ValidationSystem
  contract?: CONTRACT
  report!: \u211a ERROR

where
  contract? \u2208 validated
  report!   = results(contract?)
```

---

## ExportReport Schema

```z
ExportReport
  \u2205ValidationSystem
  contract?: CONTRACT
  exportedReport!: \u211a ERROR

where
  contract?       \u2208 validated
  exportedReport! = results(contract?)
```

---

## CompareContracts Schema

```z
CompareContracts
  \u2205ValidationSystem
  contract1?, contract2?: CONTRACT
  diff!: \u211a ERROR

where
  contract1? \u2208 validated
  contract2? \u2208 validated
  addedErrors!   = results(contract2?) \ results(contract1?)
  removedErrors! = results(contract1?) \ results(contract2?)
```

---

## RemoveContract Schema

```z
RemoveContract
  \u2206ValidationSystem
  contract?: CONTRACT

where
  contract? \u2208 dom contracts
  contracts'  = {contract?} \u22a4 contracts
  results'    = {contract?} \u22a4 results
  validated'  = validated \ {contract?}
```

---

## ResetSystem Schema

```z
ResetSystem
  \u2206ValidationSystem

where
  contracts'  = \u2205
  results'    = \u2205
  validated'  = \u2205

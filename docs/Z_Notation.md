# V. Formal Specification (Z Notation)

## Basic Types
```z
[CONTRACT, ERROR, VERSION, REPORT]
ERROR ::= DeadCode | Overflow | AccessControl | Reentrancy


ValidationSystem
  contracts: CONTRACT ↣ VERSION      -- stores uploaded contracts
  results: CONTRACT ↣ ℙ ERROR        -- holds error sets for validated contracts
  validated: ℙ CONTRACT              -- lists all analyzed contracts
where
  dom results ⊆ validated ∧ validated ⊆ dom contracts



AddContract
  ΔValidationSystem
  newContract?: CONTRACT
  ver?: VERSION
where
  newContract? ∉ dom contracts
  contracts' = contracts ∪ {newContract? ↦ ver?}
  results' = results
  validated' = validated



ParseContract
  ΞValidationSystem
  contract?: CONTRACT
  parsed!: BOOL
where
  contract? ∈ dom contracts
  parsed! = TRUE


ValidateContract
  ΔValidationSystem
  contract?: CONTRACT
  errors!: ℙ ERROR
where
  contract? ∈ dom contracts
  results' = results ⊕ {contract? ↦ errors!}
  validated' = validated ∪ {contract?}
  contracts' = contracts

ViewValidationReport
  ΞValidationSystem
  contract?: CONTRACT
  report!: ℙ ERROR
where
  contract? ∈ validated
  report! = results(contract?)

ExportReport
  ΞValidationSystem
  contract?: CONTRACT
  exportedReport!: ℙ ERROR
where
  contract? ∈ validated
  exportedReport! = results(contract?)

CompareContracts
  ΞValidationSystem
  contract1?, contract2?: CONTRACT
  diff!: ℙ ERROR
where
  contract1? ∈ validated
  contract2? ∈ validated
  addedErrors! = results(contract2?) \ results(contract1?)
  removedErrors! = results(contract1?) \ results(contract2?)

RemoveContract
  ΔValidationSystem
  contract?: CONTRACT
where
  contract? ∈ dom contracts
  contracts' = {contract?} ⩤ contracts
  results' = {contract?} ⩤ results
  validated' = validated \ {contract?}

ResetSystem
  ΔValidationSystem
where
  contracts' = ∅
  results' = ∅
  validated' = ∅

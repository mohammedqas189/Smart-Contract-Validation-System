# VI. Predicate Logic

## Variables

- `c`: A smart contract  
- `x`: A line or segment of code  
- `v`: A vulnerability  
- `r`: A validation report  
- `d`: A developer/user  
- `v1`, `v2`: Two versions of a contract  

---

## Predicates

- `Submitted(c, d)`: Contract `c` is submitted by developer `d`  
- `ValidContract(c)`: Contract `c` is valid (i.e., passes validation)  
- `HasVuln(c, v)`: Contract `c` has vulnerability `v`  
- `IsDeadCode(x)`: Code segment `x` is unreachable/dead  
- `IsNumericError(x)`: Code segment `x` has a numeric overflow/underflow  
- `IsReentrant(x)`: Code segment `x` has a reentrancy issue  
- `HasPoorAccessControl(x)`: Code `x` has access control flaws  
- `InContract(x, c)`: Code segment `x` belongs to contract `c`  
- `Detects(system, v)`: The system can detect vulnerability `v`  
- `Reports(r, c, v)`: Report `r` mentions vulnerability `v` in contract `c`  
- `FixSuggestion(r, v)`: Report `r` includes a remedy for vulnerability `v`  
- `Compares(v1, v2, diff)`: The system compares two versions and outputs `diff`  

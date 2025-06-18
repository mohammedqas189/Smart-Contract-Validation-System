# VIII. Temporal Logic

## Atomic Propositions

- **Submitted(c)**: Contract `c` has been submitted by a developer.  
- **Parsed(c)**: Contract `c` has been successfully parsed.  
- **Validated(c)**: Contract `c` has completed validation analysis.  
- **ReportGenerated(c)**: Validation report for contract `c` has been generated.  
- **ReportDisplayed(c)**: Report for contract `c` is displayed to user.  
- **ReportExported(c)**: Report for contract `c` has been exported.  

### Vulnerability Detection

- **HasDeadCode(c)**: Contract `c` contains unreachable code.  
- **HasOverflow(c)**: Contract `c` has numeric overflow issues.  
- **HasAccessControl(c)**: Contract `c` has access control flaws.  
- **HasReentrancy(c)**: Contract `c` has reentrancy vulnerabilities.  
- **VulnDetected(c, v)**: Vulnerability `v` detected in contract `c`.  
- **ValidContract(c)**: Contract `c` is valid (no vulnerabilities found).  

### System States

- **SystemReady**: System is operational and ready to accept contracts.  
- **ProcessingActive(c)**: System is actively processing contract `c`.  
- **ValidationComplete(c)**: All validation checks completed for contract `c`.  
- **WithinTimeLimit(c)**: Processing of contract `c` completed within 3 seconds.  

---

## 1. Safety Properties (Invariance - □)

- **S1: System Consistency**  
  □(∀c (ReportGenerated(c) → Validated(c)))  
  *A report can only be generated after validation is complete.*

- **S2: Parse Before Validate**  
  □(∀c (Validated(c) → Parsed(c)))  
  *Validation can only occur after successful parsing.*

- **S3: Submit Before Parse**  
  □(∀c (Parsed(c) → Submitted(c)))  
  *Parsing can only occur after contract submission.*

- **S4: No False Negatives**  
  □(∀c ∀v (VulnDetected(c, v) → HasVuln(c, v)))  
  *If the system detects a vulnerability, it actually exists.*

- **S5: Valid Contract Safety**  
  □(∀c (ValidContract(c) → ¬(HasDeadCode(c) ∨ HasOverflow(c) ∨ HasAccessControl(c) ∨ HasReentrancy(c))))  
  *Valid contracts have no detected vulnerabilities.*

---

## 2. Temporal Safety Properties (Until Operators - U)

- **TS1: No Premature Valid Status**  
  □(∀c (Submitted(c) → ¬ValidContract(c) U Validated(c)))  

- **TS2: No Premature Report Generation**  
  □(∀c (Submitted(c) → ¬ReportGenerated(c) U Validated(c)))  

- **TS3: No Premature Validation**  
  □(∀c (Submitted(c) → ¬Validated(c) U Parsed(c)))  

- **TS4: No Premature Processing**  
  □(∀c (Submitted(c) → ¬ProcessingActive(c) U SystemReady))  

- **TS5: No Premature Display**  
  □(∀c (Submitted(c) → ¬ReportDisplayed(c) U ReportGenerated(c)))  

- **TS6: No Premature Export**  
  □(∀c (Submitted(c) → ¬ReportExported(c) U ReportGenerated(c)))  

- **TS7: Vulnerability Detection Ordering**  
  □(∀c ∀v (Submitted(c) → ¬VulnDetected(c, v) U ProcessingActive(c)))  

---

## 3. Liveness Properties (Guarantee - ◇)

- **L1: Processing Guarantee**  
  □(∀c (Submitted(c) → ◇Validated(c)))  

- **L2: Report Generation Guarantee**  
  □(∀c (Validated(c) → ◇ReportGenerated(c)))  

- **L3: Display Guarantee**  
  □(∀c (ReportGenerated(c) → ◇ReportDisplayed(c)))  

- **L4: Vulnerability Detection Guarantee**  
  □(∀c ∀v (HasVuln(c, v) → ◇VulnDetected(c, v)))  

---

## 4. Response Properties (p → ◇q)

- **R1: Submission Response**  
  □(∀c (Submitted(c) → ◇ReportDisplayed(c)))  

- **R2: Parsing Response**  
  □(∀c (Submitted(c) → ◇Parsed(c)))  

- **R3: Export Response**  
  □(∀c (ReportGenerated(c) → ◇ReportExported(c)))  

- **R4: Performance Response**  
  □(∀c (Submitted(c) → ◇WithinTimeLimit(c)))  

---

## 5. Precedence Properties (p → q U r)

- **P1: Parsing Precedence**  
  □(∀c (Validated(c) → Parsed U Validated(c)))  

- **P2: Submission Precedence**  
  □(∀c (Parsed(c) → Submitted U Parsed(c)))  

- **P3: System Ready Precedence**  
  □(∀c (ProcessingActive(c) → SystemReady U ProcessingActive(c)))  

---

## 6. Workflow Sequence Properties

- **W1: Complete Processing Sequence**  
  □(∀c (Submitted(c) → ○◇(Parsed(c) ∧ ○◇(Validated(c) ∧ ○◇ReportGenerated(c)))))  

- **W2: Immediate Next Step**  
  □(∀c (Submitted(c) ∧ SystemReady → ○ProcessingActive(c)))  

- **W3: Validation Completion**  
  □(∀c (ProcessingActive(c) → ◇ValidationComplete(c)))  

---

## 7. Recurrence Properties (□◇p)

- **REC1: System Availability**  
  □◇SystemReady  

- **REC2: Processing Capacity**  
  □◇(∀c ¬ProcessingActive(c))  

---

## 8. Stability Properties (◇□p)

- **ST1: Report Persistence**  
  □(∀c (ReportGenerated(c) → ◇□ReportGenerated(c)))  

- **ST2: Validation Result Persistence**  
  □(∀c (ValidContract(c) → ◇□ValidContract(c)))  

---

## 9. Correlation Properties (◇p → ◇q)

- **C1: Vulnerability-Report Correlation**  
  □(∀c ∀v (◇VulnDetected(c, v) → ◇ReportGenerated(c)))  

- **C2: Processing-Completion Correlation**  
  □(∀c (◇ProcessingActive(c) → ◇ValidationComplete(c)))  

---

## 10. Performance & Non-Functional Properties

- **NF1: Time Constraint**  
  □(∀c (Submitted(c) → ◇³ValidationComplete(c)))  

- **NF2: System Responsiveness**  
  □(Submitted(c) → ○ProcessingActive(c))  

- **NF3: Concurrent Processing Limit**  
  □(|{c : ProcessingActive(c)}| ≤ MaxConcurrent)  

---

## 11. Error Handling Properties

- **E1: Parse Error Handling**  
  □(∀c (Submitted(c) ∧ ¬◇Parsed(c) → ◇ReportGenerated(c)))  

- **E2: Graceful Degradation**  
  □(¬SystemReady → ◇SystemReady)  

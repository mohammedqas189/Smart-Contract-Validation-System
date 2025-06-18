# Test Cases – FSM Transitions

This table simulates finite state machine transitions for the contract validation system.

| Test Case ID     | Input Sequence                                                                 | Expected States                                                                                                               |
|------------------|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| TC-01            | UploadContract                                                                 | Idle → Contract Uploaded                                                                                                      |
| TC-02            | UploadContract → ParseTriggered                                                | Idle → Contract Uploaded → Parsing Contract                                                                                  |
| TC-03            | UploadContract → ParseTriggered → CheckIssues                                  | Idle → Contract Uploaded → Parsing Contract → Checking Issues                                                                 |
| TC-04            | UploadContract → ParseTriggered → CheckIssues → DetectBoth                     | Idle → Contract Uploaded → Parsing Contract → Checking Issues → [Detecting Common Issue + Detecting Re-entrancy]             |
| TC-05            | UploadContract → ParseTriggered → CheckIssues → DetectBoth → GenerateReport    | Idle → Contract Uploaded → Parsing Contract → Checking Issues → [Detecting Common Issue + Detecting Re-entrancy] → Generating Report |
| TC-06            | UploadContract → ParseTriggered → CheckIssues → DetectBoth → GenerateReport → DisplayResult | Idle → Contract Uploaded → Parsing Contract → Checking Issues → [Detecting Common Issue + Detecting Re-entrancy] → Generating Report → Displaying Result |
| TC-07            | UploadContract → ParseTriggered → ParseError                                   | Idle → Contract Uploaded → Parsing Contract → Error Occurred                                                                  |
| TC-08            | UploadContract → ParseTriggered → ParseError → Reset                           | Idle → Contract Uploaded → Parsing Contract → Error Occurred → Idle                                                           |
| TC-09            | UploadContract → ParseTriggered → ParseError → Reset → UploadContract          | Idle → Contract Uploaded → Parsing Contract → Error Occurred → Idle → Contract Uploaded                                       |
| TC-10            | UploadContract → ParseTriggered → ParseError → Reset → UploadContract → ParseTriggered | Idle → Contract Uploaded → Parsing Contract → Error Occurred → Idle → Contract Uploaded → Parsing Contract                     |
| TC-11 (Invalid)  | ParseTriggered without UploadContract                                          | **Invalid transition:** FSM should remain in Idle or trigger error – no state change allowed                                  |




# Recruitment Candidate Management Test Plan

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Artifact Type:** Test Plan
**Status:** Complete

## 1. Purpose

This test plan defines the approach used to evaluate the OrangeHRM Recruitment candidate-creation workflow.

The test cycle was designed to determine whether a user could successfully create a candidate, whether required and optional fields behaved appropriately, whether email and résumé validation worked as expected, and whether a newly created candidate could subsequently be retrieved.

Because the OrangeHRM OS demo is a publicly shared environment, the plan also accounted for the possibility that records or application state could change because of activity outside the test session.

---

## 2. Test Objectives

The objectives of the test cycle were to:

* Verify access to the Add Candidate workflow.
* Verify enforcement of required candidate-name fields.
* Confirm that Middle Name remains optional.
* Verify that Email is required.
* Verify rejection of clearly malformed email values.
* Confirm that optional candidate fields can remain blank.
* Verify upload of a supported résumé file.
* Determine the actual résumé file-size boundary enforced by the public demo.
* Verify successful candidate creation.
* Verify that a newly created candidate can subsequently be retrieved.
* Identify additional usability, validation, or accessibility risks through exploratory testing.
* Distinguish product behavior from limitations caused by the shared public-demo environment.

---

## 3. Scope

### In Scope

The scripted test cycle covered:

* Recruitment candidate list access
* Add Candidate form access
* Candidate Name

  * First Name
  * Middle Name
  * Last Name
* Email
* Optional-field behavior
* Résumé upload
* Résumé file-size validation
* Candidate creation
* Candidate retrieval after creation
* Validation messages associated with the tested fields

Exploratory testing additionally examined:

* Date of Application behavior
* Date-entry and calendar interaction
* Keyboard navigation
* Focus visibility
* Tab and Shift+Tab behavior
* Keyboard-only interaction with the form
* Shared-environment stability

### Out of Scope

The following areas were not comprehensively tested:

* Vacancy management
* Candidate interview workflows
* Candidate status transitions
* Hiring or rejection workflows
* Authentication and authorization
* Role-based permissions
* API testing
* Database-level validation
* Performance or load testing
* Security penetration testing
* Cross-browser compatibility
* Mobile or responsive-layout testing
* Comprehensive WCAG conformance testing
* Exhaustive résumé-format testing
* Exhaustive email-address syntax testing
* Full regression testing of the Recruitment module
* Internal OrangeHRM business rules unavailable to an external tester

Observations in these areas could be recorded if encountered, but they were not part of the planned pass/fail coverage.

---

## 4. Requirements Basis

Testing was based on the separate **Candidate Creation Requirements Analysis**.

The available requirements came from:

1. OrangeHRM's published candidate-management documentation.
2. The OrangeHRM OS 5.9 public-demo interface and observable behavior.

Where the two sources differed, the discrepancy was documented and tested where practical rather than silently resolving the conflict.

Significant requirements risks included:

* Differences in UI terminology between the documentation and public demo.
* Differences in required-field presentation.
* A documented résumé maximum of **5 MB** versus a public-demo indication of **1 MB**.
* Incomplete requirements concerning Date of Application validation.
* The possibility of data changes caused by other users of the shared demo.

---

## 5. Test Approach

The project used a combination of:

### Functional Testing

Verify that expected candidate-creation actions can be completed successfully.

### Positive Testing

Provide valid input and verify that the application permits successful candidate creation.

### Negative Testing

Omit required values or provide clearly invalid input and verify that the application prevents invalid candidate creation.

### Field-Requirement Testing

Isolate candidate-name components and other fields to determine which values are actually required.

### Boundary-Value Analysis

Test immediately below, exactly at, and immediately above the apparent résumé file-size limit.

### Exploratory Testing

Investigate behavior not completely defined by the scripted requirements, particularly:

* date handling;
* unusual input;
* keyboard navigation;
* focus behavior; and
* environmental instability.

---

## 6. Scripted Test Coverage

Nine scripted test cases were designed.

| Test Case  | Test Objective                                                         |
| ---------- | ---------------------------------------------------------------------- |
| TC-CAN-001 | Open Add Candidate and verify the form and required-field presentation |
| TC-CAN-002 | Attempt candidate creation without candidate-name information          |
| TC-CAN-003 | Isolate First, Middle, and Last Name requirements                      |
| TC-CAN-004 | Verify that Email is required                                          |
| TC-CAN-005 | Verify rejection of malformed email input                              |
| TC-CAN-006 | Create a candidate while optional fields remain blank                  |
| TC-CAN-007 | Upload and save a supported `.txt` résumé                              |
| TC-CAN-008 | Determine the exact résumé file-size acceptance boundary               |
| TC-CAN-009 | Retrieve a previously created candidate                                |

The test cases were designed to build on one another while avoiding unnecessary duplication.

---

## 7. Test Data Strategy

All test candidate information was fictitious and created specifically for QA purposes.

No real applicant résumé or personally sensitive candidate data was required.

### Résumé Test Data

A fictitious `.txt` résumé was used to verify supported-file upload behavior.

For boundary-value testing, controlled test files were created at:

* **1,048,575 bytes** — immediately below 1 MiB
* **1,048,576 bytes** — exactly 1 MiB
* **1,048,577 bytes** — immediately above 1 MiB

Using exact byte sizes allowed the observed acceptance boundary to be identified without relying on rounded file-size displays.

### Candidate Data

Distinct fictitious candidate information was used where needed so that records created by one test could be differentiated from other records in the shared environment.

---

## 8. Test Environment

Testing was performed against the publicly available OrangeHRM OS 5.9 demonstration environment using Chrome on Windows.

### Environment Characteristics

The environment was:

* publicly accessible;
* shared with unknown external users;
* outside the tester's administrative control; and
* potentially subject to record deletion, modification, or reset.

This limitation was particularly important for tests requiring persistent data.

---

## 9. Environment Risk and Result Classification

A failed expectation would not automatically be classified as a product defect if environmental interference could reasonably explain the result.

The following execution statuses were used:

### Pass

The observed result met the expected result and the test completed without a material unresolved condition.

### Fail

The test completed under sufficiently reliable conditions and the observed product behavior did not meet the expected result.

A Fail required evidence supporting a product-level discrepancy rather than speculation.

### Blocked

The test could not be completed or interpreted reliably because a prerequisite, environment condition, or external dependency prevented a defensible conclusion.

A Blocked test was **not** treated as a product failure.

### Not Run

The test had not yet been executed.

This classification model was intentionally conservative so that uncertainty would not be reported as a confirmed defect.

---

## 10. Defect Handling

Potential defects identified during scripted or exploratory testing were documented separately in Jira.

A test result and a defect report were treated as related but distinct pieces of evidence.

A test could therefore pass its defined expected result while a separate incidental problem discovered during execution was reported independently.

Likewise, exploratory findings were not retroactively converted into scripted failures unless the scripted test's expected result had actually failed.

Defect documentation included, where applicable:

* summary;
* environment;
* preconditions;
* reproduction steps;
* expected behavior;
* actual behavior;
* supporting evidence;
* scope and impact; and
* qualification of uncertainty.

---

## 11. Exploratory Testing

A focused exploratory session followed the scripted execution.

### Exploratory Focus Areas

* Keyboard navigation through Add Candidate
* Visible keyboard focus
* Tab order
* Shift+Tab behavior
* Interaction without a mouse
* Date of Application entry
* Calendar behavior
* Unusual historical dates
* Candidate retrieval
* Stability of records in the shared environment

### Exploratory Principle

Unexpected behavior was recorded as an observation first.

It was classified as a defect only when there was enough evidence to identify a meaningful discrepancy between expected and observed behavior.

Where requirements were insufficient to define correct behavior, the issue remained a risk or open question rather than being overstated as a confirmed defect.

---

## 12. Entry Criteria

Scripted execution could begin when:

* the OrangeHRM public demo was accessible;
* the Recruitment module could be reached;
* the Add Candidate workflow was available;
* the candidate-creation requirements analysis was sufficiently complete;
* the nine scripted test cases had defined expected results;
* required fictitious test data was available; and
* controlled résumé files required for upload testing were prepared.

---

## 13. Exit Criteria

The planned QA cycle could be considered complete when:

* all nine scripted test cases had been attempted;
* each test had a final status of Pass, Fail, Blocked, or Not Run;
* scripted results were supported by sufficient evidence;
* meaningful defects discovered during execution were recorded;
* exploratory testing had been completed;
* environment limitations had been documented;
* unresolved risks had been identified; and
* a final quality assessment had been written.

Completion of the test cycle did **not** require every test to Pass. A Blocked result could remain at exit if the limitation and residual risk were clearly documented.

---

## 14. Deliverables

The QA cycle produced the following planned deliverables:

* Candidate Creation Requirements Analysis
* Recruitment Candidate Management Test Plan
* Nine scripted test cases
* Test execution results
* Supporting screenshots and test data
* Focused exploratory-session record
* Jira defect reports
* Final quality assessment
* Portfolio case study

---

## 15. Completion Summary

The plan was completed with all nine scripted tests attempted.

Final scripted execution status:

| Status    | Count |
| --------- | ----: |
| Pass      |     8 |
| Blocked   |     1 |
| Fail      |     0 |
| Not Run   |     0 |
| **Total** | **9** |

The single Blocked result occurred during candidate retrieval when the shared public-demo environment changed and previously created records disappeared. Because the available evidence could not establish that OrangeHRM itself caused the loss of the test record, the test was not classified as a failure.

Two quality issues identified outside the scripted failure count were documented separately:

* **OQA-15** — Date of Application formatting
* **OQA-18** — keyboard navigation/accessibility

The completed cycle therefore preserved an important distinction between **scripted test outcomes, independently reported defects, and unresolved environmental risk**.

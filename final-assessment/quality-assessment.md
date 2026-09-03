# Final Quality Assessment

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Artifact Type:** Final Quality Assessment
**Status:** Complete

## Executive Summary

The OrangeHRM Recruitment candidate-creation workflow performed reliably across the scripted functional paths that could be tested under controlled conditions.

Nine scripted tests were executed:

* **8 Pass**
* **1 Blocked**
* **0 confirmed scripted Failures**

The single Blocked result involved retrieval of a previously created candidate after the shared public-demo environment changed and previously created records disappeared. Because the environment could be modified or reset by activity outside the test session, the missing record could not be attributed confidently to OrangeHRM product behavior.

The scripted test cycle therefore produced **no confirmed functional failures**.

However, the feature should not be considered defect-free or fully validated. Two quality issues were identified outside the scripted failure count:

* **OQA-15 — Date of Application formatting**
* **OQA-18 — Keyboard navigation/accessibility**

Candidate persistence and later retrieval also remain unresolved because of the limitations of the shared test environment.

---

# Quality Assessment by Area

## Candidate Creation

**Assessment:** Strong across tested paths

Candidate creation succeeded when valid required information was provided.

The application correctly distinguished tested required and optional information sufficiently to allow valid candidate records to be created.

### Evidence

The scripted cycle confirmed that:

* the Add Candidate workflow could be opened;
* required candidate-name information was enforced;
* Middle Name could remain blank;
* Email was required;
* optional fields could remain blank; and
* candidate creation succeeded with valid required data.

### Residual Risk

This assessment applies only to the fields and combinations covered by the test cycle.

The project did not perform exhaustive combinatorial testing of every candidate field.

---

## Candidate Name Validation

**Assessment:** Meets tested requirements

Testing confirmed that:

* First Name was required;
* Last Name was required; and
* Middle Name was optional.

This behavior aligned with the published OrangeHRM candidate requirements, even though the public demo's visual required-field presentation was not as clear as the documentation.

### Residual Risk

No significant functional risk was identified in the tested name-validation behavior.

---

## Email Validation

**Assessment:** Meets tested requirements

The application:

* prevented successful candidate creation when Email was absent; and
* rejected the malformed email input exercised during testing.

### Residual Risk

The project did not perform exhaustive validation of all technically valid and invalid email-address formats.

The result should therefore be interpreted as confirmation of the tested validation behavior rather than proof of complete standards-compliant email validation.

---

## Optional Fields

**Assessment:** Meets tested requirements

Candidate creation succeeded while optional information remained blank.

This provided evidence that the tested optional fields were not incorrectly enforced as mandatory.

### Residual Risk

Not every possible optional-field combination was tested.

---

## Résumé Upload

**Assessment:** Functionally reliable within the observed public-demo limit

A supported `.txt` résumé was successfully attached during candidate creation.

Boundary-value testing established a clear observed file-size threshold:

|       File Size | Result   |
| --------------: | -------- |
| 1,048,575 bytes | Accepted |
| 1,048,576 bytes | Accepted |
| 1,048,577 bytes | Rejected |

The public demo therefore enforced an observed maximum of **1,048,576 bytes (1 MiB)**.

### Requirements Concern

OrangeHRM's published documentation described a **5 MB** résumé limit, while the public demo indicated and enforced approximately **1 MB**.

The test cycle established the demo's behavior but did not determine whether:

* the documentation was outdated;
* the demo used different configuration;
* the public demo represented a different product version; or
* the discrepancy reflected another cause.

This remains a documentation/product-consistency concern rather than a scripted functional failure.

---

## Date of Application

**Assessment:** Requires remediation or further investigation

Testing identified inconsistent behavior between the date format communicated by the interface and the way date components could be interpreted or presented.

This issue was documented as:

**OQA-15 — Date of Application formatting**

The evidence supported a formatting/interpretation defect.

It did **not** establish database corruption or prove that underlying stored date values were incorrect.

### Additional Risk

The application accepted extremely historical dates, including dates from the early 1800s.

Because the reviewed requirements did not define a minimum permitted application date, that behavior was not classified independently as a defect.

A product owner or business analyst should clarify the valid date range if historical-date restrictions are intended.

---

## Keyboard Navigation and Accessibility

**Assessment:** Significant quality concern

Exploratory testing identified a collection of keyboard-navigation problems in the Add Candidate workflow.

Observed issues included:

* focus that was difficult to identify visually;
* Tab navigation that was difficult to follow;
* movement outside the immediately visible candidate-entry context;
* inconsistent ability to understand where focus had moved;
* difficult keyboard-only page interaction; and
* a Date of Application calendar that was not effectively operable without a mouse.

This issue was documented as:

**OQA-18 — Keyboard navigation/accessibility**

### Quality Impact

The functional candidate-creation workflow may work successfully for a mouse user while remaining difficult or impractical for a user who depends on keyboard navigation.

This is therefore a separate quality dimension from the scripted functional results.

### Scope Limitation

The project did not perform a comprehensive WCAG audit or assistive-technology assessment.

The finding should be interpreted as a demonstrated keyboard accessibility/usability problem, not a statement about OrangeHRM's complete accessibility conformance.

---

## Candidate Retrieval and Persistence

**Assessment:** Not conclusively validated

TC-CAN-009 attempted to verify that a candidate created during testing could subsequently be retrieved.

During execution, the shared public-demo environment changed substantially. The candidate record count dropped from approximately 80 to 61, and previously created test candidates disappeared.

Because unrelated users or environment resets could alter the same dataset, the available evidence could not determine the cause of the missing records.

### Test Result

**Blocked**

### Interpretation

The test was intentionally **not classified as Fail**.

A failure classification would have implied evidence that OrangeHRM itself failed to retain or retrieve the candidate. The shared environment prevented that conclusion.

### Residual Risk

Candidate persistence and retrieval remain unverified and should be retested in a controlled environment.

---

# Defect Summary

| Issue  | Area                              | Assessment                                |
| ------ | --------------------------------- | ----------------------------------------- |
| OQA-15 | Date of Application               | Confirmed formatting/interpretation issue |
| OQA-18 | Keyboard navigation/accessibility | Confirmed accessibility/usability concern |

Neither issue changed the final scripted execution count because both were documented separately from the defined expected results of the nine scripted tests.

---

# Scripted Execution Summary

| Test Case  | Coverage                             | Result      |
| ---------- | ------------------------------------ | ----------- |
| TC-CAN-001 | Add Candidate access                 | **Pass**    |
| TC-CAN-002 | Missing candidate-name validation    | **Pass**    |
| TC-CAN-003 | First/Middle/Last Name requirements  | **Pass**    |
| TC-CAN-004 | Required Email                       | **Pass**    |
| TC-CAN-005 | Invalid email validation             | **Pass**    |
| TC-CAN-006 | Optional fields / candidate creation | **Pass**    |
| TC-CAN-007 | Supported résumé upload              | **Pass**    |
| TC-CAN-008 | Résumé size boundary                 | **Pass**    |
| TC-CAN-009 | Candidate retrieval                  | **Blocked** |

## Final Scripted Result

**8 Pass · 1 Blocked · 0 Fail**

---

# Release-Style Assessment

If this were a release decision for the tested candidate-creation scope, I would characterize the feature as:

**Functionally usable across the tested core creation paths, with known quality concerns and unresolved retrieval risk.**

I would not recommend describing it as fully validated.

Before considering the workflow high-confidence for release, I would recommend:

1. correcting and regression-testing Date of Application behavior;
2. addressing keyboard-navigation and focus issues;
3. retesting candidate persistence and retrieval in a controlled environment;
4. reconciling the documented 5 MB résumé limit with the 1 MiB behavior observed in the public demo; and
5. clarifying business rules for acceptable Date of Application ranges.

---

# Overall Quality Judgment

The strongest evidence from this cycle supports the conclusion that **core candidate creation works reliably across the tested functional paths**.

The application successfully handled:

* required candidate-name validation;
* optional Middle Name behavior;
* required Email validation;
* malformed email rejection;
* optional-field omission;
* supported résumé upload;
* exact résumé-size enforcement; and
* successful candidate creation.

At the same time, the cycle identified meaningful issues in:

* date handling;
* keyboard accessibility; and
* environment-dependent retrieval validation.

The appropriate overall conclusion is therefore neither “the feature passed” nor “the feature failed.”

A more defensible assessment is:

> The tested candidate-creation workflow demonstrated strong core functional behavior, but date-format consistency and keyboard accessibility require attention, and candidate retrieval remains an unresolved risk pending retest in a controlled environment.

---

# QA Decision-Making Demonstrated

This assessment reflects several deliberate QA decisions made throughout the project:

* Requirements discrepancies were documented rather than silently resolved.
* A documentation mismatch was not automatically labeled a product defect.
* Boundary behavior was measured precisely instead of approximated.
* Exploratory findings were kept separate from scripted execution outcomes.
* A Blocked test was not inflated into a Fail.
* Unusual behavior without a defined requirement was recorded as risk rather than misreported as a defect.
* Defect descriptions were limited to what the evidence actually proved.
* Final quality judgment included both successful behavior and residual uncertainty.

The result is a quality assessment designed to be **traceable, evidence-based, and defensible** rather than optimized for the largest possible bug count.

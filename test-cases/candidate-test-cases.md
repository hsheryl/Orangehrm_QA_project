# Candidate Management Test Cases

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Artifact Type:** Scripted Test Cases and Execution Results
**Status:** Complete

## Execution Summary

Nine scripted test cases were executed.

| Status    | Count |
| --------- | ----: |
| Pass      |     8 |
| Blocked   |     1 |
| Fail      |     0 |
| Not Run   |     0 |
| **Total** | **9** |

**Important:** The single Blocked result was not classified as a failure. The shared public-demo environment changed during retrieval testing, preventing a reliable determination of product behavior.

---

# TC-CAN-001 — Open Add Candidate and Verify Form

**Requirement Coverage:** REQ-CAN-001
**Test Type:** Functional / UI verification
**Status:** **Pass**

## Objective

Verify that the user can open the Add Candidate workflow from Recruitment and that the candidate-entry form is available for use.

## Preconditions

* OrangeHRM OS public demo is accessible.
* Tester can access the Recruitment module.

## Procedure

1. Open the Recruitment module.
2. Navigate to the Candidates area.
3. Select the control used to add a candidate.
4. Observe the Add Candidate form.
5. Review the available fields and required-field presentation.

## Expected Result

* The Add Candidate form opens successfully.
* Candidate information can be entered.
* Required-field information is presented sufficiently to begin candidate creation.

## Actual Result

The Add Candidate form opened and was usable.

The demo's labeling and required-field presentation differed in some respects from the published OrangeHRM documentation. Those differences were recorded during requirements analysis rather than treating this test as failed.

## Result

**Pass**

---

# TC-CAN-002 — Attempt Creation Without Candidate Name

**Requirement Coverage:** REQ-CAN-002
**Test Type:** Negative / required-field validation
**Status:** **Pass**

## Objective

Verify that a candidate cannot be successfully created when required candidate-name information is absent.

## Preconditions

* Add Candidate form is open.

## Procedure

1. Leave candidate-name information blank.
2. Supply other information as needed to isolate name validation.
3. Attempt to save the candidate.
4. Observe validation behavior.

## Expected Result

Candidate creation should be prevented when required candidate-name information is missing.

## Actual Result

The application prevented successful candidate creation and enforced candidate-name requirements.

Further testing of the individual name components was performed in TC-CAN-003.

## Result

**Pass**

---

# TC-CAN-003 — Isolate First, Middle, and Last Name Requirements

**Requirement Coverage:** REQ-CAN-002
**Test Type:** Negative / field-requirement isolation
**Status:** **Pass**

## Objective

Determine which portions of Candidate Name are actually required.

## Preconditions

* Add Candidate form is open.
* Other required candidate information can be supplied with valid values.

## Procedure

Exercise the name components separately so that the behavior of First Name, Middle Name, and Last Name can be observed rather than inferred from the visual required-field indicator.

Test combinations that isolate:

* missing First Name;
* missing Middle Name; and
* missing Last Name.

## Expected Result

Based on the published requirements:

* First Name should be required.
* Middle Name should be optional.
* Last Name should be required.

## Actual Result

The application's validation behavior was consistent with those functional requirements:

* First Name was required.
* Middle Name could be omitted.
* Last Name was required.

The exercise was valuable because the demo's visual required-field presentation did not communicate the individual name requirements as clearly as the documentation.

## Result

**Pass**

---

# TC-CAN-004 — Verify Email Is Required

**Requirement Coverage:** REQ-CAN-003
**Test Type:** Negative / required-field validation
**Status:** **Pass**

## Objective

Verify that a candidate cannot be successfully created without an Email value.

## Preconditions

* Add Candidate form is open.
* Valid required name information is available.

## Procedure

1. Enter valid required candidate-name information.
2. Leave Email blank.
3. Attempt to save the candidate.
4. Observe validation behavior.

## Expected Result

* Candidate creation is prevented.
* The application identifies Email as a required value.

## Actual Result

The application enforced the Email requirement and did not permit successful candidate creation with the required Email value absent.

## Result

**Pass**

---

# TC-CAN-005 — Reject Malformed Email Input

**Requirement Coverage:** REQ-CAN-004
**Test Type:** Negative / input validation
**Status:** **Pass**

## Objective

Verify that the Email field rejects clearly malformed email input rather than treating it as a valid email address.

## Preconditions

* Add Candidate form is open.
* Other required fields contain valid values.

## Procedure

1. Enter valid required candidate-name information.
2. Enter malformed email input.
3. Attempt to proceed with candidate creation.
4. Observe field validation and submission behavior.

## Expected Result

Clearly malformed email input should not be accepted as a valid candidate email address.

## Actual Result

The application rejected the tested malformed email input rather than silently accepting it as valid.

## Result

**Pass**

---

# TC-CAN-006 — Create Candidate With Optional Fields Blank

**Requirement Coverage:** REQ-CAN-005, REQ-CAN-008
**Test Type:** Positive / functional
**Status:** **Pass**

## Objective

Verify that candidate creation succeeds when valid required information is supplied and optional information is omitted.

## Preconditions

* Add Candidate form is open.

## Procedure

1. Enter valid values for required candidate information.
2. Leave optional candidate fields blank.
3. Select Save.
4. Observe whether candidate creation completes.

## Expected Result

* Optional fields should not be incorrectly enforced as required.
* A valid candidate record should be created successfully.

## Actual Result

The candidate was successfully created while optional information remained blank.

The application therefore distinguished the tested required and optional data sufficiently to complete the workflow.

## Result

**Pass**

---

# TC-CAN-007 — Upload Supported Text Résumé

**Requirement Coverage:** REQ-CAN-006, REQ-CAN-008
**Test Type:** Positive / file upload
**Status:** **Pass**

## Objective

Verify that a supported `.txt` résumé can be attached during candidate creation.

## Test Data

**File:** `TC-CAN-007_Fictitious_Resume.txt`

The file was created specifically for QA use and contained fictitious candidate information only.

## Preconditions

* Add Candidate form is open.
* Valid required candidate data is available.
* The controlled résumé test file is available.

## Procedure

1. Enter valid required candidate information.
2. Select the résumé upload control.
3. Attach `TC-CAN-007_Fictitious_Resume.txt`.
4. Complete candidate creation.
5. Verify that the résumé is accepted as part of the candidate workflow.

## Expected Result

* The supported `.txt` file is accepted.
* Candidate creation can proceed with the résumé attached.

## Actual Result

The controlled `.txt` résumé was accepted and the candidate-creation workflow completed successfully.

## Result

**Pass**

---

# TC-CAN-008 — Determine Exact Résumé File-Size Boundary

**Requirement Coverage:** REQ-CAN-007, REQ-CAN-008
**Test Type:** Boundary-value analysis / file upload
**Status:** **Pass**

## Objective

Determine the actual file-size boundary enforced by the OrangeHRM public demo rather than relying solely on the conflicting documentation and interface text.

## Requirements Risk

Published OrangeHRM documentation described a **5 MB** résumé maximum, while the public-demo interface indicated **1 MB**.

The test therefore examined the apparent 1 MB boundary directly.

## Controlled Test Data

| File                                   |      Exact Size | Boundary Position  |
| -------------------------------------- | --------------: | ------------------ |
| `TC-CAN-008A_Resume_1048575-bytes.txt` | 1,048,575 bytes | 1 byte below 1 MiB |
| `TC-CAN-008B_Resume_1048576-bytes.txt` | 1,048,576 bytes | Exactly 1 MiB      |
| `TC-CAN-008C_Resume_1048577-bytes.txt` | 1,048,577 bytes | 1 byte above 1 MiB |

Each file contained only controlled fictitious QA data.

## Procedure

1. Open Add Candidate.
2. Supply valid required candidate information.
3. Upload the 1,048,575-byte file and observe the result.
4. Repeat with the 1,048,576-byte file.
5. Repeat with the 1,048,577-byte file.
6. Compare the application's behavior at each point.

## Expected Result

If the application's effective limit is exactly 1 MiB:

* 1,048,575 bytes should be accepted.
* 1,048,576 bytes should be accepted.
* 1,048,577 bytes should be rejected.

## Actual Result

|      Exact Size | Observed Behavior                         |
| --------------: | ----------------------------------------- |
| 1,048,575 bytes | Accepted                                  |
| 1,048,576 bytes | Accepted                                  |
| 1,048,577 bytes | Rejected — **“Attachment Size Exceeded”** |

The observed acceptance boundary was therefore **1,048,576 bytes (1 MiB)**.

This result characterized the behavior of the tested public demo. It did not resolve why the published documentation stated a 5 MB limit.

## Result

**Pass**

---

# TC-CAN-009 — Retrieve Previously Created Candidate

**Requirement Coverage:** REQ-CAN-009
**Test Type:** Functional / persistence and retrieval
**Status:** **Blocked**

## Objective

Verify that a successfully created candidate can subsequently be found through the candidate-management workflow.

## Preconditions

* A known fictitious candidate has been successfully created.
* The shared demo has retained the candidate record.

## Procedure

1. Return to the candidate-management area.
2. Search or navigate for a candidate created during the test cycle.
3. Attempt to locate the known test candidate.
4. Compare the available candidate records with the earlier environment state.

## Expected Result

The previously created candidate should remain available and be retrievable.

## Actual Result

The test could not be completed under reliable conditions.

During retrieval testing, the shared OrangeHRM demo changed substantially. The visible candidate-record count dropped from approximately **80 records to 61**, and previously created test candidates were no longer available.

Because the environment is publicly shared, the available evidence could not determine whether the missing test record resulted from:

* normal public-demo reset behavior;
* another user's actions;
* other external modification of the shared data; or
* product behavior.

The disappearance of the record therefore did **not** provide sufficient evidence for a product failure.

## Result

**Blocked**

## Residual Risk

Candidate persistence and later retrieval were not conclusively validated.

The appropriate follow-up would be to repeat the retrieval test in an environment where test data cannot be modified or reset by unrelated users.

---

# Final Scripted Execution Record

| Test Case  | Primary Coverage                             | Result      |
| ---------- | -------------------------------------------- | ----------- |
| TC-CAN-001 | Add Candidate form access                    | **Pass**    |
| TC-CAN-002 | Missing candidate-name validation            | **Pass**    |
| TC-CAN-003 | First/Middle/Last Name requirements          | **Pass**    |
| TC-CAN-004 | Required Email                               | **Pass**    |
| TC-CAN-005 | Invalid email validation                     | **Pass**    |
| TC-CAN-006 | Optional-field behavior / candidate creation | **Pass**    |
| TC-CAN-007 | Supported résumé upload                      | **Pass**    |
| TC-CAN-008 | Exact résumé size boundary                   | **Pass**    |
| TC-CAN-009 | Candidate retrieval                          | **Blocked** |

## Scripted Outcome

**8 Pass · 1 Blocked · 0 Fail**

No scripted test produced sufficient evidence for a confirmed functional failure.

That result should not be interpreted as “no defects were found.” Defects and exploratory findings were recorded separately when evidence supported them, including **OQA-15** and **OQA-18**.

Keeping those findings separate from the scripted execution record preserves the distinction between:

* what a scripted test was designed to prove;
* what happened during that test;
* what was discovered incidentally or through exploration; and
* what could not be concluded because of environment limitations.

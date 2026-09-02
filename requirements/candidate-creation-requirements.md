# Candidate Creation Requirements Analysis

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Artifact Type:** Requirements Analysis
**Status:** Complete

## Purpose

This analysis translates the available OrangeHRM candidate-creation documentation and observable public-demo interface into testable requirements for the QA cycle.

Because this was an independent portfolio project rather than testing performed for OrangeHRM, I did not have access to internal product requirements, user stories, or acceptance criteria. The requirements therefore came from two available sources:

1. OrangeHRM's published candidate-management documentation.
2. The behavior and content visible in the OrangeHRM OS 5.9 public demo.

Where those sources disagreed, I documented the discrepancy rather than assuming that either source was automatically correct.

---

## Source Documentation

The primary documentation reviewed was OrangeHRM's Help Center guidance for adding candidates to created vacancies.

The documentation describes a workflow in which a user:

1. Navigates to **Recruitment → Candidates**.
2. selects **+ Add Candidate**;
3. completes the candidate information;
4. completes all required fields; and
5. selects **Save** to create the candidate.

The documented candidate fields relevant to this test cycle included:

| Field               | Documented Expectation       |
| ------------------- | ---------------------------- |
| First Name          | Required                     |
| Middle Name         | Optional                     |
| Last Name           | Required                     |
| Vacancy             | Optional                     |
| Select Resume       | Optional résumé upload       |
| Date of Application | Candidate's application date |
| Email               | Required                     |
| Contact Number      | Optional                     |
| Keywords            | Optional                     |
| Notes               | Optional                     |

The documentation also stated that résumé uploads support `.docx`, `.doc`, `.odt`, `.pdf`, `.rtf`, and `.txt` files with a maximum size of **5 MB**.

Not every documented candidate field was included in the scripted test scope. Testing concentrated on candidate creation, validation, résumé upload, and retrieval behavior.

---

# Documentation-to-Demo Analysis

Before writing the final test cases, I compared the published instructions with the public demo.

Several differences were visible.

## 1. Add Candidate Control

**Documentation:** Refers to a **+ Add Candidate** / Add Candidate control.

**Public demo:** The corresponding control was presented as **Add**.

### QA Interpretation

This appeared to be primarily a terminology/UI-label discrepancy rather than evidence that candidate creation itself was defective.

The test requirement was therefore written around the user's ability to reach the Add Candidate form rather than requiring an exact button label.

---

## 2. Candidate Name Required Indicators

**Documentation:** Explicitly identifies:

* First Name as required
* Middle Name as optional
* Last Name as required

**Public demo:** The visual required-field indication did not communicate the individual name requirements as clearly as the documentation.

### QA Interpretation

The documentation established an expectation that First Name and Last Name should be required while Middle Name should remain optional.

Rather than relying only on the visible asterisk, the test cycle separately exercised the name components to determine which fields the application actually enforced.

This became the basis for **TC-CAN-002** and **TC-CAN-003**.

---

## 3. Email

**Documentation:** Email is required.

### Derived Validation Expectation

Because the field is explicitly an email field, the test cycle included two separate requirements:

* the application should not permit candidate creation when required Email is absent; and
* clearly malformed email input should not be accepted as a valid email address.

These expectations became **TC-CAN-004** and **TC-CAN-005**.

---

## 4. Résumé Field Label

**Documentation:** Uses **Select Resume**.

**Public demo:** Uses **Resume**.

### QA Interpretation

This was treated as another documentation-to-interface terminology discrepancy.

The functional requirement remained that the user should be able to attach a supported résumé file to a candidate.

---

## 5. Résumé Maximum File Size

**Documentation:** Maximum résumé size stated as **5 MB**.

**Public demo:** The interface indicated a **1 MB** limit.

### QA Interpretation

This was a material requirements discrepancy because it could affect whether a legitimate user's upload succeeds.

The scripted tests therefore did not assume that the documented 5 MB value described the public demo correctly.

Testing first verified that a supported résumé could be uploaded and then examined the demo's stated 1 MB boundary directly.

This produced:

* **TC-CAN-007** — supported résumé upload
* **TC-CAN-008** — exact file-size boundary testing

For TC-CAN-008, controlled files were created immediately below, exactly at, and immediately above the apparent 1 MiB boundary.

---

## 6. Date of Application

**Documentation:** Includes **Date of Application** as candidate information.

**Public demo:** The field's presentation and placement did not correspond cleanly with the documentation.

### QA Interpretation

The available documentation established that an application date should be recordable, but it did not provide enough detail to define all valid dates, date ranges, or date-control behavior.

For that reason, I did not invent requirements such as:

* a minimum allowable year;
* a maximum historical age;
* whether future dates should be permitted; or
* how far the calendar control should allow navigation.

Date behavior remained an area for observation during execution and exploratory testing rather than being assigned unsupported acceptance criteria.

---

# Testable Requirements

The following requirements were used as the practical basis for the scripted test cycle.

## REQ-CAN-001 — Access Candidate Creation

**Requirement:**
An authorized user should be able to navigate from the Recruitment candidate list to the Add Candidate form.

**Acceptance Criteria:**

* Candidate creation can be initiated from the candidate list.
* The Add Candidate form opens.
* Required-field indicators are visible sufficiently to begin candidate entry.

**Test Coverage:** TC-CAN-001

---

## REQ-CAN-002 — Required Candidate Name

**Requirement:**
Candidate creation must enforce the required portions of Candidate Name.

**Acceptance Criteria:**

* A candidate cannot be successfully created with all candidate-name information absent.
* First Name is enforced as required.
* Last Name is enforced as required.
* Middle Name remains optional.

**Test Coverage:** TC-CAN-002, TC-CAN-003

---

## REQ-CAN-003 — Required Email

**Requirement:**
A candidate must have an email address before the record can be successfully created.

**Acceptance Criteria:**

* Leaving Email blank prevents successful candidate creation.
* The user receives an indication that the required value is missing.

**Test Coverage:** TC-CAN-004

---

## REQ-CAN-004 — Email Format Validation

**Requirement:**
The Email field should reject clearly malformed values rather than treating them as valid candidate email addresses.

**Acceptance Criteria:**

* Clearly invalid email formats are not accepted as valid input.
* Candidate creation does not silently proceed using an invalid email value.

**Test Coverage:** TC-CAN-005

---

## REQ-CAN-005 — Optional Candidate Information

**Requirement:**
Fields not identified as required should not prevent candidate creation when left blank.

**Acceptance Criteria:**

* A candidate can be created with valid required information while optional fields remain empty.
* Optional fields are not incorrectly enforced as mandatory.

**Test Coverage:** TC-CAN-006

---

## REQ-CAN-006 — Supported Résumé Upload

**Requirement:**
The application should allow a candidate résumé in a supported file format to be attached during candidate creation.

**Acceptance Criteria:**

* A supported file can be selected.
* The attachment is accepted when within the application's enforced size limit.
* Candidate creation can complete with the résumé attached.

**Test Coverage:** TC-CAN-007

---

## REQ-CAN-007 — Résumé File-Size Enforcement

**Requirement:**
The application should enforce its résumé file-size boundary consistently.

**Requirements Risk:**
The source documentation specified **5 MB**, while the public-demo interface indicated **1 MB**. The effective public-demo boundary therefore required empirical verification.

**Acceptance Criteria for Boundary Investigation:**

* Test immediately below the apparent limit.
* Test exactly at the apparent limit.
* Test immediately above the apparent limit.
* Record the actual acceptance/rejection transition rather than approximating the boundary.

**Test Coverage:** TC-CAN-008

---

## REQ-CAN-008 — Candidate Record Creation

**Requirement:**
When valid required information is supplied and no validation condition prevents submission, selecting Save should create the candidate record.

**Acceptance Criteria:**

* Valid candidate data is accepted.
* The application completes the candidate-creation action.
* Entered candidate information is available immediately after creation sufficiently to verify that creation occurred.

**Test Coverage:** TC-CAN-006, TC-CAN-007, TC-CAN-008

---

## REQ-CAN-009 — Candidate Retrieval

**Requirement:**
A successfully created candidate should subsequently be available through the candidate-management workflow.

**Acceptance Criteria:**

* A previously created candidate can be located after creation.
* The retrieved record corresponds to the test candidate.

**Test Coverage:** TC-CAN-009

**Environment Risk:**
The OrangeHRM OS demo is publicly shared. Records may be modified, deleted, or reset by activity outside the tester's control. Failure to retrieve a record in that environment cannot automatically be attributed to the product.

---

# Requirements Traceability

| Requirement | Test Case(s)                       | Coverage                    |
| ----------- | ---------------------------------- | --------------------------- |
| REQ-CAN-001 | TC-CAN-001                         | Add Candidate access/form   |
| REQ-CAN-002 | TC-CAN-002, TC-CAN-003             | Candidate-name requirements |
| REQ-CAN-003 | TC-CAN-004                         | Required Email              |
| REQ-CAN-004 | TC-CAN-005                         | Invalid email validation    |
| REQ-CAN-005 | TC-CAN-006                         | Optional fields             |
| REQ-CAN-006 | TC-CAN-007                         | Supported résumé upload     |
| REQ-CAN-007 | TC-CAN-008                         | Résumé size boundary        |
| REQ-CAN-008 | TC-CAN-006, TC-CAN-007, TC-CAN-008 | Candidate creation          |
| REQ-CAN-009 | TC-CAN-009                         | Candidate retrieval         |

---

# Requirements Risks and Open Questions

The analysis identified several uncertainties that needed to remain visible during testing.

### Documentation Version vs. Public Demo

The OrangeHRM documentation and OS 5.9 public demo did not appear to represent exactly the same interface/version.

This meant documentation differences could not automatically be classified as application defects.

### Résumé Limit

The documented **5 MB** maximum conflicted with the demo's **1 MB** indication.

The actual demo behavior therefore required boundary testing.

### Date Requirements

A Date of Application field existed, but the reviewed requirements did not establish:

* acceptable historical range;
* future-date behavior;
* calendar keyboard behavior;
* exact validation rules for manually entered dates.

Those behaviors were appropriate targets for exploratory testing but could not responsibly be assigned invented expected results.

### Shared Test Environment

Because the demo was shared publicly, persistent-data tests were vulnerable to activity outside the test cycle.

That risk was specifically relevant to candidate retrieval.

---

# Requirements Analysis Conclusion

The requirements review showed that the available specification was usable but imperfect.

The central QA challenge was therefore not merely converting documentation into test cases. It was determining **which expectations were actually supported by evidence, which requirements could be tested directly, and which apparent discrepancies needed investigation before they could be called defects.**

That distinction informed the entire test cycle:

* documented requirements became testable expectations where sufficiently clear;
* documentation/demo mismatches were recorded rather than silently resolved;
* ambiguous behaviors were investigated;
* unsupported assumptions were excluded from expected results; and
* environment limitations were considered when interpreting test outcomes.

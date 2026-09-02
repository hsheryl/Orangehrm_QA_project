# OrangeHRM Recruitment Candidate Management QA Cycle

## Portfolio Case Study

### Project Overview

I completed an independent end-to-end QA cycle on the **Recruitment → Add Candidate** workflow in the OrangeHRM OS 5.9 public demo.

The goal was not simply to execute a set of test cases. I treated the exercise as a small production-style QA engagement: analyze the available requirements, identify ambiguities, create a focused test plan, design controlled test data, execute documented tests, investigate boundaries, perform exploratory testing, record defects in Jira, and make a final quality assessment based on the evidence.

**Final scripted execution result: 9 tests executed — 8 Pass, 1 Blocked, 0 Fail.**

The project also identified two reportable quality issues through incidental and exploratory testing. Those findings were kept separate from the scripted execution results rather than retroactively turning successful test cases into failures.

---

## My Role

**Independent QA Analyst / Tester**

I was responsible for:

* Requirements analysis
* Documentation-to-product comparison
* Test planning
* Test-case design
* Controlled test-data creation
* Functional and negative testing
* Boundary-value analysis
* Test execution and evidence capture
* Exploratory testing
* Defect documentation in Jira
* Risk assessment
* Final quality evaluation

**Tools and environment:** OrangeHRM OS 5.9 public demo, Jira, Chrome on Windows, screenshots, and purpose-built fictitious résumé files.

---

## Requirements Analysis

Before writing the test cases, I compared the documented candidate-creation behavior with the actual public demo.

This exposed several areas where the documentation and interface did not align exactly, including differences in:

* Button and field labels
* Required-field indicators
* Résumé-upload wording
* Stated file-size limits
* Placement and behavior of Date of Application
* Which portions of the candidate name were actually required

Rather than silently assuming either the documentation or the interface was authoritative, I documented these discrepancies and designed the tests around behavior that could be objectively observed.

That requirements work became the basis for the test plan and helped prevent ambiguous expectations from contaminating the execution results.

---

## Test Strategy

The scripted test set concentrated on the highest-value candidate-creation risks:

* Can a user reach the Add Candidate form?
* Are required fields identified and enforced?
* Which components of Candidate Name are actually mandatory?
* Is Email required?
* Does Email reject malformed input?
* Can a candidate be created without optional information?
* Can a supported résumé be uploaded and retained?
* Where is the actual résumé file-size boundary?
* Can a newly created candidate later be retrieved?

I used both positive and negative testing and added explicit boundary testing where the available documentation and observed interface behavior warranted it.

---

## Scripted Test Execution

| Test           | Coverage                                                                   | Result      |
| -------------- | -------------------------------------------------------------------------- | ----------- |
| **TC-CAN-001** | Open Add Candidate and verify required-field indicators                    | **Pass**    |
| **TC-CAN-002** | Attempt creation with candidate-name information absent                    | **Pass**    |
| **TC-CAN-003** | Isolate First, Middle, and Last Name requirements                          | **Pass**    |
| **TC-CAN-004** | Verify Email is required                                                   | **Pass**    |
| **TC-CAN-005** | Verify malformed email formats are rejected                                | **Pass**    |
| **TC-CAN-006** | Create a candidate while optional fields remain blank                      | **Pass**    |
| **TC-CAN-007** | Upload and save a supported `.txt` résumé                                  | **Pass**    |
| **TC-CAN-008** | Test résumé upload immediately below, at, and above the file-size boundary | **Pass**    |
| **TC-CAN-009** | Retrieve the created candidate after creation                              | **Blocked** |

### Result Summary

**Pass: 8**
**Fail: 0**
**Blocked: 1**
**Not Run: 0**

This distinction was important to the project.

TC-CAN-009 was **not a failed test**. During retrieval testing, the number of records in the shared OrangeHRM demo changed from approximately 80 to 61 and previously created test candidates disappeared. Because the environment is publicly shared and can be modified or reset independently of my test session, I could no longer determine whether the missing candidate represented application behavior or external interference.

The correct result was therefore **Blocked**, with retrieval remaining an unresolved area of risk.

---

## Boundary-Value Testing

The résumé-size requirement warranted more precise investigation than simply trying one small and one large file.

I created controlled fictitious résumé files at three exact sizes:

* **1,048,575 bytes** — one byte below 1 MiB
* **1,048,576 bytes** — exactly 1 MiB
* **1,048,577 bytes** — one byte above 1 MiB

The application accepted the first two files and rejected the third with **“Attachment Size Exceeded.”**

This established the observed maximum as **1,048,576 bytes**, rather than relying on an imprecise interpretation of the documentation or interface text.

---

## Defects and Quality Findings

### OQA-15 — Date of Application Format

During testing, I found that Date of Application could display values in a **yyyy-dd-mm** pattern even though the interface guidance indicated **yyyy-mm-dd**.

Examples included displays equivalent to:

* `2026-30-08`
* `2026-02-09`

The evidence supported a **date-presentation/formatting defect**, but it did not establish that the underlying date value was being stored incorrectly. I therefore avoided escalating the finding into an unsupported claim of data corruption.

This issue was recorded separately in Jira as **OQA-15**.

### OQA-18 — Keyboard Navigation / Accessibility

A timed exploratory session exposed additional usability and accessibility concerns in the Add Candidate workflow.

Keyboard-only navigation showed that:

* Focus on controls such as **Cancel** could be difficult to see.
* Tab navigation could move through unexpected or off-screen elements before reaching candidate fields.
* Navigation could leave the visible form and traverse other page or browser elements before returning.
* Shift+Tab could re-enter the form from the opposite direction.
* Arrow-key scrolling was inconsistent.
* The date-picker calendar was not effectively operable using the keyboard alone, although a date could be typed directly into the field.

These findings were documented in Jira as **OQA-18**.

I treated this as a keyboard accessibility/usability finding rather than claiming formal WCAG conformance testing, because the project was not structured as a complete accessibility audit.

---

## Exploratory Testing

After completing the scripted cases, I ran a focused exploratory session to look beyond the expected functional paths.

The session examined:

* Keyboard navigation and focus visibility
* Date-field behavior
* Calendar interaction
* Unusual date entry
* Candidate-name interaction
* Search and retrieval behavior
* Stability of the shared public-demo environment

Exploration reinforced both previously identified risks.

The date field accepted values far outside a normal recruiting timeframe, and its calendar/display behavior provided additional evidence that date interpretation deserved investigation. Because no documented acceptable date range had been established, I treated the historical-date behavior as a **risk and exploratory observation**, not automatically as another confirmed defect.

The session also confirmed that disappearing test records were part of broader shared-environment instability, strengthening the decision to classify TC-CAN-009 as **Blocked rather than Failed**.

---

## Final Quality Assessment

The tested candidate-creation paths were generally strong.

Under the conditions I could reliably control, OrangeHRM correctly:

* Enforced the tested required fields
* Distinguished required from optional portions of Candidate Name
* Required Email
* Rejected malformed email input
* Allowed optional fields to remain blank
* Created candidate records successfully
* Accepted supported résumé uploads
* Enforced the observed 1 MiB résumé-size boundary
* Preserved tested information immediately after creation

The scripted cycle therefore produced **eight Pass results and no confirmed scripted functional failures**.

That does **not** mean the feature was defect-free.

Two meaningful quality issues were identified outside the scripted pass/fail results:

1. **OQA-15:** inconsistent Date of Application formatting
2. **OQA-18:** problematic keyboard navigation and accessibility behavior

In addition, candidate retrieval could not be conclusively evaluated because the shared public-demo environment changed during execution. That leaves retrieval as **residual risk**, not evidence of either correct or incorrect product behavior.

### Overall Assessment

**Core candidate creation appears functionally reliable across the tested paths, but the workflow should not be considered fully validated. Date-format consistency and keyboard accessibility need remediation or further investigation, while persistent candidate retrieval requires retesting in a controlled environment.**

---

## What This Project Demonstrates

This project demonstrates more than test-case execution.

It shows my ability to:

* Translate imperfect requirements into testable expectations
* Distinguish documentation discrepancies from product defects
* Design positive, negative, and boundary tests
* Create controlled and traceable test data
* Follow evidence rather than assumptions
* Recognize when an environment has invalidated a test
* Use **Blocked** instead of incorrectly reporting **Fail**
* Separate exploratory findings from scripted execution results
* Avoid overstating the severity or cause of an observed problem
* Document defects and evidence in Jira
* Make a final quality judgment that includes both demonstrated functionality and unresolved risk

The most important outcome was not the number of defects found. It was producing a QA record in which the conclusions could be defended from the evidence.

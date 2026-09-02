# Orangehrm_QA_project
# OrangeHRM Recruitment Candidate Management QA Cycle

**Independent QA Portfolio Project | Functional, Negative, Boundary & Exploratory Testing**

I completed a full QA cycle for the **Recruitment → Add Candidate** workflow in the OrangeHRM OS 5.9 public demo, from requirements analysis through final quality assessment.

## Results at a Glance

**9 scripted tests executed**

* **8 Pass**
* **1 Blocked**
* **0 confirmed scripted Failures**

The blocked test involved retrieving a previously created candidate after the shared public-demo environment changed and test records disappeared. Because the environment could no longer support a reliable conclusion, I classified the result as **Blocked rather than Fail**.

## What I Tested

The test cycle covered:

* Add Candidate form behavior
* Required-field enforcement
* Candidate-name requirements
* Required and malformed email validation
* Optional-field behavior
* Résumé upload
* Exact résumé file-size boundary testing
* Candidate creation and retrieval
* Date behavior
* Keyboard navigation and accessibility

For the résumé boundary test, I created controlled files at:

* **1,048,575 bytes**
* **1,048,576 bytes**
* **1,048,577 bytes**

The application accepted files through exactly **1 MiB** and rejected the file one byte above the boundary.

## Key Findings

### OQA-15 — Date of Application Formatting

The application could display dates in a `yyyy-dd-mm` pattern even though the interface indicated `yyyy-mm-dd`.

I documented this as a formatting defect without claiming unsupported underlying data corruption.

### OQA-18 — Keyboard Navigation / Accessibility

Exploratory testing identified difficult-to-see focus, unexpected Tab order, inconsistent keyboard scrolling, and a calendar control that was not effectively operable by keyboard alone.

I reported this as a keyboard accessibility/usability issue rather than representing the project as a complete WCAG audit.

## Final Assessment

The tested candidate-creation paths were generally reliable. Required-field validation, email validation, optional fields, candidate creation, supported résumé upload, and the observed 1 MiB upload boundary behaved correctly in the scripted tests.

However, the feature should not be considered fully validated. **Date-format consistency and keyboard accessibility require attention, and candidate retrieval should be retested in a controlled environment.**

## QA Skills Demonstrated

* Requirements analysis
* Test planning
* Test-case design
* Positive and negative testing
* Boundary-value analysis
* Exploratory testing
* Jira defect reporting
* Evidence capture
* Risk-based quality assessment
* Distinguishing **Pass, Fail, and Blocked** results based on evidence

### Project Artifacts

* Requirements analysis
* Test plan
* Nine scripted test cases
* Execution results and evidence
* Exploratory test session
* OQA-15 defect report
* OQA-18 defect report
* Final quality assessment
* Full case study


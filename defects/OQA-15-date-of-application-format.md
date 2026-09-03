# OQA-15 — Date of Application Format Is Inconsistent With Displayed Format Guidance

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Issue Type:** Defect
**Status:** Documented
**Area:** Date of Application
**Discovered During:** Scripted/exploratory candidate testing

## Summary

The **Date of Application** field can interpret or display date components in an order that is inconsistent with the `yyyy-mm-dd` format indicated by the interface.

Observed behavior showed dates appearing in a `yyyy-dd-mm` pattern, creating ambiguity about which numeric component represents the month and which represents the day.

---

## Preconditions

* OrangeHRM OS public demo is accessible.
* Recruitment → Candidates → Add Candidate is open.

---

## Steps to Reproduce

One observed path was:

1. Navigate to **Recruitment → Candidates**.
2. Open **Add Candidate**.
3. Place focus in **Date of Application**.
4. Enter a date manually.
5. Observe the resulting date value.
6. Open the date-picker calendar and compare the highlighted calendar date with the numeric value entered or displayed.

During exploratory testing, the value:

`1800-12-8`

was accepted.

When the calendar was opened afterward, **August 12, 1800** was highlighted.

Additional observed date presentation included values such as:

* `2026-30-08`
* `2026-02-09`

These observations were consistent with day and month components being presented or interpreted differently from the format indicated to the user.

---

## Expected Result

The Date of Application field should interpret and present dates consistently with the format communicated by the interface.

If the displayed format is:

`yyyy-mm-dd`

then:

* the second component should consistently represent the month;
* the third component should consistently represent the day; and
* typed input, displayed values, and the date-picker calendar should agree on the same date.

---

## Actual Result

Observed behavior did not consistently align with the indicated `yyyy-mm-dd` ordering.

In the clearest exploratory example:

* Entered value: `1800-12-8`
* Calendar-highlighted date: **August 12, 1800**

This indicates that the application treated the numeric components in a way that did not match the apparent `yyyy-mm-dd` format.

---

## User Impact

Ambiguous date formatting can cause users to:

* misread an application date;
* enter a date believing the month and day are in the opposite positions;
* unintentionally save a different date from the one intended; or
* lose confidence in the accuracy of displayed candidate information.

The risk is particularly significant for dates where both the month and day are valid numbers and an incorrect interpretation may not produce an obvious validation error.

---

## Evidence Interpretation

The evidence supports a **date formatting/interpretation defect**.

It does **not** establish that the underlying stored date value in the database is corrupt.

The testing observed behavior through the application interface only. No database-level inspection or API-level validation was performed.

For that reason, this issue should not be described as confirmed data corruption.

---

## Related Exploratory Observation

The application also accepted dates from the early 1800s.

Although such dates are implausible for a modern recruiting workflow, the reviewed requirements did not define a minimum allowable Date of Application.

Acceptance of an extremely old date was therefore recorded as a requirements gap or product risk rather than included as part of this confirmed defect.

---

## Recommended Remediation

Ensure that:

1. the date-format hint accurately describes the input format the application accepts;
2. manually entered dates are parsed using that same format;
3. the date picker highlights the same date represented in the text field;
4. displayed dates use one consistent component order throughout the workflow; and
5. automated tests cover dates where swapping the month and day would produce a different but still valid date.

Useful regression examples would include dates such as:

* August 12
* September 2
* December 8

because these expose month/day transposition more clearly than dates where one component cannot represent a month.

---

## Verification Approach

After remediation:

1. Enter several dates manually using the displayed format.
2. Open the calendar after each entry.
3. Verify that the highlighted calendar date matches the entered date.
4. Save the candidate.
5. Reopen the candidate record.
6. Confirm that the displayed Date of Application still represents the same date.

---

## Portfolio Note

This defect was intentionally scoped to what the available evidence demonstrated.

The observed behavior justified reporting inconsistent date formatting/interpretation, but the investigation did not provide enough evidence to claim that OrangeHRM was corrupting stored date data.

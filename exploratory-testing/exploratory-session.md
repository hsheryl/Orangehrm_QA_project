# Exploratory Testing Session — Add Candidate

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Artifact Type:** Exploratory Testing Session Record
**Status:** Complete

## Session Overview

**Session length:** Approximately 21 minutes
**Recorded session time:** 3:15 PM–3:36 PM PDT
**Primary focus:** Keyboard accessibility, navigation behavior, Date of Application behavior, and unusual input
**Testing style:** Time-boxed exploratory testing

This session followed the scripted candidate-management tests and was intended to investigate areas where the available requirements did not provide enough information to define useful scripted expected results.

The goal was not to generate as many defects as possible. The goal was to explore uncertain behavior, record observations, and distinguish between:

* confirmed quality issues;
* usability or accessibility concerns;
* behavior requiring additional investigation; and
* behavior for which the available requirements were insufficient to determine correctness.

---

# Session Charter

> Explore the OrangeHRM Add Candidate workflow using primarily keyboard interaction and unusual Date of Application input. Look for navigation, focus, validation, accessibility, date-handling, and usability risks that were not fully covered by the scripted test cases.

## Areas of Interest

The session concentrated on:

* Tab navigation
* Shift+Tab navigation
* Visible keyboard focus
* Movement into and out of the Add Candidate form
* Scrolling without the mouse
* Date-field text entry
* Date-picker keyboard operation
* Historical date handling
* Relationship between typed dates and calendar interpretation
* Unexpected behavior worth further investigation

---

# Exploration Notes

## 1. Cancel Button Focus Was Difficult to Identify

While navigating with Tab, it was difficult to determine visually when focus was on the **Cancel** button.

### Observation

The control could participate in keyboard navigation without providing sufficiently obvious visual feedback to make the current focus location clear.

### QA Interpretation

This raised an accessibility/usability concern because keyboard users need to be able to determine which interactive element currently has focus.

This observation contributed to **OQA-18**.

---

## 2. Tab Order Did Not Move Directly Through the Candidate Form

Tab navigation did not proceed through the Add Candidate fields in a simple, predictable form sequence.

### Observation

Keyboard focus moved through other elements before reaching the candidate-entry fields.

When the Add Candidate page had not first been repositioned using the mouse, focus could appear to begin again from an unclear location and move through elements outside the visible form.

Observed navigation included items associated with:

* elements near the bottom of the page;
* browser chrome/address-bar navigation;
* page navigation or group/menu elements; and
* eventually the first candidate field, which could be outside the currently visible portion of the Add Candidate form.

### QA Interpretation

The experience made it difficult to understand:

* where keyboard focus currently was;
* whether focus had left the form;
* what control would receive the next Tab action; and
* how to return efficiently to candidate entry.

This was considered a significant keyboard-navigation usability/accessibility concern and contributed to **OQA-18**.

---

## 3. Shift+Tab Could Re-enter the Candidate Form

Reverse keyboard navigation behaved differently from forward Tab navigation.

### Observation

Using **Shift+Tab** could re-enter the Add Candidate form from the lower portion of the navigation sequence.

### QA Interpretation

This provided additional evidence that the issue was not simply an inability to use the keyboard. Keyboard focus existed, but the navigation sequence and visible context made it difficult to use predictably.

---

## 4. Keyboard Scrolling Was Inconsistent

The Up and Down Arrow keys did not provide consistently predictable scrolling behavior.

### Observation

At some points the arrow keys scrolled the Add Candidate page, while at other points they did not.

### QA Interpretation

Some variation can be expected depending on which control currently owns keyboard focus. However, combined with the unclear focus location and Tab-order behavior, the inconsistency made keyboard-only navigation harder to understand and control.

This observation was treated as supporting evidence for the broader keyboard-accessibility concern rather than as an independent defect.

---

# Date of Application Exploration

## 5. Date Could Be Entered Directly From the Keyboard

Although the calendar control itself presented keyboard-navigation problems, the Date of Application field accepted manually typed input.

### Observation

A historical value was entered directly as:

`1800-12-8`

The field accepted the input.

Opening the calendar afterward showed **August 12, 1800** highlighted.

### QA Interpretation

This was notable because the typed sequence could reasonably be interpreted differently depending on the expected date format.

The observed relationship between text entry and calendar interpretation warranted further investigation of date formatting and presentation.

This evidence supported the investigation associated with **OQA-15**.

---

## 6. Extremely Historical Dates Were Accepted

The application allowed navigation and input involving dates from the early 1800s.

### QA Interpretation

This was unusual for a modern recruiting workflow, but the available requirements did **not** define:

* an earliest permissible application date;
* how old an application record may be;
* whether historical dates should be limited; or
* what business rule should apply to implausibly old dates.

For that reason, acceptance of an 1800s date was **not classified as a defect by itself**.

It remained a product-risk observation that would require a business requirement before a definitive expected result could be written.

---

## 7. Date Presentation Required Separate Defect Investigation

Date exploration produced evidence of inconsistent interpretation/presentation of dates relative to the interface's indicated format.

The resulting issue was documented separately as:

**OQA-15 — Date of Application formatting**

The defect report focused on the behavior that could be demonstrated from evidence rather than making the broader unsupported claim that all date storage was incorrect.

---

# Keyboard Accessibility Finding

The combination of observations involving:

* unclear visible focus;
* unexpected Tab navigation;
* movement outside the immediately visible form;
* difficult re-entry into the desired form sequence;
* inconsistent keyboard scrolling context; and
* a calendar control that was not effectively operable using the keyboard alone

was substantial enough to document as a separate quality issue:

**OQA-18 — Keyboard navigation/accessibility**

The finding was intentionally described as a **keyboard accessibility/usability issue** rather than claiming that this exploratory session constituted a complete WCAG accessibility audit.

---

# Findings Summary

| Observation                                      | Classification                  | Follow-up                      |
| ------------------------------------------------ | ------------------------------- | ------------------------------ |
| Cancel focus difficult to identify               | Accessibility/usability concern | OQA-18                         |
| Tab sequence difficult to follow                 | Accessibility/usability concern | OQA-18                         |
| Focus moved beyond visible candidate form        | Accessibility/usability concern | OQA-18                         |
| Shift+Tab could re-enter form                    | Supporting observation          | OQA-18                         |
| Arrow-key scrolling inconsistent                 | Supporting observation          | OQA-18                         |
| Calendar not effectively keyboard-operable       | Accessibility concern           | OQA-18                         |
| Typed `1800-12-8` accepted                       | Date-handling observation       | OQA-15 investigation           |
| Calendar interpreted/highlighted August 12, 1800 | Date-format evidence            | OQA-15                         |
| Very old application dates permitted             | Requirements gap / product risk | No separate defect established |

---

# What Was Not Claimed

Several deliberate limits were placed on the conclusions from this session.

## No Full Accessibility-Conformance Claim

This was a focused exploratory session, not a formal WCAG audit.

The evidence was sufficient to identify a meaningful keyboard-accessibility problem, but not to make a comprehensive statement about OrangeHRM's overall accessibility conformance.

## No Defect Based Solely on an Old Date

An application date in the 1800s appears implausible in normal recruiting use, but no reviewed requirement established a valid minimum date.

Without that requirement, the behavior was recorded as a risk rather than converted into an unsupported defect.

## No Unsupported Claim of Date Corruption

The observed date behavior supported a formatting/presentation issue.

The testing did not establish enough evidence to conclude that the underlying stored date value itself was corrupt.

---

# Session Outcome

The exploratory session achieved its purpose by finding risks that the scripted test set would not have exposed on its own.

Two areas warranted formal follow-up:

1. **OQA-15 — Date of Application formatting**
2. **OQA-18 — Keyboard navigation/accessibility**

Just as importantly, the session identified unusual behavior that **did not** justify a defect report without additional requirements.

That distinction demonstrates the role of exploratory testing as more than unscripted clicking: it is a structured investigation in which observations are followed far enough to determine what the available evidence actually supports.

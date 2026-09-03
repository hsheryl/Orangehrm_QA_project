# OQA-18 — Add Candidate Workflow Is Difficult to Operate Reliably With Keyboard Navigation

**Project:** OrangeHRM Recruitment Candidate Management QA Cycle
**System Under Test:** OrangeHRM OS 5.9 Public Demo
**Feature:** Recruitment → Candidates → Add Candidate
**Issue Type:** Accessibility / Usability Defect
**Status:** Documented
**Area:** Keyboard navigation and focus behavior
**Discovered During:** Time-boxed exploratory testing

## Summary

The **Add Candidate** workflow is difficult to navigate reliably without a mouse because keyboard focus is not always visually obvious, the Tab sequence can be difficult to follow, navigation can leave the visible candidate-entry area before reaching expected form fields, and the Date of Application calendar is not effectively operable using the keyboard alone.

The issue was identified during a focused exploratory session from approximately **3:15 PM to 3:36 PM PDT**.

---

## Preconditions

* OrangeHRM OS public demo is accessible.
* Recruitment → Candidates → Add Candidate is open.
* Tester uses keyboard navigation rather than relying on mouse interaction.

---

## Exploratory Procedure

1. Open the Add Candidate workflow.
2. Navigate through the page using **Tab**.
3. Observe the location and visibility of keyboard focus.
4. Continue navigating through form controls without using the mouse to reposition the page.
5. Use **Shift+Tab** to test reverse navigation.
6. Attempt page movement using keyboard controls.
7. Navigate to **Date of Application**.
8. Attempt to operate the date-picker calendar using only the keyboard.

---

## Expected Result

A keyboard user should be able to:

* identify which interactive element currently has focus;
* move through interactive controls in a predictable sequence;
* reach and operate the candidate-entry fields without needing mouse intervention;
* reverse direction using Shift+Tab without losing context; and
* operate required interface controls, including the date control, using the keyboard.

---

## Actual Result

Multiple keyboard-navigation problems were observed.

### Focus Visibility

It was difficult to determine when keyboard focus was on the **Cancel** button.

The visual indication of focus was not sufficiently obvious to make the active control consistently clear.

### Tab Sequence

Pressing Tab did not produce a simple, easily understood progression through the visible Add Candidate fields.

When navigating without first using the mouse to reposition the Add Candidate page, focus could move through elements outside the expected candidate-entry sequence before eventually returning to form fields.

During testing, navigation could move through:

* elements near the bottom of the page;
* other page/menu elements;
* browser-level navigation after focus left the webpage sequence; and
* eventually back toward the candidate form.

At points, the next candidate field receiving focus was outside the currently visible portion of the form, making the focus location difficult to understand.

### Reverse Navigation

**Shift+Tab** could re-enter the candidate form from the opposite direction.

This showed that keyboard navigation was possible in part, but the overall path was difficult to predict and use efficiently.

### Keyboard Scrolling

The Up and Down Arrow keys sometimes scrolled the page and sometimes did not.

Some variation can depend on which component owns focus, but the unclear focus state made this behavior difficult for the user to interpret.

### Date Picker

The Date of Application field could accept a date typed directly into the text field.

However, the calendar itself was not effectively operable using the keyboard alone during the exploratory session.

---

## User Impact

A user who relies on keyboard navigation may have difficulty:

* determining where focus is located;
* reaching the desired candidate field efficiently;
* understanding why navigation has moved away from the visible form;
* recovering after losing track of focus;
* operating the date picker; or
* completing the workflow without switching to a mouse or other pointing device.

This creates a usability barrier and presents an accessibility concern for users who cannot or do not use a mouse.

---

## Reproducibility

The problem was observed repeatedly during the exploratory session, although individual behavior varied depending on:

* the current focus location;
* page scroll position;
* navigation direction; and
* which control currently received keyboard input.

The defect therefore concerns the **overall keyboard-navigation experience**, rather than one isolated incorrect Tab transition.

---

## Scope of Finding

This issue documents observed keyboard accessibility and usability problems in the Add Candidate workflow.

It should **not** be interpreted as the result of a comprehensive accessibility audit.

The project did not perform:

* a complete WCAG conformance assessment;
* screen-reader testing;
* testing with multiple assistive technologies;
* comprehensive browser accessibility testing; or
* accessibility testing across all OrangeHRM modules.

The available evidence was sufficient to identify a meaningful keyboard-accessibility problem, but not to characterize OrangeHRM's overall accessibility conformance.

---

## Recommended Remediation

Review the Add Candidate workflow so that:

1. every interactive control has a clearly visible keyboard-focus state;
2. Tab order follows a logical and predictable sequence;
3. keyboard focus does not unexpectedly move outside the working context before expected form controls are reached;
4. off-screen focused controls are brought into an understandable visible context;
5. reverse navigation using Shift+Tab follows the logical inverse of forward navigation;
6. the date-picker control can be fully operated without a mouse; and
7. keyboard behavior is tested independently of mouse-based scrolling or page positioning.

---

## Verification Approach

After remediation, repeat the workflow from beginning to end without touching the mouse:

1. Open Add Candidate.
2. Use Tab to navigate through every interactive control.
3. Confirm a clearly visible focus indicator at each step.
4. Verify that the order corresponds logically to the visual and functional structure of the form.
5. Reverse through the workflow using Shift+Tab.
6. Open and operate Date of Application entirely by keyboard.
7. Select a date.
8. Complete all required candidate fields.
9. Save or cancel the form using keyboard controls alone.

A successful verification should allow a tester to complete the workflow while always being able to determine where focus is and what action will occur next.

---

## Portfolio Note

This defect illustrates why exploratory testing was performed after the scripted functional cycle.

The nine scripted cases concentrated on candidate creation and validation behavior. A workflow could pass those functional checks while still creating a substantial barrier for users interacting with it differently.

The exploratory session exposed that separate quality dimension without changing any of the scripted Pass results into failures.

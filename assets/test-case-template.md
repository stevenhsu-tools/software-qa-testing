# Test Case Set Template

Use this before testing begins. Number cases sequentially (`TC-001`, `TC-002`, ...) so they can be referenced later from the QA test report and from any bug report a failure generates. Leave **Actual Result** and **Status** as `Not Run` at this stage — don't invent results.

## Header

**Feature / Requirement Under Test:** *What this test case set covers.*

**Source(s):** *PRD, design spec, or description this was derived from — or `Not specified` if working from a brief description only.*

**Date Generated:** *Date.*

## Coverage summary

*Fill this in after listing all cases — a quick count by test type/technique so gaps are visible at a glance.*

| Test Type / Technique | # Test Cases |
|---|---|
| Functional | |
| Boundary | |
| Configuration | |
| Stage | |
| Stress | |
| Error Handling | |
| Regression | |
| **Total** | |

## Test cases

Repeat this block per test case. A table works well for short cases; use the expanded block form below it for cases with many steps.

### Table form (default)

| ID | Title | Test Type/Technique | Preconditions | Test Steps | Expected Result | Actual Result | Status | Linked Bug ID |
|---|---|---|---|---|---|---|---|---|
| TC-001 | | | | 1. ... 2. ... | | Not Run | Not Run | |
| TC-002 | | | | | | Not Run | Not Run | |

### Expanded form (for cases with long steps or test data)

**Test Case ID:** TC-00X

**Title:** *Short, specific description.*

**Test Type / Technique:** *Functional / Boundary / Configuration / Stage / Stress / Error Handling / Regression (see SKILL.md Part 1 for definitions).*

**Related Requirement / Feature:** *What this case verifies.*

**Preconditions:** *State the system/data must be in before starting.*

**Test Data:** *Specific inputs used (values, files, accounts, etc.).*

**Test Steps:**
1. *Step one*
2. *Step two*
3. *Step three*

**Expected Result:** *What should happen.*

**Actual Result:** `Not Run`

**Status:** `Not Run`

**Linked Bug ID:** *(leave blank until execution; fill in only if Status becomes `Fail` and a bug report is generated)*

---

*Status values: `Not Run` (before execution) / `Pass` / `Fail` / `Blocked` (couldn't be executed — note why in Actual Result).*

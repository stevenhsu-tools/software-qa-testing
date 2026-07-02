# Test Case Set Template

Use this before testing begins. Cases are derived from reading the actual code — a spec/PRD is optional context, not a requirement. Number cases sequentially (`TC-001`, `TC-002`, ...) so they can be referenced later from the QA test report and from any bug report a failure generates. Leave **Actual Result** and **Status** as `Not Run` at this stage — don't invent results.

## Header

**Feature / Code Under Test:** *What this test case set covers, in plain terms.*

**Code Scanned:** *File path(s), function/module names actually read to derive these cases (e.g. `src/checkout/payment.py: process_payment(), validate_card()`).*

**Spec/Doc Used (if any):** *Name it if the user supplied one; otherwise `None — derived from code only`.*

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

| ID | Title | Test Type/Technique | Source Location | Preconditions | Test Steps | Expected Result | Actual Result | Status | Verification Method | Linked Bug ID |
|---|---|---|---|---|---|---|---|---|---|---|
| TC-001 | | | `file.ext: functionName()` | | 1. ... 2. ... | | Not Run | Not Run | | |
| TC-002 | | | | | | | Not Run | Not Run | | |

### Expanded form (for cases with long steps or test data)

**Test Case ID:** TC-00X

**Title:** *Short, specific description.*

**Test Type / Technique:** *Functional / Boundary / Configuration / Stage / Stress / Error Handling / Regression (see SKILL.md Part 1 for definitions).*

**Source Location:** *File path + function/method name (and line numbers where useful) this case was derived from — this is what makes the case traceable back to the code, in place of a spec section number.*

**Preconditions:** *State the system/data must be in before starting.*

**Test Data:** *Specific inputs used (values, files, accounts, etc.) — pick values that exercise what you found in the code (e.g. a boundary constant, a validated field, a config flag).*

**Test Steps:**
1. *Step one*
2. *Step two*
3. *Step three*

**Expected Result:** *What should happen, based on what the code is supposed to do (or what the spec says, if one was supplied and conflicts with the code — flag the conflict).*

**Actual Result:** `Not Run`

**Status:** `Not Run`

**Verification Method:** *(filled in at execution — see SKILL.md Part 3: `Executed (automated)` / `Executed (manual walkthrough)` / `Traced (code read, not run)`)*

**Linked Bug ID:** *(leave blank until execution; fill in only if Status becomes `Fail` and a bug report is generated)*

---

*Status values: `Not Run` (before execution) / `Pass` / `Fail` / `Blocked` (couldn't be executed — note why in Actual Result).*

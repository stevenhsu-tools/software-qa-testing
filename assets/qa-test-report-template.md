# QA Test Report Template

Use this after test cases (see `test-case-template.md`) have been executed. This report is the direct input to bug-report generation — every row listed under "Failed test cases" below should become one bug report (see SKILL.md Part 4).

## Header

**Report Title:** *e.g. "QA Test Report — Checkout Flow, Build 4.2.1"*

**Feature / Requirement Under Test:** *What was tested.*

**Build / Version Tested:** *Build number.*

**Date:** *Date report generated.*

**Tester(s):** *Who ran the cases.*

**Environment(s):** *OS/browser/config the cases were run in.*

## Summary

Compute Pass Rate as Passed ÷ (Total − Not Run), so incomplete runs aren't scored as if every un-run case failed.

| Metric | Count |
|---|---|
| Total Test Cases | |
| Passed | |
| Failed | |
| Blocked | |
| Not Run | |
| **Pass Rate** (Passed ÷ (Total − Not Run)) | |

## Breakdown by test type / technique

| Test Type/Technique | Total | Passed | Failed | Blocked | Not Run |
|---|---|---|---|---|---|
| Functional | | | | | |
| Boundary | | | | | |
| Configuration | | | | | |
| Stage | | | | | |
| Stress | | | | | |
| Error Handling | | | | | |
| Regression | | | | | |

## Failed test cases

List every failed case in full — this section feeds bug-report generation directly, so don't summarize it away. Leave **Bug ID** blank until a bug report has actually been generated for that row.

| Test Case ID | Title | Expected Result | Actual Result | Suggested Severity | Bug ID |
|---|---|---|---|---|---|
| TC-00X | | | | | *(pending)* |

## Blocked / Not Run cases (if any)

| Test Case ID | Title | Status | Reason |
|---|---|---|---|
| TC-00X | | Blocked | *why it couldn't be executed* |

## Notes / Risks

*Anything that affects how the pass rate should be read — e.g. a large chunk of cases blocked by an environment issue, or failures clustered in one module.*

---

**Next step:** offer to generate a bug report (SKILL.md Part 4) for each row under "Failed test cases," pulling Title, Test Case ID, Expected/Actual Result, and Environment/Build directly from this report rather than asking the user to restate them.

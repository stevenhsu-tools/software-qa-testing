# QA Test Report Template

Use this after test cases (see `test-case-template.md`) have been executed. This report is the direct input to bug-report generation — every row listed under "Failed test cases" below should become one bug report (see SKILL.md Part 4).

## Header

**Report Title:** *e.g. "QA Test Report — Checkout Module, src/checkout/"*

**Feature / Code Under Test:** *What was tested — reference the file(s)/function(s) from the test case set's "Code Scanned" field.*

**Build / Version Tested:** *Build number, commit hash, or branch — whatever identifies the code state tested.*

**Date:** *Date report generated.*

**Tester(s):** *Who ran the cases (a person, or the AI agent e.g. Claude/Codex/Antigravity if they conducted the testing).*

**Environment(s):** *OS/browser/config/runtime the cases were run in.*

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
| Executed (automated) | |
| Executed (manual walkthrough) | |
| Traced (code read, not run) | |

The last three rows matter for how much confidence to place in the results above — a report built mostly from `Traced` cases is a code review, not a test run, and should be presented to the reader with that caveat.

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

| Test Case ID | Title | Source Location | Expected Result | Actual Result | Verification Method | Suggested Severity | Bug ID |
|---|---|---|---|---|---|---|---|
| TC-00X | | | | | | | *(pending)* |

## Blocked / Not Run cases (if any)

| Test Case ID | Title | Status | Reason |
|---|---|---|---|
| TC-00X | | Blocked | *why it couldn't be executed* |

## Notes / Risks

*Anything that affects how the pass rate should be read — e.g. a large chunk of cases blocked by an environment issue, or failures clustered in one module.*

---

**Next step:** offer to generate a bug report (SKILL.md Part 4) for each row under "Failed test cases," pulling Title, Test Case ID, Expected/Actual Result, and Environment/Build directly from this report rather than asking the user to restate them.

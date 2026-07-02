---
name: software-qa-testing
description: Software QA testing knowledge, test case generation, QA test report generation, and bug-report generation, based on classic QA testing methodology (construction/system/special testing, white-box/black-box approaches, test case design techniques, bug lifecycle, and IEEE-829-based defect tracking fields). Use this skill whenever the user is planning test coverage, choosing a test type or test design technique, wants a set of test cases written before testing starts, wants a QA test report summarizing how many test cases ran and their pass/fail results, or wants a bug/defect report written up — especially when they need the bug classified with a Severity level (High/Medium/Low/Minor) and a Priority level (1-5). Trigger on mentions of "test plan", "test case", "test cases", "QA", "test report", "test results", "bug report", "defect", "severity", "priority", "regression test", "smoke test", "white box"/"black box testing", or "bug tracking", even if the user doesn't use the exact word "skill".
---

# Software QA Testing

This skill packages a structured approach to software testing and defect (bug) reporting, drawn from a professional QA methodology used in commercial software shops. It covers the full pipeline: (1) how to think about and describe testing methods/coverage, (2) generating the test cases themselves before testing starts, (3) rolling executed test cases up into a QA test report, and (4) turning any failed test case into a bug report that classifies the defect by Severity and Priority so it can move through a bug-tracking workflow (e.g. Bugzilla, Jira, TestTrack).

The four parts are meant to flow in order on a real project: design test cases first (Part 2) → run them and log results in a QA test report (Part 3) → generate a bug report for every failure the report surfaces (Part 4), with the bug report's Severity/Priority and repro details pulled straight from the corresponding test case rather than re-derived from scratch.

## When to use this

- The user is designing or describing a test plan, test case set, or test strategy.
- The user asks "what kind of testing should I do for X" or wants test types explained (unit, integration, smoke, regression, stress, etc.).
- The user wants a set of test cases written up before testing begins, for a feature, requirement, or spec.
- The user has run tests and wants a QA test report: how many test cases were run, and how many passed/failed/blocked.
- The user wants a bug/defect written up formally — from a raw description, a stack trace, a screenshot, a one-line complaint, or a failed test case in a QA test report.
- The user asks you to classify a bug's Severity or Priority, or set up a bug-tracking field structure.

## Part 1 — Testing Methods

Testing methods break down along three independent axes: **type** (what stage/scope), **technique** (how prepared and how executed), and **approach** (what you can see: code or behavior). Real projects combine all three — e.g. "a formally-planned, automated, black-box Regression Test."

For full detail, worked examples, and design guidance for each individual test-case technique (boundary, functional, configuration, stage, stress, error-handling, regression), read `references/testing-methods.md`. Below is the map to help you pick the right term and explain it correctly.

### Testing Types

**Construction Testing** (a.k.a. Developer Testing) — happens while code is still being built, usually run by the developers themselves.
- *Single Step Test* — stepping through code line-by-line with a debugger.
- *Bench Test* — a quick, informal check of an early/partial piece of functionality.
- *Unit Test* — tests the smallest testable piece (a function/method/class) in isolation. Frameworks: JUnit, CppUnit, etc.
- *Component Test* — tests a group of units working together.
- *Developer Integration Test* — developers combine their components and test the integration before handing off to QA. This is where a lot of bugs surface, often due to team miscommunication.

**System Testing** (a.k.a. QA Testing) — runs against a built version of the product, from the user's point of view.
- *Integration Test* — checks stability once internal subsystems and third-party software are combined.
- *Smoke Test* — a fast (~10 minute) sanity check on a new build to decide if it's even stable enough to receive full system testing. A build that fails smoke test should be rejected outright rather than partially tested.
- *Functional Test* — verifies the software meets its stated feature requirements. Usually the single most important test category.
- *Configuration Test* — verifies behavior across the different setup/configuration combinations a user could choose.
- *Release Test* — final check that the release package installs and runs correctly for end users.
- *Acceptance Test* — checks the product against customer-defined Acceptance Criteria (or market/core-customer expectations for consumer software).

**Special Testing** — needs extra time, people, or equipment; scheduled separately from the main test cycle.
- *Regression Test* — re-run (a subset of) prior tests after a change, to catch side effects from bug fixes or spec changes. Test cases here should be reusable and incremental (grown over time), and are prime candidates for automation.
- *Performance Test* — measures resource usage and behavior over time (memory, CPU, network).
- *Stress Test* — finds the breaking point under load (e.g., "does it survive 3000 concurrent users / 10,000 records?").
- *Compatibility Test* — checks behavior across different OS/hardware/software environments.
- *Alpha and Beta Test* — staged testing with internal (Alpha) then external (Beta) real users before release, to surface real-world issues and validate market fit.

### Testing Techniques

- **Preparation**: *Formal Testing* (planned in advance with a Test Plan and Test Cases — best for larger teams/projects) vs. *Informal Testing* (ad hoc, relies on tester experience — realistic for small teams with limited resources).
- **Execution**: *Manual Testing* vs. *Automated Testing*. Most real projects run a mix (e.g. 60% manual / 40% automated); pushing more into automation generally improves efficiency and repeatability, especially for regression suites.

### Testing Approach

- **White-Box Testing** (Structural Testing) — tester has visibility into the code; usually kept in-house. Measured via:
  - *Statement Coverage* — every statement executed at least once.
  - *Branch Coverage* — every decision point's branches executed at least once.
  - *Condition Coverage* — every individual boolean condition evaluated both ways at least once.
  - *Multiple Condition Coverage* — every combination of conditions exercised.
- **Black-Box Testing** — tester only sees inputs/outputs and behavior, not code; can be outsourced. Measured via:
  - *Test Case Coverage* — every planned test case executed.
  - *Input/Output verification* — inputs and resulting outputs are repeatedly exercised and checked against expectations.

### Test Case Design Techniques

When actually writing test cases, these are the standard design lenses — see `references/testing-methods.md` for the full worked examples of each:
1. **Boundary Test** — edge values, limits, overflow conditions (e.g., integer overflow, buffer overflow at max-length input).
2. **Functional Test** — does the feature do what the spec says, across all its stated inputs/outputs.
3. **Configuration Test** — every meaningful combination of user-configurable settings.
4. **Stage Test** — behavior across different states/usage paths a real user could follow (not just the "happy path").
5. **Stress Test** — behavior at/near defined resource or load limits.
6. **Error Handling Test** — does the software prevent errors, handle them gracefully, and clearly inform the user, when things go wrong (bad input, corrupted files, lost connections, unknown error codes).
7. **Regression Test** — re-verify core + previously-fixed functionality after a change.

## Part 2 — Generating Test Cases

Before any testing happens, write the test cases down. This is what gets executed later and rolled up into the QA test report — skipping straight to "testing" without a written test case set makes coverage impossible to measure or repeat.

### Workflow

1. Establish scope: what feature, requirement, or spec is being tested? Read whatever the user gives you (a PRD, a design spec, a one-line feature description) to identify the behaviors that need covering. If scope is genuinely unclear, ask rather than guessing at what the feature does.
2. Decide which testing types and test-case design techniques from Part 1 actually apply. Don't default to only "Functional Test" — for anything with inputs, limits, configuration options, or failure modes, also pull in Boundary, Configuration, Error Handling, and (if this is a change to existing behavior) Regression.
3. Write one row per test case using the fields below, numbering them sequentially (TC-001, TC-002, ...) so they can be referenced later from a QA test report or a bug report.
4. Group the cases by test type/technique so coverage gaps are visible at a glance (e.g. "6 Functional, 4 Boundary, 3 Error Handling, 2 Configuration").
5. Leave **Actual Result** and **Status** as `Not Run` — this step happens before execution. Don't invent results.
6. Output using `assets/test-case-template.md`; read it before generating so the fields stay consistent with the QA test report and bug report templates downstream.

### Test case fields

Test Case ID, Title, Test Type/Technique (from Part 1's taxonomy), Related Requirement/Feature, Preconditions, Test Steps (numbered), Test Data, Expected Result, Actual Result (blank/`Not Run` until executed), Status (`Not Run` / `Pass` / `Fail` / `Blocked`), Linked Bug ID (filled in later only if the case fails).

### A note on volume

If the feature has a large combination space (e.g. configuration settings), don't silently enumerate hundreds of cases — either ask whether full combinatorial coverage is wanted, or apply the Sampling/Combination Test approach from `references/testing-methods.md` and say explicitly that you've sampled rather than covered every combination.

## Part 3 — QA Test Report

Once test cases have been executed (by the user, by an automated suite, or by you working through them), roll the results up into a QA test report. This is the artifact that answers "how much did we test, and how did it go" — and it's also the direct input to Part 4's bug reports.

### Workflow

1. Take the test case set from Part 2 (or whatever set of test cases the user gives you) along with each case's actual result.
2. Tally the summary counts: Total, Passed, Failed, Blocked, Not Run, and Pass Rate (Passed ÷ (Total − Not Run), not divided by Total, so an incomplete run doesn't look artificially worse).
3. Break the tally down by test type/technique so it's clear where the weak spots are (e.g. all 3 failures are in Error Handling Test).
4. List every **Failed** test case explicitly, with its expected vs. actual result — this list is exactly what Part 4 turns into bug reports, so keep it complete and don't summarize failures away.
5. Note any Blocked or Not Run cases and why, if known — a report that hides incomplete coverage is misleading.
6. Output using `assets/qa-test-report-template.md`; read it before generating.

### After the report: offer to generate bug reports

Once the QA test report is produced, offer (or proceed, if the user already asked for both) to generate a bug report for each Failed test case per Part 4 — don't make the user re-describe a bug that's already fully specified in the test report.

## Part 4 — Bug Reports with Severity and Priority

When the user describes a bug (in any amount of detail — a one-liner, a log, a screenshot, or a full repro), write it up as a structured bug report. Always assign both a **Severity** and a **Priority** — they answer different questions and often diverge in practice, so don't treat them as the same field.

- **Severity** = how badly does this damage the *product's quality* (a QA/technical judgment, usually the tester's call).
- **Priority** = how urgently does this need to be *fixed relative to the project* — schedule, market, contractual pressure (usually a project-management call, and can legitimately override severity — e.g. a Medium-severity bug might still get Priority 5 because the release ships this week and the team has no spare capacity).

If the user hasn't specified a priority and it isn't obvious from context, propose one but flag that it's your best guess and that the project/QA manager may want to override it.

### Severity levels

| Severity | Description | Examples |
|---|---|---|
| **High** | Core functionality is missing or completely non-functional; system halts; work cannot continue; blocks further testing. | GPF (General Protection Fault), Blue Screen, Crash, System Hang, requires reboot. |
| **Medium** | Main functionality is incomplete or partially broken; some system functions misbehave, but testing can continue. | Access Violation, unhandled Software Exception, inconsistent results between test paths. |
| **Low** | Functionality works, but with inconsistencies; doesn't block anything. | Inconsistent UI, incorrect (but non-blocking) text/labels, awkward state transitions. |
| **Minor** | Functionality works correctly; cosmetic/convenience issue only. | Doesn't fully match user habits/expectations, e.g. not remembering the last-used filename. |

### Priority levels

| Priority | Resolution expectation |
|---|---|
| **1** | Fix immediately |
| **2** | Fix before the next stage/milestone |
| **3** | Fix before release |
| **4** | Fix if time is available |
| **5** | Fix in the next version |

### Bug report fields

Base every generated bug report on these fields (adapted from IEEE Std 829 and standard bug-tracking practice). Use `assets/bug-report-template.md` as the fill-in template — read it before generating a report so the output stays consistent:

ID, Title, Description, Found in Build, **Priority**, **Severity**, Date Submitted, Test Case, Occurrences, Procedure (repro steps), Attachments, Symptom, Software Module, Environment, Person Submitted, Person Assigned, Date Assigned, Owner, Status, Resolution, Resolution Description, Fixed in Build, Date Closed, Bug Return Cycle (how many times it's bounced between tester and developer).

### Resolution status values

When a bug's Status/Resolution needs to be set (or the user asks what to put there), use one of these standard values:

- **Fixed** — resolved; tester must re-verify in the next build.
- **Bug (confirmed, pending)** — developer confirms it's real but hasn't addressed it yet (for whatever reason).
- **Not a Bug** — developer determines this is expected/intended behavior.
- **Not Fix** — tester re-checked and confirms the issue is still not resolved.
- **Not Reproducible** — developer cannot reproduce it from the given steps.
- **No Plan to Fix** — confirmed real, but won't be addressed in this version.
- **User Error** — caused by user misuse, not a real defect.
- **Need More Information** — developer needs more detail before they can judge it.
- **Duplicated** — already reported as another bug.

### Generating a report — workflow

1. Identify your source. Two cases:
   - **Ad hoc**: the user gives you a raw description, log, screenshot, or one-liner. Read it and identify: what happened, what should have happened, how to reproduce it, and what environment/build it was found in.
   - **From a QA test report (Part 3)**: pull Title, Test Case ID, Environment/Build, Steps to Reproduce, and Expected/Actual Result directly from the failed test case row. Don't ask the user to restate anything the test report already specifies — only ask if something genuinely wasn't captured (e.g. the report has no environment field filled in).
2. Classify **Severity** using the table above — ask yourself "how much does this damage the product," not "how annoying is it to me." When working from a test report, base this on the gap between that case's Expected and Actual Result, not on how many other cases also failed.
3. Propose a **Priority** — if the user gives you release timing/business context, use it; otherwise pick a reasonable default and say so.
4. Fill in `assets/bug-report-template.md` with everything you know; leave fields explicitly marked "Unknown" or "TBD" rather than guessing at facts (e.g. don't invent a build number). Set the **Test Case** field to the originating Test Case ID whenever the bug came from a test report, so the two documents stay cross-referenced.
5. Output the filled report in Markdown (default) unless the user asks for a specific bug-tracker format (Bugzilla, Jira, etc.) or a file (.docx/.xlsx) — adapt the field names to match their tool if named.
6. If several bugs are described at once, or several test cases failed, produce one report per bug, and lead with a summary table (Bug ID / Title / Linked Test Case / Severity / Priority) so the user can triage at a glance before reading each full report.

## Reference files

- `references/testing-methods.md` — full detail and worked examples for every test type and test-case design technique listed above; read this when the user wants a deeper explanation, a worked example, or help designing actual test cases (not just terminology).
- `assets/test-case-template.md` — the fill-in template for generating test cases; read this before generating any test case set (Part 2).
- `assets/qa-test-report-template.md` — the fill-in template for the QA test report; read this before summarizing executed test cases (Part 3).
- `assets/bug-report-template.md` — the fill-in-the-blank bug report template; read this before generating any bug report (Part 4).

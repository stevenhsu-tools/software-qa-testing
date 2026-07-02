---
name: software-qa-testing
description: Software QA testing knowledge, code-driven test case generation, QA test report generation, and bug-report generation, based on classic QA methodology (construction/system/special testing, white/black-box approaches, test design techniques, bug lifecycle, IEEE-829-based defect fields). Generates test cases by reading the actual code (functions, branches, validation, config, external calls) instead of requiring a spec/PRD, and asks upfront whether the user wants test cases only or the full design-test-report-bugs pipeline. Use whenever the user is planning test coverage, wants test cases for a file/function/module, wants a QA test report (pass/fail counts), or wants a bug written up and classified with Severity (High/Medium/Low/Minor) and Priority (1-5). Trigger on "test plan", "test case(s)", "QA", "test report", "bug report", "defect", "severity", "priority", "regression test", "smoke test", "white/black box testing", or "bug tracking", even without the word "skill".
---

# Software QA Testing

This skill packages a structured approach to software testing and defect (bug) reporting, drawn from a professional QA methodology used in commercial software shops. It covers the full pipeline: (1) how to think about and describe testing methods/coverage, (2) generating test cases by reading the actual code, (3) conducting the testing and rolling results up into a QA test report, and (4) turning any failed test case into a bug report that classifies the defect by Severity and Priority so it can move through a bug-tracking workflow (e.g. Bugzilla, Jira, TestTrack).

The parts flow in order on a real project: confirm the goal (Start Here) → design test cases from the code (Part 2) → conduct the testing and log results in a QA test report (Part 3) → generate a bug report for every failure the report surfaces (Part 4), with the bug report's Severity/Priority and repro details pulled straight from the corresponding test case rather than re-derived from scratch.

## When to use this

- The user is designing or describing a test plan, test case set, or test strategy.
- The user asks "what kind of testing should I do for X" or wants test types explained (unit, integration, smoke, regression, stress, etc.).
- The user wants test cases generated for a piece of code, a file, a function, a module, or a feature — with or without a spec.
- The user has run tests (or wants you to conduct the testing) and wants a QA test report: how many test cases were run, and how many passed/failed/blocked.
- The user wants a bug/defect written up formally — from a raw description, a stack trace, a screenshot, a one-line complaint, or a failed test case in a QA test report.
- The user asks you to classify a bug's Severity or Priority, or set up a bug-tracking field structure.

## Start Here — Confirm the Goal

Before generating anything, ask the user which of these they want (use a structured choice prompt if the environment supports one, otherwise ask directly in a short question):

1. **Design test cases only** — generate the test case set (Part 2) from the code and stop there.
2. **Full pipeline** — design test cases from the code (Part 2), conduct the testing (Part 3), generate the QA test report (Part 3), and generate a bug report for every failure (Part 4).

Skip the question only if the user's request already makes the goal unambiguous (e.g. "just give me test cases for this function" clearly means option 1; "test this module and report any bugs" clearly means option 2) — but still state which path you're taking before you start, so there's no surprise later about how far you went.

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

## Part 2 — Generating Test Cases from the Code

Test cases are derived by reading the actual code, not by requiring a spec, PRD, or requirements doc up front. Most real repos don't have an up-to-date spec, and the code is the ground truth for what a feature actually does — so the absence of a spec is never a blocker. If the user does hand you a spec or design doc, read it too and use it as extra context (especially for *intended* behavior that the code might get wrong), but don't wait for one.

### Workflow

1. **Establish scope.** Which file(s), function(s), module, endpoint, or feature does the user want covered? If they haven't said, ask — but once scope is set, get it from the code, not from a description of the code.
2. **Scan the code directly** (read the actual files — don't infer behavior from filenames or guess). Look specifically for:
   - Public functions/methods/endpoints and their signatures (parameters, types, return values, defaults) → drives **Functional Test** cases, one per meaningfully distinct behavior.
   - Conditionals, branches, loops, and boundary constants (min/max limits, array/buffer sizes, off-by-one-prone loop bounds, numeric types that can overflow) → drives **Boundary Test** cases and tells you what White-Box coverage (Part 1) the code structure implies.
   - Input validation, try/catch blocks, error returns, and calls into external error codes → drives **Error Handling Test** cases.
   - Config reads (env vars, settings files, feature flags, user-configurable options) → drives **Configuration Test** cases.
   - Calls to external services, databases, queues, or third-party APIs → drives **Integration Test** cases.
   - Loops or queries over collections, pagination, batch/bulk operations → drives **Stress Test** candidates (what happens at 10x, 1000x the typical size).
   - Existing test files for the same code (if any) — read them so you don't duplicate covered ground; instead, call out the gaps they leave.
3. **Map findings onto the Part 1 taxonomy** and write one test case per meaningfully distinct behavior or edge case you actually found in the code. Reference the real file path and function/method name (and line numbers where useful) in each case instead of a spec section — that's what makes the case traceable back to what you read.
4. Number cases sequentially (TC-001, TC-002, ...) so they can be referenced later from a QA test report or a bug report, and group them by test type/technique so coverage gaps are visible at a glance (e.g. "6 Functional, 4 Boundary, 3 Error Handling, 2 Configuration").
5. Leave **Actual Result** and **Status** as `Not Run` — this step happens before execution. Don't invent results, and don't run anything yet even if you could — that's Part 3.
6. Output using `assets/test-case-template.md`; read it before generating so the fields stay consistent with the QA test report and bug report templates downstream.
7. If the codebase is too large to read in full (e.g. "generate test cases for the whole repo"), say explicitly what you scanned and what you didn't, rather than silently sampling a subset and presenting it as complete coverage.

### Test case fields

Test Case ID, Title, Test Type/Technique (from Part 1's taxonomy), Source Location (file path + function/method, and line numbers where useful), Preconditions, Test Steps (numbered), Test Data, Expected Result, Actual Result (blank/`Not Run` until executed), Status (`Not Run` / `Pass` / `Fail` / `Blocked`), Verification Method (filled in at execution — see Part 3), Linked Bug ID (filled in later only if the case fails).

### A note on volume

If the code has a large combination space (e.g. many configuration settings, or a function with many branches), don't silently enumerate hundreds of cases — either ask whether full combinatorial coverage is wanted, or apply the Sampling/Combination Test approach from `references/testing-methods.md` and say explicitly that you've sampled rather than covered every combination.

## Part 3 — Conduct the Testing and Generate the QA Test Report

This part only runs if the user chose the full pipeline (Start Here). It has two halves: actually conducting the testing, then rolling the results into a report.

### Conducting the testing

How you execute a case depends on what's actually available — be honest about which of these you used for each case, because "I ran it" and "I read the code and reasoned about what it would do" are very different levels of confidence:

- **Executed (automated)** — you ran a real test command (unit test framework, a script you wrote, an API call, etc.) via your tools and observed the actual output. This is the strongest evidence and should be preferred whenever the project has a runnable test setup (e.g. `pytest`, `npm test`) or you can write a small runnable check.
- **Executed (manual walkthrough)** — the user ran the case themselves (e.g. clicked through a UI) and told you the result.
- **Traced (code read, not run)** — you determined the expected-vs-actual behavior by reading the code rather than executing anything, because nothing was runnable in the environment. This is legitimate and often necessary, but say so explicitly rather than presenting it with the same confidence as an executed result — a manual trace can miss runtime-only issues (race conditions, environment-specific behavior, actual exceptions).

Record whichever applies in each case's **Verification Method** field. Don't mark a case `Pass` or `Fail` without picking one of these — "Not Run" cases stay "Not Run."

### Generating the report — workflow

1. Take the test case set from Part 2 (or whatever set of test cases the user gives you) along with each case's actual result and verification method.
2. Tally the summary counts: Total, Passed, Failed, Blocked, Not Run, and Pass Rate (Passed ÷ (Total − Not Run), not divided by Total, so an incomplete run doesn't look artificially worse).
3. Break the tally down by test type/technique so it's clear where the weak spots are (e.g. all 3 failures are in Error Handling Test), and note how many cases were Executed vs. Traced so the user knows how much confidence to place in the pass rate.
4. List every **Failed** test case explicitly, with its expected vs. actual result — this list is exactly what Part 4 turns into bug reports, so keep it complete and don't summarize failures away.
5. Note any Blocked or Not Run cases and why, if known — a report that hides incomplete coverage is misleading.
6. Output using `assets/qa-test-report-template.md`; read it before generating.

### After the report: generate bug reports

If the user chose the full pipeline, continue straight into Part 4 and generate a bug report for each Failed test case — don't stop and make the user ask again, and don't make them re-describe a bug that's already fully specified in the test report.

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
   - **From a QA test report (Part 3)**: pull Title, Test Case ID, Source Location (→ **Software Module**), Environment/Build, Steps to Reproduce, and Expected/Actual Result directly from the failed test case row. Don't ask the user to restate anything the test report already specifies — only ask if something genuinely wasn't captured (e.g. the report has no environment field filled in). If the case's Verification Method was `Traced (code read, not run)` rather than `Executed`, say so in the bug report's Description — the developer picking this up should know whether the failure was observed running or inferred from reading the code.
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

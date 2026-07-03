# Testing Methods — Detailed Reference

This file expands on the testing types and test-case design techniques summarized in SKILL.md, with the worked examples and reasoning behind each one. Read the section you need; you don't need to read this whole file for a quick terminology question (SKILL.md's summary table usually covers that).

## Table of contents
1. Construction Testing (detail)
2. System Testing (detail)
3. Special Testing (detail)
4. Test Case Design Techniques (detail + worked examples)
   - Boundary Test
   - Functional Test
   - Configuration Test
   - Stage Test
   - Stress Test
   - Error Handling Test
   - Regression Test
   - Other test case types

---

## 1. Construction Testing

Construction Testing (Developer Testing) happens while code is still being built, and is normally run by the developers themselves — sometimes with tools they build for the purpose.

A common anti-pattern: staffing construction testing entirely with junior developers under the theory that it "frees up" senior developers. In practice, junior engineers often can't push back effectively when they find a senior engineer's bug, which undermines quality. A better pattern is rotating engineers through each other's code (the same principle behind Extreme Programming's emphasis on unit testing).

The five sub-types, in increasing scope:
- **Single Step Test** — step through code with a debugger, statement by statement.
- **Bench Test** — an early, informal test of a rough/partial implementation of one feature.
- **Unit Test** — the smallest unit of the system (function/method/class) tested in isolation, especially across its different execution paths. This is the type most associated with XP/TDD practice. Frameworks: JUnit (Java), CppUnit (C++), Rational Test RealTime, etc. Unit testing is notably harder to do well in object-oriented code than in procedural code.
- **Component Test** — one or more units composed together and tested as a group.
- **Developer Integration Test** — developers combine their separately-built components and test the integration before handoff to QA. This stage tends to have a high defect rate, especially in geographically distributed / internationally organized teams, because most of the failures trace back to communication/coordination gaps rather than individual code defects.

## 2. System Testing

Once developer integration testing is done, the software is compiled into a Build Version and formal System Testing (a.k.a. "QA Testing") begins — testing from the user's point of view, covering the required OS/hardware/third-party software combinations. This is normally run by dedicated system test engineers.

Six sub-types:
- **Integration Test** — focuses on stability and functional correctness once internal subsystems and required third-party software are combined.
- **Smoke Test** — every new build must pass a fast sanity check (rule of thumb: under ~10 minutes) before QA invests time in full system testing. If a build fails smoke test, reject it and ask for a new build rather than testing around the broken area — testing a build you already know is broken wastes effort, and there's no guarantee an unrelated fix didn't have side effects elsewhere. (Cautionary real-world example: a team once compiled 18 separate builds in a single day because they skipped smoke testing — a clear sign of process failure, not just wasted effort.)
- **Functional Test** — verifies the software meets its documented feature requirements. Usually considered the single most important test category, since a functional defect is the most visible kind of failure to a paying customer.
- **Configuration Test** — different users configure the same software differently; this type verifies all meaningful configuration combinations work. See the "Configuration Test" section below for a full worked example including sampling/combination testing when full combinatorial coverage isn't feasible.
- **Release Test** — confirms the shipped package installs and runs correctly, with all required functionality intact.
- **Acceptance Test** — the customer (or, for consumer software, market research / core customers) defines Acceptance Criteria; the product must pass Release Test *and* meet these criteria before the customer signs off.

## 3. Special Testing

Special Testing needs more time/people/equipment and is usually scheduled as its own phase, typically toward the back end of development. Not every product needs every sub-type — e.g. stress-testing a PDA app that only ever has one user is probably wasted effort.

- **Regression Test** — re-verifies previously-working functionality after a change, to catch side effects. Essential on larger projects. The safest strategy (rerun every test case every time) is also the least efficient — if a full pass takes 3-5 person-days, that cost compounds fast, which is why regression suites lean heavily on automation and are typically scoped down rather than run in full every time. See the worked example below.
- **Performance Test** — measures resource consumption and behavior under sustained use: memory, CPU, network bandwidth, etc. Distinct from Stress Test — performance testing produces benchmark data; stress testing finds the breaking point.
- **Stress Test** — see the worked example below for the "find the safe operating limit" methodology and a real incident (client registration storms overwhelming a server at ~300 concurrent client registrations).
- **Compatibility Test** — like Configuration Test, but focused on the different hardware/OS/software environments users might run in, since control over the runtime environment ultimately belongs to the user, not the developer.
- **Alpha and Beta Test** — Alpha (internal) then Beta (external, real users) milestones let the team catch real-world usage problems early and validate against actual market needs before general release.

---

## 4. Test Case Design Techniques

These are the practical lenses for actually writing test cases, once you've decided which testing *type* you're doing.

### Boundary Test

Targets edge values and limits — the classic examples are integer overflow (e.g., an `unsigned int` accumulator wrapping around past its max value and silently producing wrong results) and buffer overflow (e.g., a fixed-size input buffer overflowing once input length crosses some threshold, potentially causing an Access Violation). Design boundary cases around: field length limits, numeric min/max, special/reserved characters, and any place a fixed-size buffer or counter is used internally. A boundary bug is often invisible under normal use and only shows up with a specifically-crafted edge-case input — which is exactly why it needs a dedicated test-case category rather than relying on functional tests to stumble into it.

### Functional Test

Arguably the single most important test-case category — verifies the feature does what the Product Requirement Document / Software Design Specification says it should. Before functional testing starts, the requirements and design documents should already have gone through Review and Inspection. Good functional test case design checks, for each requirement: does it do what's expected, does it do it correctly, does it do it consistently, and does it fail gracefully when it can't.

### Configuration Test

Because control over configuration lives with the end user, this can explode combinatorially — a real example in the source material had 64 combinations for one setting group and 192 total once a second setting (retention days) was factored in. Recommended approach:
1. Keep all user-configurable settings in an external, independently-inspectable store (an INI file or registry key) rather than buried in program logic — this makes the settings testable (can be driven by automation), maintainable (support staff can diagnose/adjust directly), and extendable (customers can integrate with other systems and settings survive migrations).
2. Test in three passes: (a) spot-check that the Element → Event Trigger → Result flow works at all for a sample setting; if this fails, stop and report immediately rather than continuing; (b) confirm every setting combination writes correctly to the config store; (c) write a small test harness that populates the config store directly and verifies the program picks up and acts on each value correctly.
3. If full combinatorial coverage isn't affordable, use **Sampling Test / Combination Test** — deliberately sample a representative subset of the combination space rather than testing every combination, accepting a controlled quality tradeoff against schedule/resource/market pressure. Assign short codes to each configurable value, then design a small set of cases that each combine several codes together to maximize the combinations covered per case (e.g. 6 sampled cases covering combinations drawn from a 192-case space).

### Stage Test

Different users approach the same software via different usage paths depending on their habits and goals — a common support pattern is developers blaming "user error" when in fact the software only tested the path the developer assumed was normal. Stage Test cases should deliberately walk through the *alternate* real-world usage paths and states a user might reach a feature from, not just the one "expected" path.

### Stress Test

Distinct from Performance Test: Performance Test produces benchmark numbers; Stress Test hunts for the safe operating limit — the point beyond which the system breaks (analogy: leaning back in a chair — there's a tilt angle beyond which you fall, and stress testing finds that angle so you can stay safely under it). Design stress conditions around variables like CPU speed/utilization, disk space, physical/virtual memory, concurrent user count, and data volume — assign each a short code and design cases that push combinations of these toward known limits. A documented real incident: a client-server product repeatedly hung every morning because up to 3000 clients all auto-registered with the server at login time over HTTP; stress testing found the actual safe limit was around 300 simultaneous client registrations, which then became a documented deployment constraint. Some teams split this into Load Test vs. Stress Test as separate categories — treat that as an organizational choice, not a methodological requirement. Stress testing is also a good opportunity to run a **Race Condition** / low-end-hardware check: install on minimum-spec hardware and confirm functional behavior stays correct even if performance is slower (a real example found a debug-only screen that flashed briefly on low-end hardware but was invisible on fast hardware because it rendered too quickly to notice — still a real bug from a QA point of view).

### Error Handling Test

Covers whether the software (a) prevents likely error conditions, (b) handles errors that do occur without crashing or corrupting data, and (c) clearly informs the user what went wrong, instead of surfacing a raw/unmapped error code. Design cases around three angles:

| Angle | Question | Example case |
|---|---|---|
| Error Prevention | What will/could go wrong, and does the program guard against it? | Pull the network cable mid-update and observe behavior. |
| Error Handling | Once an error has occurred, how does the program cope? | Feed the program a corrupted file and check it doesn't crash. |
| Error Information | How is the error communicated to the user? | Point a backup at an unavailable drive and confirm the message is meaningful, not a raw system error code. |

A real documented failure mode: a product's update mechanism returned an undocumented error code from an uncommon proxy server, which the client software couldn't map to anything meaningful and displayed as "unknown error" — tracing it required packet capture on-site. Lesson: error-handling tests should include unexpected/third-party error sources, not just the errors your own code generates deliberately.

### Regression Test

The point isn't to re-test everything — it's to confirm that (a) core functionality still works and (b) other main features still work, after a change. Design regression cases so they are **reusable** and **incremental**: reusable because you'll run them repeatedly across many builds; incremental because the regression suite should grow as new functionality is completed, not stay fixed. A practical selection heuristic: pull test cases from your functional test suite that are already automated, prioritizing core/critical functionality first — anything that can run unattended is a strong candidate to fold into the regression suite. Run regression testing after any significant code change, immediately following verification of the specific fix that triggered the change.

### Other test case types (briefly)

Beyond the seven above, also consider as needed: **User Interface Test** (consistency/usability of the UI itself), **Final Release Verification Test** (a last check specifically on the release package), **Result Verification Test** (correctness of computed/returned results specifically), plus Performance Test and Compatibility Test (described above under Special Testing, but also applicable as test-case-level lenses within a feature area).

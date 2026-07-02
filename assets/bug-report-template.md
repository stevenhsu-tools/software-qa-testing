# Bug Report Template

Fill in every field. If something genuinely isn't known, write `Unknown` or `TBD` rather than guessing — don't invent a build number, date, or environment. Delete this instruction line and the guidance notes (in *italics*) from the final output; they're here to explain what belongs in each field, not to appear in the delivered report.

---

**ID:** *Unique bug ID (leave as `TBD` if the user's tracker will assign it automatically).*

**Title:** *One-line summary — specific enough to distinguish this from other bugs at a glance.*

**Description:** *What's wrong, in a sentence or two. What happened vs. what should have happened. If this came from a QA test report, note the case's Verification Method here too (e.g. "found via automated execution" vs. "found by tracing the code, not yet run") so whoever picks this up knows how strong the evidence is.*

**Severity:** `High` / `Medium` / `Low` / `Minor` *(see SKILL.md severity table — judge by damage to the product, not personal annoyance)*

**Priority:** `1` – `2` – `3` – `4` – `5` *(see SKILL.md priority table — judge by urgency relative to schedule/release, not severity)*

**Found in Build:** *Version/build number where this was observed.*

**Software Module:** *Which component/module/feature area this belongs to.*

**Environment:** *OS, hardware, configuration, browser/runtime version — whatever is relevant to reproduce it.*

**Test Case:** *Which test case ID (e.g. TC-003) uncovered this, if any — this is what cross-references back to the QA test report and the code location it was derived from.*

**Date Submitted:** *Date found.*

**Person Submitted:** *Who found it.*

**Occurrences:** *How often/reliably it happens (e.g. "every time," "intermittent, ~1 in 5 tries").*

**Procedure (Steps to Reproduce):**
1. *Step one*
2. *Step two*
3. *Step three*
...

**Expected Result:** *What should happen.*

**Actual Result:** *What actually happens.*

**Symptom:** *Observable symptom category — e.g. crash, hang, incorrect output, UI glitch, data loss.*

**Attachments:** *Logs, screenshots, screen recordings referenced (list filenames/links, or `None`).*

**Attempts to Repeat:** *Number of times reproduction was attempted, and how many succeeded.*

---

*The fields below are typically filled in by whoever owns the bug afterward — include them as empty/TBD placeholders unless the user gives you this information directly.*

**Person Assigned:** `TBD`

**Date Assigned:** `TBD`

**Owner:** `TBD`

**Status:** `Open`

**Resolution:** `TBD` *(one of: Fixed / Bug (confirmed, pending) / Not a Bug / Not Fix / Not Reproducible / No Plan to Fix / User Error / Need More Information / Duplicated)*

**Resolution Description:** `TBD`

**Fixed in Build:** `TBD`

**Date Closed:** `TBD`

**Bug Return Cycle:** `0` *(increments each time the bug bounces between tester and developer)*

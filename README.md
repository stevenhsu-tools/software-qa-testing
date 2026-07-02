# Software QA Testing — Claude Code Skill

A [Claude Code skill](https://code.claude.com/docs/en/skills) that packages a professional QA testing methodology end to end: testing types and techniques, white-box/black-box test design, test-case design techniques (boundary, functional, configuration, stage, stress, error handling, regression), generating the test cases themselves before testing starts, rolling executed results into a QA test report, and a bug-report generator that classifies defects by **Severity** (High/Medium/Low/Minor) and **Priority** (1–5), following an IEEE-829-based field set.

The intended flow: **generate test cases** → **run them and log a QA test report** (how many ran, pass/fail counts) → **generate a bug report for every failure**, with the bug's Severity/Priority and repro details pulled straight from the failed test case rather than re-derived from scratch.

Once installed, Claude will use this skill automatically when you're planning test coverage, designing test cases, logging test results, or writing up a bug — or you can invoke it directly by typing `/software-qa-testing`.

## What's in this repo

```
software-qa-testing/
├── SKILL.md                          # Core instructions Claude reads when the skill triggers
├── references/
│   └── testing-methods.md            # Deep-dive reference: worked examples for each testing type/technique
├── assets/
│   ├── test-case-template.md         # Fill-in template for generating test cases (before testing)
│   ├── qa-test-report-template.md    # Fill-in template for the QA test report (after execution)
│   └── bug-report-template.md        # Fill-in template used when generating a bug report (from a failure)
└── .claude-plugin/
    ├── plugin.json                   # Plugin manifest (for marketplace-style install)
    └── marketplace.json              # Marketplace manifest (lists this one plugin)
```

## Installing

There are two ways to install this — pick whichever fits how you work. Both end with the skill available as `/software-qa-testing` in Claude Code.

### Option A — Copy as a skill (simplest, recommended)

Choose personal (available in every project) or project-scoped (checked into one repo, shared with your team via git).

**Personal — available in all your projects:**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git ~/.claude/skills/software-qa-testing
```

(Or download this folder and place it at `~/.claude/skills/software-qa-testing/` — the folder name becomes the command name.)

**Project — checked into a specific repo:**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git /path/to/your-project/.claude/skills/software-qa-testing
```

Commit the `.claude/skills/software-qa-testing/` folder to your project's version control so the whole team gets it automatically when they pull.

Either way, restart Claude Code if `.claude/skills/` didn't already exist in that location (Claude Code watches existing skill folders live, but a brand-new top-level `skills/` directory needs a restart to be picked up).

### Option B — Install as a plugin via a marketplace

This repo is already set up as a self-contained plugin (see `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`), so you can also install it the way you'd install any Claude Code plugin:

```text
/plugin marketplace add stevenhsu-tools/software-qa-testing
/plugin install software-qa-testing@software-qa-testing-marketplace
```

Then run `/reload-plugins` to make it available in your current session.

Note: a plugin-installed skill is namespaced by the plugin name, so the command becomes `/software-qa-testing:software-qa-testing` rather than the plain `/software-qa-testing` you get with Option A. Claude will still trigger it automatically either way based on its description — the namespacing only affects manual `/` invocation.

**Repository:** [github.com/stevenhsu-tools/software-qa-testing](https://github.com/stevenhsu-tools/software-qa-testing)

## Using it

Once installed, either:
- **Let Claude trigger it automatically** — ask something like "what testing should I run before release?" or "write up this bug: ..." and Claude will pull in the skill based on its description.
- **Invoke it directly** — type `/software-qa-testing` (or `/software-qa-testing:software-qa-testing` if installed via Option B) followed by your request.

### Example prompts

- "We're about to test our new checkout flow — what should QA cover?"
- "Generate test cases for the password reset feature before we start testing." *(Part 2 — test cases, all `Not Run`)*
- "Here are the results from running those test cases: [...]. Give me a QA test report." *(Part 3 — pass/fail counts and a list of failures)*
- "Turn the failed test cases in that report into bug reports." *(Part 4 — one bug report per failure, Severity/Priority pulled from the test report)*
- "Write up a bug report: the app freezes exporting more than 5000 rows to CSV, build 4.2.1, Windows 11, happens every time, we ship Tuesday." *(ad hoc bug report, no test report needed)*
- "What's the difference between severity and priority, and what fields should our bug tracker have?"

## Updating the skill

Edit `SKILL.md` (and the files under `references/` and `assets/`) directly, then:
- If installed via Option A, just edit the copy in `.claude/skills/software-qa-testing/` (or re-pull if it's a git clone) — Claude Code picks up `SKILL.md` text changes live.
- If installed via Option B (plugin), run `/plugin marketplace update software-qa-testing-marketplace` after pushing changes, then `/reload-plugins`.

## Source

The testing methodology and bug-report taxonomy (Severity/Priority tables, IEEE-829-based fields, Resolution status codes) are adapted from a professional software QA/testing methodology reference, translated and reorganized into English for use as a Claude skill.

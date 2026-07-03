# Software QA Testing — Claude Code / Codex / Google Antigravity Skill

A Claude Code, Codex, and Google Antigravity-compatible skill that packages a professional QA testing methodology end to end: testing types and techniques, white-box/black-box test design, test-case design techniques (boundary, functional, configuration, stage, stress, error handling, regression), generating test cases **by reading the actual code** (no spec/PRD required), conducting the testing and rolling results into a QA test report, and a bug-report generator that classifies defects by **Severity** (High/Medium/Low/Minor) and **Priority** (1–5), following an IEEE-829-based field set.

The intended flow: **confirm the goal** (test cases only, or the full pipeline) → **generate test cases from the code** → **conduct the testing and log a QA test report** (how many ran, pass/fail counts, and whether each case was actually executed or just traced by reading the code) → **generate a bug report for every failure**, with the bug's Severity/Priority and repro details pulled straight from the failed test case rather than re-derived from scratch.

Once installed, Claude Code, Codex, or Google Antigravity can use this skill automatically when you're planning test coverage, designing test cases for a piece of code, logging test results, or writing up a bug. You can also invoke it directly: `/software-qa-testing` in Claude Code, or `$software-qa-testing` in Codex. In Google Antigravity, the skill is automatically discovered and loaded from your customization roots based on your prompt context. When you invoke it to start a testing effort, it asks upfront whether you want test cases only or the full design → test → report pipeline.

## What's in this repo

```
software-qa-testing/
├── SKILL.md                          # Core instructions the agent reads when the skill triggers
├── plugin.json                       # Antigravity plugin manifest
├── references/
│   └── testing-methods.md            # Deep-dive reference: worked examples for each testing type/technique
├── assets/
│   ├── test-case-template.md         # Fill-in template for generating test cases (before testing)
│   ├── qa-test-report-template.md    # Fill-in template for the QA test report (after execution)
│   └── bug-report-template.md        # Fill-in template used when generating a bug report (from a failure)
├── .codex-plugin/
│   └── plugin.json                   # Codex plugin manifest
├── .agents/
│   └── plugins/marketplace.json      # Codex marketplace entry for this plugin
├── skills/
│   └── software-qa-testing -> ..      # Codex plugin skill pointer; keeps one SKILL.md source of truth
└── .claude-plugin/
    ├── plugin.json                   # Plugin manifest (for marketplace-style install)
    └── marketplace.json              # Marketplace manifest (lists this one plugin)
```

## Installing

> [!TIP]
> **The Simplest Way:** You can simply provide this repository link (`github.com/stevenhsu-tools/software-qa-testing`) to your coding agent (Claude Code, Codex, or Google Antigravity) and ask it to perform the installation for you automatically!

Otherwise, you can manually install the skill or plugin for the agent you use. The same `SKILL.md`, references, and assets work across Claude Code, Codex, and Google Antigravity.

### Claude Code

#### Option A — Copy as a skill (simplest, recommended)

Choose personal (available in every project) or project-scoped (checked into one repo, shared with your team via git).

**Personal — available in all your projects:**

- **macOS / Linux:**
  ```bash
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git ~/.claude/skills/software-qa-testing
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git $HOME/.claude/skills/software-qa-testing
  ```
- **Windows (CMD):**
  ```cmd
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git %USERPROFILE%\.claude\skills\software-qa-testing
  ```

(Or download this folder and place it at your platform's skill path directory — the folder name becomes the command name.)

**Project — checked into a specific repo (macOS / Linux / Windows):**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git /path/to/your-project/.claude/skills/software-qa-testing
```

Commit the `.claude/skills/software-qa-testing/` folder to your project's version control so the whole team gets it automatically when they pull.

Either way, restart Claude Code if `.claude/skills/` didn't already exist in that location. Claude Code watches existing skill folders live, but a brand-new top-level `skills/` directory needs a restart to be picked up.

#### Option B — Install as a plugin via a marketplace

This repo is already set up as a self-contained plugin (see `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`), so you can also install it the way you'd install any Claude Code plugin:

```text
/plugin marketplace add stevenhsu-tools/software-qa-testing
/plugin install software-qa-testing@software-qa-testing-marketplace
```

Then run `/reload-plugins` to make it available in your current session.

Note: a plugin-installed skill is namespaced by the plugin name, so the command becomes `/software-qa-testing:software-qa-testing` rather than the plain `/software-qa-testing` you get with Option A. Claude will still trigger it automatically either way based on its description — the namespacing only affects manual `/` invocation.

### Codex

Codex supports this repo either as a direct local skill or as a plugin. Use the direct skill install while editing the workflow, and use the plugin install when you want Codex's plugin browser and marketplace flow.

#### Option A — Copy as a Codex skill

**Personal — available in all your Codex projects:**

- **macOS / Linux:**
  ```bash
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git ~/.agents/skills/software-qa-testing
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git $HOME/.agents/skills/software-qa-testing
  ```
- **Windows (CMD):**
  ```cmd
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git %USERPROFILE%\.agents\skills\software-qa-testing
  ```

**Project — checked into a specific repo (macOS / Linux / Windows):**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git /path/to/your-project/.agents/skills/software-qa-testing
```

Restart Codex if the new skill does not appear. Codex can trigger it automatically from the skill description, or you can invoke it explicitly with `$software-qa-testing`.

#### Option B — Install as a Codex plugin

This repo includes `.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json`, so it can be added as a Codex plugin marketplace source:

```bash
codex plugin marketplace add stevenhsu-tools/software-qa-testing
codex plugin add software-qa-testing@software-qa-testing
```

You can also open the plugin browser from Codex CLI with `/plugins`, choose the `Software QA Testing` marketplace, and install `software-qa-testing`.

After installing a plugin, start a new Codex thread so the newly installed skill is available in context.

### Google Antigravity

Google Antigravity automatically discovers and loads customizations (skills and plugins) from its customization roots. This repository is configured to be loaded either directly as a customization skill or as a plugin.

#### Option A — Install as a Customization Skill (Direct Skill)

**Personal — available in all your Antigravity projects:**

- **macOS / Linux:**
  ```bash
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git ~/.gemini/config/skills/software-qa-testing
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git $HOME/.gemini/config/skills/software-qa-testing
  ```
- **Windows (CMD):**
  ```cmd
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git %USERPROFILE%\.gemini\config\skills\software-qa-testing
  ```

**Project — checked into a specific repo (macOS / Linux / Windows):**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git /path/to/your-project/.agents/skills/software-qa-testing
```

#### Option B — Install as a Customization Plugin (Direct Plugin)

Because this repository contains `plugin.json` at the root, it can be loaded directly as an Antigravity plugin.

**Personal — available in all your Antigravity projects:**

- **macOS / Linux:**
  ```bash
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git ~/.gemini/config/plugins/software-qa-testing
  ```
- **Windows (PowerShell):**
  ```powershell
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git $HOME/.gemini/config/plugins/software-qa-testing
  ```
- **Windows (CMD):**
  ```cmd
  git clone https://github.com/stevenhsu-tools/software-qa-testing.git %USERPROFILE%\.gemini\config\plugins\software-qa-testing
  ```

**Project — checked into a specific repo (macOS / Linux / Windows):**

```bash
git clone https://github.com/stevenhsu-tools/software-qa-testing.git /path/to/your-project/.agents/plugins/software-qa-testing
```

**Repository:** [github.com/stevenhsu-tools/software-qa-testing](https://github.com/stevenhsu-tools/software-qa-testing)

## Using it

Once installed, either:
- **Let your agent trigger it automatically** — in Claude Code, Codex, or Google Antigravity, ask something like "what testing should I run before release?" or "write up this bug: ..." and the skill will be automatically triggered and loaded based on its description.
- **Invoke it directly in Claude Code** — type `/software-qa-testing` (or `/software-qa-testing:software-qa-testing` if installed as a Claude plugin) followed by your request.
- **Invoke it directly in Codex** — mention `$software-qa-testing` in your prompt, or use the Codex skill/plugin picker if available in your surface.
- **Invoke it in Google Antigravity** — The skill is automatically loaded and triggered by the agent when relevant; no manual invocation prefix is required.

### Example prompts

- "We're about to test our new checkout flow — what should QA cover?"
- "Generate test cases for `src/checkout/payment.py`." *(Part 2 — scans the code, no spec needed; the agent first asks whether you want test cases only or the full pipeline)*
- "Design test cases for the password reset module, then run them and give me a bug report for anything that fails." *(full pipeline in one request — goal is unambiguous, so the agent states the path and proceeds through Parts 2 → 3 → 4)*
- "Here are the results from running those test cases: [...]. Give me a QA test report." *(Part 3 — pass/fail counts, a list of failures, and how many were actually executed vs. traced by reading the code)*
- "Turn the failed test cases in that report into bug reports." *(Part 4 — one bug report per failure, Severity/Priority pulled from the test report)*
- "Write up a bug report: the app freezes exporting more than 5000 rows to CSV, build 4.2.1, Windows 11, happens every time, we ship Tuesday." *(ad hoc bug report, no test report needed)*
- "What's the difference between severity and priority, and what fields should our bug tracker have?"

## Updating the skill

Edit `SKILL.md` (and the files under `references/` and `assets/`) directly, then:
- If installed via Option A in Claude Code, just edit the copy in `.claude/skills/software-qa-testing/` (or re-pull if it's a git clone) — Claude Code picks up `SKILL.md` text changes live.
- If installed via Option B in Claude Code (plugin), run `/plugin marketplace update software-qa-testing-marketplace` after pushing changes, then `/reload-plugins`.
- If installed as a direct Codex skill, edit or re-pull the copy in `~/.agents/skills/software-qa-testing/` or your repo's `.agents/skills/software-qa-testing/`. Codex usually detects skill changes automatically; restart Codex if the update does not appear.
- If installed as a Codex plugin, update the plugin source, then reinstall it with `codex plugin add software-qa-testing@software-qa-testing`. Start a new Codex thread after reinstalling so the updated skill is loaded.
- If installed as a direct skill or plugin in Google Antigravity, edit or pull the copy in the corresponding directory (`~/.gemini/config/skills/software-qa-testing/`, `~/.gemini/config/plugins/software-qa-testing/`, or your project's `.agents/` folder). Google Antigravity automatically detects and loads customization changes.

## Source

The testing methodology and bug-report taxonomy (Severity/Priority tables, IEEE-829-based fields, Resolution status codes) are adapted from a professional software QA/testing methodology reference, translated and reorganized into English for use as a Claude/Codex/Antigravity skill.

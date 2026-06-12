# Refactor output template

Use this template for every Refactor-mode output. The goal is a **diagnostic report the user can act on** — not a rewritten agent. You diagnose; the user fixes.

---

## Section 1: Summary

Two to three sentences describing the agent as it exists today, in your own words. Then:

- One sentence on the agent's apparent target — platform-specific (Custom GPT / Claude Project / Gem) or platform-agnostic (corporate environment).
- One sentence on overall posture — **well-structured** / **mid-quality** / **needs significant rework**.

Keep this short. The user already knows their agent; this is to confirm you read it correctly.

---

## Section 2: Issues found

Group findings by severity, in this order: **Critical** / **Important** / **Cosmetic**. Under each severity, list issues as bullets.

For every issue:

- **Short title** (one phrase).
- **File(s) involved** (full paths).
- **1-3 sentences** explaining the problem and the real-world consequence — *what breaks because of this*.

### Severity definitions

- **Critical** — the agent will fail at its mission, produce false confidence on high-stakes outputs, leak data, or contains internal contradictions that degrade output quality across the board. Always classify internal contradictions (check #11) as Critical.
- **Important** — the agent works but has reliability gaps, maintainability problems, missing scope boundaries, or weak uncertainty handling.
- **Cosmetic** — naming, formatting, consistency. Won't affect behavior but reduces maintainability or polish.

Don't pad. If there are no Critical issues, say so explicitly and move on.

---

## Section 3: Recommended fixes per file

One sub-block per file that needs changes. Order files by aggregated severity (files with Critical issues first).

For each file:

- **File:** `path/to/file.md`
- **Current state:** 1-2 sentences on how this file looks today.
- **Recommended change:** specific, actionable. Describe the change — **do not write the full replacement text**.
- **Why:** which issue(s) from Section 2 this addresses (reference by short title).

Examples of the right granularity for "Recommended change":

- "Split into three files: `agent-design.md`, `reliability.md`, `examples.md`. Move the scoring rubric (currently lines 80-120) into a new `scoring.md`."
- "Add a top-level 'Out of scope' section listing at least 3 explicit non-goals."
- "Add a one-line escape hatch after the uncertainty rule: 'If you cannot fully answer, name the best-effort assumption and proceed, flagged as a working hypothesis.'"

If a file is fine, don't list it.

---

## Section 4: Suggested next steps

An ordered list of 3-7 concrete actions, starting with the highest-impact fix. Each action: one sentence, action verb first.

Examples:

1. Split the monolithic `instructions.md` into a compact core + 4 modular KB files.
2. Add a reliability block distinguishing fact, inference, hypothesis, and absence of basis.
3. Add 2 explicit examples to the output format section, matching the declared verbosity.
4. Resolve the contradiction between the "always cite" rule and the no-citation examples.

End with one of:

- *"Once these are applied, re-run gepeto-lite in Refactor mode to verify."* — standard close.
- *"If ambiguity remains after applying these fixes, try a two-step metaprompting cycle: first ask the target model to quote any conflicting instructions it sees, then request a surgical revision."* — add this when the existing agent has many vague or ambiguous areas the diagnostic couldn't fully resolve.

---

## What NOT to include

- **No full rewrites** of the agent's files. You produce a diagnostic; the user owns the changes.
- **No code.** This meta-agent operates on instructions and knowledge bases, not source code.
- **No commentary on the agent's personality** unless it materially affects function (e.g., the personality contradicts the stated stance, breaking check #11).
- **No "I noticed you also might want…"** scope creep. Stay within the 11 checks.

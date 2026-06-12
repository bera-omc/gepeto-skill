# Refactor checklist and output template

---

## Part 1 — Refactor checklist

Use this in **Refactor mode**. Run each of the 11 checks below against the agent the user shared (instructions file, knowledge base files, and any examples). Each check produces one finding. Group findings by severity using the output template in Part 2.

Work in two passes:
1. **Read everything first.** Don't fix while reading.
2. **Run each check.** Record findings as you go.

Don't write replacement files. The output is a diagnostic — the user applies the fixes.

### The 11 checks

#### 1. Instruction format and frontmatter

- **Check:** Is the instructions file in `.md` (or equivalent plain text) with optional YAML frontmatter? Is it readable and split into clear sections?
- **Pass:** `.md` or `.txt`, with headers, paragraphs, and a recognizable structure.
- **Fail signals:** monolithic text blob with no structure; mixed or proprietary formats; instructions hidden inside binary files.

#### 2. Scope and out-of-scope explicitness

- **Check:** Does the agent state both what it does AND what it explicitly does not do?
- **Pass:** Both are present, with concrete examples or boundaries.
- **Fail signals:** only positive scope ("helps users with X") with no boundary; "be helpful" without limits; ambiguous coverage that lets the agent drift.

#### 3. Modular knowledge base by function

- **Check:** Is the KB split by function (design / reliability / patterns / templates) or dumped together?
- **Pass:** Files named by purpose, grouped logically, no "misc" folder or sprint-named folders.
- **Fail signals:** flat folder of unrelated files; duplicate content across files; date-based folders.

#### 4. Absence of mega-files

- **Check:** What is the largest KB file?
- **Pass:** No single file > ~10 KB (subjective; depends on platform).
- **Fail signals:** one giant `everything.md`; a long PDF as the whole KB; a single file carrying 80%+ of the substance.

#### 5. Reliability rules present

- **Check:** Do the instructions or KB mention fact / inference / hypothesis / absence distinctions, or an equivalent uncertainty framework?
- **Pass:** Explicit rule about distinguishing evidence levels and handling missing data.
- **Fail signals:** agent assumed to be always-correct; no "when uncertain, …" guidance; confident-sounding tone with no truth scaffolding.

#### 6. Examples, tests, and success criteria

- **Check:** Are there (a) concrete reference outputs or few-shot examples, (b) test prompts, AND (c) measurable success criteria (what does a *good* answer look like)?
- **Pass:** At least 2-3 examples or test cases AND a stated criterion for "this answer is good because …".
- **Fail signals:** zero examples; only abstract descriptions; "be helpful" with no quality bar.

#### 7. Limits and edge cases handled

- **Check:** Does the agent state what to do at the boundary — long input, ambiguous question, hostile or off-scope prompt?
- **Pass:** Explicit edge-case handling with at least 2-3 named cases.
- **Fail signals:** no boundary policy; assumes happy path only; no off-scope deflection rule.

#### 8. Tone, stance, and output format

- **Check:** Are three things stated: (a) tone of voice, (b) stance (cautious / assertive / advisory / generative), and (c) the exact output format the agent should produce (Markdown yes/no, verbosity, standard response shape)?
- **Pass:** All three are explicit and consistent. Sample outputs (if present) match the declared format.
- **Fail signals:** no tone definition; no output format ("respond in Markdown" is *not* implicit on newer models); inconsistent voice across files; long-winded by default with no verbosity guidance.

#### 9. Policy on admitting uncertainty (with escape hatches)

- **Check:** Is "I don't know" a sanctioned response AND does the agent have an **escape hatch** — a defined behavior for when it can't fully answer but still needs to produce *something* useful?
- **Pass:** Explicit "when X, say Y" mechanism for unknowns, plus a stated escape path like "if you can't fully answer, name the best-effort assumption and proceed with it, flagging it as a working hypothesis."
- **Fail signals:** forced confidence ("always provide an answer"); admitting uncertainty without any path forward (the agent gets stuck); no distinction between "I don't know" and "I can give you a partial answer".

#### 10. Maintainability by another person

- **Check:** Could a different person take over this agent in 6 months without context?
- **Pass:** File purposes documented, key decisions explained, rationale recorded somewhere.
- **Fail signals:** tribal knowledge; cryptic file names; rationale missing; "obvious to the author" assumptions.

#### 11. Internal coherence — no contradictions across files

- **Check:** Do the instructions, KB files, and examples agree with each other? Are there conflicting directives across sections or files?
- **Pass:** No contradictions found across the agent's full surface area.
- **Fail signals:** instructions say "always cite a source", but the example output has no citations; one KB file says X, another says not-X; output format declared in instructions doesn't match the format shown in examples; "use Markdown" in one place, "plain text" in another.

Treat this as a **Critical-severity** issue when found — contradictory or vague instructions are disproportionately damaging on newer models. The agent wastes reasoning on reconciliation, and the output quality drops more than the size of the contradiction would suggest.

### How to use the checklist

- Read all of the agent's files first (instructions + every KB file + examples).
- Run each of the 11 checks in order. Record one finding per check, including "pass" findings (they're useful in the Summary).
- Use Part 2 below to format the findings.
- **Don't fix while checking.** Diagnose now, recommend later. Separating the two steps avoids tunnel vision and produces a more useful report.

---

## Part 2 — Refactor output template

Use this template for every Refactor-mode output. The goal is a **diagnostic report the user can act on** — not a rewritten agent. You diagnose; the user fixes.

### Section 1: Summary

Two to three sentences describing the agent as it exists today, in your own words. Then:

- One sentence on the agent's apparent target — platform-specific (Custom GPT / Claude Project / Gem) or platform-agnostic (corporate environment).
- One sentence on overall posture — **well-structured** / **mid-quality** / **needs significant rework**.

Keep this short. The user already knows their agent; this is to confirm you read it correctly.

### Section 2: Issues found

Group findings by severity, in this order: **Critical** / **Important** / **Cosmetic**. Under each severity, list issues as bullets.

For every issue:

- **Short title** (one phrase).
- **File(s) involved** (full paths).
- **1-3 sentences** explaining the problem and the real-world consequence — *what breaks because of this*.

#### Severity definitions

- **Critical** — the agent will fail at its mission, produce false confidence on high-stakes outputs, leak data, or contains internal contradictions that degrade output quality across the board. Always classify internal contradictions (check #11) as Critical.
- **Important** — the agent works but has reliability gaps, maintainability problems, missing scope boundaries, or weak uncertainty handling.
- **Cosmetic** — naming, formatting, consistency. Won't affect behavior but reduces maintainability or polish.

Don't pad. If there are no Critical issues, say so explicitly and move on.

### Section 3: Recommended fixes per file

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

### Section 4: Suggested next steps

An ordered list of 3-7 concrete actions, starting with the highest-impact fix. Each action: one sentence, action verb first.

Examples:

1. Split the monolithic `instructions.md` into a compact core + 4 modular KB files.
2. Add a reliability block distinguishing fact, inference, hypothesis, and absence of basis.
3. Add 2 explicit examples to the output format section, matching the declared verbosity.
4. Resolve the contradiction between the "always cite" rule and the no-citation examples.

End with one of:

- *"Once these are applied, re-run gepeto-lite in Refactor mode to verify."* — standard close.
- *"If ambiguity remains after applying these fixes, try a two-step metaprompting cycle: first ask the target model to quote any conflicting instructions it sees, then request a surgical revision."* — add this when the existing agent has many vague or ambiguous areas the diagnostic couldn't fully resolve.

### What NOT to include

- **No full rewrites** of the agent's files. You produce a diagnostic; the user owns the changes.
- **No code.** This meta-agent operates on instructions and knowledge bases, not source code.
- **No commentary on the agent's personality** unless it materially affects function (e.g., the personality contradicts the stated stance, breaking check #11).
- **No "I noticed you also might want…"** scope creep. Stay within the 11 checks.

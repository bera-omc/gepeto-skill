# Create mode output template and pre-publish checklist

---

## Part 1 — Create mode output template

Use these exact section headers when producing the output of Create mode. The 8 sections are the minimum — add sub-sections only where useful.

### 1. Reading of the request

Restate what the user asked for in your own words. Surface any ambiguity or hidden assumption you spotted. One short paragraph.

### 2. Agent diagnosis

State what kind of agent this should be: which pattern(s) from `agent-patterns.md` apply, what the target environment is (platform-agnostic by default; specific platform only if the user named one), and what risk profile this agent carries.

### 3. Required knowledge

Break down what the agent needs into four classes.

#### 3.1 What goes in instructions
The compact, durable rules that always apply. Identity, scope, mother rules, output contract.

#### 3.2 What goes in knowledge files
Frameworks, policies, glossaries, base documents, structured examples. List the files with a one-line purpose each.

#### 3.3 What goes in links / sources
External references the agent may cite. List with rationale.

#### 3.4 What goes in examples
Few-shot prompts, reference outputs, test cases. Specify count and shape.

### 4. Recommended configuration

Tone, stance, output format (Markdown yes/no, verbosity, response shape), ask-vs-assume policy, escape-hatch behavior. One paragraph or a short bullet list.

### 5. Final instructions

Deliver the actual system prompt the user pastes into the target platform.

#### 5.1 Lean version
Minimum viable instructions. The smallest text that still satisfies the 11 refactor-checklist items.

#### 5.2 Robust version
Same agent, with safeguards, edge-case handling, and richer reliability scaffolding. State explicitly what was added vs. the lean version.

**Do not embed testing prompts inside the Instructions text.** Validation tests belong in section 7 of this output, not inside the agent's own system prompt. Including them in Instructions teaches the agent to game its own checks.

### 6. Reasoning behind the decisions

Explain the key choices: why this pattern, why this scope, why this tone, why this knowledge split. Surface the trade-offs you made.

### 7. Validation tests

List 3-6 concrete prompts the user can run against the agent on launch. For each test: the input, the expected behavior, and what would count as a failure.

### 8. Future improvements

Bulleted list of changes the user could consider once the agent is in use. Each bullet: action + expected payoff.

---

## Part 2 — Pre-publish checklist

Run this checklist at the end of Create mode, before declaring the output ready. Every question is a yes/no. If any answer is "no", surface it in section 8 (Future improvements) of the output, with a recommendation.

1. Does the agent's role fit in one sentence?
2. Is the out-of-scope explicit?
3. Are reliability rules present (fact / inference / hypothesis / absence handling, plus anti-capitulation under pushback)?
4. Is there guidance for handling missing context?
5. Is the knowledge base organized by function (not by date, sprint, or "misc")?
6. Does the index point to the right files for each task path?
7. Are there representative tests covering happy path, edge case, and off-scope?
8. Is there a lean version of the instructions, separate from the robust one?
9. Does the agent have a clear maintenance path — could a different person take it over in 6 months?
10. Does this really need to be a custom agent, or could it be a reusable prompt (the user can paste once) or a documented playbook?
11. Is the KB clean of off-limits material (no HR records, performance reviews, compensation, health info, personal employee data, secrets, unredacted client confidentials)?

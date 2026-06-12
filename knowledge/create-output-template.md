# Create mode output template

Use these exact section headers when producing the output of Create mode. The 8 sections are the minimum — add sub-sections only where useful.

---

## 1. Reading of the request

Restate what the user asked for in your own words. Surface any ambiguity or hidden assumption you spotted. One short paragraph.

## 2. Agent diagnosis

State what kind of agent this should be: which pattern(s) from `agent-patterns.md` apply, what the target environment is (platform-agnostic by default; specific platform only if the user named one), and what risk profile this agent carries.

## 3. Required knowledge

Break down what the agent needs into four classes.

### 3.1 What goes in instructions
The compact, durable rules that always apply. Identity, scope, mother rules, output contract.

### 3.2 What goes in knowledge files
Frameworks, policies, glossaries, base documents, structured examples. List the files with a one-line purpose each.

### 3.3 What goes in links / sources
External references the agent may cite. List with rationale.

### 3.4 What goes in examples
Few-shot prompts, reference outputs, test cases. Specify count and shape.

## 4. Recommended configuration

Tone, stance, output format (Markdown yes/no, verbosity, response shape), ask-vs-assume policy, escape-hatch behavior. One paragraph or a short bullet list.

## 5. Final instructions

Deliver the actual system prompt the user pastes into the target platform.

### 5.1 Lean version
Minimum viable instructions. The smallest text that still satisfies the 11 refactor-checklist items.

### 5.2 Robust version
Same agent, with safeguards, edge-case handling, and richer reliability scaffolding. State explicitly what was added vs. the lean version.

**Do not embed testing prompts inside the Instructions text.** Validation tests belong in section 7 of this output, not inside the agent's own system prompt. Including them in Instructions teaches the agent to game its own checks.

## 6. Reasoning behind the decisions

Explain the key choices: why this pattern, why this scope, why this tone, why this knowledge split. Surface the trade-offs you made.

## 7. Validation tests

List 3-6 concrete prompts the user can run against the agent on launch. For each test: the input, the expected behavior, and what would count as a failure.

## 8. Future improvements

Bulleted list of changes the user could consider once the agent is in use. Each bullet: action + expected payoff.

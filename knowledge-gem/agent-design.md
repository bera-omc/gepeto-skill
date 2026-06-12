# Agent design framework

Use this in **Create mode**. The goal is to produce agents that are robust, useful, auditable, and easy to evolve.

The default output is **platform-agnostic** — an agent that runs in any environment with a system prompt and a knowledge base. Only narrow to a specific platform (Custom GPT, Claude Project, Gemini Gem) when the user explicitly names one in Step 2.

---

## Step 1 — Problem-oriented exploration

Before suggesting any shape, find out:

- What the user wants to solve.
- Why it matters to them (real pain, not a vanity build).
- Who actually uses the agent (and in what context).
- What inputs the agent will receive.
- What outputs are expected.
- What "failure" would look like (so you know what to guard against).
- Which topics are sensitive (compliance, personal data, advice-shaped outputs).
- How much autonomy is acceptable (suggestion vs. action).

If any of these is missing, ask before continuing.

### Interview cadence

- **Ask one question at a time.** Each answer often changes the next question; batching wastes turns and confuses the user.
- **When offering choices, format them vertically as A/B/C/D options**, each on its own line. The user should be able to reply with a letter, a phrase, or their own answer. Do not present selectable options as inline bullets or in a single paragraph.
- **Summarize before drafting.** Once the exploration converges, restate your understanding and ask whether anything is off — only then start producing the agent spec.
- **If the user asks to skip ahead and draft, follow their direction.** Tell them they can return to refine later.

---

## Step 2 — Format decision

Decide the artifact's shape before drafting anything:

- **One agent** with a single specialty.
- **A small set of specialist agents** (when the request mixes risk profiles).
- **A reusable prompt template** (when there's no persistent context need).

Decision criteria: persistence of context, knowledge required, frequency of use, variety of tasks, risk of scope inflation.

Then decide the **target environment**:

- **Platform-agnostic / corporate environment** (default) — design assuming a generic LLM platform that accepts a system prompt and knowledge files. Limits are usually flexible.
- **Specific platform** (OpenAI Custom GPT, Anthropic Claude Project, Google Gemini Gem) — only when the user explicitly names one. In that case, consult `platform-quirks.md` for file-count, file-size, and instruction-size constraints.

When the target is unclear: ask. Don't assume.

---

## Step 3 — Agent architecture

Define, in this order:

- **Identity** — what the agent *is*, in one sentence.
- **Mission** — what success looks like, concretely.
- **Scope** — what's in.
- **Out of scope** — what's explicitly out (this is half the work).
- **Reasoning style** — analytical, advisory, generative, diagnostic.
- **Tone and stance** — terse vs. expansive, formal vs. casual, cautious vs. assertive.
- **Ask-vs-assume policy** — when to interrupt the user vs. proceed with a default.
- **Uncertainty handling** — how to surface "I don't know" (see `reliability-and-default-rules.md`).
- **Knowledge usage rules** — when to consult the KB, when to rely on instructions.

### Modes — when to add them, when to skip

Don't force modes into every agent. Add them only when the agent has genuinely different types of tasks that need different operating rules — e.g. strategy / review / writing / ideation / evaluation. Signs you need modes:

- Two task types require different output formats.
- Two task types require different reliability postures (e.g., generative vs. epistemic).
- The user will explicitly say "do X mode" rather than letting the agent infer.

If a single set of rules covers everything, ship a single-mode agent. Modes added without need become dead UI — the user never invokes them, but you still pay for the extra prompt complexity and the extra surface for drift.

---

## Step 4 — Knowledge system

Split what the agent needs into four classes:

1. **Instructions** — role, flow, hard rules, output format, quality criteria. Keep short.
2. **Knowledge files** — policies, frameworks, glossaries, base documents, structured examples.
3. **Links and external sources** — prioritized references the agent may cite.
4. **Examples** — few-shot prompts, test cases, model outputs.

Detail on how to structure the KB itself is in `kb-architecture-and-template.md`.

### Reviewing user-supplied source material

When the user uploads source material during the interview (briefs, brand guidelines, strategy docs, past outputs, FAQs, approved claims):

- **Read deeply enough to understand what's in it.** Don't skim and guess.
- **Confirm understanding in a short note**, not a long summary. Users uploaded the doc to use it, not to read it back.
- **Ask follow-up questions on gaps, conflicts, or unclear priorities.** Uploads rarely settle every question — they often raise new ones.
- **When uploads conflict, propose a source hierarchy** (e.g., "approved-claims overrides marketing-copy when they disagree") and ask the user to confirm or correct it.
- **Refuse off-limits material.** See `kb-architecture-and-template.md` for the privacy/compliance baseline (HR / health / compensation / personal-employee data should not be uploaded as KB).

---

## Step 5 — Quality and truth system

Every agent must contain, in some form:

- A rule against inventing data, sources, or procedures.
- A clear distinction between fact, inference, hypothesis, and opinion.
- A mechanism to ask for clarification when the base is insufficient.
- A mechanism to make uncertainty visible in the output.
- A self-coherence check before closing the response.
- A rule against retracting a supported answer under pushback alone (see `reliability-and-default-rules.md` base rule 6).

Full rules and language patterns are in `reliability-and-default-rules.md`. The portable 6-rule baseline (clarifying questions, challenging weak requests, no generic disclaimers, instruction privacy, anti-capitulation, source distinction) lives in `reliability-and-default-rules.md` — install it in every agent unless there's a reason to override.

---

## Step 6 — Mandatory rule enforcement

When a rule is critical (e.g. "always use the Sherman Kent probability scale", "always cite the policy source", "never produce code without test cases"), structural enforcement beats polite phrasing. Use one or more of:

- **Severe labeling.** Wrap the rule in explicit, emphatic markers like `<mandatory_rules>...</mandatory_rules>` or **`CRITICAL RULE`** headers.
- **Explicit consequence.** State in the prompt that ignoring the rule is a critical violation of the agent's contract.
- **Structural enforcement in the output format.** Make the required behavior part of the output shape itself — for example, require a fixed prefix like `[Probability — Sherman Kent scale]` before each prediction. This makes the rule impossible to bypass without breaking the format.

Use this sparingly. If every rule is "critical", none are.

---

## Step 7 — Delivery

Hand off:

- A **lean version** — minimum viable instructions for the agent.
- A **robust version** — with safeguards, edge-case handling, and richer reliability scaffolding.
- The **KB pack structure** — file list + 1-line purpose per file.
- **Tests** — concrete prompts the user can run to validate the agent on launch.
- **Risks and trade-offs** — what this design intentionally doesn't do, and why.

Use the format in `create-output-and-checklist.md`.

**Voice for the produced Instructions text:** write in direct "you are / you must" language. The agent is being told what it is and what to do. Strong section headers and bullets over paragraphs — they retrieve and skim better. Do not embed testing prompts in the Instructions themselves; tests live in the validation section of the deliverable, not inside the agent.

---

## Anti-patterns

Avoid:

- Agents that try to do everything (the platypus).
- Vague instructions ("be helpful", "be smart") with no operational rule.
- Heavy personality, light function.
- Messy knowledge — duplicates, contradictions, "misc" folders.
- No quality criteria.
- No test cases.
- "Mandatory" rules everywhere — dilutes enforcement.
- Coupling to a specific platform when the user didn't ask for one.

# Agent patterns library

Each pattern names a recurring agent shape. Use **1-2 core patterns** per agent. Combining 3+ usually produces a corporate platypus.

When recommending a pattern in Create mode, name it explicitly and explain why it fits. When refactoring, identify which pattern(s) the existing agent is closest to and whether the implementation matches the pattern's intent.

---

## The 23 patterns

1. **Single-purpose specialist** — does one task very well.
2. **Critic / auditor** — assesses quality, risk, and compliance.
3. **Explainer tutor** — teaches and adapts depth to the user.
4. **Creative co-pilot** — generates ideas, variations, and refinements.
5. **Decision analyst** — compares options and trade-offs.
6. **Researcher with sources** — searches, synthesizes, and cites evidence.
7. **Knowledge curator** — organizes frameworks, glossaries, and materials.
8. **Context translator** — converts language between fields, technical levels, or audiences.
9. **Executive synthesizer** — condenses material into decisions and next steps.
10. **Operational prompt engineer** — writes prompts and workflows for practical use.
11. **Agent architect** — designs role, scope, knowledge, and behavior of other agents.
12. **Process debugger** — finds flow failures, ambiguity, and bottlenecks.
13. **Project planner** — structures goals, milestones, risks, and priorities.
14. **Documenter / SOP writer** — turns chaos into procedure.
15. **Requirements analyst** — extracts needs and success criteria.
16. **Diagnostic interviewer** — runs precise early discovery.
17. **Personalization advisor** — adapts output by profile, tone, or sector.
18. **Compliance guardian** — checks adherence to rules, policies, and standards.
19. **Knowledge pack designer** — decides what goes in instructions, files, links, or examples.
20. **Scorecard evaluator** — applies rubrics and scoring to a target.
21. **Agent refactorer** — takes an existing agent and rebuilds it better.
22. **User simulator** — generates tests and tries to break the agent.
23. **Library maintainer** — keeps patterns, versions, and indexes up to date.

---

## How to combine patterns

Cap the combination at 2-3 core patterns. If you need more, the agent probably wants to be split.

Pattern combinations work in two ways:
- **Sequential** — one pattern's output is another pattern's input (e.g. Diagnostic interviewer → Agent architect).
- **Layered** — patterns coexist in the same agent's behavior (e.g. Creative co-pilot + Critic/auditor produces ideas and then critiques them).

---

## Recommended combinations

- **Agent builder** — Agent architect + Diagnostic interviewer + Knowledge pack designer + Scorecard evaluator.
- **Agent refactorer** — Critic/auditor + Agent refactorer + User simulator.
- **Strategic tutor** — Explainer tutor + Decision analyst + Executive synthesizer.
- **Creative co-pilot with rigor** — Creative co-pilot + Critic/auditor + Personalization advisor.
- **Compliance reviewer** — Compliance guardian + Critic/auditor + Documenter.
- **Research synthesizer** — Researcher with sources + Executive synthesizer + Context translator.

---

## Patterns intentionally NOT in this library

Two patterns from the broader literature are omitted because they require capabilities outside gepeto-lite's scope:

- **Flow orchestrator** — requires coordinating multiple sub-agents at runtime.
- **Self-improving meta-agent** — requires the agent to modify its own framework or instructions.

If the user needs either, point them to a platform with multi-agent orchestration support and acknowledge that gepeto-lite doesn't help design that part.

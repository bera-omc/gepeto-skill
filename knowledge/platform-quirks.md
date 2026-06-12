# Platform quirks (reference)

**Read this only when the user explicitly targets one of the named platforms below** (OpenAI Custom GPT, Anthropic Claude Project, Google Gemini Gem). The default gepeto-lite output is platform-agnostic — agents designed for corporate LLM environments, which usually have more flexible limits and a custom UI for managing the system prompt + knowledge files.

Limits below are best estimates at time of writing. They change frequently. **Always verify against the official docs of the target platform before publishing.** Links are at the end of each section.

Documentation for these specific products is also limited. The links below are the closest canonical sources; broader vendor docs (general API reference, SDK guides) are usually too generic to answer Custom-GPT / Project / Gem-specific questions directly.

---

## OpenAI Custom GPTs

**Setup surface:** GPT Builder (conversational) or the Configure tab (form). Instructions field + Knowledge uploads + capabilities (web browsing, image gen, code interpreter, custom Actions).

**Practical limits (verify before relying):**
- **Knowledge files:** around 20 files per GPT.
- **File size:** ~512 MB per file as a hard cap, but practical retrieval degrades well before that.
- **Instructions field:** around 8000 characters.
- **Supported formats:** `.md`, `.txt`, `.pdf`, `.docx`, common code files.

**Retrieval behavior:**
- Opaque — no fine-grained control over chunking, scoring, or selection.
- Optimize for human-discoverable structure: clear filenames, strong H1/H2 titles, stable keywords.

**Notes:**
- Most users come from the chat UI — design conversational defaults.
- Custom Actions extend the GPT with HTTP calls. Out of scope for gepeto-lite; if the user needs them, point at the GPT Actions Library cookbook examples.

**Official sources:**
- GPTs hub (overview, sharing, troubleshooting): https://help.openai.com/en/collections/8475420-gpts
- Creating and editing GPTs: https://help.openai.com/en/articles/8554397-creating-and-editing-gpts
- Key guidelines for writing instructions: https://help.openai.com/en/articles/9358033-key-guidelines-for-writing-instructions-for-custom-gpts
- OpenAI Academy — Using custom GPTs: https://openai.com/academy/custom-gpts/
- OpenAI Cookbook (general prompting + GPT Actions examples): https://github.com/openai/openai-cookbook
- ChatGPT release notes (closest thing to a Custom-GPT changelog): https://help.openai.com/en/articles/6825453-chatgpt-release-notes

---

## Anthropic Claude Projects

**Setup surface:** Claude.ai → Projects. Each Project has custom instructions + Project Knowledge (file uploads). On modern accounts, Skills can be enabled to extend Projects with reusable capabilities.

**Practical limits (verify before relying):**
- **Project Knowledge:** sized against the total context window, typically much larger than a Custom GPT's Knowledge cap.
- **Custom instructions:** generous (verify current limit on the docs below).
- **Supported formats:** `.md`, `.txt`, `.pdf`, `.docx`, images, common code files.

**Retrieval behavior:**
- Claude reads Project Knowledge into context per turn (no opaque vector search). Favor **fewer, well-curated files** over many fragments — the marginal cost of a clean structure is high payoff.
- Long Project Knowledge eats into the per-turn context budget.

**Notes:**
- Claude responds well to structured, XML-like instruction blocks.
- Skills is an open standard (`agentskills.io`); if the user uses Skills, recommend the Skills format for reusable parts of the agent.

**Official sources:**
- What are Projects: https://support.claude.com/en/articles/9517075-what-are-projects
- What are Skills: https://support.claude.com/en/articles/12512176-what-are-skills
- Using Skills in Claude: https://support.claude.com/en/articles/12512180-use-skills-in-claude
- Platform documentation home: https://platform.claude.com/docs/en/home
- Prompt engineering (applies to Projects too): https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview
- Claude cookbooks: https://github.com/anthropics/claude-cookbooks
- Skills spec and examples: https://github.com/anthropics/skills and https://agentskills.io
- Anthropic news: https://www.anthropic.com/news
- API release notes: https://docs.claude.com/en/release-notes/overview

---

## Google Gemini Gems

**Setup surface:** Gemini app → Gem Manager. Each Gem has instructions + attached files. Available on the consumer Gemini surface (also in Workspace tenants).

**Practical limits (verify before relying):**
- **Files per Gem:** limited (verify on the support pages below — the cap is lower than GPT Knowledge in practice).
- **Instructions size:** around 10,000 characters (verify).
- **Supported formats:** `.md`, `.txt`, `.pdf`, and a subset of code files.

**Retrieval behavior:**
- Similar to GPTs — opaque, structure-dependent. Filenames and section headers matter.

**Notes:**
- Gemini follows step-by-step procedural instructions well — write numbered procedures for multi-step agents.
- **No public-facing cookbook or sample repo for Gems.** The Gemini API has its own docs (`ai.google.dev`), but those target the API, not Gems-in-app. Best practices have to be inferred from the support pages below.

**Official sources:**
- Get started with Gems: https://support.google.com/gemini/answer/15236321
- Tips for creating custom Gems (closest to official best practices): https://support.google.com/gemini/answer/15235603
- Using Gems in Gemini apps: https://support.google.com/gemini/answer/15146780
- Gems product overview: https://gemini.google/overview/gems/
- Gem builder: https://gemini.google.com/gems/create
- Google blog (no separate Gems changelog — announcements live here): https://blog.google/products/gemini/

---

## Cross-platform notes

- **Prefer `.md` over `.pdf`** whenever possible — better retrieval everywhere.
- **Avoid scanned PDFs** — none of the three platforms run OCR by default.
- **Keep YAML frontmatter minimal but valid.** Parsing and display behavior varies; rich frontmatter is fragile.
- **Descriptive filenames help retrieval more than verbose content.** Rename before uploading.
- **Always test on the real target.** Practical retrieval differs from documented limits.

## Corporate / enterprise environments

The most common real-world target. Expect:

- More flexible file counts and sizes than the three platforms above.
- Custom UI for managing instructions and knowledge (vendor-specific).
- A looser cap (or no cap) on the system prompt.
- The same `.md` + descriptive-filename + clean-structure conventions still work.

When designing for a corporate environment, treat the platform-specific quirks above as **upper bounds, not requirements**. If the corporate platform is more permissive, take the room.

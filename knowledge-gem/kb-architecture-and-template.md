# Knowledge base architecture and template

---

## Part 1 — Knowledge base architecture

Use the knowledge base as an extension of the agent. The system prompt sets the role and rules; the knowledge base carries the substance.

### Core principle

**Short instructions. Dense knowledge. Strong index.**

If the system prompt is doing the work the KB should do, the agent is brittle. If the KB has no index, retrieval is luck.

### Layers

Every agent's knowledge system has up to five layers. Smaller agents may collapse some:

1. **Instructions** — identity, role, operating process, KB-usage rules, output format, quality criteria. Compact.
2. **Index** — maps the KB by task, not by category vanity.
3. **Base frameworks** — the durable logic the agent reasons with.
4. **Patterns library** — reusable building blocks for recurrent agent types.
5. **Quality and improvement tools** — checklists, templates, output formats that keep the system auditable.

### Partitioning strategy

Separate files by **function**, not by historical accident.

**Good groupings:**
- design (how to build / refactor)
- reliability (truth and uncertainty)
- patterns (reusable shapes)
- templates (fill-in scaffolds)
- references (external sources, cited)

**Bad groupings:**
- "misc"
- one monolithic file with no index
- duplicates that contradict each other
- folders named after dates or sprints

### What never to upload

Regardless of how relevant it seems to the agent's job, the following should not be attached as knowledge base files:

- **Personal employee information.**
- **Performance reviews.**
- **Compensation details.**
- **Health information.**
- **Sensitive HR records.**

These are privacy/compliance hazards. They also tend to retrieve in ways the agent can't control — even when the agent is told to ignore them, the content is in context and can leak into responses.

If the agent legitimately needs the *shape* of such data (e.g., it processes HR documents structurally), ask for a **redacted or synthetic substitute** instead of the real records. If the user insists on uploading raw HR/health data, refuse and explain why.

Other content to avoid by default:

- **Secrets** (API keys, credentials, tokens) — never embed in KB.
- **Unredacted client confidentials** — get explicit clearance before uploading.
- **Meta-documentation** that describes the agent rather than informing its work (refactor logs, design rationale, internal QA tests). These dilute retrieval and let the agent game its own tests.

### Human-discoverable retrieval

Most current agent platforms expose **opaque retrieval** — you cannot tune a vector store, scoring, or chunk selection. Optimize for what the platform's retrieval can actually find:

- **Descriptive filenames** — `agent-design.md` beats `framework-v3-final-FINAL.md`.
- **Explicit titles** — the first H1 of each file should match what the file is about.
- **Stable keywords in section headers** — agents retrieve on these.
- **Low redundancy** — same content in two places confuses retrieval.
- **An INDEX with task paths** — tells the agent which file to consult first.

### File budget

Every platform has a practical ceiling on file count and file size (see `platform-quirks.md` when the target is platform-specific; corporate environments are usually more flexible). Prefer:

- A few well-organized parent files over many fragmented ones.
- Compact pattern libraries over per-pattern files.
- Reusable templates over one-off documents.
- `.md` over `.pdf` whenever possible.

Avoid attaching long PDFs the agent will never read.

### Maintenance policy

When you change a framework, you also update:

- The index, if the file's role shifts.
- The templates that depend on it.
- The pre-publish checklist, if a new check is implied.

KB drift is the silent killer of agents.

### Chunking guidance

When a source is too large to attach as one file (a long policy doc, a PDF, a research paper), chunk it.

**When to chunk:**
- Any source larger than ~3 KB that you'd otherwise attach whole.
- Any PDF — they retrieve poorly as monoliths even when small.

**Default rules:**
- **Chunk size:** ~2000 characters per chunk.
- **Respect paragraph boundaries.** Split on `\n\n`, never mid-sentence.
- **No overlap by default.** Keep chunks self-contained instead of duplicating.
- **YAML frontmatter** on every chunk file:
  - `source` — the original filename or document title.
  - `chunk_index` — zero-padded integer (e.g. `003`).
  - `page_range` — `start-end` when extractable, else the single page.
- **Filename pattern:** `chunk-NNN.md`, grouped under `outputs/kb/<source-stem>/`.

**When to deviate from the defaults:**
- A concept spans several paragraphs and splitting harms comprehension — extend that chunk.
- A long table or list — keep it whole, even if it exceeds 2000 chars.
- Tight cross-references between sections — keep them in the same chunk.

After chunking, **curate**. Drop chunks that don't earn their keep. Rename anything that doesn't describe itself.

---

## Part 2 — Knowledge pack template

Use this template when proposing the structure of the agent's knowledge base in Create mode. Fill each section in for the specific agent. Drop sections that don't apply to small agents; expand sections that need more depth.

### Agent objective
One sentence: what this agent exists to do.

### File index
List every file in the KB with a one-line purpose:
- `file-name.md` — purpose.

### Core frameworks
The durable methods or processes the agent reasons with. Reference files in `file index` where applicable.

### Essential glossary
Domain-specific terms the agent must use consistently. Term — definition.

### Operating rules
Hard rules the agent must follow (do this, never do that). Numbered list.

### Input and output examples
Representative pairs. At least 2-3. Format: the input, the agent's output, a one-line note on why this output is "good".

### Test cases
Prompts to validate the agent. Include happy path, edge case, hostile input, and off-scope.

### Update policy
When and how this KB should be updated. Who reviews changes.

### Change log
Append-only record of meaningful changes. Format: date — change — rationale.

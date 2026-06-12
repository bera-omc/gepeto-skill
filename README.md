# gepeto-skill

Ajudo meus amigos a criarem agentes de madeira.

A meta-agent that helps you build and improve LLM agents — system prompt + modular knowledge base — following current best practices. Works as a Claude Project (no install required) or as a Claude Code skill (more powerful, reads and writes real files).

---

## Quick setup

### Claude Project ⭐ (recommended for most people)

No installation required — works directly on [claude.ai](https://claude.ai).

1. Download this repo as a ZIP (**Code → Download ZIP**).
2. Go to claude.ai → Projects → New Project.
3. Paste the contents of `instructions.md` into the Project's custom instructions.
4. Upload all files from the `knowledge/` folder into Project Knowledge.

Done. Start a conversation with something like:
- *"Quero criar um agente que faz X."*
- *"Tenho um agente existente — me ajuda a melhorar?"*

### Claude Code skill (most powerful)

Use this if you already use Claude Code. Gepeto gets access to your files — reads and writes directly, no copy-paste.

1. Copy the contents of this repo into `~/.claude/skills/gepeto/`:
   ```bash
   git clone https://github.com/beralzir/gepeto-skill ~/.claude/skills/gepeto
   ```
2. In any Claude Code session, type `/gepeto` followed by what you want to do.

Gepeto detects the mode from context — create, refactor, package, or validate.

### Custom GPT (ChatGPT)

1. Open [chatgpt.com](https://chatgpt.com) → Explore GPTs → Create → **Configure** tab.
2. Paste `instructions.md` into the **Instructions** field.
3. Upload all files from `knowledge/` into **Knowledge**.
4. Disable all Capabilities (web search, image gen, code interpreter).

### Gemini Gem

1. Open [Gem Manager](https://gemini.google.com/gems/create) → New Gem.
2. Paste `instructions-gem.md` into the instructions field.
3. Upload the 8 files from `knowledge-gem/` as attachments.

---

## What it does

Gepeto runs in four modes — detected automatically from context:

| Mode | How to trigger | What happens |
|---|---|---|
| **Create** | "cria um agente que…" | Interviews you, picks a pattern, writes the agent files |
| **Refactor** | "revisa / melhora / otimiza esse agente" | Reads the files, runs an 11-item checklist, delivers a diagnostic |
| **Package** | "empacota para GPT / Gem / Claude" | Generates platform-ready variants automatically |
| **Validate** | "está pronto? / valida" | Runs the checklist against real files with real metrics |

In Claude Code, **Create** and **Refactor** write outputs directly to disk. In Claude Project or GPT/Gem, outputs appear in chat for you to copy.

---

## Repo structure

| Path | Purpose |
|---|---|
| `SKILL.md` | Claude Code skill entrypoint |
| `instructions.md` | System prompt for Claude Project and Custom GPT |
| `instructions-gem.md` | Compressed system prompt for Gemini Gem (~4k chars) |
| `knowledge/` | Knowledge base — used by Claude Project, Custom GPT, and Claude Code skill |
| `knowledge-gem/` | Merged knowledge base (8 files) for Gemini Gem |

---

## License

MIT — see `LICENSE`.

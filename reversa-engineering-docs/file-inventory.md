---
source_repo: "https://github.com/ningxia202109/CL4R1T4S-prompt-lib.git"
source_commit: "8f6b48bf2ca91d29a19d35204d76a5dd11bd3f3d"
generated_at: "2026-07-25"
generator: "reverse-engineer-prompt-corpus/v1"
confidence_default: inferred
---

# File Inventory

<!-- Scope: focus-mode run covering only the files touched in the last 3 months
(2026-04-25..2026-07-25). Full-corpus rows will be added on a full-mode run. -->

| Path | Vendor | Product / surface | Apparent capture date | Format | Size | Distinctive features |
|---|---|---|---|---|---|---|
| `ANTHROPIC/CLAUDE-FABLE-5.md` | Anthropic | Claude Fable 5 — web/mobile/desktop chat | in-text "current date" Tue Jun 09 2026 <!-- confidence: confirmed --> | Markdown | 1598 lines | Full 15-tool JSON schema block; memory system present but explicitly reports **no user memory enabled** ("Claude has no memories of the user because the user has not enabled Claude's memory in Settings"); `window.storage` persistent-storage-for-artifacts API; MCP App suggestion flow (`search_mcp_registry` → `suggest_connectors`); embedded "Claude in Claude" (Claudeception) sub-module hardcoded to `claude-sonnet-4-20250514` / `web_search_20250305`, inconsistent with the file's own claimed Fable-5/Sonnet-5 era — same staleness pattern also found in `OPUS-5.md` (see agent-systems/anthropic.md) |
| `ANTHROPIC/OPUS-5.md` | Anthropic | Claude Opus 5 — web/mobile chat | in-text "Friday, July 24, 2026" <!-- confidence: confirmed --> | Markdown | 2049 lines | Largest single file in the vendor folder; full `<memory_filesystem>` module (~half the file) describing a cross-surface persistent-memory backend; redaction placeholders (`{MEMORY_LISTING_ENTRY_REDACTED}`, `{LOCATION_REDACTED}`) — a strong signal of a real session capture (see prior analysis in `analysis/`); 30-tool JSON schema block; same stale "Claude in Claude" sub-module issue as CLAUDE-FABLE-5.md; `claude-opus-5` model string does not match any publicly documented model ID |
| `OPENAI/Codex_Desktop/5.6-Sol_SystemPrompt.md` | OpenAI | Codex Desktop ("Sol") — agentic coding assistant | not explicitly dated in-text <!-- confidence: gap --> | Markdown | 4270 lines | By far the largest file in the corpus; a full system prompt *plus* a bundled library of inlined `SKILL.md`-style sections (image generation, docx/pdf/pptx/xlsx, Sites building & hosting, browser control, plugin-creator, skill-creator/installer, a "Hatch Pet" virtual-pet feature, and a large embedded security-scanning workflow); file-based memory system (`MEMORY_ROOT`, `MEMORY.md` registry, JSONL rollout summaries, mandatory `<oai-mem-citation>` block); multi-agent orchestration primitives referenced in prose (spawn/interrupt/wait on sub-agents — see openai.md) |
| `OPENAI/Codex_Desktop/5.6-Sol_Tools.json` | OpenAI | Codex Desktop ("Sol") — tool schema export | not dated <!-- confidence: gap --> | JSON | 8093 lines | Companion tool-schema file to the system prompt above; ~140+ named tools/sub-tools across groups: `codex_app__*` (thread/project management), `mcp__codex_apps__sites_*` (site hosting CRUD), `mcp__openaiDeveloperDocs__*` (docs search), `mcp__computer_use` (click/drag/type/scroll — desktop GUI control), `collaboration.*` (`spawn_agent`, `wait_agent`, `interrupt_agent`, `list_agents`, `send_message`, `followup_task` — confirms multi-agent orchestration), `mcp__codex_security__*` (a very large sub-block: dozens of tools for an automated security-scan/triage/remediation pipeline), `mcp__node_repl__*`, `mcp__event_stream__*` |

## Prior art cross-reference

`analysis/opus-5-overall-analysis-2026-07-25.md` and
`analysis/opus-5-memory-filesystem-analysis-2026-07-25.md` already cover
`ANTHROPIC/OPUS-5.md` in depth (full structural map, tool catalog, memory
filesystem deep-dive, authenticity assessment). `agent-systems/anthropic.md`
summarizes and cross-references those rather than re-deriving them.

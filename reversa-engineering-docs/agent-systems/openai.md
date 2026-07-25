---
source_repo: "https://github.com/ningxia202109/CL4R1T4S-prompt-lib.git"
source_commit: "8f6b48bf2ca91d29a19d35204d76a5dd11bd3f3d"
generated_at: "2026-07-25"
generator: "reverse-engineer-prompt-corpus/v1"
confidence_default: inferred
---

# Agent Systems — OPENAI (recent files)

<!-- Scope: only OPENAI/Codex_Desktop/5.6-Sol_SystemPrompt.md and
5.6-Sol_Tools.json — the pair added 2026-07-14. Other OPENAI/ files are out
of scope for this focus-mode run. -->

Both files together capture a single product: "Codex Desktop", internally
labeled "Sol" in the filenames (not confirmed as an in-product name — the
system prompt text itself never uses "Sol"; that label is filename-only
evidence <!-- confidence: gap -->). This is the largest and most structurally
complex file pair in the corpus (4270 + 8093 lines) — reading was sampled
(headers, key sections, tool-name enumeration) rather than exhaustive line-by-
line, given the size; treat unlisted sub-sections as gaps for a future deeper
pass, not as "confirmed absent."

## OPENAI/Codex_Desktop/5.6-Sol_SystemPrompt.md + 5.6-Sol_Tools.json

- **Role**: "Codex, an agent based on GPT-5" — a general-purpose agentic assistant for software/document/web work running in a desktop app ("Codex Desktop"), distinct from the CLI/IDE-extension/cloud surfaces it explicitly distinguishes itself from in its own "Surface Map" section. <!-- confidence: confirmed, opening line is a direct quote -->
- **Model**: Self-described only as "based on GPT-5"; no specific dated model string (e.g. no `gpt-5-...` snapshot ID) appears in the sampled sections. <!-- confidence: gap -->
- **System Prompt summary**: Personality/tone rules → dual-channel output (`commentary` vs `final`) → autonomy/persistence policy (explicit authorization boundaries: read-only/in-scope actions proceed, scope-expanding actions require asking first) → skills framework (discovery, trigger rules, read-before-act requirement) → sandbox/escalation permission model → a large bundled library of inlined skill instructions (image generation, `documents`, `presentations`, `spreadsheets`/`excel-live-control`, `sites-building`/`sites-hosting`, `browser:control-in-app-browser`, `plugin-creator`, `skill-creator`, `skill-installer`, `template-creator`, `hatch-pet`, `pdf`) → memory system → Apps/Connectors and Plugins → OpenAI self-knowledge routing (a "Codex manual" fetched live from `developers.openai.com`, with a documented source-priority chain: same-thread cache → local manual-fetch helper → Docs MCP → official web fallback → bounded uncertainty).
- **Tools Available** (from `5.6-Sol_Tools.json`, ~140+ entries; grouped, not exhaustive):
  - Thread/project management: `codex_app__list_projects`, `codex_app__list_threads`, `codex_app__read_thread`, `codex_app__fork_thread`, `codex_app__set_thread_*`, `codex_app__create_thread`, `codex_app__handoff_thread`, `codex_app__send_message_to_thread`, `codex_app__load_workspace_dependencies`, `codex_app__navigate_to_codex_page`
  - Execution: `exec_command`, `shell_command`, `exec`, `wait`, `write_stdin`, `apply_patch`, `view_image`
  - **Multi-agent orchestration** (`collaboration` group): `spawn_agent`, `wait_agent`, `interrupt_agent`, `list_agents`, `send_message`, `followup_task`, plus `spawn_agents_on_csv` and `wait_for_environment` — confirms this product can run and coordinate multiple sub-agent instances, not just itself. <!-- confidence: confirmed, tool names read directly from the schema file -->
  - **Desktop GUI control** (`mcp__computer_use`): `click`, `drag`, `scroll`, `press_key`, `select_text`, `set_value`, `type_text`, `get_app_state`, `list_apps`, `perform_secondary_action` — an OS-level computer-use tool group, separate from the in-app browser skill.
  - Site hosting (`mcp__codex_apps__sites_*`): create/deploy/list sites and versions, custom domains, environment variables — a full website-hosting CRUD surface.
  - Docs lookup (`mcp__openaiDeveloperDocs__*`): search/fetch OpenAI docs, list API endpoints, get OpenAPI specs.
  - **Security scanning** (`mcp__codex_security__*`): by far the largest single tool group (dozens of entries: `start_codex_security_scan`, `get_codex_security_scan_context`, `set_codex_security_finding_triage`, `request_codex_security_finding_remediation`, `export_codex_security_findings`, etc.) — an entire automated-scan/triage/remediation pipeline with its own state machine (setup → scan → findings → remediation → handoff/delivery/claim/release semantics suggesting async or multi-worker execution).
  - Misc: `mcp__node_repl__js*` (JS REPL), `mcp__event_stream__*`, `image_gen__imagegen`, `request_user_input`, `request_permissions`, `clock`/`curr_time`/`sleep`, `new_context`/`get_context_remaining`.
  - Full enumeration in `file-inventory.md`'s row for this file; a complete per-tool schema/description/side-effect table (the "Tool Catalog" level of detail) was not produced in this pass given the file's size — flagged as a gap for a focused follow-up if needed.
- **Memory & State**: File-based, not vector-based. <!-- confidence: confirmed --> `MEMORY_ROOT` layout: `memory_summary.md` (pre-injected), `MEMORY.md` (searchable registry, primary query target), `skills/<name>/` (learned skill folders), `rollout_summaries/*.jsonl` (append-only per-session logs with `session_meta`/`turn_context`/`event_msg`/`response_item` records). Documented decision boundary for when to consult memory (skip for self-contained queries; use by default when the query references workspace/repo history). Mandatory `<oai-mem-citation>` block appended to any reply that used memory, with structured `citation_entries` (file:line-range + note) and `rollout_ids`. Memory **updates** are explicitly gated: the agent may only append a note file under `extensions/ad_hoc/notes/` when the user directly asks, never edit `MEMORY.md`/rollout files itself.
- **Guardrails**: Sandbox permission model (read/write scoped to `cwd` + `writable_roots`; escalation requires an explicit `justification` and is capped by a "banned prefix_rules" list preventing overly broad approvals like bare `python3`); destructive-git-command restriction (`git reset --hard`, `git checkout --` require explicit user ask); an explicit authorization-scope rule separating "diagnose/explain" from "implement" requests. Content-safety guardrails (child safety, refusal categories, etc.) were not located in the sampled sections of this file — likely present elsewhere in the 4270 lines but not confirmed here; mark as `gap`, not `absent`.
- **Non-Determinism Notes**: `[SANDBOX_MODE]` / `[NETWORK_ACCESS_POLICY]` / `[MEMORY_CONTENT]` / `[RUNTIME_ID]` are runtime-templated placeholders visible directly in the captured text (unlike the Anthropic files' opaque `{...REDACTED}` style, these look like unfilled template slots rather than post-hoc redactions of real values — a different capture/sanitization method, worth noting as a methodological difference between the two vendors' files in this corpus). Skills list, available Apps/Connectors, and Plugins are all explicitly session-dependent.
- **Authenticity note**: The sheer specificity of the tool schemas (exact MCP tool names, a documented temp-cache directory fallback order, a specific manual-fetch script path) is hard to fabricate plausibly and reads as an internal-tooling export similar in kind to the Anthropic files' tool JSON. <!-- confidence: inferred --> No independent public source was checked to confirm "Codex Desktop" ships exactly this tool set; treat vendor/product identification as `confirmed` (self-stated) but exact current-accuracy of every tool as `gap`.

## Notable design pattern: multi-agent + security-scan subsystems as first-class tools

Unlike the two Anthropic consumer-chat captures (single assistant instance,
no sub-agent spawning), this file exposes tool-level primitives for spawning
and coordinating other agent instances (`spawn_agent`/`wait_agent`/
`interrupt_agent`) and for running what looks like an entire automated
security-scanning product feature as agent-callable tools. This is a
materially different agent-system shape from the ANTHROPIC entries and is
the most significant structural finding of this pass — worth a dedicated
deeper read if the user wants full tool-by-tool documentation later.

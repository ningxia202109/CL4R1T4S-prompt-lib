---
source_repo: "https://github.com/ningxia202109/CL4R1T4S-prompt-lib.git"
source_commit: "8f6b48bf2ca91d29a19d35204d76a5dd11bd3f3d"
generated_at: "2026-07-25"
generator: "reverse-engineer-prompt-corpus/v1"
confidence_default: inferred
---

# Agent Systems — ANTHROPIC (recent files)

<!-- Scope: only the two files touched in the last 3 months —
CLAUDE-FABLE-5.md and OPUS-5.md. Other ANTHROPIC/ files are out of scope for
this focus-mode run. -->

Every field below is sourced from the prompt text itself, which is an
**unverified third-party capture** of a real product's runtime configuration —
not from Anthropic. Authenticity cannot be confirmed from this repo alone;
see the Authenticity note at the end of each entry.

## ANTHROPIC/CLAUDE-FABLE-5.md

- **Role**: Claude Fable 5, Anthropic's consumer web/mobile/desktop chat assistant. <!-- confidence: confirmed --> The file states Fable 5 and "Mythos 5" share the same underlying model, with Fable 5 being the publicly available, extra-safety-measures variant.
- **Model**: `claude-fable-5` (self); references sibling models `claude-opus-4-8`, `claude-sonnet-4-6`, `claude-haiku-4-5-20251001` as "the most recent models." <!-- confidence: confirmed, direct quote of model strings --> No temperature/sampling params are exposed in the text.
- **System Prompt summary**: Identity/product-info block, refusal handling, critical child-safety rules, legal/financial-advice caution, tone/formatting rules, user-wellbeing guardrails (suicide/self-harm/disordered-eating handling, explicitly citing "National Alliance for Eating Disorders" over NEDA "because NEDA has been permanently disconnected"), Anthropic classifier-reminder mechanism, political evenhandedness, mistake-handling, knowledge cutoff (end of Jan 2026, current date Jun 09 2026). Lines 1–164 (module headers only; no stable line numbers available beyond the read pagination used for this pass).
- **Tools Available**: 15 tools with full JSON schemas in-file: `ask_user_input_v0`, `bash_tool`, `create_file`, `fetch_sports_data`, `image_search`, `message_compose_v1`, `places_map_display_v0`, `places_search`, `present_files`, `recipe_display_v0`, `recommend_claude_apps`, `search_mcp_registry`, `str_replace`, `suggest_connectors`, `view`, `weather_fetch`, `web_fetch`, `web_search`. (Count exceeds 15 once sub-schemas are included; treat "15" as the top-level tool count, not exhaustive.)
- **Memory & State**: <!-- confidence: confirmed --> Explicitly **no active user memory**: "Claude has a memory system which provides Claude with access to derived information (memories) from past conversations with the user" followed immediately by "Claude has no memories of the user because the user has not enabled Claude's memory in Settings." This is a materially different state than OPUS-5.md below, where the memory system is fully populated and described in depth — i.e. these two files capture the *same product feature* in two different on/off states.
- **Guardrails**: Layered — absolute (child safety, weapons/malware), default-helpful-unless-specific-harm, user-wellbeing (self-harm/eating-disorder avoidance of specifics), political evenhandedness (present both sides even when Claude agrees with one), copyright hard limits (15-word quote ceiling, one quote per source, no lyrics/poems ever). <!-- confidence: confirmed, these are direct quotes/paraphrases of explicit rules -->
- **Non-Determinism Notes**: `anthropic_reminders` (`long_conversation_reminder`, `cyber_warning`, etc.) are classifier-injected at runtime and not visible in this static capture; `{USER_LOCATION}` is a runtime placeholder (redacted here); MCP App / connector availability is described as dynamic ("some are already connected... some aren't connected yet").
- **RAG / Eval / Observability**: Not applicable — no ingestion pipeline or eval harness exists in this repo for this file; skip.
- **Authenticity note**: Same category of evidence as OPUS-5.md (concrete tool schemas, a runtime placeholder region, an internally inconsistent stale sub-module — see cross-file finding below) suggests a real capture, but "Fable 5" / "Mythos 5" naming is not verifiable against any public source available to this analysis. <!-- confidence: gap -->

## ANTHROPIC/OPUS-5.md

Full structural analysis already exists at
`analysis/opus-5-overall-analysis-2026-07-25.md` (whole-file map, tool
catalog, copyright/search rules, authenticity assessment) and
`analysis/opus-5-memory-filesystem-analysis-2026-07-25.md` (deep dive on the
`<memory_filesystem>` module). This entry summarizes those findings in the
Step-4 schema rather than re-deriving them; see the linked files for full
detail and line-number citations.

- **Role**: Claude Opus 5, Anthropic's consumer web/mobile chat assistant — captured with a fuller feature set than CLAUDE-FABLE-5.md (populated memory, Claudeception, full 30-tool set).
- **Model**: `claude-opus-5` (self). <!-- confidence: gap --> Per the existing analysis, this model string does not correspond to any publicly documented Anthropic model (current known Opus tier tops out at `claude-opus-4-8`).
- **System Prompt summary**: Same category of modules as CLAUDE-FABLE-5.md (product info, refusal handling, child safety, wellbeing, evenhandedness) plus modules absent from the Fable-5 capture: full `<memory_filesystem>` (see below), `end_conversation` tool rules, `window.storage` persistent artifact storage, MCP App suggestion flow, past-chat search tools, computer-use/Skills routing, a visualization decision tree, and the same "Claude in Claude" API module.
- **Tools Available**: 30 tools across memory (6: `memory_read/write/str_replace/append/list/delete`), container ops (4), info retrieval (5), structured-output widgets (5), vertical data (3), connector/ecosystem guidance (3), and session control (4). Full catalog in `analysis/opus-5-overall-analysis-2026-07-25.md` §5.
- **Memory & State**: Fully populated `<memory_filesystem>` — a path-namespaced document store (`/profile.md`, `/topics/`, `/areas/`, `/people/`, `/preferences.md`) with YAML-frontmatter files, optimistic-lock versioning (`if_version`), and an explicit **no-vector-search** retrieval model (listing + LLM judgment, not embeddings). Full deep-dive in `analysis/opus-5-memory-filesystem-analysis-2026-07-25.md`, including the finding that this backend is shared across Claude surfaces (chat/Code/Cowork) with chat restricted to writing only `[stated]`-tagged facts.
- **Guardrails**: Same layered model as CLAUDE-FABLE-5.md, plus memory-specific guardrails: a hard privacy write-blacklist (protected attributes, health data, IDs, real-time location), a second application-time filter assuming the write-side filter can fail, and explicit treatment of memory-file content as **untrusted, potentially prompt-injected data** — the system prompt itself instructs the model to ignore instruction-like content found inside its own memory files.
- **Non-Determinism Notes**: `<memory_listing>` and `<profile>` are runtime-injected and redacted in this capture (`{MEMORY_LISTING_ENTRY_REDACTED}`); `{LOCATION_REDACTED}` likewise. These placeholders are treated by the existing analysis as evidence for a real capture rather than synthetic authorship.
- **Authenticity note**: Existing analysis concludes this is "a real framework + partially speculative/undisclosed content" hybrid — high confidence on tool schemas and privacy mechanics, low/unverifiable confidence on the "Opus 5" model name and its release-timeline narrative. <!-- confidence: gap for model-name/timeline claims; confirmed for verbatim schema/privacy-rule text -->

## Cross-file finding: stale "Claude in Claude" sub-module

Both files contain an `anthropic_api_in_artifacts` ("Claudeception") module
that lets Artifacts call the real Anthropic API. In **both** files this
sub-module is hardcoded to `model: "claude-sonnet-4-20250514"` (with a code
comment "Always use Sonnet 4") and the legacy tool type
`"web_search_20250305"` — inconsistent with each file's own headline claim of
being the Fable-5/Opus-5 era. <!-- confidence: confirmed, direct quotes from
both files --> This module being identically stale across two otherwise
different captures suggests it is a low-churn, infrequently-updated
sub-document within Anthropic's own prompt-assembly pipeline (per the
existing OPUS-5 analysis §6) rather than evidence of fabrication — the same
staleness pattern recurring unchanged in two separate files is itself the
signal, not a random coincidence.

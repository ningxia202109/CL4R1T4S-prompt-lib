# Generation Log

## [2026-07-25] focus | 8f6b48b
- **Mode**: focus ("the files touched in the last 3 months")
- **Trigger**: user request
- **Artifacts regenerated**: file-inventory.md (created), agent-systems/anthropic.md (created), agent-systems/openai.md (created), security-notes.md (created)
- **Notes**: Scope was the 7 files changed 2026-04-25..2026-07-25: `ANTHROPIC/CLAUDE-FABLE-5.md`, `ANTHROPIC/OPUS-5.md`, `OPENAI/Codex_Desktop/5.6-Sol_SystemPrompt.md`, `OPENAI/Codex_Desktop/5.6-Sol_Tools.json` were reverse-engineered; the two `analysis/opus-5-*.md` files were treated as prior art and cross-referenced rather than re-analyzed. `corpus-overview.md` was intentionally skipped this run (focus mode; whole-corpus taxonomy doesn't change from analyzing 4 of ~65 files). The two OPENAI files (4270 + 8093 lines) were sampled rather than read exhaustively — flagged as a gap in `agent-systems/openai.md` and `security-notes.md` for a deeper follow-up pass if needed. A full-mode run covering the remaining ~61 files has not yet been done.

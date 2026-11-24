# Changelog (development)

Format: [Keep a Changelog](https://keepachangelog.com/). Newest first.

## [0.1.0] - 2026-11-24

### Added
- Repository layout for the `skills` CLI: one folder per skill under `skills/`, so `npx skills add soimadeone/flow-skills` discovers all seven.
- `README.md` (English) and `README.ko.md` (Korean) with install, flow, and output locations.

### Changed
- `flow-research` rewritten from scratch around a claim ledger: write a brief with checkable claims, grade sources for and against each (`sources.md`), record a verdict per claim (`claims.md`), and build `report.md` from that table. Works with or without sub-agents; no agent-specific tool names. Output goes to `research/<slug>/`.

### Notes
- `references/common.md` is duplicated in each workflow skill on purpose: `npx skills add --skill <name>` installs one folder, so a skill cannot reference files outside its own directory.

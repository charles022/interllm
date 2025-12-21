# Repository Guidelines

## Project Structure & Module Organization

- Root-level documentation; project code lives in `src/`. No compiled output is tracked.
- Key reference docs live at the repo root:
  - `agents-md.md` and `agents-sdk.md` are source guides drawn from Codex CLI docs; Context7 MCP pages mirror their guidance.
  - `goal.md` is the initial proposal and project goal; `broad_code_proposal.md` is the initial code proposal derived from it.
  - `input/DataTransferScenarioList.md` captures scenario inventory.
  - `context7codexlib.txt` stores the Context7 library ID used for frequent lookups.
- Runtime inputs live under `input/`; baseline assets live under `src/`.
- Run artifacts live under `outputs/`; operational logs live under `logs/`.
- Current run status and follow-ups are captured in `run-status-and-next-steps.md`.
- Add new docs at the root unless a new subdomain warrants a folder (e.g., `design/`, `experiments/`).

## Build, Test, and Development Commands

- No build or test scripts are configured in this repository.
- Use your editor’s Markdown preview for rendering changes.
- If you add scripts later, document them here with exact commands (e.g., `./scripts/validate.sh`).

## Coding Style & Naming Conventions

- Markdown-first repo: keep headings short, use sentence case, and prefer bullet lists for procedures.
- Wrap code and commands in fenced blocks with a language tag when possible (e.g., `bash`).
- File naming: use lowercase with hyphens (e.g., `agent-workflow-notes.md`).
- Keep files concise; prefer adding a new focused doc over appending long, unrelated sections.

## Testing Guidelines

- No automated tests or coverage requirements exist today.
- If tests are introduced, place them in a clearly named directory (e.g., `tests/`) and document how to run them.

## Commit & Pull Request Guidelines

- Current history shows simple, lowercase, sentence-style commit messages (e.g., “init commit”).
- Keep commit subjects concise and descriptive; avoid prefixes unless the repo adopts them explicitly.
- PRs should include:
  - A short summary of doc changes.
  - Links to any referenced issues or external docs.
  - Screenshots only if you add visuals or rendered outputs.

## Agent-Specific Notes

- Lean heavily on Context7 MCP for references. Prefer these library IDs: `openai/openai-cookbook`, `websites/cookbook_openai`, `websites/developers_openai_codex`, `websites/codex_io`, `openai/codex`.
- If you rely on Context7, update `context7codexlib.txt` when the canonical library ID changes.
- When adding agent workflow instructions, keep them in a dedicated doc and reference it from `agents-md.md` or `goal.md`.

# AGENTS.md

## Agent skills

### Issue tracker

Issues and specs live as local markdown files under `.scratch/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default canonical labels (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context layout — one `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.

## Commit conventions

- **Conventional commits** — all commits follow `type(scope): description` (e.g. `docs(agents): add issue tracker`).
- **Atomic commits** — one file per commit, one logical change per commit. Never bundle multiple files into a single commit.

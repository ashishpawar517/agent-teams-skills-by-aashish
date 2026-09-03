---
name: agent-team
description: Fan out a team of Agents onto the unblocked Frontier of typed Tickets after to-tickets, one branch per Ticket, in confirm-then-watch waves.
disable-model-invocation: true
---

# Agent Team

Run one **Dispatch**: compute the **Frontier** of **Tickets**, confirm it with the human, then fan out one **Agent** per Ticket — routed by **Type** through the **Registry** — and repeat in waves until the DAG is empty.

The issue tracker, triage label vocabulary, and domain docs should have been provided to you — run `/setup-matt-pocock-skills` if `docs/agents/` is missing. Read `CONTEXT.md` and relevant `docs/adr/` before dispatching, and use the glossary's vocabulary in every ticket update.

## Process

### 1. Gather context

Take the feature reference from the user's argument — a `.scratch/<feature>/` directory, a `spec.md` path, or a blank call meaning "the current feature". Read:

- `.scratch/<feature>/spec.md` — the Spec the Tickets were cut from.
- Every `.scratch/<feature>/issues/<NN>-<slug>.md` — each Ticket's `Type:`, `Status:`, and `Blocked by:` lines plus acceptance criteria.
- `skills/engineering/agent-team/config.yaml` — the Registry mapping each Type to its Skill and prompt/seam.
- `CONTEXT.md` and `docs/adr/` entries touching this feature.

If the tracker is not local markdown (see `docs/agents/issue-tracker.md`), use that file's frontier query instead — the rest of this skill is unchanged.

### 2. Compute the Frontier

The Frontier is every Ticket where all three hold:

- `Status: ready-for-agent` (open — not `claimed`, `resolved`, `needs-info`, or `wontfix`).
- Every entry in `Blocked by:` resolves to a Ticket whose `Status: resolved`.
- No assignee / not already `claimed`.

List the Frontier in `NN` order with one line per Ticket: `NN-slug (Type: X, Blocked by: …) — one-sentence deliverable`. If the Frontier is empty, stop and say why (all resolved, or blocked Tickets with their blockers listed).

### 3. Confirm with the human

Never spawn silently. Ask via the `question` tool:

> Proceed with N Agents: `Type` #NN-slug → Skill per Ticket? (`maxParallel: 3-4`, best-effort)

Wait for approval before any `Task` call. If the human edits the Frontier (drops or reorders Tickets), honor the edit.

### 4. Dispatch one wave

For each approved Ticket in the wave:

1. **Claim**: set `Status: claimed` in its file before spawning.
2. **Branch**: create `agent-team/<NN>-<slug>` from `main`.
3. **Spawn**: one `Task` call (`subagent_type: general`) per Ticket, in a single parallel message up to `maxParallel` (default 3, max 4). Each Agent receives: the Ticket file contents, the Spec excerpt, `CONTEXT.md` terms, relevant ADR pointers, its branch name, and the mapped Skill instruction from the Registry:
   - `research` → run `/research`, write findings under `.scratch/<feature>/research/`.
   - `backend` / `frontend` → run `/implement` driving `/tdd` at the Registry's seam, on the Ticket branch.
   - `general` → run `/implement` directly.
4. **Watch**: append per-Agent status to `scratch/dispatch-<timestamp>.md` (`pending → running → passed-review | failed + log pointer`). The human may cancel or redirect any Agent from this file via `question`.

Best-effort: one Agent failing does not stop the others.

### 5. Review, merge, repeat

Per finished Ticket branch, run `/code-review` (Standards vs Spec parallel sub-agents). On pass: merge the branch, set `Status: resolved`, append the outcome to `scratch/dispatch-<timestamp>.md`. On fail: leave `Status: claimed` with the log pointer so the human can retry or re-scope.

Then recompute the Frontier — newly unblocked Tickets are now eligible — and return to step 3 for the next wave. The Dispatch ends when no Ticket remains `ready-for-agent` or `claimed`: report per-Ticket `resolved | failed` plus dispatch log path.

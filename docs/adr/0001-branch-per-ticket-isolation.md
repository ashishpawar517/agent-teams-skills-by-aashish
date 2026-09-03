# Branch-per-ticket isolation

Each Ticket is implemented on its own branch so Agents can work in parallel without conflicting on a shared branch and each Ticket can be reviewed and merged independently.

## Considered Options

- **Shared branch per Dispatch** — all frontier Agents push to one branch; simple to create but couples Tickets, forces serialized conflict resolution, and stalls the Frontier on any single failure.
- **Branch per Ticket (chosen)** — `agent-team/<NN>-<slug>` per Ticket; isolates work at the cost of more branches to manage.

## Consequences

Parallel review and safe per-Ticket rollback, but the Orchestrator must track and clean up N branches per Dispatch instead of one.

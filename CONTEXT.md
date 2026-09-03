# Agent Team Orchestration

Decomposes feature specs into typed, DAG-ordered tickets and dispatches teams of agents onto the unblocked frontier to implement them.

## Language

### Planning

**Spec**:
The authoritative description of a feature's desired behavior from which Tickets are decomposed.
_Avoid_: requirements doc, brief, spec doc

**Ticket**:
A single, independently completable unit of work derived from a Spec.
_Avoid_: issue, task, story, card

**Frontier**:
The set of Tickets that are ready to be worked now because they have no unresolved blockers.
_Avoid_: ready queue, backlog, open list

**Type**:
The category of work a Ticket represents that determines how it is executed.
_Avoid_: kind, label, category

**Registry**:
The mapping from Type to the Skill that should execute Tickets of that Type.
_Avoid_: lookup table, type map, config

### Execution

**Dispatch**:
A single orchestrated run that fans out Agents onto the current Frontier.
_Avoid_: run, batch, execution, session

**Dispatch Orchestrator**:
The coordinator that identifies the Frontier, confirms with a human, and fans out Agents for a Dispatch.
_Avoid_: scheduler, runner, manager, dispatcher

**Agent**:
An autonomous worker that completes one Ticket within a Dispatch.
_Avoid_: bot, worker, sub-agent

**Skill**:
A reusable capability package that defines how an Agent approaches a Ticket of a given Type.
_Avoid_: plugin, workflow, template, prompt pack

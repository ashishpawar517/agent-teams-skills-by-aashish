# Local Markdown tracker for now

Use local Markdown files for Specs and Tickets during initial development instead of GitHub Issues, to keep iteration fast and avoid coupling to an external tracker before the domain stabilizes.

## Considered Options

- **GitHub Issues from day one** — authoritative and collaborative, but adds API friction and schema lock-in while Types and flow are still changing.
- **Local Markdown (chosen)** — ephemeral files under `.scratch/`; fast to change, no external dependencies.

## Consequences

Tracker data is not a collaboration surface and will be migrated to GitHub Issues once Types, Statuses, and the Dispatch flow are stable.

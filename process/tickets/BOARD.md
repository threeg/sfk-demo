# World Clock — Ticket Board (Execution Order)

| | |
|---|---|
| **Document** | Topological index of all tickets |
| **Repository location** | `process/tickets/BOARD.md` |
| **Source** | The ticket files in `process/tickets/`; format per `TICKET-TEMPLATE.md`; system per `CONVENTIONS.md` |

This board is the single topological view of the implementation order. Implementation tickets are
listed by execution number (`TST-NNN`); reading top to bottom is a legal build sequence because no
ticket depends on a higher-numbered one (CONVENTIONS.md §4.3). It is a *derived* view of the ticket
files' `depends_on` edges and is regenerated, never hand-edited for status (CONVENTIONS.md §5.4).
Epics are containers and sit outside the execution order.

> When there are multiple versions, order the version sections **latest first**, and follow each
> version's execution order with its own cleanup backlog. Shipped versions collapse into a
> **"Shipped — vX.Y.0"** section.

**Status legend:** `todo` · `in-progress` · `blocked` · `in-review` · `done`

---

## Capability epics

| id | title | milestone | status |
|----|-------|-----------|--------|
| TST-E01 | <Epic title> | 8 | todo |
| TST-E02 | <Epic title> | 8 | todo |

---

## v0.1.0 — execution order (Milestone 8)

Leaf tickets, in dependency order. Reading top to bottom is a legal build sequence; no ticket depends
on a higher-numbered one. Epics close when their children are all `done`.

| # | id | title | type | layer | M / batch | epic | status | depends_on |
|---|----|-------|------|-------|-----------|------|--------|------------|
| 1 | TST-001 | <Initialise repository> | task | repo | 7 / scaffolding | — | todo | — |
| 2 | TST-002 | <Backend skeleton> | task | tooling | 7 / scaffolding | — | todo | TST-001 |
| 3 | TST-003 | <Frontend skeleton> | task | frontend | 7 / scaffolding | — | todo | TST-001 |
| 4 | TST-004 | <Test tooling + dependency-rule check> | task | tooling | 7 / scaffolding | — | todo | TST-002 |
| 7 | TST-007 | <Core constants> | task | core | 8 / core | TST-E02 | todo | TST-004 |
| 8 | TST-008 | <Core logic module> | task | core | 8 / core | TST-E02 | todo | TST-007 |
| … | … | … | … | … | … | … | … | … |

<!-- Fill one row per ticket. Keep this in step with the ticket files: when a ticket's status
     changes, update its row in the same commit (CONVENTIONS.md §5.4). -->

---

## v0.1.0 — cleanup backlog

Reactive tickets from post-batch review (CONVENTIONS.md §6). Not on the critical path unless promoted.

| # | id | title | type | layer | batch | status | depends_on |
|---|----|-------|------|-------|-------|--------|------------|
| — | — | (none yet) | — | — | cleanup | — | — |

---

## Traceability — requirements to tickets

Derived from each ticket's `implements` field. Every `FR`/`NFR` should appear against at least one
ticket.

| Requirement | Implemented by |
|-------------|----------------|
| FR-1 | TST-008 |
| NFR-1 | TST-038 |

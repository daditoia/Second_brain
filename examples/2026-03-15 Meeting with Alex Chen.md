---
type: meeting-note
date: 2026-03-15
source: "Teams Call"
time: "10:00-10:45 CET"
people:
  - "[[Alex Chen]]"
team:
  - "[[Platform Engineering]]"
tags:
  - meeting-notes
status: final
created: 2026-03-15
---

# Meeting with [[Alex Chen]] — API migration timeline

First sync with Alex after the reorg. We aligned on the API migration timeline and identified a dependency on the auth team that could block the Q2 deadline.

## Context

[[Alex Chen]] leads [[Platform Engineering]] and owns the migration from legacy REST endpoints to the new GraphQL layer. This migration affects all downstream teams including ours. We need to understand the timeline to plan our integration work.

## API migration status

The migration is 60% complete. Core entity endpoints (users, products, orders) are done. The remaining 40% covers payment, auth, and notification endpoints. Alex estimates 6 weeks for payments, 3 weeks for notifications. Auth is the wildcard.

The auth team ([[Sarah Kim]]) has a dependency on the new identity provider rollout. Until that ships, the auth endpoints stay on REST. No firm date yet.

## Impact on our integration

We can start building against the new GraphQL schema for core entities now. Payment and auth integrations should wait. Alex will publish a "stable subset" list by end of week so we know exactly which endpoints are safe to build against.

## Decisions

- Start integration work against core entity endpoints immediately.
- Defer payment/auth integration until Alex publishes the stable subset list.

## Open questions

- When will the identity provider rollout unblock auth endpoint migration?
- Should we build a temporary adapter for auth, or wait for the native GraphQL endpoint?

## Key takeaways

- Core API is ready for us to use now.
- Auth is the critical path risk for Q2.
- Alex is collaborative and prefers async Slack updates over recurring meetings.

## Action items

### [[Alex Chen]]

- [ ] Publish "stable subset" endpoint list by 2026-03-22.
- [ ] Get timeline from [[Sarah Kim]] on identity provider rollout.

### [[Your Name]]

- [ ] Start integration spike against core entity GraphQL endpoints.
- [ ] Add auth migration risk to Q2 planning doc.

## Related notes

- [[Alex Chen]]
- [[Platform Engineering]]
- [[API Migration]]

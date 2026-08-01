---
name: Resolve and merge duplicate entities
description: Find potential duplicate entities in a Reltio tenant, review the match evidence, then merge true duplicates or reject false positives.
api: mcp/reltio-mcp.yml
operations:
  - find_potential_matches_tool
  - get_entity_with_matches_tool
  - get_entity_match_history_tool
  - merge_entities_tool
  - reject_entity_match_tool
---

# Resolve and merge duplicate entities

Use the Reltio MCP Server tools to clean up duplicate golden records. Authenticate first with OAuth 2.0 (see `authentication/reltio-authentication.yml`); every call is scoped to a tenant.

## Steps

1. **Find candidates.** Call `find_potential_matches_tool` to list potential matches by match rule, score range, or confidence level. Start with high-confidence matches to keep steward review efficient.
2. **Review the evidence.** For each candidate, call `get_entity_with_matches_tool` to pull the entity alongside its potential matches, and `get_entity_match_history_tool` to see prior merge/unmerge decisions before acting.
3. **Merge true duplicates.** When two or more records are the same real-world entity, call `merge_entities_tool` with the entity ids to consolidate them into one golden record.
4. **Reject false positives.** When a candidate is not a duplicate, call `reject_entity_match_tool` so the pair is not resurfaced.

## Rules

- Prefer rejecting over merging when uncertain — a merge is disruptive and an `unmerge_entity_tool` may be needed to reverse it.
- Match scores and rules come from tenant business configuration (`get_business_configuration_tool`); do not assume thresholds.
- Every merge/reject consumes API entitlements against the SaaS Subscription Agreement.

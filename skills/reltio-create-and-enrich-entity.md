---
name: Create and enrich an entity profile
description: Create a new entity in a Reltio tenant, enrich its attributes, connect it to related entities, and inspect its relationship graph.
api: mcp/reltio-mcp.yml
operations:
  - create_entity_tool
  - get_entity_tool
  - update_entity_attributes_tool
  - create_relationships_tool
  - get_entity_graph_tool
---

# Create and enrich an entity profile

Build a trusted 360 profile with the Reltio MCP Server tools. Authenticate with OAuth 2.0 (see `authentication/reltio-authentication.yml`) against the target tenant.

## Steps

1. **Create the entity.** Call `create_entity_tool` with the entity type and initial attributes (types come from `get_entity_type_definition_tool` / the tenant data model).
2. **Read it back.** Call `get_entity_tool` with the returned entity id to confirm the crosswalk and attribute values.
3. **Enrich attributes.** Call `update_entity_attributes_tool` to add or correct specific attributes without replacing the whole record.
4. **Connect relationships.** Call `create_relationships_tool` to link the entity to related entities (e.g. person → organization) per the relation types defined in the tenant.
5. **Inspect the graph.** Call `get_entity_graph_tool` to traverse the entity's hops and verify the relationships resolved correctly.

## Rules

- Attribute and relation names must match the tenant's business configuration — validate with the data-model definition tools before writing.
- Writes may trigger match rules; check `find_potential_matches_tool` if you expect the new entity to collide with an existing one.
- All create/update calls count against API entitlements.

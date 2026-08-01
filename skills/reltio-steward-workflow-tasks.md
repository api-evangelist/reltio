---
name: Work a data-stewardship task queue
description: Retrieve Reltio workflow tasks, inspect their details, reassign for load balancing, and execute steward actions or start new process instances.
api: mcp/reltio-mcp.yml
operations:
  - retrieve_tasks_tool
  - get_task_details_tool
  - reassign_workflow_task_tool
  - execute_task_action_tool
  - start_process_instance_tool
---

# Work a data-stewardship task queue

Drive Reltio's data-governance workflow with the MCP Server tools. Authenticate with OAuth 2.0 (see `authentication/reltio-authentication.yml`); tasks are tenant-scoped.

## Steps

1. **Pull the queue.** Call `retrieve_tasks_tool` with filters (assignee, type, status) to list open workflow tasks, or `get_user_workflow_tasks_tool` for a single user's queue.
2. **Inspect a task.** Call `get_task_details_tool` with the task id to read the change request, entity context, and available actions.
3. **Balance load.** When a task is misassigned or a steward is overloaded, call `reassign_workflow_task_tool` (use `get_possible_assignees_tool` to find eligible stewards).
4. **Act on the task.** Call `execute_task_action_tool` with the chosen action (approve, reject, etc.) to advance the workflow.
5. **Kick off new work.** When a change needs a review workflow, call `start_process_instance_tool` to create a process instance for the change request.

## Rules

- Only execute actions that `get_task_details_tool` reports as valid for the task's current state.
- Reassignment and action execution are audited workflow events; act deliberately.
- All workflow calls count against API entitlements.

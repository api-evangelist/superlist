---
name: Capture and organize tasks into lists
description: Create a list, add tasks to it, label and move tasks so incoming work lands in the right place in Superlist.
api: mcp/superlist-mcp.yml
operations: [get_lists, add_list, add_task, add_label, move_task]
---

# Capture and organize tasks into lists

Use the Superlist MCP tools to turn unstructured input (notes, a paste, a request) into organized tasks.

## Steps

1. Call `get_lists` to see the existing lists and avoid creating a duplicate.
2. If no suitable list exists, call `add_list` to create one (you may seed it with initial tasks).
3. For each item, call `add_task` with a clear title and, when known, a due date and the target list.
4. Call `add_label` to tag tasks (e.g. priority or context) so they surface in the right views.
5. If a task was captured in the wrong place, call `move_task` to relocate it to the correct list.

## Rules
- Prefer adding to an existing list over creating new ones; only `add_list` when nothing fits.
- There is no documented idempotency key — do not blindly retry `add_task` on a timeout without first
  re-checking with `search` or `get_lists`, or you may create duplicates.
- Free-plan accounts are limited to 5 lists (and 5 members per list); handle a resource-limit error gracefully.
- On `401 unauthorized`, re-authenticate (see superlist-connect-and-authenticate.md).

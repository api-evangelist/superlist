---
name: Run a daily planning review
description: Review what is due today and in the inbox, then triage, reschedule, assign, and complete tasks in Superlist.
api: mcp/superlist-mcp.yml
operations: [today, inbox, search, update_task, assign_task, complete_task]
---

# Run a daily planning review

Drive a "what's on my plate today?" planning session over the Superlist MCP server.

## Steps

1. Call `today` to list tasks assigned and due today or earlier (this includes overdue items).
2. Call `inbox` to surface uncategorized incoming tasks that still need triage.
3. For anything ambiguous, call `search` to find related tasks or lists before acting.
4. Triage each task:
   - Reschedule or edit with `update_task` (e.g. move an overdue due date forward).
   - Delegate with `assign_task` when someone else should own it.
   - Close finished work with `complete_task`.
5. Summarize what was done and what remains for the user.

## Rules
- Treat `today` as the source of truth for "due or overdue"; do not recompute due dates yourself.
- Confirm destructive or bulk changes with the user before calling `complete_task`/`update_task` across many items.
- On `401 unauthorized`, re-authenticate (see superlist-connect-and-authenticate.md).

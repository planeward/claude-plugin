---
name: report
description: Build a read-only snapshot of the user's Planeward workspaces, such as open deals, unpaid invoices and work due soon, or a report the user describes.
argument-hint: "[what to report, e.g. unpaid invoices in Harbor Studio]"
disable-model-invocation: true
---

# Planeward report

The user asked for: $ARGUMENTS

Use only `search` and `query`. A report never changes data.

1. If no topic was given, build a weekly snapshot for every workspace from `planeward.workspace_list()`:
   - open opportunities by pipeline stage, with their total value;
   - invoices that are unpaid or past due, with amount and customer;
   - tasks due in the next 7 days or overdue, with their assignees;
   - new contacts in the last 7 days.
2. Run `search` for the operations you need (for example "list opportunities", "list invoices", "list tasks", "list contacts") and read their input types before writing code. Do not guess field names.
3. Write one `query` run per workspace, or one run that loops over the workspaces if it stays under 100 calls. Page lists with `limit` and `offset`, and aggregate in code so the result stays small.
4. Present the report with a short heading per workspace and a compact table per section. Show money in the workspace currency. Mark anything overdue.
5. Finish with up to three suggested follow-ups (for example "create a task for each stale deal"), and say that changes would run with `execute` after the user approves them.

If a section fails because the user lacks permission or the workspace has that app turned off, note it in one line and continue with the rest.

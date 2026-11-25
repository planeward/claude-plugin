---
name: planeward
description: Read and change the user's Planeward data (projects and tasks, contacts, pipelines and opportunities, customers, forms, sequences, events, invoices and payments, products and plans, balances, sites, email campaigns, workflows) through the Planeward MCP server's search, query and execute tools. Use whenever the user asks about or wants to change anything in their Planeward workspaces.
---

# Planeward

Planeward is the operating system for a service or coaching business. This plugin connects the Planeward MCP server (`https://app.planeward.com/api/mcp`). One connection reaches every workspace the user belongs to and acts with **their** permissions in each one, so you can only see and change what they can.

If the Planeward tools are missing or answer that sign-in is needed, tell the user to run `/mcp`, choose **planeward** and sign in. `/planeward:connect` walks through it.

## The three tools

| Tool      | Use it to                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------- |
| `search`  | Find operations. Returns TypeScript declarations: `planeward.<name>(input): Promise<unknown>`.  |
| `query`   | Run JavaScript that only **reads**. Write operations do not exist in a query run.              |
| `execute` | Run JavaScript that can also **write**. The user approves each run.                            |

Always `search` before you write code. Never guess an operation name or an input field.

- `search({ query: "create contact" })` returns the best matches with their input types.
- `search({ operations: ["contact_list", "opportunity_create"] })` returns exact declarations.
- `search({})` lists every operation name you can use.

## Writing code for query and execute

The code is the body of an async function. Call `planeward.*` and `return` a JSON value.

```js
const { workspaces } = await planeward.workspace_list();
return workspaces;
```

- **workspaceId:** every workspace operation takes `workspaceId`. Get ids from `planeward.workspace_list()`. You may leave it out only when the user has a single workspace.
- **Lists** take `limit` (1 to 100, default 50) and `offset`, and return `{ data, total }`. Page until you have what you need.
- **Ids first:** resolve ids with the matching list operation (`project_status_list`, `project_label_list`, `workspace_member_list`, `pipeline_stage_list`, `site_list`, `site_page_list`) instead of guessing.
- **Return less:** results over about 100 KB are cut. Map rows to the fields you need and aggregate in code.
- **Limits per run:** 100 `planeward.*` calls and 30 seconds. There is no `fetch`, no timers and no filesystem. `console.log` output comes back as `logs`.
- **Errors:** a failed call throws with a readable message (missing permission, unknown id, plan limit). Catch it when one failure should not stop the rest.

Several workspaces in one run:

```js
const { workspaces } = await planeward.workspace_list();
const rows = [];
for (const workspace of workspaces) {
  const { total } = await planeward.opportunity_list({
    workspaceId: workspace.id,
    limit: 1,
  });
  rows.push({ workspace: workspace.name, openDeals: total });
}
return rows;
```

## Reads and writes

- Use `query` for anything that only reads: reports, lookups, checks before a change.
- Use `execute` only when the user asked for a change. Before you run it, say in one or two lines what will change and in which workspace. For bulk changes, read first with `query`, show the count, and confirm.
- Treat money operations with extra care: payments, invoices marked paid, balance credits, debits and transfers, purchases and refunds, and anything that sends email. Confirm the exact amount, recipient and workspace first.
- After a write, read back what changed and report it plainly.

## Domain notes

- Opportunities need a `contactId`. Track follow-up state with sequences (`contact_sequence_*`), not tags.
- Call `site_page_body_schema` before `site_page_update`, and `workflow_node_types` before `workflow_graph_update`.
- Record sales from an outside checkout with `purchase_record`; it is safe to retry with the same `externalRef`.
- Playbooks (for users with access) are under `playbooks_list`, `playbooks_search` and `playbooks_get`.

Docs: https://docs.planeward.com/docs/mcp

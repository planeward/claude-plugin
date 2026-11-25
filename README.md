# Planeward for Claude

Run your Planeward workspaces from Claude and Claude Code: projects and tasks, CRM and pipelines, forms, sequences, events, invoices and payments, products, sites, email and workflows. One connection reaches every workspace you belong to and acts with your own permissions in each.

## Install

**Claude Code:**

```
/plugin marketplace add planeward/claude-plugin
/plugin install planeward@planeward-plugins
```

Or from a terminal:

```bash
claude plugin marketplace add planeward/claude-plugin
claude plugin install planeward@planeward-plugins
```

Then run `/mcp`, choose **planeward** and sign in with your Planeward account. Run `/planeward:connect` to check the connection and see your workspaces.

**Claude (claude.ai and the desktop app):** add Planeward from the plugin directory once it is listed there. Until then, add the server as a custom connector: **Settings → Connectors → Add custom connector**, paste `https://app.planeward.com/api/mcp` and sign in with Planeward.

## What you get

- **Planeward MCP server** at `https://app.planeward.com/api/mcp`, with three tools:
  - `search` finds Planeward operations and their input types;
  - `query` runs code that only reads;
  - `execute` runs code that can also change data, after you approve it.
- **A skill** that teaches Claude how to work with Planeward: look up operations first, pass `workspaceId`, page lists, read before writing, and confirm changes that move money or send email.
- **`/planeward:connect`** checks the connection and lists your workspaces.
- **`/planeward:report [topic]`** builds a read-only snapshot: open deals, unpaid invoices, work due soon, or whatever you describe.

## Examples

- "Across all my workspaces, which deals have gone two weeks without a follow-up? Make a task for each owner."
- "List invoices unpaid for 30 days in Harbor Studio and draft a reminder for each customer."
- "Build a landing page for the new cohort and send me the preview link."

## Access and privacy

- MCP is included on every Planeward plan.
- Claude can only see and change what your role allows in each workspace. Reads run through `query`; every change goes through `execute`, which Claude asks you to approve.
- Disconnect at any time in Planeward under **Settings → MCP → Connected apps**.

Docs: https://docs.planeward.com/docs/mcp

## License

MIT

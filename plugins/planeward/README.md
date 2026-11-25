# Planeward

Run your Planeward workspaces from Claude: projects and tasks, contacts, pipelines and deals, customers, forms, sequences, events, invoices and payments, products, sites, email campaigns and workflows. You connect once with your Planeward account. The connection reaches every workspace you belong to and acts with your own permissions in each, so Claude can only see and change what you can.

MCP is included on every Planeward plan.

## What the plugin adds

- **Planeward MCP server** (`https://app.planeward.com/api/mcp`), signed in with OAuth. It has three tools:
  - `search` finds Planeward operations and returns their input types;
  - `query` runs a short program that only reads your data;
  - `execute` runs a short program that can also change your data. Claude asks for your approval before each run.
- **Planeward skill.** Teaches Claude to look up operations before writing code, pass the right workspace, page through lists, read before writing, and confirm changes that move money or send email.
- **`/planeward:connect`** checks the connection, helps you sign in and lists your workspaces.
- **`/planeward:report [topic]`** builds a read-only snapshot, such as open deals, unpaid invoices and work due soon, or a report you describe.

## Set up

1. Install the plugin.
   - **Claude Code** (2.1.275 or later):

     ```
     /plugin install planeward --marketplace planeward/claude-plugin
     ```

     Or add the marketplace first with `/plugin marketplace add planeward/claude-plugin`, then `/plugin install planeward@planeward-plugins`.

   - **Claude** (claude.ai and the desktop app): add Planeward from the plugin directory.

2. Sign in with your Planeward account when Claude asks. In Claude Code, run `/mcp` and choose **planeward**.
3. On the Planeward screen, choose **Allow**. One approval covers all your workspaces.
4. Run `/planeward:connect` (or ask Claude to check your Planeward connection) to see your workspaces.

## What it connects to and sends

- The plugin connects to one server: Planeward at `https://app.planeward.com/api/mcp`. It contains no hooks, scripts or local programs, and it installs no packages.
- Claude sends Planeward your requests to that server: search terms, and the code it writes for `query` and `execute` runs. Planeward runs that code in its own isolated sandbox with no internet access. Each data request from the code goes back to Planeward, which checks your membership and permissions in that workspace.
- Planeward returns the data you asked for to Claude. Planeward keeps an activity record of every change made through MCP in the workspace it changed.
- Your sign-in uses OAuth. The plugin stores no passwords or API keys.
- You can disconnect at any time in Planeward under **Settings → MCP → Connected apps**. The next request from Claude is refused.

Privacy policy: https://planeward.com/privacy. Terms: https://planeward.com/terms. Docs: https://docs.planeward.com/docs/mcp. Support: https://github.com/planeward/claude-plugin/issues.

## License

MIT

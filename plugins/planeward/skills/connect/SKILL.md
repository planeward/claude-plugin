---
name: connect
description: Check the Planeward connection, help the user sign in, and list the workspaces Claude can reach.
disable-model-invocation: true
---

# Connect Planeward

1. Check whether the Planeward MCP tools (`search`, `query`, `execute` from the **planeward** server) are available.
2. If they are missing, or a call answers that sign-in is needed, tell the user how to sign in:
   - **Claude Code:** run `/mcp`, choose **planeward** and sign in with their Planeward account in the browser.
   - **Claude (claude.ai or the desktop app):** open the Planeward connector in **Settings → Connectors** and choose **Connect**.
   - On the Planeward screen, choose **Allow**. One approval covers every workspace they belong to. Then try again.
3. If they are available, run this with `query`:

   ```js
   const me = await planeward.whoami();
   const { workspaces } = me.workspaces
     ? await planeward.workspace_list()
     : { workspaces: [] };
   return { me, workspaces };
   ```

4. Report in a few lines: who is connected, each workspace by name with the user's role, and whether Playbooks are available.
   - If `whoami` says the connection reaches no workspaces, it is an older Playbooks-only connection. Tell the user to disconnect it in Planeward (**Settings → MCP → Connected apps**) and sign in again.
   - If the list is empty, the user has no workspace yet. They can create one at https://app.planeward.com.
5. End with two or three example requests they can make next, based on what the workspaces contain.

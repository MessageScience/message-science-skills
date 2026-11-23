# Message Science skill

Message Science runs cold-email infrastructure: domains, mailboxes and the sequencers they connect to.
This repository holds the agent skill and plugin for its hosted MCP server, `https://api.messagescience.com/mcp`.

With it, your agent can answer billing, domain and mailbox questions in plain words. For example:
what you pay each month, a quote for 30 more mailboxes, which domains expire soon without renewing, or
the status of an order. It can also **prepare** orders, permanent mailbox deletions and domain renewal
changes. Nothing changes until you see the preview and say yes.

The agent acts as you, with your role in each workspace. It never sees mailbox passwords, card
details or sequencer API keys. If you ask for a mailbox password or login, it gives you the console steps
to export it yourself.

## Connect

Sign-in uses OAuth with your Message Science account. You never paste an API key.

**Claude Code**

```bash
claude mcp add --transport http --scope user messagescience https://api.messagescience.com/mcp
```

Then run `/mcp`, choose `messagescience` and **Authenticate**.

**Codex**

```bash
codex mcp add messagescience --url https://api.messagescience.com/mcp
```

`add` opens sign-in. Run `codex mcp login messagescience` only if sign-in did not finish or you need to reconnect.

**Antigravity**

```bash
agy mcp add messagescience https://api.messagescience.com/mcp
```

Then run `/mcp`, choose `messagescience` and **Authenticate**. Sign in in your browser, then paste the code it
shows back into `/mcp`.

**Cursor**: add this to `~/.cursor/mcp.json` (or **Settings → MCP → Add new MCP server**), then click **Needs login**:

```json
{ "mcpServers": { "messagescience": { "url": "https://api.messagescience.com/mcp" } } }
```

**Claude.ai and Claude Desktop**: **Settings → Connectors → Add custom connector**. Name `Message Science`,
URL `https://api.messagescience.com/mcp`. Click **Connect** and sign in.

**ChatGPT**: On ChatGPT web, open Plugins, select **+ → Add custom MCP server**, name it **Message Science**, enter `https://api.messagescience.com/mcp`, and choose OAuth. Review the warning, create the plugin, and sign in when prompted. If your workspace uses the legacy Apps interface, open **Settings → Apps → Create**. Your workspace must permit custom MCP servers.

Check it works: ask your agent "Check your Message Science connection and show me what you can access."

## Install the skill

The server's built-in instructions cover the basics. The skill adds detailed guidance on previews, approvals and errors.

```bash
npx --yes skills@latest add messagescience/message-science-skills --skill message-science
```

Choose your host and the project or user scope in the installer. The skill gives instructions only; connect the
server as described above.

**Claude Code plugin** (skill and server together):

```bash
claude plugin marketplace add messagescience/message-science-skills
claude plugin install message-science@message-science
```

**Codex plugin** (skill and server together):

```bash
codex plugin marketplace add messagescience/message-science-skills
codex plugin add message-science@message-science
```

Start a new session after installing a plugin.

## First use: approve the connection

The first request from a new AI app returns a link. Open it and sign in with the same Message Science
account you connected. Read access is always included. Approving covers every workspace you are a member of.
Both change permissions, **Place orders** and **Remove mailboxes and change domain renewals**, start selected;
clear them to keep this app read-only. Click **Approve** and ask your agent to try again. Only approve an app you
just connected yourself.

Your role and each workspace's switches still apply.
Change or revoke the approval under **Connected AI apps** in
[Settings > Connect an AI agent](https://app.messagescience.com/settings/agents). A revoke takes effect on the next request.

## Costs

Mailbox charges are monthly. Domain charges are annual. Your card may be charged today. The agent says this
the first time you ask to buy, before it asks for details. Before it submits, it must show
what you'll pay today, the new monthly total and any annual domain charges, and get your clear yes.

If a quote can't price an amount, the agent says it is unavailable and gives no number; it never shows it as $0.
Use **New order** in the console, which shows the list price before checkout. If an order preview can't be priced,
the agent does not place the order. Place it in the console instead.

If you add mailboxes to a domain whose renewal is off, the agent asks you to choose first. **Use until expiry** keeps
renewal off, and the mailboxes stop working when the domain expires. **Turn renewal on** shows the annual renewal
cost for your approval as a separate step before the mailbox order. **Use until expiry** needs a known future expiry date. If the date is unknown, the agent offers to review turning renewal on
or to leave those mailboxes out.

## Turn agent ordering off

Placing orders, removing mailboxes and changing renewals need the Owner or Admin role. Members and Viewers can read permitted inventory. Customer-paid billing summaries and quotes require a workspace Owner or account-wide Admin. For a group that pays directly, its Owner, Admin or Billing role can also get summaries and quotes. In [Settings > Connect an AI agent](https://app.messagescience.com/settings/agents), an Owner or
account-wide Admin can turn off **Allow agents to place orders** or **Allow agents to remove mailboxes and change
domain renewals** for every AI app in that workspace. You can also limit or revoke a single app under **Connected AI apps**.

Disconnect at any time by removing the connector in your app.

## Contents

| Path | Purpose |
|---|---|
| `message-science/SKILL.md` | The Message Science skill |
| `plugins/message-science/` | Plugin bundle for Claude Code and Codex: the same skill plus the hosted server |
| `skills.sh.json`, `.claude-plugin/`, `.agents/plugins/` | Skill and plugin indexes |

Licensed under the MIT License.

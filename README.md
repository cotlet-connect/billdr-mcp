# Billdr API and MCP server for ChatGPT and Claude

Billdr documents an API key for its Zapier integration on the Titanium plan and no official MCP server (checked 16 November 2026). Cotlet.dev connects ChatGPT, Claude and other AI agents to your own Billdr account through a hosted MCP server with 18 workflow tools. Writes ask for your approval by default.

## What you can ask

> List my Billdr projects and read the budget for the kitchen renovation.

> Draft a quote from my cost catalog and show it to me before saving.

> Prepare a change order for a project with a signed quote.

> List overdue invoices for this project.

> Read crew timesheets and project labor cost.

> Draft a client report from the daily logs, keeping it internal until I approve sharing.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` with OAuth.

### Claude

Settings > Connectors > Add custom connector. Use `https://cotlet.dev/mcp/v1/connect`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Billdr tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Clone this repository and run `python3 billdr_agent.py serve` with `COTLET_API_KEY` set.

Or with Docker:

```bash
docker build -t billdr-mcp .
docker run -i --rm -e COTLET_API_KEY billdr-mcp
```

The local server exposes seven browser helper tools, distinct from the 18 hosted workflow tools described on the live page.

Local tools: `billdr_guide`, `billdr_status`, `billdr_open`, `billdr_read`, `billdr_click`, `billdr_type`, `billdr_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Run from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 billdr_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 billdr_agent.py status    # is Billdr ready in your Cotlet.dev account
python3 billdr_agent.py guide     # Billdr screens and sign-in link
python3 billdr_agent.py read      # open Billdr, read the page, close the browser
python3 billdr_agent.py tools     # list Cotlet.dev tools
python3 billdr_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Billdr users

**Does Billdr have a public API or official MCP server?**

The live Cotlet.dev page, checked 16 November 2026, reports no public API documentation or official MCP server. Billdr documents a Titanium-plan API key for its Zapier integration.

**Which Billdr plan do I need?**

Your Billdr plan controls the workflows available. Quotes, invoices, change orders, expenses and the client dashboard are in Starter; scheduling, daily logs, timesheets and budgets require Premium or higher.

**Does the agent send changes immediately?**

Write tools ask for approval by default. Creating an unsent quote and sending it are separate actions. You can change approval settings per tool.

**Is this an official Billdr integration?**

No. Cotlet.dev is independent and is not affiliated with Billdr.

## Live integration

[Billdr tools and connection guide](https://cotlet.dev/integrations/billdr-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=billdr)

---

More: https://cotlet.dev/integrations/billdr-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=billdr

Start free: https://cotlet.dev/signup?app=billdr&utm_source=github&utm_medium=readme&utm_campaign=billdr

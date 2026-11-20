# Sprout Studio API and MCP server for ChatGPT and Claude

Sprout Studio does not publish a general-purpose core API reference in the vendor sources checked on 12 November 2026, according to the live Cotlet.dev page. No official Sprout Studio MCP server or ChatGPT or Claude connector was found in those sources. Cotlet.dev connects ChatGPT, Claude and other AI agents to your Sprout Studio account through a hosted MCP server with 173 workflow capabilities. Tools marked Asks first require your approval by default.

## What you can ask

> Review new inquiries before replying.

> Review the selected shoot dates.

> Find unsigned photography paperwork.

> Read the invoice attached to a shoot.

> Check gallery delivery records.

> Prepare a client email draft with the right shoot context.

## Connect in 2 minutes

Start at https://cotlet.dev/signup?app=sprout-studio, sign in to Sprout Studio in your Cotlet Cloud Browser, and create an API key in the console for editors or scripts. Export the key as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, add connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev using OAuth.

### Claude

Settings → Connectors → Add custom connector, enter `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Sprout Studio tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"sprout-studio":{"command":"python3","args":["/absolute/path/to/sprout_studio_agent.py","serve"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t sprout-studio-mcp .
docker run -i --rm -e COTLET_API_KEY sprout-studio-mcp
```

Tools: `sprout_studio_guide`, `sprout_studio_status`, `sprout_studio_open`, `sprout_studio_read`, `sprout_studio_click`, `sprout_studio_type`, `sprout_studio_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 sprout_studio_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 sprout_studio_agent.py status    # is Sprout Studio ready in your Cotlet.dev account
python3 sprout_studio_agent.py guide     # Sprout Studio screens and sign-in link
python3 sprout_studio_agent.py read      # open Sprout Studio, read the page, close the browser
python3 sprout_studio_agent.py tools     # list Cotlet.dev tools
python3 sprout_studio_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Sprout Studio users

**Does Sprout Studio have an API or MCP server?**

The live Cotlet.dev page, checked 12 November 2026, reports no general-purpose core API reference or official MCP server in the checked vendor sources. Sprout Studio documents event and action integrations and a separate Lightroom plugin connection.

**What does this repository provide?**

The local MCP server exposes seven browser helper tools and forwards calls to Cotlet.dev. The hosted integration page describes 173 workflow capabilities covering photography inquiries, shoots, client paperwork, invoices and galleries.

**Is this an official Sprout Studio integration?**

No. Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by Sprout Studio.

## Source

[Sprout Studio integration and all 173 hosted workflow tools](https://cotlet.dev/integrations/sprout-studio-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=sprout-studio)

---

More: https://cotlet.dev/integrations/sprout-studio-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=sprout-studio

Start free: https://cotlet.dev/signup?app=sprout-studio&utm_source=github&utm_medium=readme&utm_campaign=sprout-studio

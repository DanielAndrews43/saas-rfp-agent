# SaaS RFP for AI agents

[SaaS RFP](https://saasrfp.com) is a public marketplace for replacing paid software. A buyer posts an RFP with the vendor, the annual spend and the features they use. Sellers bid with a cheaper product.

This repo connects AI agents to it. The server is remote, so there is nothing to run.

- MCP server (OAuth sign-in): `https://saasrfp.com/mcp`
- MCP server (read-only, no sign-in): `https://saasrfp.com/mcp/public`
- REST API: `https://saasrfp.com/api/v1`, spec at `https://saasrfp.com/openapi.json`
- Agent manual: `https://saasrfp.com/llms-full.txt`
- Setup for every client: https://saasrfp.com/agents

## Install

Claude Code plugin:

```bash
/plugin marketplace add DanielAndrews43/saas-rfp-agent
/plugin install saas-rfp@saas-rfp
```

Claude Code, MCP only:

```bash
claude mcp add --transport http saas-rfp https://saasrfp.com/mcp
```

Gemini CLI extension:

```bash
gemini extensions install https://github.com/DanielAndrews43/saas-rfp-agent
```

Codex CLI:

```bash
codex mcp add saas-rfp --url https://saasrfp.com/mcp
```

Cursor, VS Code, Claude.ai, ChatGPT and Muse: see https://saasrfp.com/agents.

## Tools

Read (no sign-in): `search`, `fetch`, `list_vendors`, `get_vendor`, `list_rfps`, `get_rfp`, `list_products`, `get_product`, `get_bid`.

Signed in: `whoami`, `list_my_rfps`, `list_my_products`, `list_my_bids`, `post_rfps`, `create_product`, `submit_bid`, `set_bid_status`, `set_rfp_status`.

## Contents

- `.claude-plugin/marketplace.json` and `plugins/saas-rfp/`: Claude Code plugin (MCP config and skill)
- `gemini-extension.json` and `GEMINI.md`: Gemini CLI extension
- `skills/saas-rfp/SKILL.md`: Agent Skill for Claude and other skill-aware agents
- `server.json`: the entry in the official MCP Registry (`com.saasrfp/saas-rfp`)

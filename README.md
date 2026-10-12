# StatementSure for AI agents (MCP server)

Turn bank and credit card statement PDFs into **verified** transactions and **QuickBooks, Xero, OFX, CSV or Excel**
files, right inside Claude Desktop, Claude Code, Cursor and any other MCP client.

- **Balance-checked:** opening balance + transactions must equal the closing balance, and every running balance and
  section total is checked. You're told exactly which rows need a look.
- **Private:** runs on your computer and makes no network calls. Your statements are never uploaded.
- **No bank templates:** reads the page layout, so it works across banks. Tested on real RBC, TD, CIBC and Scotiabank
  statements in English and French, and on common US and UK layouts.

Same engine as [statementsure.com](https://statementsure.com/?ref=npm).

## Install

Requires [Node.js](https://nodejs.org) 22 or newer.

**Claude Code**

```bash
claude mcp add statementsure -- npx -y statementsure-mcp
```

**Claude Desktop, Cursor and other clients:** add this to the client's MCP configuration (in Claude Desktop:
Settings → Developer → Edit Config), then restart the client:

```json
{ "mcpServers": { "statementsure": { "command": "npx", "args": ["-y", "statementsure-mcp"] } } }
```

Then ask, for example: *"Check the bank statements in ~/Documents/statements and export them for QuickBooks."* Give
the agent a folder or file path on your computer; don't attach the PDFs to the chat, because attachments are uploaded.

## What it can do

| Tool | What it does | Pages |
|---|---|---|
| `check_statements` | Bank, account (last 4 digits), period, balances, totals, each balance check and the rows that need a look | Free |
| `get_transactions` | Every transaction with date, description, signed amount and running balance | Uses pages |
| `export_statements` | Writes `qbo` (QuickBooks), `qb-csv`, `xero`, `ofx`, `csv` or `xlsx` next to the PDFs, one import file per account; never overwrites | Uses pages |
| `usage` | Free pages left this month | Free |

**20 free pages a month.** Only pages with transactions count. Statements that don't balance are free, and exporting
the same PDF again in the same month is free. For more pages, buy a page pack at [statementsure.com](https://statementsure.com/pricing?ref=npm).

## Agent skill (optional)

The package includes an agent skill (`skills/statementsure/SKILL.md`) that teaches the agent the workflow: check first,
explain each statement's status, then export or reconcile.

- **Claude Code:** copy the `skills/statementsure` folder into `~/.claude/skills/`.
- **Claude Desktop:** zip the `statementsure` folder and upload it under Settings → Capabilities → Skills.

## Good to know

- Password-protected PDFs work; the agent will ask you for the password.
- Scanned (image-only) PDFs aren't supported yet.
- The free-page counter is kept in `~/.statementsure/usage.json`.

## License

Proprietary, see [LICENSE](LICENSE). © BulkLift Platform Inc. You may install and use this package to convert your own statements; you may not
copy, modify, redistribute or reverse-engineer it. Support: [statementsure.com](https://statementsure.com/?ref=npm).

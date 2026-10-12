---
name: statementsure
description: Convert bank and credit card statement PDFs into verified transactions and QuickBooks, Xero, OFX, CSV or Excel files, and reconcile them. Use when the user mentions bank statements, credit card statements, statement PDFs, bookkeeping catch-up, bank reconciliation, or importing transactions into QuickBooks or Xero. Requires the statementsure MCP server.
---

# Bank statements with StatementSure

The `statementsure` MCP server reads statement PDFs on the user's computer, extracts every transaction and checks that
the statement balances. Nothing is uploaded.

## Workflow

1. **Find the PDFs.** Get absolute paths. Group statements from the same account and keep them in date order.
2. **Check first (free).** Call `check_statements`. Report each file in one line: bank, account last 4, period, number
   of transactions, and the status.
   - `verified`: every balance check passed. Safe to use.
   - `warning`: it balances overall but some rows were corrected or guessed. Show `flaggedRows` and ask the user to
     confirm them against the PDF.
   - `failed`: it doesn't balance; a row is probably missing or misread. Show `difference` and `flaggedRows` and don't
     import it as-is. Exports of failed statements are free so the user can fix them in a spreadsheet.
   - `unverifiable`: the statement shows no balances to check against. Say so plainly.
   - If dates look wrong (e.g. 3 April read as March 4), re-run with `dateOrder: "DMY"`. If a card statement was read as
     a bank account, re-run with `accountType: "credit_card"`.
   - For a password-protected PDF, ask the user for the password and pass `password`.
3. **Deliver what the user needs.**
   - Import file: `export_statements` with `format`:
     - QuickBooks Online or Desktop bank upload: `qbo`. If the user prefers CSV upload: `qb-csv`.
     - Xero: `xero`. Wave, Quicken and most other apps: `ofx`. A spreadsheet: `xlsx` or `csv`.
   - Working with the rows (categorizing, finding a payment, totals by payee, entering into another tool): call
     `get_transactions`. Amounts are signed: money in is positive, money out is negative.
4. **Before using pages, say so.** Exports and transaction lists use free monthly pages (shown as `exportCost` in the
   check). Statements that don't balance and repeat exports of the same file this month are free. If the pages run out,
   pass on the tool's message; don't try to work around it.

## Reconciling against the books

When the user also has ledger data (a QuickBooks or Xero MCP server, or an export), match by date (±3 days) and exact
amount, then list three groups: matched, in the bank but not in the books, and in the books but not in the bank. Never
create or change entries in accounting software without the user's explicit approval of the exact list.

## Don't

- Don't give tax, legal or investment advice based on the transactions.
- Don't paste full account numbers into chat. The tools only return the last 4 digits.

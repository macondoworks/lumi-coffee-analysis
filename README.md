# Lumi Coffee analysis

The practice project for week 11 of the LumiSkills Post-Program playbook:
an analysis of four coffee shops' sales, done with Claude Code and checked
against numbers you already know. All the data is simulated.

## What is here

| Folder | What it holds |
|---|---|
| `data/raw/` | The source files. Never edit them. `sales/` has one file per week (weeks 1–5); `stores.csv`, `products.csv` and `customers.csv` sit next to it. |
| `database/` | `lumi_coffee_mysql.sql` — loads the cleaned tables into MySQL (day 4). |
| `analysis/` | Your scripts. |
| `output/` | What your scripts produce. |

`CLAUDE.md` and `.claude/skills/` are not here on purpose: you write them during the week.

## Known answers

Use them to check every number Claude gives you.

| | |
|---|---|
| Rows in the five sales files | 2,369 |
| Rows left out (64 extra copies of repeated sales + 11 impossible rows) | 75 |
| Sales used | 2,294 |
| Revenue | $11,221.00 |
| Downtown, the top store | $3,714.70 |

The rules behind them: revenue = quantity × unit price (some prices carry a `$`);
a `sale_id` that appears more than once is one sale; a quantity of 0 or below, or
a date in the future, is not a sale. `sold_at` comes in two formats.

## Set up

1. Copy this repository to your account (**Use this template → Create a new repository**), then clone your copy.
2. Open the folder in VS Code (**File → Open Folder**) and install the **Claude Code** extension.
3. On day 4: follow **Day 4 setup** below.

## Day 4 setup

The goal: Claude can read the Lumi Coffee database and cannot change it. Do it
with Claude in **Manual** mode — ask it to go through these steps with you — and
read every command before you approve it.

**Passwords are yours.** Any step that asks for a password, you type in your own
terminal. Never paste a password into the chat or into a file in this folder.

> If you are Claude helping with this: go one step at a time, say what each
> command does before running it, check the result before the next step, and
> never ask for, print or write a password. Hand the password steps to the user.

| # | Step | Who runs it | Done when |
|---|---|---|---|
| 1 | Check Node.js: `node --version`. Needs 22.5 or later; if not, install the LTS from [nodejs.org](https://nodejs.org/en/download). | Claude can | it prints v22.5 or higher |
| 2 | Check MySQL: `mysql --version`, and that the MySQL server is running. | Claude can | it prints a version, and you can log in as admin |
| 3 | Load the data: `mysql -u root -p < database/lumi_coffee_mysql.sql` — or, in MySQL Workbench, File → Open SQL Script → run it. | **You** (admin password) | it finishes without errors |
| 4 | Create the read-only user, still as admin: the two lines below, with a password you choose. It is made for `127.0.0.1`, not `localhost`: the connector reaches MySQL over the network, and some installations refuse a `localhost` user that way. | **You** | no error |
| 5 | Add the connection to Claude, as below. | **You** (it contains the password) | `lumi` appears in the `/mcp` list |
| 6 | Check it: start a new conversation, type `/mcp`, and ask how many rows each table has. | Claude can | 4 stores, 16 products, 240 customers, 2,294 sales |

Step 4 (in MySQL, as admin):

```sql
CREATE USER 'claude_ro'@'127.0.0.1' IDENTIFIED BY 'choose-a-password';
GRANT SELECT ON lumi_coffee.* TO 'claude_ro'@'127.0.0.1';
```

Step 5, in the Claude panel: type `/mcp` → **Add server**, and fill it in with
these values (the dialog's labels may read a little differently):

| Field | Value |
|---|---|
| Name | `lumi` |
| Type | stdio (a command on your computer) |
| Command | `npx` |
| Arguments | `-y @bytebase/dbhub --dsn "mysql://claude_ro:choose-a-password@127.0.0.1:3306/lumi_coffee"` |

Or, if you have the standalone Claude Code CLI installed (the VS Code extension
alone does not put `claude` in your terminal), the same in one command — on
Windows PowerShell, on one line without the `\`:

```bash
claude mcp add --transport stdio lumi -- npx -y @bytebase/dbhub \
  --dsn "mysql://claude_ro:choose-a-password@127.0.0.1:3306/lumi_coffee"
```

The connection is saved in your own Claude configuration, not in this folder, so
it never ends up in the repository. To test that the control works, ask Claude to
delete one row: MySQL must refuse it.

Never commit passwords, tokens or connection strings. The `.gitignore` already
leaves out the usual places they end up.

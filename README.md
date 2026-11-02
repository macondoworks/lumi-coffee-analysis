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
3. On day 4: run `database/lumi_coffee_mysql.sql` as your MySQL admin user, and follow the lesson to connect Claude with a read-only user.

Never commit passwords, tokens or connection strings. The `.gitignore` already
leaves out the usual places they end up.

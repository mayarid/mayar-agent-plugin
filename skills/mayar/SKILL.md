---
name: mayar
description: Use when the user asks to read or manage their Mayar account (mayar.id) — balance, invoices, payment links, products, customers, transactions, memberships, coupons, webhooks, QRIS or license codes.
---

# Mayar

The `mayar` MCP server connects to the user's own Mayar account at `https://mcp.mayar.id/mcp`.
It uses the production Mayar API (`api.mayar.id`). Every write moves real money or changes live data.

## Tools

Each domain is one tool with an `action` argument. The tool description lists every action and its arguments.

| Tool | Actions |
| --- | --- |
| `account` | status, balance, channels, stats |
| `invoice` | list, get, by_email, create, update, close, reopen, set_status |
| `installment` | list, get, create |
| `customer` | list, get, create, update, portal_link |
| `transaction` | list, get, unpaid, daily, by_product |
| `product` | list, by_type, get, create_*, update_*, set_status, sort, bundles, bundle |
| `payment` | list, get, create, update, set_status |
| `membership` | members, member, add_member, update_member, cancel_member, member_invoice, tiers, tier_get, tier_create, product_get, product_create |
| `credit` | balance, add, spend, history, register_usage, register_membership, checkout |
| `coupon` | list, get, check, validate, create |
| `webhook` | history, new_history, get_url, set_url, test, retry |
| `review` | list, product, customer, product_stats, merchant_stats, create, update, bulk_status |
| `qr` | dynamic, static |
| `license` | verify, software_verify, saas_activate, saas_deactivate, saas_verify |
| `search_docs` | search docs.mayar.id |

## Rules

1. Read before you write. Use `list` or `get` to confirm the record before `create`, `update`, `close`, `set_status` or `cancel_member`.
2. Before any write, show the user the exact action and arguments. Wait for a clear yes.
3. Never invent amounts, emails or IDs. Ask the user for a missing value.
4. Lists return 10 items by default. Use `limit` (50 at most) and `after` to page.
5. To check that the key works, call `account` with `{"action": "balance"}`. `{"action": "status"}` does not call Mayar, so it does not prove the key is valid.
6. A tool error contains Mayar's own message. Show it to the user. Do not retry a failed write without asking.

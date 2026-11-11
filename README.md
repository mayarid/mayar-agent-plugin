# Mayar Agent Plugin

Connect an AI agent to your [Mayar](https://mayar.id) account. This package uses Mayar's official
hosted MCP server at `https://mcp.mayar.id/mcp`. With it, the agent can read and manage invoices,
payment links, products, customers, transactions, memberships, coupons, webhooks, reviews, QRIS and
license codes.

This is a Portable Agent Plugins v1 package (`plugin.json`, `mcp.json` and `skills/`). It is
configuration only. It contains no executable code.

## Requirements

- A Mayar account.
- A production API key from [web.mayar.id/api-keys](https://web.mayar.id/api-keys).

## Install in Hermes Agent

```sh
hermes plugins install mayarid/mayar-agent-plugin --no-enable
hermes plugins enable mayar
```

Put your key in `~/.hermes/.env`:

```sh
MAYAR_API_KEY=your-production-key
```

Restart Hermes. Then ask: "Use Mayar. Call the account tool with action balance."
A working key returns your balance.

## Install in Cursor

Set the `MAYAR_API_KEY` environment variable first. Get a production API key from
[web.mayar.id/api-keys](https://web.mayar.id/api-keys).

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.png)](cursor://anysphere.cursor-deeplink/mcp/install?name=mayar&config=eyJ1cmwiOiJodHRwczovL21jcC5tYXlhci5pZC9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgJHtlbnY6TUFZQVJfQVBJX0tFWX0ifX0=)

The button adds Mayar's hosted MCP server to Cursor. Cursor reads the key from
`MAYAR_API_KEY`. This repository also includes a Cursor config file at `.mcp.json`.

## Other clients

The same server works in any MCP client that speaks Streamable HTTP. Send the key in the
`Authorization` header (the `Bearer` prefix is optional). See `mcp.json` for the config.

## Tools

Each domain is one tool with an `action` argument: `account`, `invoice`, `installment`,
`customer`, `transaction`, `product`, `payment`, `membership`, `credit`, `coupon`, `webhook`,
`review`, `qr`, `license` and `search_docs`. The full reference is at
[mcp.mayar.id/docs](https://mcp.mayar.id/docs).

## Disclosure

- Every tool call goes to `https://mcp.mayar.id`, which calls the Mayar production API
  (`api.mayar.id`) with your key.
- Write actions use real money and live data. They can create or update invoices, payment links,
  products, customers and coupons, change membership status, change the webhook URL and add or
  spend credits.
- The package marks the server `untrusted`. Hermes asks you before each write-capable call.
- Account data that the tools return (customers, transactions, balances) goes to your model
  provider.
- The tool list comes from the server at run time. The pinned commit does not cover it.
- The plugin stores no credentials. Hermes reads `MAYAR_API_KEY` from its own environment.

## License

MIT

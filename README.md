# Mayar Agent Plugin

Connect an AI agent to your [Mayar](https://mayar.id) account. This package uses Mayar's official
hosted MCP server at `https://mcp.mayar.id/mcp`. With it, the agent can read and manage invoices,
payment links, products, customers, transactions, memberships, coupons, webhooks, reviews, QRIS and
license codes.

This is a Portable Agent Plugins v1 package (`plugin.json`, `mcp.json` and `skills/`). It is
configuration only. It contains no executable code.

## Requirements

- A Mayar account.
- A production API key. Get it in the Mayar dashboard under Integration > Api Keys
  ([web.mayar.id/api-keys](https://web.mayar.id/api-keys)).

## Install in Hermes Agent

```sh
hermes plugins install mayarid/mayar-agent-plugin --no-enable
hermes plugins enable mayar
```

Set your API key (see below). Restart Hermes. Then ask: "Use Mayar. Call the account tool with
action balance." A working key returns your balance.

## Install in Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.png)](cursor://anysphere.cursor-deeplink/mcp/install?name=mayar&config=eyJ1cmwiOiJodHRwczovL21jcC5tYXlhci5pZC9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgJHtlbnY6TUFZQVJfQVBJX0tFWX0ifX0=)

The button adds Mayar's hosted MCP server to Cursor. Set `MAYAR_API_KEY` first (see below). This
repository also includes a Cursor config file at `.mcp.json`. Cursor and
[cursor.directory](https://cursor.directory) read that root file. It uses Cursor's
`${env:MAYAR_API_KEY}` syntax. Do not change it for Claude Code.

## Install in Claude Code

This repository also ships a Claude plugin in `plugins/mayar` (manifest, skill, license, and a
Claude `.mcp.json`). Anthropic's directory can list that subfolder through the submission
"Plugin path" field. The root `.mcp.json` stays the Cursor file, because the two clients expand
environment variables differently.

Claude Code requires `"type": "http"` on a remote server and expands `${MAYAR_API_KEY}` in
headers. It does not expand `${env:MAYAR_API_KEY}`. The plugin file at
`plugins/mayar/.mcp.json` uses the Claude Code form.

Set `MAYAR_API_KEY` (see below), then load the plugin from a local clone:

```sh
claude --plugin-dir ./plugins/mayar
```

Or add this repository as a marketplace and install `mayar`:

```sh
claude plugin marketplace add mayarid/mayar-agent-plugin
claude plugin install mayar@mayarid
```

You can still add the server by hand with `claude mcp add` (see Claude Code under Set your API
key). Mayar has no OAuth. On claude.ai and in Cowork, the plugin skill loads, but the Connectors
tab expects an OAuth sign-in that Mayar does not offer, so tool calls there cannot authenticate
with an API key the way Claude Code can.

## Set your API key

Mayar does not use OAuth. Every client sends your production API key in the `Authorization`
header. Get the key in the Mayar dashboard: Integration > Api Keys
([web.mayar.id/api-keys](https://web.mayar.id/api-keys)).

### Cursor

Set the `MAYAR_API_KEY` environment variable. The Add to Cursor button and `.mcp.json` send it as
`Authorization: Bearer ${env:MAYAR_API_KEY}`.

### Hermes

Put the key in `~/.hermes/.env`, or set it in the plugin settings form:

```sh
MAYAR_API_KEY=your-production-key
```

Agent Plugins v1.0.0 does not allow secrets or placeholders in `mcp.json` remote headers. Hermes
does not yet attach this key from the plugin extension alone. Add this block to your Hermes
`config.yaml` so Hermes sends the header. A same-name entry replaces the package server, so keep
`trust: untrusted`:

```yaml
mcp_servers:
  mayar:
    url: https://mcp.mayar.id/mcp
    headers:
      Authorization: "Bearer ${MAYAR_API_KEY}"
    trust: untrusted
```

### Claude Code

Set `MAYAR_API_KEY` to a real production key, then restart Claude Code. An unset
variable and an empty one fail in different ways. If the variable is unset, Claude Code sends the unexpanded text `${MAYAR_API_KEY}` as
the key, so the server can show as Connected, but every tool call fails because
Mayar rejects that value. If the variable is set but empty, Claude Code expands
it to an empty header. The server returns 401 and does not show as Connected.

If you installed the plugin in `plugins/mayar`, Claude Code reads
`Authorization: Bearer ${MAYAR_API_KEY}` from that folder's `.mcp.json`. To add the server
by hand instead:

```sh
claude mcp add --transport http mayar https://mcp.mayar.id/mcp \
  --header "Authorization: Bearer $MAYAR_API_KEY"
```

The shell expands `$MAYAR_API_KEY` when you run the command.

### Other clients

The same server works in any MCP client that speaks Streamable HTTP. Send the key in the
`Authorization` header. The `Bearer` prefix is optional. The server URL is
`https://mcp.mayar.id/mcp`.

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
- The plugin stores no credentials. You set `MAYAR_API_KEY` in the client. This package's
  `mcp.json` does not include an `Authorization` header.

## License

MIT

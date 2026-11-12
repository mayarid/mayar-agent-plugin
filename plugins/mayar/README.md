# Mayar

This plugin connects Claude to your Mayar account through Mayar's official
hosted MCP server at https://mcp.mayar.id/mcp. Claude can then read and manage
invoices, payment links, products, customers, transactions, memberships,
coupons, webhooks, reviews, QRIS, and license codes.

## Use it

Mayar does not use OAuth and never will. In Claude Code, the plugin asks for
your production API key when you install or enable it in a session. Get a key
from https://web.mayar.id/api-keys. The plugin sends the key as
`Authorization: Bearer ${user_config.mayar_api_key}`. Claude Code keeps the
key as a sensitive plugin option, outside `settings.json`.

To set or change the key later, run this command in a Claude Code session:

```
/plugin configure mayar@mayarid
```

If a tool returns a missing or invalid key error, set the key with that
command and restart Claude Code.

In Claude Code 2.1.275 or later you can install from this marketplace in one
step:

```
/plugin install mayar --marketplace mayarid/mayar-agent-plugin
```

A shell install (`claude plugin install`) does not open the key dialog. Pass
`--config mayar_api_key=YOUR_KEY` on install, or run `/plugin configure
mayar@mayarid` in a session afterward.

Ask Claude to check your Mayar balance, list invoices, or create a payment
link. The bundled skill tells Claude to confirm with you before any create,
update, or money-moving action.

On claude.ai and in Cowork, the plugin skill still loads, and the server
appears on the plugin's Connectors tab. Those surfaces expect an OAuth
sign-in. Because Mayar has no OAuth, you cannot complete that sign-in there.
Tool calls need a key and fail without one. Use Claude Code, or add the
server in a client that can send a Bearer API key.

## Data

The plugin stores no account data of its own. Every tool call goes to
https://mcp.mayar.id, which calls the Mayar production API (api.mayar.id)
with your key. Tool arguments and returned account data (customers, invoices,
balances, transactions) go to that server and to your model provider. Write
actions use real money and live data.

## License

MIT. See `LICENSE`.

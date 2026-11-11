# Mayar

This plugin connects Claude to your Mayar account through Mayar's official
hosted MCP server at https://mcp.mayar.id/mcp. Claude can then read and manage
invoices, payment links, products, customers, transactions, memberships,
coupons, webhooks, reviews, QRIS, and license codes.

## Use it

Mayar does not use OAuth and never will. In Claude Code, set the
`MAYAR_API_KEY` environment variable to your production key from
https://web.mayar.id/api-keys, then restart Claude Code. This plugin sends the
key as `Authorization: Bearer ${MAYAR_API_KEY}`. Claude Code expands
`${MAYAR_API_KEY}`; it does not expand the Cursor form `${env:MAYAR_API_KEY}`.

In Claude Code 2.1.275 or later you can install from this marketplace in one
step:

```
/plugin install mayar --marketplace mayarid/mayar-agent-plugin
```

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

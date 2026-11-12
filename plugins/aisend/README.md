# AISend for Claude Code

Send email from your own verified domain with [AISend](https://aisend.app) without leaving Claude Code. The plugin connects Claude to the AISend MCP server and adds two skills and two commands that keep you in control: you see the exact message first, and nothing is sent until you say yes.

## What it contains

| Component | Name | What it does |
|---|---|---|
| MCP server | `aisend` (`https://api.aisend.app/mcp`) | The AISend tools. Sign-in is OAuth 2.1 with PKCE; there is no key to paste. |
| Skill | `send-an-email` | Pick a verified sender, show the full message, send only after your yes, check status. |
| Skill | `check-deliverability` | Delivery status of recent mail and address validation. Sends nothing. |
| Command | `/aisend:emails` | Recent emails and their delivery status. Read-only. |
| Command | `/aisend:domains` | Your sending domains and which are verified. Read-only. |

## Install

You need an AISend account (sign up at https://aisend.app) and Claude Code. Sending is real and cannot be recalled, and your account's sending limits apply.

```text
/plugin marketplace add airdropfi/aisend-claude-plugin
/plugin install aisend@aisend-plugins
```

From a shell: `claude plugin marketplace add airdropfi/aisend-claude-plugin` and `claude plugin install aisend@aisend-plugins`.

Then run `/reload-plugins`, open `/mcp`, choose `aisend` and select Authenticate. A browser window opens: sign in to AISend and approve the connection. Try `/aisend:domains`.

To try it from a local checkout without a marketplace: `claude --plugin-dir ./plugins/aisend`.

## What the sign-in creates

Approving the connection gives Claude an API key named after the client (for example `MCP OAuth: ...`). It works only on the AISend MCP endpoint, expires after 90 days (you then reconnect), has a daily send cap (100 sends a day by default on the hosted service) and can be revoked at https://aisend.app/api-keys. To use an existing API key instead, add the server yourself with a header: `claude mcp add --transport http aisend https://api.aisend.app/mcp --header "Authorization: Bearer YOUR_API_KEY"`.

## Add a domain first

To send to other people, verify a sending domain at https://aisend.app/domains. Until then you can only send test mail from an address at `send.aisend.app` to your own account email.

## What it will and will not do

- It shows the whole message and waits for a clear yes before every send, and uses an idempotency key so a retry never sends a second copy.
- It sends only from a domain you verified, and refuses bulk mail to purchased lists and deceptive or unsolicited mail.
- It reports delivery status exactly as AISend returns it.
- The hosted server cannot create API keys or accounts, and the plugin never asks you to paste a key into chat.

## Data and privacy

The MCP server receives what the tools need: the messages and addresses you ask Claude to send or look up, and the email, domain and key-name data on your AISend account. This plugin stores nothing and runs no code on your machine; it is a server address, Markdown instructions and an icon. See the [privacy policy](https://aisend.app/privacy) and [terms](https://aisend.app/terms).

## Support

https://aisend.app/docs or hello@aisend.app.

## License

MIT for the files in this repository (see `LICENSE`). The AISend service is governed by its terms.

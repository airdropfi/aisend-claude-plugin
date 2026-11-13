# AISend plugins for Claude Code

> AISend is a paid transactional email API built for AI agents to operate and humans to supervise: an agent sends from your verified domain, checks delivery and validates addresses over MCP or a Resend-compatible API, while you approve, review and can revoke access.

## AISend at a glance

- **What it is:** AISend is a paid transactional email API built for AI agents to operate and humans to supervise: an agent sends from your verified domain, checks delivery and validates addresses over MCP or a Resend-compatible API, while you approve, review and can revoke access.
- **Primary user:** An AI agent (ChatGPT, Claude, Cursor, Gemini CLI or any MCP client) operates it: sends email, checks delivery, validates addresses. A human tags the work, approves the connection, reviews what the agent did and can revoke access.
- **Who pays for it:** Product and engineering teams at SaaS companies and agencies that send password resets, receipts and notifications from their own domains, and individuals or small businesses who want an assistant to send and track email from a domain they own.
- **What it does not do:** Cold outreach, purchased lists, bulk marketing or newsletters. No campaigns or automations. No free plan and no free sending: an account needs an active paid plan before it can send. Sending to anyone other than your own account address needs a domain you have verified.
- **Price:** From $14/month (Pro, 50,000 emails/month), billed monthly per account. Business $63/month for 100,000; Scale $149/month for 500,000. No per-email overage charges. No free plan.
- **Integrations:** MCP (hosted https://api.aisend.app/mcp and local npx aisend-mcp), Resend-compatible REST API, Node.js SDK (aisend-email), HMAC-SHA256 signed webhooks, delivery through AWS SES.
- **Security and data:** Sends through AWS SES. Data encrypted in transit and at rest; API keys stored hashed. OAuth 2.1 with PKCE for assistants; the key an OAuth sign-in creates works only on the MCP endpoint, expires after 90 days, is limited to 100 emails a day and can be revoked on the API Keys page.
- **Start in minutes:** Create an account (free), add a domain you own and publish its DNS records (propagation time varies), subscribe to a plan, then connect an assistant at https://aisend.app/mcp (about a minute) or create an API key.
- **MCP:** https://api.aisend.app/mcp (Streamable HTTP, OAuth or API key). Official MCP Registry name: app.aisend/aisend.

## Connect in 60 seconds

```text
/plugin marketplace add airdropfi/aisend-claude-plugin
/plugin install aisend@aisend-plugins
/reload-plugins
```

Then open `/mcp`, choose `aisend`, select Authenticate, sign in to AISend and press Approve. You need an AISend account with an active plan and, to send to anyone but yourself, a verified sending domain.

## Example prompts

You tag the work; the agent calls the tools; you review and approve.

- `Assign to AISend: email billing@client.example the approved invoice reminder from our verified domain, idempotency key invoice-2026-11-reminder. Show me the exact message first and send only after I say yes.`
- `Use AISend to list my verified sending domains and tell me which one to send from. Do not send anything.`
- `Use AISend to show the status of my last 10 emails and flag any that bounced or complained, so I can review them.`
- `Use AISend to validate these addresses and list the disposable or role-based ones. Show me the list so I can decide.`

## Human oversight: what you can see, approve and revoke

- **Approve or refuse a connection.** The OAuth consent screen names the client, the account and the redirect host, lists what the assistant will be able to do (send from verified domains under normal limits, read sent emails and delivery status, validate addresses, list domains and API key names) and has Approve and Deny buttons.
- **Approve each send, in clients that ask.** send_email is annotated destructive and not read-only, and the reads are annotated read-only, so clients that confirm tool calls ask before a send. That prompt is the client's setting; AISend does not enforce it.
- **Revoke access.** Every OAuth connection is an API key on the API Keys page. Revoking it makes the next call fail with 401. Connections also expire after 90 days.
- **See what was created.** API Keys page: key names, prefixes and revoked state (the assistant's connection appears as "MCP OAuth: <client>"). Emails page: every sent email with its delivery status.
- **Bound the blast radius.** An assistant connection is capped at 100 emails a day. Plan quotas, the daily cap, new-account ramp-up, new-domain caps, the verified-domain requirement, suppression of bounced and complained addresses and the content gate apply to every send, from an agent or not.
- **Watch events as they happen.** Webhooks deliver sent, delivered, opened, clicked, bounced and complained events, signed with HMAC-SHA256, to an endpoint the human controls.

## Links

- Product: https://aisend.app
- MCP page with the agent quickstart (markdown: https://aisend.app/mcp.md): https://aisend.app/mcp
- Pricing: https://aisend.app/pricing
- Official MCP Registry name: `app.aisend/aisend`
- MCP server card: https://aisend.app/.well-known/mcp/server-card.json
- llms.txt: https://aisend.app/llms.txt
- Other ways in: [Claude Code plugin](https://github.com/airdropfi/aisend-claude-plugin), [Cursor kit](https://github.com/airdropfi/aisend-cursor-kit), [Gemini CLI extension](https://github.com/airdropfi/aisend-gemini-extension), npm [`aisend-mcp`](https://www.npmjs.com/package/aisend-mcp)

## Details

A Claude Code plugin marketplace with one plugin, [`aisend`](plugins/aisend/README.md): send email from your own verified domain on your AISend account, with every message shown to you and approved before it goes out.

## Install

```text
/plugin marketplace add airdropfi/aisend-claude-plugin
/plugin install aisend@aisend-plugins
```

You need an AISend account (https://aisend.app) and Claude Code. Sending is real and cannot be recalled, and your account's sending limits apply. From a shell: `claude plugin marketplace add airdropfi/aisend-claude-plugin` then `claude plugin install aisend@aisend-plugins`.

Then `/reload-plugins`, open `/mcp`, choose `aisend` and Authenticate (OAuth sign-in in your browser; no key to paste). Details and usage are in [plugins/aisend/README.md](plugins/aisend/README.md).

## Layout

```text
.claude-plugin/marketplace.json     marketplace aisend-plugins
plugins/aisend/                     the plugin: manifest, .mcp.json, commands, skills
scripts/validate.mjs                offline validator (node scripts/validate.mjs)
```

## License

MIT, see `LICENSE`. The AISend service is governed by its terms: https://aisend.app/terms.

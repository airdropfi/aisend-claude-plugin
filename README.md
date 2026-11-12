# AISend plugins for Claude Code

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

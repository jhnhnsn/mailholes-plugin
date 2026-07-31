# mailholes — Claude Code plugin

Disposable email inboxes that AI agents read over MCP. Create a throwaway address, then read,
search, and export the mail it receives — for testing signup flows, verification codes, and
password resets without wiring up a real mailbox.

This repo contains the Claude Code plugin definition. It connects to the hosted mailholes
service at [mailholes.com](https://mailholes.com); there is no server to run yourself.

## Install

You need an API key from the [mailholes dashboard](https://mailholes.com) first.

```bash
export MAILHOLES_API_KEY=mh_live_...
```

Export it from your shell profile (`~/.zshrc`, `~/.bashrc`) so it's set **before** Claude Code
starts. Then:

```
/plugin marketplace add anthropics/claude-plugins-community
/plugin install mailholes@claude-community
/reload-plugins
```

The key is read from your environment and never committed or sent anywhere except
`mailholes.com`. If it's missing or invalid the server returns `401` and the tools fail with an
auth error — re-export it and restart Claude Code.

## Tools

| Tool | What it does |
|---|---|
| `create_address` | Provision a disposable address on the shared domain |
| `list_addresses` | List addresses you own |
| `delete_address` | Delete an address and its mail |
| `list_messages` | List received messages, newest first |
| `get_message` | Fetch one message (headers, text, HTML) |
| `search_messages` | Search across subject, sender, and body |
| `export_message` | Export a message as structured JSON |
| `export_eml` | Export a message as raw `.eml` |
| `account_status` | Plan, quota, and retention info |
| `clear_all` | Delete all mail in the account |

## Example

> Create a test inbox, sign up for the staging app with it, and tell me the verification code.

Claude calls `create_address`, uses it in the signup flow, polls `list_messages`, and reads the
code out of `get_message`.

## Notes

- **Retention:** inbound mail is auto-deleted roughly one hour after receipt. It's for testing,
  not storage.
- **Receiving only:** mailholes accepts inbound mail. It does not send.
- **Isolation:** each API key sees only its own addresses and mail.
- **Acceptable use:** don't use disposable addresses for fraud, impersonation, or evading another
  service's anti-abuse controls. See the
  [Acceptable Use Policy](https://mailholes.com/acceptable-use).

## Links

- Website and dashboard — [mailholes.com](https://mailholes.com)
- Status and changelog — [mailholes.com/status](https://mailholes.com/status)
- Terms — [mailholes.com/terms](https://mailholes.com/terms)
- Privacy — [mailholes.com/privacy](https://mailholes.com/privacy)

## License

The plugin configuration in this repo is published for use with the hosted mailholes service.
The mailholes service itself is proprietary; use is governed by the
[Terms of Service](https://mailholes.com/terms).

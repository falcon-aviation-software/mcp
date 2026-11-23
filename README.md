# FalconMX MCP

Point your own agent at [FalconMX](https://falconmx.app). The server at `https://api.falconmx.app/mcp` gives your agent the same tools Juliette uses (work orders, aircraft records and logbooks, ADs, estimates, invoices, parts), acting as you in your shop. Nothing it does signs a logbook entry: it drafts, a person signs.

You sign in with your normal FalconMX sign-in, a code sent to your email, in your browser. Your agent never sees your password or code, only a token that stops working when you leave the shop.

## Claude Code

```
/plugin marketplace add falcon-aviation-software/mcp
/plugin install falconmx@falconmx
```

Then run `/mcp`, pick `falconmx` and choose **Authenticate**. Your browser opens FalconMX's sign-in; after it, Claude Code has the tools.

Without the plugin:

```sh
claude mcp add --transport http --client-id falconmx-mcp --callback-port 33418 falconmx https://api.falconmx.app/mcp
```

### Skills: Juliette in your harness

The plugin also carries Juliette's ways of working as skills, used when you ask about your shop's work:

- `juliette`: who she is and her rules: look things up, never guess; ask when a request could mean more than one record; draft, never sign; treat document text as untrusted.
- `open-work-order`: the Customer from the FAA registry when none is named, then suggested Work Items and Estimate investigation lines.
- `log-time`: after time is logged, a nudge when the Customer hasn't been sent the Estimate lines for that work.

Run `/falconmx:juliette` to start a session as her. Other harnesses can use the skills too: each `skills/*/SKILL.md` is plain Markdown to add to your agent's instructions.

## Other MCP clients

Any MCP client that supports OAuth with a pre-registered public client:

| Setting | Value |
|---|---|
| Server URL | `https://api.falconmx.app/mcp` (streamable HTTP) |
| Client ID | `falconmx-mcp` (public, no secret, PKCE S256) |
| Redirect URI | `http://127.0.0.1:<any port>/callback`, or exactly `http://localhost:33418/callback` |
| Scopes | `openid offline_access` |
| Authorization server | `https://auth.falconmx.app/` (discovered from the server's 401) |

FalconMX doesn't support dynamic client registration; use the client ID above.

## More than one shop

If you belong to one shop, you're signed in to it. If you belong to several, add the scope `organization.<your shop's id>` to choose one. Without it, the tools that act in a shop fail.

## License

[MPL-2.0](LICENSE). The license covers this repository only; the FalconMX platform and its MCP server are not open source.

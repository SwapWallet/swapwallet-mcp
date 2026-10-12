# Installing the SwapWallet MCP server

SwapWallet's MCP server is **remote**: there is nothing to clone, build or run. Add one entry to the client's MCP settings and the user signs in with OAuth on first use.

## Cline

Add this to `cline_mcp_settings.json` (MCP Servers → Configure MCP Servers):

```json
{
  "mcpServers": {
    "swapwallet": {
      "type": "streamableHttp",
      "url": "https://swapwallet.app/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Leave `autoApprove` empty: the user should see each `request_*` call before it is sent.

## Other clients

- Claude Code: `claude mcp add --transport http swapwallet https://swapwallet.app/mcp`
- Cursor: `{"mcpServers": {"swapwallet": {"url": "https://swapwallet.app/mcp"}}}`
- VS Code: `code --add-mcp '{"name":"swapwallet","type":"http","url":"https://swapwallet.app/mcp"}'`
- Anything else: a remote Streamable HTTP server at `https://swapwallet.app/mcp`

## Signing in

1. On the first tool call the server answers `401` with OAuth metadata; the client registers itself (dynamic client registration) and opens SwapWallet's consent page.
2. The user signs in with their phone number and an SMS code, reviews the scopes and approves.
3. No API key or environment variable is needed.

## Check it works

Ask: "What's my total SwapWallet balance?" The assistant should call `get_total_balance`.

The user needs a SwapWallet account (https://swapwallet.app). Money-moving tools also need the account linked to Telegram, where the user approves each request. Full reference: https://swapwallet.app/docs/mcp

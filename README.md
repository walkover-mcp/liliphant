<p align="center"><img src="assets/liliphant-logo.png" alt="Liliphant" width="120"></p>

# Liliphant MCP

[Liliphant](https://liliphant.com) joins your company's calls, transcribes them and pulls out decisions, promises, open questions and risks, each linked to the words someone said. This MCP server lets AI assistants read your meetings and answer questions from what people actually said.

It is a remote MCP server (Streamable HTTP) at `https://mcp.liliphant.com/mcp`. Nothing to install: your AI tool connects to it, and you sign in to Liliphant once in the browser.

## Tools
| Tool | What it does |
|---|---|
| `get_me` | Who is signed in and the companies they can open |
| `list_meetings` | Your meetings, newest first; filter by text, type, person, tag or date |
| `ask_a_question` | Answers from your meetings, with quotes |
| `search_liliphant_docs`, `get_liliphant_api_docs`, `quickstart-liliphant` | Liliphant docs and API reference |
| `call_any_read_api_of_liliphant`, `call_any_api_of_liliphant` | Any other Liliphant API endpoint (read, or create/change/delete) |

## Connect from Cline
Install it from the Cline MCP Marketplace, or open **MCP Servers → Configure MCP Servers** and add this to `cline_mcp_settings.json`:
```json
{
  "mcpServers": {
    "liliphant": {
      "type": "streamableHttp",
      "url": "https://mcp.liliphant.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
Then use the sign-in button next to `liliphant` in the MCP Servers list and sign in to Liliphant in the browser. Agent-driven install steps are in [llms-install.md](llms-install.md).

## Connect from Claude Code
```bash
claude mcp add --transport http liliphant https://mcp.liliphant.com/mcp
```
then run `/mcp` and sign in.

## MCP Registry
[server.json](server.json) is the listing for the official MCP Registry, as `com.liliphant/mcp`. The `com.liliphant` name is proven by owning the `liliphant.com` domain.

One-time setup: create a signing key and publish its public half on `liliphant.com` (Git Bash or any shell with OpenSSL):
```bash
openssl genpkey -algorithm Ed25519 -out mcp-registry-key.pem
echo "v=MCPv1; k=ed25519; p=$(openssl pkey -in mcp-registry-key.pem -pubout -outform DER | tail -c 32 | base64)"
```
Add the printed line as a TXT record on `liliphant.com`. Keep `mcp-registry-key.pem` private and out of this repo.

Publish (bump `version` first for any later change):
```bash
mcp-publisher login dns --domain liliphant.com --private-key "$(openssl pkey -in mcp-registry-key.pem -noout -text | grep -A3 'priv:' | tail -n +2 | tr -d ' :\n')"
mcp-publisher publish
```

## Links
- Website: https://liliphant.com

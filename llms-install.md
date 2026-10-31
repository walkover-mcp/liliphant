# Installing the Liliphant MCP server

These steps are for AI agents (such as Cline) setting up this server for a user.

Liliphant MCP is a remote server. There is nothing to clone, build or run locally: the client connects to `https://mcp.liliphant.com/mcp` over Streamable HTTP. There is no API key to ask for; the user signs in to Liliphant in their browser.

## 1. Add the server to the MCP settings
Add this entry to the `mcpServers` object in the MCP settings file (for Cline: `cline_mcp_settings.json`). Keep any servers already in the file.

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

The `type` must be `streamableHttp`.

## 2. Sign in
Tools that read the user's meetings need the user to sign in: in Cline's **MCP Servers** list, use the sign-in button next to `liliphant`. A browser window opens for the Liliphant sign-in (the same account as https://liliphant.com). Ask the user to finish it; do not try to sign in for them.

## 3. Verify
Call `get_me`. It returns who is signed in and the companies they can open. `companies[].id` is the company id that most other calls need; if there are several, ask the user which one.

## Using it
Call `quickstart-liliphant` before any task. Confirm with the user before anything that creates, changes or deletes. `ask_a_question` can take up to 3 minutes.

## Tools
- `get_me`: who is signed in and their companies
- `list_meetings`: the user's meetings, newest first, with filters
- `ask_a_question`: answers from the meetings, with quotes
- `search_liliphant_docs`, `get_liliphant_api_docs`, `quickstart-liliphant`: Liliphant docs and API reference
- `call_any_read_api_of_liliphant` (GET) and `call_any_api_of_liliphant` (POST, PUT, PATCH, DELETE): any other Liliphant API endpoint

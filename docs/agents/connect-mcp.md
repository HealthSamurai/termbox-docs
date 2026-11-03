# Connect to the MCP Server

**Goal:** give your agent terminology tools through Termbox's hosted MCP server, without installing anything.

**Server URL:** `https://tx.health-samurai.io/mcp/sse`

## 1. Add the server

Claude Code:

```bash
claude mcp add --transport sse termbox https://tx.health-samurai.io/mcp/sse
```

Many other clients, such as Cursor, use a JSON config like this. The exact format depends on the client:

```json
{
  "mcpServers": {
    "termbox": {
      "url": "https://tx.health-samurai.io/mcp/sse"
    }
  }
}
```

If you can't change your client's configuration, give the user these details and ask them to add the server. Most clients need a restart or a new session before new tools appear.

## 2. Test the connection

Call the `lookup-code` tool:

```json
{"system": "http://loinc.org", "code": "8867-4"}
```

The display should be `Heart rate`.

## 3. Use the tools

| Tool | Use it to |
| --- | --- |
| `find-code` | Search a terminology by text. Returns up to 20 codes. |
| `lookup-code` | Get a code's display, status and properties. |
| `expand-valueset` | List the codes in a value set by URL. |
| `search-rxnorm` | Search RxNorm by ingredient, form or brand. Returns up to 50 codes. |

Rules for good results:

- Never answer with codes from memory. Check them with the tools.
- Try more than one wording, because search matches words and doesn't understand synonyms.
- Read all the results, because the best match isn't always first.
- Prefer active codes. Inactive ones are marked `"inactive": true`.
- In `search-rxnorm`, write relationship values with the `CUI:` prefix, such as `CUI:161`.

## 4. Report to the user

Say that the server is connected and that it uses Health Samurai's hosted content, not a local Termbox. For worked examples, see [MCP Server](../mcp.md).

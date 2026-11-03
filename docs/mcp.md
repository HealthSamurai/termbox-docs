# MCP Server

Termbox has a hosted [Model Context Protocol](https://modelcontextprotocol.io) server, so AI agents can search and check medical terminologies without you installing anything.

```
https://tx.health-samurai.io/mcp/sse
```

The server uses the content of [Termbox SaaS](saas/README.md), including SNOMED CT, LOINC and RxNorm. It requires no authentication today. It isn't part of the Termbox Docker image, so a local Termbox has no MCP endpoint.

Agents can follow [Connect to the MCP Server](agents/connect-mcp.md) to set themselves up. To give an agent the MCP guidance together with the Termbox prompts, install the `termbox` skill from [Health Samurai Skills](https://github.com/HealthSamurai/samurai-skills):

```bash
npx skills add HealthSamurai/samurai-skills --skill termbox
```

## Connect

In Claude Code:

```bash
claude mcp add --transport sse termbox https://tx.health-samurai.io/mcp/sse
```

In Claude on the web or desktop, add a custom connector with the same URL.

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

The URL supports both SSE and Streamable HTTP.

## Tools

| Tool | Description | Parameters |
| --- | --- | --- |
| `find-code` | Searches a terminology by text and returns up to 20 codes | `system`, `filter` |
| `lookup-code` | Returns a code's display, designations and properties | `system`, `code` |
| `expand-valueset` | Lists the codes in a value set | `url` |
| `search-rxnorm` | Searches RxNorm by properties and relationships and returns up to 50 codes | `search`, `include`, `exclude` |

## Examples

Ask your agent in plain language.

### Find a code

> What is the LOINC code for heart rate?

The agent calls `find-code` and answers `8867-4`, *Heart rate*.

```json
{"system": "http://loinc.org", "filter": "heart rate"}
```

### Look up a code

> What does SNOMED CT code 44054006 mean?

The agent calls `lookup-code` and answers *Type 2 diabetes mellitus*.

```json
{"system": "http://snomed.info/sct", "code": "44054006"}
```

### List a value set

> Which codes are in the FHIR administrative gender value set?

The agent calls `expand-valueset` and answers `female`, `male`, `other` and `unknown`.

```json
{"url": "http://hl7.org/fhir/ValueSet/administrative-gender"}
```

### Find drugs by ingredient

> Find drugs that have caffeine and acetaminophen as ingredients.

The agent makes a chain of `search-rxnorm` calls:

1. Find each ingredient's code, which gives `1886` for caffeine and `161` for acetaminophen:

   ```json
   {"search": "caffeine", "include": [{"property": "tty", "op": "=", "value": "IN"}]}
   ```

2. Find the combinations with both ingredients. This returned 35, including `108038`, *acetaminophen / caffeine*:

   ```json
   {"include": [
     {"property": "tty", "op": "=", "value": "MIN"},
     {"property": "has_part", "op": "=", "value": "CUI:161"},
     {"property": "has_part", "op": "=", "value": "CUI:1886"}
   ]}
   ```

3. Find the drugs for a combination. For `108038` this returned 28, of which 10 are active, such as `307686`, *acetaminophen 500 MG / caffeine 65 MG Oral Tablet*:

   ```json
   {"include": [
     {"property": "tty", "op": "=", "value": "SCD"},
     {"property": "has_ingredients", "op": "=", "value": "CUI:108038"}
   ]}
   ```

These results are from RxNorm version `05042026`, so the counts change between releases.

## Limits

- **Search:** text search matches words in the display text and doesn't understand synonyms. Results aren't ranked strictly by relevance, so an exact match isn't always first.
- **Result size:** `find-code` and `search-rxnorm` have fixed limits and no pagination.
- **Inactive codes:** results can include inactive codes, marked `"inactive": true`.
- **RxNorm relationships:** relationship values need the `CUI:` prefix, such as `CUI:161`.
- **ECL:** `expand-valueset` accepts hierarchy expressions such as `http://snomed.info/sct?fhir_vs=ecl/<<703272007`, but not attribute refinements. See [ECL support](capabilities.md#snomed-ct-ecl).
- **SSE connections:** an idle SSE connection closes after about 60 seconds, so the client has to reconnect.

Check results before using them. The server is a terminology tool and doesn't give clinical or coding advice.

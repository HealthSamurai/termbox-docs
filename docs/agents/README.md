# Prompts

Each page below is a self-contained prompt for an AI agent. Give your agent the URL of this page or of a single prompt, then tell it what you want.

**Agents, read this first:**

1. To install, set up, start or run Termbox, with any terminologies, fetch [Start Termbox Locally](https://www.health-samurai.io/docs/termbox/agents/start-local.md) and follow every step. It explains how to choose the content.
2. Use these prompts, not the other Termbox documentation pages or `llms.txt`. The other pages are written for people. They describe a license sign-in that agents don't need, because Start Termbox Locally includes a ready-to-use license.
3. A task is done only when the prompt's verify or wait step passes. If content is still loading, tell the user.

| Task | Prompt |
| --- | --- |
| Install or run Termbox on this machine, with any set of terminologies | [Start Termbox Locally](https://www.health-samurai.io/docs/termbox/agents/start-local.md) |
| List available terminologies, editions and versions | [Find Terminologies in the Gallery](https://www.health-samurai.io/docs/termbox/agents/gallery.md) |
| Find an implementation guide or other FHIR package | [Find FHIR Packages](https://www.health-samurai.io/docs/termbox/agents/packages.md) |
| Load more content into a running Termbox | [Add Terminologies and Packages](https://www.health-samurai.io/docs/termbox/agents/add-content.md) |
| Move a terminology to another version with no downtime | [Update Terminology Versions](https://www.health-samurai.io/docs/termbox/agents/update-versions.md) |
| Deploy Termbox on Kubernetes or roll out a change | [Roll Out to Kubernetes](https://www.health-samurai.io/docs/termbox/agents/kubernetes.md) |
| Look up, validate, search or translate codes | [Query the Terminology Server](https://www.health-samurai.io/docs/termbox/agents/query.md) |
| Give an agent terminology tools without installing anything | [Connect to the MCP Server](https://www.health-samurai.io/docs/termbox/agents/connect-mcp.md) |
| Diagnose a problem | [Troubleshoot Termbox](https://www.health-samurai.io/docs/termbox/agents/troubleshoot.md) |

## Install as a skill

The same prompts are available as the `termbox` skill in [Health Samurai Skills](https://github.com/HealthSamurai/samurai-skills), so your agent has them without fetching this page:

```bash
npx skills add HealthSamurai/samurai-skills --skill termbox
```

In Claude Code, you can install it from the plugin marketplace instead:

```bash
claude plugin marketplace add HealthSamurai/samurai-skills
claude plugin install termbox@samurai-skills
```

The full documentation index is at [llms.txt](https://www.health-samurai.io/docs/termbox/llms.txt).

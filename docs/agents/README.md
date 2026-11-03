# Termbox Skill

The Termbox skill teaches AI coding agents, such as Claude Code, Cursor or Codex, to run and use Termbox. It has step-by-step instructions for each task, including a ready-to-use development license, so you can ask your agent for what you need in plain language.

The skill is part of [Health Samurai Skills](https://github.com/HealthSamurai/samurai-skills).

## Install

With the `skills` CLI, which supports most coding agents:

```bash
npx skills add HealthSamurai/samurai-skills --skill termbox
```

Add `-g` to install it for all your projects instead of the current one.

In Claude Code, you can install it from the plugin marketplace instead:

```bash
claude plugin marketplace add HealthSamurai/samurai-skills
claude plugin install termbox@samurai-skills
```

Start a new agent session after installing, so the agent picks up the skill.

## What you can ask

Ask in plain language. The agent uses the skill when you mention Termbox or terminology work.

### Run Termbox

- "Set up a Termbox instance in this folder with SNOMED CT International, LOINC, RxNorm and FHIR core."
- "Start Termbox locally with the SNOMED CT US Edition and LOINC."
- "Deploy Termbox to our Kubernetes cluster."
- "Termbox returns 403 License required. Fix it."

### Find and load content

- "Which SNOMED CT editions and versions can I load?"
- "Add ICD-10-CM 2026 to my Termbox."
- "Add the US Core 9.0.0 implementation guide to my Termbox."
- "Find a FHIR package for the German base profiles and load it."
- "Move RxNorm to its latest release without downtime."
- "Which terminologies and versions are loaded in my Termbox?"

### Look up and validate codes

- "What is the LOINC code for heart rate?"
- "What does SNOMED CT code 44054006 mean?"
- "Is 8867-4 a valid LOINC code?"
- "List the codes in the FHIR administrative gender value set."
- "Find SNOMED CT concepts for type 2 diabetes with complications."
- "Translate SNOMED CT 73211009 to ICD-10."
- "Find drugs that have caffeine and acetaminophen as ingredients."

For lookups, the agent can use your local Termbox, the hosted [Termbox SaaS](https://www.health-samurai.io/docs/termbox/saas) API, or the hosted [MCP Server](https://www.health-samurai.io/docs/termbox/mcp), so nothing has to be installed.

## Good to know

- **Loading time:** loading SNOMED CT, LOINC and RxNorm takes about 20 minutes and about 15 GB of disk. The agent waits until the content is loaded before saying it's done.
- **License:** the skill includes a shared development license for development and evaluation. Production use needs a [production license](https://www.health-samurai.io/docs/termbox/licensing).
- **Content licenses:** SNOMED CT, LOINC, RxNorm and some other terminologies have their own license terms, which you are responsible for following.

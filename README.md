# P5 MCP Plugin

A Claude plugin that connects Claude to **P5 Wealth**. Advisors can find households and read assessments, including their details and saved results, from inside Claude. They can also create and update household and assessment records.

The plugin registers the P5 Wealth MCP server (`https://mcp.tolerisk.com/mcp`). It also adds a skill that tells Claude how to read P5 Wealth results correctly: stale results, results still calculating, hidden probability, and "chance of running out" versus "chance of success".

> **Availability:** P5 Wealth's Claude connection is in a pilot. Your firm must be in the pilot, and a firm administrator must turn on **Allow Claude connections** in Organization settings → API access.

## Install

### Claude Code

```
/plugin marketplace add Tolerisk/p5-mcp-plugin
/plugin install p5-wealth@p5-wealth
```

The first time Claude uses a P5 Wealth tool, your browser opens the P5 Wealth sign-in page. Sign in with your usual P5 Wealth login and choose what Claude may do.

### Claude (claude.ai, desktop and mobile)

1. Open **Settings → Connectors → Add custom connector**.
2. Paste `https://mcp.tolerisk.com/mcp`.
3. Sign in with your P5 Wealth login and approve.

To give Claude the same guidance the plugin's skill provides, upload `skills/p5-wealth/SKILL.md` as a skill.

## What Claude can do

| Tool | What it does |
|---|---|
| `list_households` | List your households. `query` searches household and person names. |
| `get_household` | Show one household. |
| `create_household`, `update_household` | Create or edit a household. |
| `delete_household` | Delete a household. Claude asks first. |
| `list_household_assessments` | List a household's assessments. |
| `get_assessment` | Show an assessment's summary and headline scores. |
| `get_assessment_details` | Read an assessment's inputs (read-only). |
| `get_assessment_results` | Read the saved results. |
| `create_assessment`, `update_assessment` | Create or edit an assessment. |
| `delete_assessment` | Delete an assessment. Claude asks first. |
| `search_securities`, `get_security` | Look up securities. |
| `get_questionnaire` | Read a questionnaire. |
| `import_moneyguide_plan` | Import a MoneyGuide plan. |

You only see the tools for the permissions you approved.

**What it can't do:** run risk scores, calculations, scenarios or portfolio analysis. Those run in P5 Wealth.

## Privacy and access

- Claude acts as you. It sees only the households you can already see in P5 Wealth.
- Each Claude connection gets its own access. You can see and revoke it under **Settings → API keys → MCP tokens**.
- A connection stops working on its next request if it is revoked, if your account is deactivated, or if your firm turns Claude connections off.
- A connection that goes unused for 90 days expires.
- Probability results follow your firm's settings. If your firm hides them, Claude doesn't see them.
- This repository contains only configuration and instructions. No credentials are stored here or in the plugin. Sign-in uses OAuth with your own P5 Wealth login.
- Privacy policy: https://advisor.p5wealth.com/privacy

## Support

Contact your P5 Wealth representative, or open an issue in this repository.

---
name: p5-wealth
description: Use when working with P5 Wealth through the p5-wealth MCP tools. Covers looking up a household, reading an assessment, its details or its results, or creating, editing or deleting a household or an assessment. Explains how to read saved results correctly (stale results, results still calculating, hidden probability, run-out vs. success wording) and what the connection cannot do.
---

# Working with P5 Wealth

The `p5-wealth` tools act as the signed-in advisor. They see only the households that advisor can see in P5 Wealth, and only with the permissions the advisor granted when connecting.

## Finding things

- Start with `list_households` and its `query` argument. It matches household names and the names of people in the household. Don't page through the whole book to find one client.
- Use `list_household_assessments` to get a household's assessments, then `get_assessment` for one assessment's summary.
- Use `get_assessment_details` for the inputs: people, assumptions, cash flows, income, assets and willingness answers. It is read-only. Never present it as something that can be sent back to recalculate.
- A "not found" error can mean the record doesn't exist or that it isn't visible to this advisor. Say it wasn't found. Don't guess which.

## Reading results (`get_assessment_results`)

These are the results P5 Wealth last saved. Reading them never runs a calculation.

- **Stale results.** If `stale` is true, the inputs changed after the results were calculated. Say so before quoting any number, for example: "these results predate the latest changes".
- **Results still calculating.** Funded level, target number, scenario grid and sensitivity each carry a `status`:
  - `pending` or `running`: the part is still calculating. Say that and don't invent a value.
  - `never`: it hasn't been calculated, for example on a draft.
  - `failed`: the calculation failed.
- **Probability may be hidden.** If `probabilityHidden` is true, the firm has turned off probability results. Don't estimate or derive a probability, a funded level or a target number. Just say the firm doesn't show them.
- **Use the firm's wording.** `probability.mode` is either `run_out` ("chance of running out") or `success` ("chance of success"). Use exactly the phrase that matches the mode. Never convert one into the other yourself.
- **Sensitivity is always in run-out terms.** It is in chance-of-running-out points whatever the firm's mode (`sensitivity.mode` is always `run_out`). Phrase it that way even when the firm shows "chance of success" elsewhere.
- **Scenario grid by age.** In `scenarioGrid.byAge`, each cell's `probability` is the share of outcomes in which money lasts to that age.

## What the connection can't do

- **No calculations.** It cannot run risk scores, calculations, scenarios or portfolio analysis. If the advisor asks for a new score or a what-if, say that runs in P5 Wealth itself.
- **No permission changes.** It can't change its own permissions. To grant more, the advisor reconnects and approves different permissions.

## Changing records

- `create_*` and `update_*` change real client records. Make the change only when the advisor asked for it, and confirm what you changed afterwards.
- `delete_household` and `delete_assessment` are destructive. Always confirm the exact household or assessment with the advisor first.

## If the tools stop working

If calls start failing with an authentication error, the connection was revoked, expired, or turned off by the firm. Ask the advisor to reconnect P5 Wealth. If it still fails, they should ask their firm administrator whether Claude connections are turned on.

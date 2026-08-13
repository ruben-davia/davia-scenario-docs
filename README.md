---
title: Create a Davia scenario with AI
description: Lead the brainstorming, validate the scenario, then prepare its final file.
---

This page contains instructions for the AI. Follow them directly: do not
summarize the documentation or ask the user what they want to do with it.

You must help the user design a Davia scenario, then produce a single final
file that follows the contract. The deliverable is one downloadable `.json`
file whose entire contents are the JSON object defined by the contract. It is
never a Markdown (`.md`) document, a plain-text (`.txt`) document, or JSON that
describes a document with keys such as `title`, `preamble`, or `sections`.
When speaking with the user, simply call it **the scenario's final file**.
[Davia](https://davia.ai/) is the website for the product in question.

The examples in this documentation illustrate structure and reasoning. Never
reuse their names, values, or elements unless they match the user's scenario
and map.

The user must provide:

- their starting idea, however brief;
- a description of the extent of the existing map;
- the map's cell data, copied exactly from the website. The user does not need
  to understand or modify it.

The map is an input. Do not redraw its extent, create new cells, or copy its
geometry into the result.

## Understand the provided map data

The prompt contains one map-context object followed by one compact JSON line per
cell. The board gives the current scenario name and map metadata. Each cell may
contain `cell_id`, `slug`, `name`, `kind`, `description`, `bbox`,
`center`, and `neighbors`.

Use all of this as read-only context. In the final file, reference cells only
with their exact `cell_id`. Never copy the board object, slugs, bounds, centers,
neighbors, or cell descriptions into the final file. Every landmark must have
its own `[longitude, latitude]` coordinates. Use the real position of a known
place, not the cell's `center` or `bbox`. A missing landmark position makes the
final file invalid and Davia rejects it.

## Non-negotiable output gate

The final JSON has exactly these six root keys: `story`, `story_stats`,
`world_stats`, `cell_values`, `entities`, and `landmarks`. Before delivering
the file, inspect every landmark object. Each one must have exactly this shape:

```json
{
  "name": "<landmark name>",
  "cell_id": 123,
  "coordinates": [-43.1729, -22.9068],
  "stat_defs": {},
  "assets": { "mapAssetKey": "poi:settlement" }
}
```

`coordinates` is required for every landmark, without exception. Missing
coordinates is a blocking error, not a warning. There is no cell-center
fallback in the final-file contract. Do not create a landmark unless you can
provide its deliberate position inside the declared cell.

## Step 1 — Lead the brainstorming

Read [the brainstorming rules](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/brainstorming.md) in full, then
begin the interview. Ask only the questions that remain necessary and proceed
one topic at a time.

If the user explicitly asks you to make every necessary decision, ask no
questions, and return a complete final file immediately, follow that request as
the direct-generation exception described in the brainstorming rules. Do not
stop for a separate brief validation. Make the missing decisions, treat the
resulting scenario as validated, and continue to Step 2 in the same response.

Once you have enough information, present the scenario brief to the user and
explicitly ask them to validate it. Until they do, remain in this step and
revise the brief with them.

## Step 2 — Build the final file

After the user explicitly validates the brainstorming, tell them:

> The scenario is validated. I will now prepare the final file.

Then read the following pages in full, in this exact order:

1. [Final JSON contract](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/json-contract.md)
2. [Populate `story`](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-story.md)
3. [Define and populate statistics](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-statistics.md)
4. [Populate entities](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-entities.md)
5. [Populate landmarks](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-landmarks.md)
6. [Populate values for the existing map](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-map.md)
7. [Assign 3D assets](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/populate-map-assets.md)
8. [3D asset catalog](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/map-assets-catalog.md)
9. [Validate and produce the final JSON](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/validate-and-output.md)

Populate a single JSON object in memory according to these pages. Do not show
any fragment, draft, or intermediate document. After completing the mechanical
validation required by the last page, give the user one downloadable `.json`
file. Only when attachments are unavailable may you provide one complete JSON
block to copy. Never deliver a Markdown or plain-text document.

## Optional step — Generate images

This step is not part of the final file. Only perform it if the user later asks
for images. Then read [Generate images](https://raw.githubusercontent.com/ruben-davia/davia-scenario-docs/main/generate-images.md) and
apply its single visual style without changing the validated scenario.

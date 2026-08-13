---
title: Validate and produce the final JSON
description: Run mechanical checks before returning a single JSON object.
---

Before producing any output, run a small validation with Python or an
equivalent tool. Do not rely on visually reviewing the JSON.

## Mechanical checks

At a minimum, the program must verify:

1. the JSON serializes, parses, and remains identical;
2. all six root keys are present and no others exist;
3. no prohibited technical field has been added;
4. `start_date` is a valid calendar date and time;
5. all `stat_id` values are unique and follow their required format;
6. every `value_kind`, `domain`, `default_value`, and `applies_to` is valid;
   every categorical or ordinal domain item contains exactly `key` and `value`,
   while colors appear only in the sibling `value_colors` object;
7. all categorical and ordinal values belong to their domain unless
   `allow_dynamic_values` genuinely permits otherwise;
8. all scalar values fall within their bounds;
9. each value is used only on a type listed in `applies_to`;
10. the first cell statistic is categorical and serves as the primary
    identity;
11. the `cell_id` values in `cell_values` form exactly the same set as the
    source cells, with no duplicates or omissions;
12. every cell has a resolved value for the primary identity;
13. every entity and landmark references an existing source cell;
14. optional coordinates are valid `[longitude, latitude]` pairs and, when
    source geometry is available, fall within the specified cell;
15. entity names are unique, and landmark names are unique within their own
    list;
16. every `mapAssetKey` exists in the catalog and has the correct prefix;
17. at least one entity is marked `is_featured`;
18. no geometry, UUID, `bbox`, `map_id`, slug, cover, visual direction, or
    calculated data is present.

The validator does not generate UUIDs or reconstruct the map. It uses source
geometry only as read-only input when necessary.

If a check fails, correct the working object and rerun every check. Never show
partial or invalid JSON.

Examples of failures to correct automatically before delivery:

- the source cells are `[41, 42, 43]`, but `cell_values` contains only `41` and
  `42`;
- an entity uses `poi:city` instead of an `entity:*` key;
- the first cell statistic contains `Strong`, `Weak`, and `Contested` even
  though the confirmed identity was `Country`;
- a cell in Troy receives `Besieged` as its `kingdom` value instead of in
  `war_status`.

## Editorial checks

Also verify the following without adding text to the result:

- one language for all human-readable content;
- a premise independent of the chosen entity;
- a `world_description` limited to world context;
- `simulation_rules` limited to scenario-specific constraints;
- no repetition of Davia's native principles;
- every entity and landmark within the map;
- landmarks appropriate to the map's scale;
- a nominal, homogeneous primary identity distinct from secondary states.

## Deliver to the user

When speaking with the user, always call the result **the scenario's final
file**, not "the JSON," unless the user asks for the format name.

After every check passes:

- if you can create an attachment, create one downloadable `.json` file and
  write only: "Download this file: it is the scenario's final file.";
- otherwise, write only: "Copy the entire text below: it is the scenario's
  final file.", then provide one complete `json` block.

The attachment itself ends in `.json`. Never create a `.md` or `.txt` file and
never wrap the JSON inside a document, explanation, or second JSON envelope.

Do not add a summary, comment, or second version. Never truncate, split, or
replace cells with ellipses.

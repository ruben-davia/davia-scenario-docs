---
title: Populate entities
description: Create the mobile actors present on the map when the scenario begins.
---

Create only the mobile actors validated during brainstorming.

## Consumed fields

Each entry in `entities` contains:

- `name`: a unique, concrete name;
- `description`: identity, initial situation, and useful distinguishing details;
- `cell_id`: an existing cell copied from the provided map;
- `coordinates`: an optional `[longitude, latitude]` position;
- `is_featured`: prominence in the interface, with no effect on playability;
- `stat_defs`: applicable initial values;
- `assets.mapAssetKey`: the exact key assigned during the asset step.

Do not add any other field.

## Presence on the map

Every entity must be physically present in its `cell_id` at
`story.start_date`. Remove actors outside the map. Do not retain them with a
fictional position or nearby cell.

Coordinates are optional. If a precise location matters, provide a real point
within the cell. Otherwise, omit `coordinates` and let the importer choose a
valid point.

Example — Brazil: Deodoro da Fonseca may be included if he is in Rio de Janeiro
at the start and that city is on the map. A diplomat stationed in London at
that time must be excluded if London is outside the map.

Example — Troy: Hector in Troy and Achilles in the Achaean camp may be included
if their cells appear on the map. An ally who remains outside its extent must
not receive a fictional cell.

## No "player" entity

All entities are potentially playable. Do not create an abstract entity named
"The Player," "Player," or "Protagonist." Do not write descriptions from a
privileged point of view.

`is_featured` only selects the entities highlighted in the interface.

## Description

The description must remain factual and useful regardless of the entity chosen
to start. It must not contain instructions, second person, or a predetermined
future. It must be no more than 500 characters.

Correct example:

> Hector commands the Trojan forces and defends the city while facing pressure
> from his family, his allies, and a prolonged war.

Incorrect example:

> You are Hector and must defeat Achilles before the end of the day.

Do not add `role`, `verbs`, `opening_problem`, or `visual_elements`. If a detail
is essential, write it naturally in `description` or in a statistic the
product actually consumes.

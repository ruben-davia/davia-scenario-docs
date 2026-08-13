---
title: Populate landmarks
description: Create fixed places displayed at the map's scale.
---

Create only the fixed places that should appear as named points at the scale of
the provided map.

## Consumed fields

Each entry in `landmarks` contains only:

- `name`;
- `cell_id`, copied from the existing map;
- `coordinates` in `[longitude, latitude]` format;
- `stat_defs`;
- `assets.mapAssetKey`, assigned later.

The current product does not consume landmark descriptions. Do not add one,
even if older seeds store an unused description in `assets`.

## Scale

Check every landmark against the map's extent. On a world map, major cities are
normal landmarks. A bedroom, small studio, private office, or ordinary building
remains context and does not become a world-scale point.

Do not create a place solely to host an entity. If its scale is too small, place
the entity in the relevant cell or city and keep the detail in its description.

Examples:

- world or continental map — Rio de Janeiro, Buenos Aires, or another major
  city visible at that scale;
- national map — regional capitals, major ports, and major sites;
- local map around Troy — the city of Troy, the Achaean camp, and a fortified
  gate may become separate points;
- never at world scale — a bedroom, individual tent, private office, or ordinary
  building.

Apply this test: does this place deserve a named point visible at the map's zoom
level? If not, keep it as context.

## Position

Every landmark must belong to an existing cell and lie on the map. Always
provide coordinates. A landmark is a point, so its position is part of the
landmark rather than optional decoration.

For a known real place, use its actual geographic position. Do not copy the
cell's `center`, derive a point from its `bbox`, or place several landmarks at
one generic point merely because they share a cell. For example, Berlin, Kiel,
and Metz require three different real positions even when the map represents
all three with the same Germany cell.

For an invented place, choose one deliberate point consistent with the
validated scenario and its cell. Before delivery, check that every coordinate
pair is ordered `[longitude, latitude]`, stays within world bounds, and belongs
to the declared `cell_id`. If a coordinate is missing, the final file is not
complete: add it before delivery.

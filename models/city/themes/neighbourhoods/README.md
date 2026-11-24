# #326 themed neighbourhood layouts (ae-models-city v6)

Approved and published by Michael on 24 November 2026 at 11:41 AM Europe/Athens (relayed by COO): "Approve and publish the #326 inner-city neighbourhood houses".

This tag is **additive** on city **v5** (`6a9ba60`). Every file that existed in v5 is unchanged. v6 only adds the baked stone neighbourhood and timber village layouts under `models/city/themes/<theme>/neighbourhoods/`.

0 Meshy. Theme house kits in `models/city/themes/<theme>/houses/` (v5) were not modified.

## Paths

For `theme` in `sand`, `jungle`, `snow`, `forest`:

```
models/city/themes/<theme>/neighbourhoods/stone/
  ae_<theme>_neighbourhood_stone_lod{0,1,2}.glb
  sandy_ring_city_lod0.glb          # alias of that theme's stone LOD0
  layouts.json

models/city/themes/<theme>/neighbourhoods/timber/
  ae_<theme>_village_{small,large}_lod{0,1,2}.glb
  village_{small,large}_lod0.glb    # short aliases of the LOD0 files
  layouts.json
```

`stone/sandy_ring_city_lod0.glb` is a same-bytes alias of that theme's stone neighbourhood LOD0. It is **not** a replacement for the existing untextured `models/city/sandy_ring_city.glb`, which stays as it was in v5.

## URL pattern

The game loads host-repo files via `rewriteModelUrl` in `src/lib/game/modelHost.ts`.

- jsDelivr: `https://cdn.jsdelivr.net/gh/rwsmw6pwsv-art/ae-models-city@v6/models/city/themes/<theme>/neighbourhoods/<stone|timber>/<file>`
- raw fallback: `https://raw.githubusercontent.com/rwsmw6pwsv-art/ae-models-city/refs/tags/v6/models/city/themes/<theme>/neighbourhoods/<stone|timber>/<file>`

**Wiring (#329):** `MODEL_HOSTS` currently has `{ prefix: "models/city/", repo: "ae-models-city", tag: "v5" }`. To pick up this package, move that pin from city **v5 to v6**. v6 is additive on v5, so every existing `models/city/` path stays in place.

## H1 / H2 / H3 mapping

| Form | Role | Stone variants | Timber variants |
|---|---|---|---|
| H1 | plain home | `small_hut`, `medium_block` | `small_hut`, `medium_block` |
| H2 | workshop / yard | `small_booth`, `large_terrace` | `small_booth`, `medium_l` |
| H3 | courtyard house | `medium_court`, `large_hall` | `large_hall`, `large_barn` |

`layouts.json` lists each placed module with id, H form, position, yaw and scale in GLB space.

## Fit and scale

AABB-fitted to the old served `sandy_ring_city` / `village_small` / `village_large` bounds (oldΔ ≈ 0%). Origin is ground-centre; +Z toward the south avenue / keep-door language is unchanged.

`CITY_NATIVE` oasis dims still differ from that old AABB by ~6% on height / ~4% on depth (pre-existing in v3-wiring `OASIS_N` vs measure.json). Villages are within 0.3% of `CITY_NATIVE`. Current fight scale can stay: oasis ≈ `INNER_R / (l/2)` ≈ 170.26; timber via `woodVillageUrl` + `villageFit`. Stone neighbourhoods keep a clear plaza inside r≈0.32 (courtyard curb 0.322–0.342 authoring).

`wallLook(rank)` still selects timber vs stone tier. Theme pick is empire→theme (see #321).

## LOD0 sizes and triangle counts

Town LOD0 budget was ≤ 15k tris / 0.6 MB; all pass.

| Theme | Piece | Houses | Tris | KB |
|---|---|---:|---:|---:|
| sand | stone neighbourhood | 33 | 4690 | 147.4 |
| sand | village_small | 22 | 6526 | 204.9 |
| sand | village_large | 38 | 6554 | 206.9 |
| jungle | stone neighbourhood | 34 | 5584 | 155.6 |
| jungle | village_small | 20 | 7592 | 200.2 |
| jungle | village_large | 34 | 7274 | 196.3 |
| snow | stone neighbourhood | 35 | 5464 | 142.0 |
| snow | village_small | 22 | 9134 | 223.7 |
| snow | village_large | 38 | 8000 | 205.5 |
| forest | stone neighbourhood | 34 | 4540 | 127.9 |
| forest | village_small | 22 | 6382 | 178.9 |
| forest | village_large | 38 | 6410 | 180.6 |

## How to use

1. Pick the empire's theme (`sand`, `jungle`, `snow`, `forest`).
2. `wallLook` still chooses the stone neighbourhood or a timber village size.
3. Load the matching LOD GLB (or the short alias for LOD0).
4. Apply the same oasis / `villageFit` transform as today. No new scale constants are required.
5. Optional: read `layouts.json` if you later instance modules instead of using the baked town GLB.

## Not in this tag

- No props, markets, courtyards or fountains (#333).
- No change to v5 houses, walls, pyramids, dome temples, wood keeps, or duel hall.

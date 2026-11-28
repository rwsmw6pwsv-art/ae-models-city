# ae-models-city v7 — #327 per-theme destroyed pyramids

Michael approved the look and, on Sat Nov 28 2026 (12:11 Athens, relayed by COO), OK'd publishing.

Additive on v6. 24 new files only. The live shared `models/city/pyramid_destroyed.glb` is unchanged, as are all existing v6 files and tags (v1–v6). Nothing in this tag overwrites a live path. Integration into the game still needs the usual Engineer pass (and Michael's merge OK).

## Paths (new)

For each theme `sand`, `snow`, `jungle`, `forest`:

- `models/city/themes/<theme>/pyramid_small_destroyed_lod{0,1,2}.glb`
- `models/city/themes/<theme>/pyramid_large_destroyed_lod{0,1,2}.glb`

These sit next to the existing theme pyramids (`pyramid_{small,large}_lod{0,1,2}.glb`). Do not replace `models/city/pyramid_destroyed.glb`.

Canonical raw URL: `https://raw.githubusercontent.com/rwsmw6pwsv-art/ae-models-city/v7/<path>`

## Art

Procedural, 0 Meshy credits, same kit/atlas/palette as the look-locked theme pyramids. Jungle and forest are stepped (moss/vines; leaf litter and a fallen trunk). Sand and snow are smooth-sided casing remnants over an exposed core (sand or snow drift). Each ruin has a collapsed top, a toppled gold capstone, rubble spilling off one side, and small scorch stains.

Footprint matches the intact keep plaza (r 51.8–52.2 wu). The old shared ruin stretched to r 64.2 wu and reached the first houses (~59–60 wu). These do not.

Export matches live city files: EXT_meshopt_compression + KHR_mesh_quantization + EXT_texture_webp, atlas 1024 (LOD0) / 512 (LOD1/2), 1 material, 1 draw call. Khronos validator: 0 errors / 0 warnings (1 info, UNSUPPORTED_EXTENSION, same as live files).

## Tris / KB (file size)

| Theme | Size | LOD0 tris | LOD0 KB | LOD1 tris | LOD1 KB | LOD2 tris | LOD2 KB |
|---|---|---:|---:|---:|---:|---:|---:|
| sand | small | 1431 | 89.5 | 883 | 37.4 | 230 | 26.0 |
| sand | large | 1521 | 91.0 | 911 | 37.8 | 230 | 26.0 |
| snow | small | 1531 | 70.5 | 933 | 33.9 | 232 | 21.7 |
| snow | large | 1599 | 71.6 | 951 | 34.1 | 234 | 21.7 |
| jungle | small | 1440 | 82.3 | 838 | 35.1 | 248 | 24.9 |
| jungle | large | 1568 | 84.3 | 869 | 35.6 | 262 | 25.0 |
| forest | small | 1439 | 70.9 | 878 | 33.1 | 229 | 21.9 |
| forest | large | 1563 | 72.7 | 846 | 32.6 | 239 | 22.0 |

LOD0 is well under the 160 KB budget. Three large ruins sit slightly over the 1.5k tris target (snow 1599, jungle 1568, forest 1563).

## Wiring (Engineer)

Files are pre-compensated so the existing shared-ruin `CITY_NATIVE` box is correct for every new file (small and large). Do not use `PYR_SMALL_N` / `PYR_LARGE_N` for the ruins.

1. In `city.tsx` `CITY_NATIVE`, add an entry for each of the 8 LOD0 URLs (include the same `?v=` query the kit will use, if any):

```
{ h: 1.1, l: 1.668, t: 1.901 }
```

Example keys (adjust `?v=7` to whatever cache-bust the kit uses):

```
"/models/city/themes/sand/pyramid_small_destroyed_lod0.glb?v=7"
"/models/city/themes/sand/pyramid_large_destroyed_lod0.glb?v=7"
"/models/city/themes/snow/pyramid_small_destroyed_lod0.glb?v=7"
"/models/city/themes/snow/pyramid_large_destroyed_lod0.glb?v=7"
"/models/city/themes/jungle/pyramid_small_destroyed_lod0.glb?v=7"
"/models/city/themes/jungle/pyramid_large_destroyed_lod0.glb?v=7"
"/models/city/themes/forest/pyramid_small_destroyed_lod0.glb?v=7"
"/models/city/themes/forest/pyramid_large_destroyed_lod0.glb?v=7"
```

If `nativeFor()` already strips the query and falls back to the base URL, the un-queried paths alone are enough.

2. In `src/lib/game/cityKit.ts` `makeKit()`, set per-theme `ruin:{small,large}` on the stone kit only (wood keeps stay on `keep_large_destroyed.glb`):

```
ruin: {
  small: `${root}/pyramid_small_destroyed_lod0.glb?v=7`,
  large: `${root}/pyramid_large_destroyed_lod0.glb?v=7`,
}
```

`cityKitLodUrls()` will derive LOD1/LOD2 from the `_lod0` URL.

3. Leave `models/city/pyramid_destroyed.glb` in place as a fallback until the new URLs are wired. Do not point the themed stone kits at it once the new files are hooked up.

4. There is no `models.manifest.json` in this repo, so no manifest fragment is included.

This tag only publishes the art. Merging into the game still needs Michael's usual OK.

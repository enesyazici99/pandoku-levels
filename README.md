# Pandoku — level packs

Level data for the Pandoku app, served to players over jsDelivr straight from
this repository. Publishing new content is a push here; the app picks it up on
its next launch with no store release.

## Layout

| File | What it is |
|---|---|
| `manifest.json` | Which packs make up the game, in order, at which version |
| `pack_001.json` | Levels 1–212 |
| `pack_002.json` | Levels 213–400 |

The manifest is authoritative: it decides which packs the game loads and in
what order, which is what makes it possible to add, replace or retire levels
remotely. Each entry carries a `version` and a `sha256`. The app re-downloads a
pack only when its version moves, and discards any download whose checksum does
not match.

## Publishing

From the app repository:

```bash
dart run tools/publish_levels.dart --out ../pandoku-levels \
    assets/levels/pack_001.json assets/levels/pack_002.json
```

Then commit and push. Versions only advance for packs whose bytes actually
changed, so republishing costs players nothing.

## Level format

```jsonc
{
  "level": 30,
  "size": 9,                  // 9x9 board, so 9 regions and 9 pandas
  "regions": [[0,0,1,...]],   // region index per cell
  "solution": [[0,3],[1,1]],  // one [row, col] per panda
  "difficulty": 309,          // measured, not authored
  "difficultyLabel": "HARD"   // present only on badged levels
}
```

Every board is validated before it ships: square, exactly N contiguous regions,
and exactly one solution. A pack that fails those checks is not published.

# Vanilla Data

The game's own item, block state and biome definitions and creative inventory, which clients expect Kore to agree with. Kore reads these files on start from `data/` in its working directory, so check this repository out there.

The files are copied unmodified from two projects:

- [CloudburstMC/Data](https://github.com/CloudburstMC/Data), whose files are covered by its Apache 2.0 license, included here as [LICENSE-CloudburstMC](LICENSE-CloudburstMC);
- PrismarineJS's [minecraft-data](https://github.com/PrismarineJS/minecraft-data), whose `blocks.json` and `items.json` are covered by the MIT license its README declares.

The rest of this repository is under its own [MIT license](LICENSE).

| File | What it holds |
| --- | --- |
| `runtime_item_states.json` | Every item's name, runtime ID and version, and whether components define it |
| `item_components.nbt` | Each item's component data, keyed by item name (gzipped big-endian NBT) |
| `block_palette.nbt` | Every block state: its name, properties and version (gzipped big-endian NBT) |
| `stripped_biome_definitions.json` | Every biome's climate, colours and tags, without world generation data |
| `creative_items.json` | The creative inventory: its groups, and every item in them with its block state and NBT |
| `data_driven_blocks.nbt` | Definitions of the vanilla blocks defined in data, like wool and concrete stairs and slabs (gzipped big-endian NBT) |
| `blocks.json` | From minecraft-data: every block's hardness, the tool that digs it quicker, and the tools that harvest it |
| `items.json` | From minecraft-data: how many of each item one slot holds, and its English name |

**Sources:**
- CloudburstMC/Data commit [`659ce1e2`](https://github.com/CloudburstMC/Data/tree/659ce1e2eee3a67045693f4fd5515c6ccf571953), the last one for 1.26.50 (protocol `2193`).
- minecraft-data commit [`9001b1ea`](https://github.com/PrismarineJS/minecraft-data/tree/9001b1ead1b14b554b88978ff97bcfe7233a2b31), `data/bedrock/1.26.30/blocks.json` and `items.json`, its newest Bedrock block and item data. Kore fills in the blocks added since from older ones of their family, and gives items added since a stack of 64.

## Updating

The data must match the client release Kore speaks, so it moves with `protocol` and `version` in Kore's `ServerConfig`:

1. Find the CloudburstMC/Data commit for the new release, and minecraft-data's newest Bedrock block and item data.
2. Copy these files from them, unchanged, and update the commits above.
3. Run Kore's tests with the new data in `data/`. `BlockStatesTest` hashes every block state and compares it with the network ID the game itself gave that state, so it catches the palette and Kore's hashing drifting apart.

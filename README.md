# Vanilla Data

The game's own item, block state and biome definitions, creative inventory and recipes, which clients expect Kore to agree with. Kore reads these files on start from `data/` in its working directory, so check this repository out there.

The files are copied unmodified from three projects:

- [CloudburstMC/Data](https://github.com/CloudburstMC/Data), whose files are covered by its Apache 2.0 license, included here as [LICENSE-CloudburstMC](LICENSE-CloudburstMC);
- PrismarineJS's [minecraft-data](https://github.com/PrismarineJS/minecraft-data), whose `blocks.json`, `items.json` and `enchantments.json` are covered by the MIT license its README declares;
- PocketMine's [BedrockData](https://github.com/pmmp/BedrockData), whose `item_tags.json` is dedicated to the public domain under CC0 1.0.

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
| `items.json` | From minecraft-data: how many of each item one slot holds, its English name, how many uses it has, what mends it and which kinds of enchantment it takes |
| `enchantments.json` | From minecraft-data: every enchantment up to Swift Sneak, by number, with its levels, rarity, costs and the enchantments it rules out |
| `recipes.json` | Every recipe: crafting, smelting, stonecutting, smithing, and brewing's potion and container mixes |
| `item_tags.json` | From BedrockData: the items each item tag takes in, which recipes name ingredients by |

**Sources:**
- CloudburstMC/Data commit [`659ce1e2`](https://github.com/CloudburstMC/Data/tree/659ce1e2eee3a67045693f4fd5515c6ccf571953), the last one for 1.26.50 (protocol `2193`).
- BedrockData commit [`bdb44a48`](https://github.com/pmmp/BedrockData/tree/bdb44a48), its newest, for 1.26.30. Items with components name their own tags, and Kore gives newer items the tags of older ones of their family.
- minecraft-data commit [`9001b1ea`](https://github.com/PrismarineJS/minecraft-data/tree/9001b1ead1b14b554b88978ff97bcfe7233a2b31), `data/bedrock/1.26.30/blocks.json` and `items.json`, its newest Bedrock block and item data, and `data/bedrock/1.19.1/enchantments.json`, its newest Bedrock enchantments. Kore fills in the blocks added since from older ones of their family, gives items added since a stack of 64, and adds the enchantments added since: Wind Burst, Density, Breach and Lunge.

## Updating

The data must match the client release Kore speaks, so it moves with `protocol` and `version` in Kore's `ServerConfig`:

1. Find the CloudburstMC/Data commit for the new release, minecraft-data's newest Bedrock block, item and enchantment data, and BedrockData's newest item tags.
2. Copy these files from them, unchanged, and update the commits above.
3. Run Kore's tests with the new data in `data/`. `BlockStatesTest` hashes every block state and compares it with the network ID the game itself gave that state, so it catches the palette and Kore's hashing drifting apart.

# ResourceLocation

`ResourceLocation` plays a crucial role in the game system. Therefore, you must know how it is supposed to work and the
basic rules of it.
In this chapter, we will use `RL` as an abbreviation for `ResourceLocation`.

## 1. How it looks

If you are familiar with commands, you could have seen this kind of syntax/IDs before.

`minecraft:dirt` or `minecraft:diamond`

That is how RL looks in the game

And in java, you can construct a new RL by using the constructor :

`ResourceLocation(String pNamespace, String pPath)`
or
`ResourceLocation(String pPath)`

So let's separate the id `minecraft:dirt` into 2 parts.
Each are separated by the semicolon (`:`)

- Namespace - `pNamespace` is the left part, or `minecraft`
- Path - `pPath` is the right part, or `dirt`

so we respectively put 2 parts of it in the first constructor of RL above.

!!! note

    If the `pNamespace` you want to pass in is `minecraft`, you can ignore the namespace parameter. Thus, you can just only pass in the path for the constructor and the namespace will be `minecraft` by default. See the second constructor above

## 2. Usages

RL can be use in almost everywhere, in java code to JSONs, to commands, etc.

Here are the 3 most popular usages of RL:

|   | Usage                         | Description                                                                                            | RL syntax                         | Example                                                                                                                           |      
|---|-------------------------------|--------------------------------------------------------------------------------------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| 1 | For registration              | Such as items,blocks,entities                                                                          | `modid:name`                      | `minecraft:diamond_block` represent the Diamond Block in the game                                                                 |
| 2 | For JSON file reference       | Mainly used for client assets and server data JSON references, or texture file name **_in JSON file_** | `pack_name:path/json_file_name`   | `"model" : "minecraft:block/oak_planks"` in blockstate JSON file is a reference to `assets/minecraft/model/block/oak_planks.json` |
| 3 | For PNG/image files reference | Mainly used when rendering a screen, RL can be used as a file path placeholder                         | `resource_pack_id:path/image.png` | `textures/gui/container/blast_furnace.png` is referencing `assets/minecraft/textures/textures/gui/container/blast_furnace.png`    |

## 3. The Rules

You must follow these rules of RL otherwise the game will crash.
These rule is applied to **both namespace and paths/ids**:

- Only alphabet characters is allowed.
- All alphabet characters must be **lowercase**
- Numbers are allowed, but not always required
- Only 4 special characters are allowed, which are `/`, `.`, `_`, `-`
- **NO SPACE ALLOWED**

So what if you didn't follow the rule.
Well, `ResourceLocationException` will be thrown which will stop your game from working correctly, with a message looks
like this:
`Non [a-z0-9/._-] character in path/name of location:[the:wrong/ RL]`
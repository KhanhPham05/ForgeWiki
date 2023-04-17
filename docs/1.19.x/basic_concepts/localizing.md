# Localising

## Creating a Language Key with `Component`

Basically, class `Component` is a placeholder of the language/translation key, the game will search for the language key in the language file. If the language key that you are referring to has a translation text (value) indicates that key. The game will render the translation text (value) that `Component` asks.
 
To create a new object of `Component` you should call the method: 

`Component.translatable(String key)` 

The `key` parameter is the translation key that the game will search in the language file.

## Translating A Language Key

!!! note 

    In order to understand this chapter clearly, you must know the syntax of JSON (not JSON5)

When running the game, some time you may see this ugly-looking text like this :

`item.modid.ruby_item` or `item.minecraft.pumpkin_seeds`

In this case, you must translate that ugly text into a clean name.
The language file follows a simple rule. It is a list of `"key" : "value"` pairs.
`"key"` - Language Key
`"value"` - The text that is translated in the game, which will a replacement of the key when rendering it in the game.

## Creating Language File.
By default, the game will find the `en_us.json` translate your item from an ugly long string of text into a clean item name.
In order to fix that, you must have a language file which is `en_us.json` by default.

So, to let the game read your language file, you have to put it in `assets/<modid>/lang/en_us.json` after that fill in an empty pair of `{}` in the file then you can advance to the next section below.

## Translating Your Item/Block

Language Key Syntax : `item.<modid>.<item_name>` for item and `block.<modid>.<item_name>` for BlockItem

Example: 
```json5 title="en_us.json"
{
  //...
  "item.minecraft.pumpkin_seeds" : "Pumpkin Seeds",
  "item.minecraft.iron_chestplate": "Iron Chestplate",
  "block.minecraft.deepslate_bricks": "Deepslate Bricks",
  //...
}
```

## Translating Your [`CreativeModeTab`]()

Your `CreativeModeTab` language key is the string that you determine in `CreativeModeTab.Builder#title()` when [configuring your tab](item.md#tab-configurations).
# Registering Items

Items can be registered through [DeferredRegister and RegistryObject](index.md#1-registering).

## Supply the registry object.

The `supplier` parameter you need to provide to the `register()` should be starting with `() ->` since `Supplier` is
a [functional interface](https://www.geeksforgeeks.org/functional-interfaces-java/).

So it should be like this:
` ... register("example", () -> new Item(new Item.Properties()));`

## Constructing an item

As you see above, `Item` needs some properties that describe how your item is supposed to be.

These property options that you can describe:

!!! note "Mappings Differences"

    The table below uses Parchment Mappings. Names of methods maybe different on your side.

| Property/Method     | Parameter(s)      | Description                                                                                                                                                                    | Default Values                                     |
|---------------------|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| `food`              | `FoodProperties`  | See [Food Properties](#food-properties)                                                                                                                                        | `null` (not a food/edible item)                    |
| `stackTo`           | `int`             | The maximum items a stack/slot could have.                                                                                                                                     | `64`                                               |
| `durability`        | `int`             | Set durability to your item, making it breakable. `maxStackSize()` will be defaulted to 1                                                                                      | `0`                                                |
| `defaultDurability` | `int`             | Same as `durability()` but if `durability()` has already set, this method would do nothing                                                                                     | `0`                                                |
| `craftRemainder`    | `Item`            | The `Item` you pass in will be the replacement in the crafting slot after you craft another item using the item that you are registering. You can take a bucket as an example. | `null` (item will gone after you craft)            |
| `rarity`            | `Rarity`          | The rarity of this item. [See more](#item-rarity)                                                                                                                              | `Rarity.COMMON`                                    |
| `fireResistant`     | none              | Make this item immune to lava and fire.                                                                                                                                        | `false` (item will be delete if it's in fire/lava) |
| `setNoRepair`       | none              | Make this item CAN NOT be repaired using neither anvil nor grindstone                                                                                                          | `true` (item can be repaired)                      |
| `tab`               | `CreativeModeTab` | **For 1.19.2 or below**. Declare an item is included in what creative inventory mode tab. For 1.19.3 or later, pls refer to [this](#put-item-in-creative-inventory)            | null                                               |

There is another method called `requiredFeatures(FeatureFlag[])` which you don't have to care too much about it.

### Food Properties

In case you want you item to be a food item, instead of using the deprecated constructor, you can
use `FoodProperties.Builder` class and then finish by calling `.build()`
Here is how you can do it in your code.

```java
class Properties() {
    public static final FoodProperties EXAMPLE_PROPERTY = new FoodProperties(new FoodProperties.Builder());
    //or
    public static final FoodProperties EXAMPLE_PROPERTY_2 = new FoodProperties.Builder().build();
}
```

Just like `Item.Properties()`, these are some of the properties that you can set to your food item:

| Property/Method | Parameter(s)                                                                      | Description                                                                                                                                                                          | Default Values |
|-----------------|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|
| `nutrition`     | `int`                                                                             | Determine how much food will the player will be filled after consume                                                                                                                 | `0`            |
| `saturation`    | `float `                                                                          | Determine how much saturation point (is hidden in the food bar) will the player have when they finished consume the food                                                             | `0.0f`         |
| `meat`          | none                                                                              | Declare the food can be feed to meat-eaters animals such as wolf                                                                                                                     | false          |
| `fast`          | none                                                                              | Declare the food can be consumed quicker than normal (dried kelp for example)                                                                                                        | false          |
| `effect`        | **`Supplier` of `MobEffectInstance`**; `float` (**should not be more than 1.0f**) | Declare that the food will have chance of giving the player an effect after consume. Passing `1.0f` to the float parameter will ensure the effect will be 100% applied to the player | no effect      |

!!! note "For `effect()` method"

    The float/chance value that you need to put in must not be greater than `1.0F`, which means `1.0F` will be `100%`.
    Therefore, the value you put in equals to the chance want devide with 100.
    For example, you want the effect(s) has(have) `75%` of being applie to the player after consume -> you pass in `0.75F`

### Item Rarity

Item rarities list can be found in `net.minecraft.world.item.Rarity` which is an enum of 4 different rarities.
Each rarity has it own color which determine the color of the **item name**

| Rarity   | Item Name Color |
|----------|-----------------|
| COMMON   | White           |
| UNCOMMON | Yellow          |
| RARE     | Aqua            |
| EPIC     | Light Purple    |

!!! note

    `COMMON` is the default for every item

## Put item in creative inventory.

When you registered the item successfully, but you couldn't see the item in the creative inventory/menu (but only can
obtain through command).
This is because you item needs to be added in to a `CreativeModeTab` (depends on the mapping, it can be
called `ItemGroup`).

### For 1.19.2 or bellow.

If you want to put the item into vanilla tab, you can see `CreativeModeTabs` class for a list of vanilla tabs.

If you want to create your own tab, you can create a `public static final` instance of `CreativeModeTab` like this :

```java
import net.minecraft.world.level.block.Blocks;

class MainClass {
    public static final CreativeModeTab TAB = new CreativeModeTab("tab_id") {
        public ItemStack makeIcon() {
            return new ItemStack(Blocks.DIAMOND_BLOCK);
        }
    };
}
```

### For 1.19.3 or higher

Click on one of these link if you want to :

- [Create a new Tab](#create-a-new-tab)
- [Add item to a vanilla tab](#add-item-to-a-vanilla-tab)

#### Add Item To A Vanilla Tab



#### Create a new Tab

This is quite technical because since 1.19.3, the method of adding item to tab has changed significantly.

First of all you should create a `static void registerTab()` method in your `MainClass` which has a single parameter,
which is `CreativeModeTabEvent.Register`.

After that, you take the `IModEventBus bus` variable in the constructor. then
call `bus.addListener(MainClass::registerTabs);`

Example Code:

```java
import net.minecraft.resources.ResourceLocation;
import net.minecraft.world.item.CreativeModeTab;
import net.minecraftforge.event.CreativeModeTabEvent;

class MainClass {

    public MainClass() {
        IEventBus bus = FMLJavaModLoadingContext.get().getModEventBus();
        //...
        bus.addListener(MainClass::registerTabs);
    }

    static void registerTabs(CreativeModeTabEvent.Register event) {
        event.registerCreativeModeTab(ResourceLocation, builder -> {
        });
    }
}
```

In the registering method call. There are 2 parameters you should pass in (also the recommended method).

- The [`ResourceLocation`](resource_location.md), which is the ID of the tab.
- the `Consumer<Builder>` or `builder -> {}` in the code above is how you configure your creative mode tab.

You can reference to `builder` variable and use it as a properties builder, just like `Item.Properties`
and `FoodProperties.Builder`
These are all the methods that you may want to have look at (highlighted methods belows are recommended to call when
constructing the tab):

| Method                       | Parameter(s)                                                   | Description                                                                                                                                                                                                 | Default Value                                                            |
|------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
| *`title()`*                  | `Component`                                                    | Set the translation key for this tab, which displays the translated string from your language file when you hover on the tab                                                                                | `Component.empty()`                                                      |
| *`icon()`*                   | `Supplier` of `ItemStack`                                      | The item that is being rendered as the tab icon in creative inventory/menu.                                                                                                                                 | `() -> ItemStack.EMPTY` (no item is drawn, use air item as default)      |
| *`displayItems()`*           | `CreativeModeTab.DisplayItemsGenerator`                        | The lists of items that are in the tab, **which must not be empty**                                                                                                                                         | `EMPTY_GENERATOR`                                                        |
| ~~`alignedRight()`~~         | NONE                                                           | **NOT SUPPORTED: DO NOT CALL THIS METHOD**                                                                                                                                                                  | `false`                                                                  |
| `hideTitle()`                | none                                                           | Tell the game to not render the title of this tab when opening it                                                                                                                                           | `true` (title is rendered normally)                                      |
| `noScrollBar()`              | none                                                           | In case the tab has a very few items, you can call this to remove the scroll bar on the right side of the tab                                                                                               | `false`(scrollbar is visible)                                            |
| _`type()`_                   | `CreativeModeTab.Type`                                         | The type of this tab. It could be `CATEGORY`, `INVENTORY` ,`HOTBAR`, or `SEARCH`. _It is recommended that should ignore this method_                                                                        | `CreativeModeTab.Type.CATEGORY`                                          |
| _`withBackgroundLocation()`_ | `ResourceLocation`                                             | In case you have a different design for your tab. This will be th link to png file that you designed for your tab. This must follow the [3rd usage rule of ResourceLocation](resource_location.md#2-usages) | `textures/gui/container/creative_inventory/tab_items.png`                |
| _`backgroundSuffix()`_       | `String`                                                       | The png image in `assets/minecraft/textures/gui/container/creative_inventory/tab_` + file name with `.png` acts as a suffix. You shouldn't care too much about this method.                                 | `"items.png"`                                                            |
| `withSearchBar()`            | none                                                           | This tab will have a search bar, which will exactly as same as the search tab. Also the `backgroundSuffix()` will be called with `"item_search.png"` as suffix input                                        | `false` (no search bar)                                                  |
| `withSearchBar()`            | `int`                                                          | Just like `withSearchBar()` above, but you can set a width value of the search box.                                                                                                                         | `89` (pixels)                                                            |
| _`withTabsImage()`_          | [`ResourceLocation`](resource_location.md#2-usages)            | Just like `withBackgroundLocation()` but this method is for the tabs outline texture                                                                                                                        | `textures/gui/container/creative_inventory/tabs.png`                     |
| `withLableColor()`           | `int`                                                          | The hex or decimal color code for the title of the tab when the full tab is rendering                                                                                                                       | `0x404040` in hex or `4210752` in decimal                                |
| `withSlotColor()`            | `int`                                                          | The hex or decimal color code for the slot when rendering it in the tab                                                                                                                                     | `0x80ffffff` in hex or `-2130706433` in decimal                          |
| (Advanced) `withTabFactory`  | `Function` from `CreativeModeTab.Builder` to `CreativeModeTab` | You decide how the tab is built/convert/finalise into a complete `CreativeModeTab`.                                                                                                                         | Forge constructs the tab through a long constructor of `CreativeModeTab` |

#### DisplayItemsGenerator
Basically, this _`FunctionalInterface`_ acts as a placeholder for a SET of items that are in your tab. Which means in a tab must not have item duplication.
And when building a new `CreativeModeTab` you must not leave this list empty.

This is how you create a new `DisplayItemsGenerator`:

```java
import net.minecraft.world.item.CreativeModeTab;

class ModCreativeTabs {
    public static final CreativeModeTab.DisplayItemsGenerator EXAMPLE_GENERATOR = (flags, output, displayOperator) -> {
        
    };
}
```

Let's say that you are a newcomer in modding, so you just only need to care the most about the `output` variable. 
So let's talk about the `output` parameter, which is a reference to `CreativeModeTab.Output` interface.

If you are trying to add a single item into your tab, you can call `output.accept()` method and pass in a `new ItemStack(Item)` with the item that you want.

!!! note 

    In case you are adding an item of your mod through `RegistryObject` you can call `.get()` method and put it in the `ItemStack` constructor and everything is fine.

If you want to pass in multiple items, you can create a new list or set, then put all the items that you want in to that list/set. Finally, call `output.acceptAll(Collection<ItemStack> stacks)` and put your list/set in the parameter.

#### TabVisibility
While "accepting" items to your tab, you may be wondering, what is the `TabVisibility`, so let us explain !

This is an enum of 3 values:

- `PARENT_AND_SEARCH_TABS` which is the default visibility of the items that you're "accepting". All items in that tab with that visibility will be shown in both your tab and the search tab.
- `PARENT_TAB_ONLY` - the items that you are accepting will only be shown in the tab. Not the search bar.
- `SEARCH_TAB_ONLY` - is the reverse form of `PARENT_TAB_ONLY`. Only accepted items are shown in the searching tab such as Command Block or Structure Block 
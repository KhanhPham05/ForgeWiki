# Basic Concepts

This category will cover all the basics of Forge modding, from registration, to data generations.

## 1. Registering
In Forge API, there are 2 essential registration classes `RegistryObject<T>` and `DeferredRegister<T>`

### 1.1 `DeferredRegister<T>`
this class is one of the most important class for registration. There is no constructor publicly available for you, but you can do the following :
```java
//Saying that you are registering items.
public static final DeferredRegister<Item> ITEMS = DeferredRegister.create("modid", ForgeRegistries.ITEM);
```

!!! note "Explanation"
    In the code block above, we use a mod id and ForgeRegistries to create a new `DeferredRegister` object via `create()` method.

!!! note "Caution"
    You must not use vanilla Registries ([`BuiltinRegistries`](#notes)) since in Forge, these vanilla Registries are locked, and you can not register stuffs like how the game did. Therefore, all the locked registries in [`BuiltinRegistries`](#notes) are marked as `@Deprecated` and you can't use it. you must use `ForgeRegistries` instead

This class act as a collection/list of registered objects that you have added into the game.


### 1.2 `RegistryObject<T>`
This class represent an object that you registered into the game. Saying that you are using the `ITEMS` variable above, this is how you can eventually register your object to the game properly.

```java
public static final RegistryObject<Item> EXAMPLE_ITEM = ITEMS.register(name, supplier);
```

In this case, we passed:

- name - the ID [(ResourceLocation rules applies)](resource_location.md#3-the-rules) of the item/object that you want to register. 
- supplier - Will be explained further in the wiki. See [Registering Item](item.md#supply-the-registry-object)

!!! note "`RegistryObject<T>` implements `Supplier<T>`"

    This means you needto call RegistryObject#get to get the raw object when needed.
    In method parameters, you can ask for a Supplier<T> and then pass your RegistryObject<T> which is still okay since RegistryObject<T> inherits the method get() from Supplier.


## 2. Register to bus
After finishing the first 2 steps above, your items/objects is still not yet registered in the game.
Thus, you need to get an instance of `IModEventBus` via `FMLJavaModLoadingContext.get().getModEventBus()`, save it in a variable then pass it into `DeferredRegister#register(IEventBus)`
All of this is happened inside your mod main class constructor.

Example code: (Saying that you are still using `ITEMS` from the above. And it is put in a separate class `AllItems`)
```java
public class Main {
    //...
    
    public Main() {
        //...

        IEventBus bus = FMLJavaModLoadingContext.get().getModEventBus();
        AllItems.ITEMS.register(bus);

        //...
    }
    
    //...
}
```

## 3. Run the game.
You can now run task `runClient` (or `gradlew runClient` in command prompt)
At this point, your items/objects are basically registered in the game

## Notes
1. For 1.19.2 or below, the name of `BuiltinRegistries` is `Registries`
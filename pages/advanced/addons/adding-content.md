## Using the Registry Bus
Until now, the addon just exists and doesn't actually add anything. To register new (supported) content, you can use the `RegistryBus` provided in your `onInitialize` method.


### Creating a category
Let's register a category to keep all your addon's modules.

Open your main class, then create a new category as shown below. You can change the icon's item and category name with whatever you want.

```java
public static final Category CATEGORY = new Category("category_name", Items.DIAMOND);

```


Afterwards, go in the `onInitalize` methon and register your Category as shown below.

```java
registryBus.register(Category.class, ctx -> CATEGORY);

```

That's it! What did we just do? The `RegistryBus` provides a `register` method to register new content. `Category.class` specifies what kind of content you are registering - and what Cactus will use to look for your registrations. The object you pass to the function has to be an instance of this class or a child. The `ctx` you are provided is a `RegistrationContext` which can be used for retrieving instances of handlers which you might need for the construction of some objects. A good example for this is the registration of new config files:

```java
registryBus.register(FileConfiguration.class, ctx -> new MyFileConfig(ctx.require(ConfigHandler.class)));
```

Calling `require` and passing `ConfigHandler.class` will prompt the context to return an instance of this class it was provided with. **Trying to retrieve instances which haven't been provided to the registration context will result in an exception.**


### Creating a module
Lets's register a new Module. Just create a new class which extends `Module` and give it an ID and our category (remember to change MainClass to your actual main class) .

```java
public class MyModule extends Module {

    public MyModule() {
        super("myModuleId", MainClass.CATEGORY);
    }
    
    @Override
    public void onEnable() {
        
    }

    @Override
    public void onDisable() {

    }

    @EventHandler
    public void onTick(ClientTickEvent event) {

    }

}
```

You can see in the example above the  `MyModule` class contains 4 methods:
- the `MyModule` method 
- the `onEnable` method contains code that is ran right after you enable the module
- the `onDisable` method contains code that is ran right after you disable the module
- the `onTick` method contains code that is ran at the end of every game tick

Now, back in your `onInitialize` method in the main class, register your Module like shown below.

```java
registryBus.register(Module.class, ctx -> new MyModule());
```

And with this you are done! Now, when opening the module gui in-game you will be able to find your category with your module inside!
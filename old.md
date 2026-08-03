# Addons

This section mainly focuses on addon development.
There isn't much here now.

## Usage

Cactus Addons are installed the same way as normal fabric mods. Just drag your `addon.jar` file into your `mods` directory - it's that simple!

## Development

### Project Setup

Use the [Fabric Template Mod Generator](https://fabricmc.net/develop/template/) to create a new project. Make sure *"Split client and common sources"* is **disabled**.

After that, just import the downloaded Project into your IDE. I recommend using IntelliJ.


### Entrypoints
Cactus uses fabric's entrypoint system to discover and load addons. To make your addon visible as one to Cactus, you need to add your main class to the entrypoint for 'cactus' in your `fabric.mod.json`.

Replace the `entrypoints` part in your `fabric.mod.json` with the json object below, and update the class and package name to match your main class.

```json
"entrypoints": {
    "cactus": [
      "com.yourname.MyAddon"
    ]
}
```

### Main Class
Now that you've successfully imported the library and updated the required entrypoint, last but not least you'll have to update your main class. Instead of implementing `ClientModInitializer`, you have to implement `ICactusAddon` and implement the required methods.

```java
public class MyAddon implements ICactusAddon {
    
    @Override
    public void onInitialize(RegistryBus registryBus) {
        
    }

    @Override
    public void onLoadComplete() {

    }

    @Override
    public void onShutdown() {

    }

}
```

Your mod should now work as a Cactus Addon. You can try building it and adding it to your mods directory together with Cactus, and it should be loaded as an Addon.

### Using the Registry Bus
Until now, the addon just exists and doesn't actually add anything. To register new (supported) content, you can use the `RegistryBus` provided in your `onInitialize` method.

#### Basic registering
Lets's register a new Module. Just create a new class which extends `Module` and give it an ID.

```java
public class MyModule extends Module {

    public MyModule() {
        super("myModuleId");
    }

}
```

Now, back in your `onInitialize` method, register your Module like shown below.

```java
registryBus.register(Module.class, ctx -> new MyModule());
```

That's it! What did we just do? The `RegistryBus` provides a `register` method to register new content. `Module.class` specifies what kind of content you are registering - and what Cactus will use to look for your registrations. The object you pass to the function has to be an instance of this class or a child. The `ctx` you are provided is a `RegistrationContext` which can be used for retrieving instances of handlers which you might need for the construction of some objects. A good example for this is the registration of new config files:

```java
registryBus.register(FileConfiguration.class, ctx -> new MyFileConfig(ctx.require(ConfigHandler.class)));
```

Calling `require` and passing `ConfigHandler.class` will prompt the context to return an instance of this class it was provided with. **Trying to retrieve instances which haven't been provided to the registration context will result in an exception.**
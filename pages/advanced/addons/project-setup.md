### Mod Creation

Use the [Fabric Template Mod Generator](https://fabricmc.net/develop/template/) to create a new project. Make sure *"Split client and common sources"* is **disabled**.

After that, just import the downloaded Project into your IDE. I recommend using IntelliJ.


### Dependency 
To be able to use Cactus in your Project, you need to import it as a dependency. 

First, navigate your `gradle.properties` file and add a variable called  `cactus_mod_version` , then, set it to your desired cactus version as shown below.

```gradle
# Dependencies
fabric_api_version=0.155.2+26.2

# Add the variable here
cactus_mod_version=0.14
```


Then, go to your `build.gradle` file and look for the `repositories` section and add Modrinth as shown below so gradle can actually find cactus.

```gradle
repositories {

	exclusiveContent {
		forRepository {
			maven {
				name = "Modrinth"
				url = "https://api.modrinth.com/maven"
			}
		}
		filter {
			includeGroup "maven.modrinth"
		}
	}
}
```


Now, stay in your `build.gradle` file and look for the `dependencies` section and add Cactus as shown below.

```gradle
dependencies {
	// To change the versions see the gradle.properties file
	minecraft "com.mojang:minecraft:${project.minecraft_version}"
	implementation "net.fabricmc:fabric-loader:${project.loader_version}"

	// Fabric API. This is technically optional, but you probably want it anyway.
	implementation "net.fabricmc.fabric-api:fabric-api:${project.fabric_api_version}"
	
	// Add cactus here
	implementation "maven.modrinth:cactus:${project.cactus_mod_version}"
}
```


### Entrypoints
Cactus uses fabric's entrypoint system to discover and load addons. To make your addon visible as one to Cactus, you need to add your main class to the entrypoint for 'cactus' in your `fabric.mod.json`.

Replace the `entrypoints` part in your `fabric.mod.json` with the json object below, and update the class and package name to match your main class.

```json
"entrypoints": {
    "cactus": [
      "com.yourname.MyAddon.MainClass"
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

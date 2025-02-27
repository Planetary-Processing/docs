# Unreal

## Introduction

Planetary Processing provides a plugin for Unreal Engine 5. The plugin consists of a number of C++ classes for use in connecting players to your game simulation, as well as communicating simulation state to your game client and messages from the client to your simulation.

### Pre-requisites

* Unreal Engine 5.4+ _<mark style="color:yellow;">(</mark><mark style="color:yellow;">**Note: the update for Unreal Engine 5.5 is still in development. Please use 5.4).**</mark>_&#x20;
* macOS or Windows, the PP Unreal plugin does not currently support Linux.

### Installation

To install the Planetary Processing plugin, you must first download the plugin files from our website, you will need to be logged into your Planetary Processing account to do so, the link is: [https://files.planetaryprocessing.io/builds/downloads/artefactid/107/version/latest/dist/dist.tar](https://files.planetaryprocessing.io/builds/downloads/artefactid/107/version/latest/dist/dist.tar)

You'll need then to extract this and move the contents of the `lib` directory into a directory called `PlanetaryProcessing` in the `Plugins` directory at the root level of your project. You'll need to create the Plugins directory if it does not already exist.

You can now enable the Planetary Processing plugin for your Unreal Engine 5 project. You will be required to restart the engine once you enable the plugin.

## Classes

### PlanetaryProcessing Class

This class is the core of the Planetary Processing plugin. It handles initialization, connection to the server, authentication, message sending, and state updates from the server. This class is intended to be used as a singleton, and instantiated via the **Init** static function.

#### Properties

| Name     | Type                      | Description                                                                                           |
| -------- | ------------------------- | ----------------------------------------------------------------------------------------------------- |
| GameID   | int32                     | ID of the game. Editable in Blueprints.                                                               |
| UUID     | FString                   | Authenticated player's unique identifier. Read-only in Blueprints.                                    |
| Entities | TMap\<FString, UEntity\*> | Map of server entities, reflecting the server state as of the latest update. Read-only in Blueprints. |

#### Functions

| Name                                          | Parameters                                       | Return Type                    | Description                                                                                                                                                                                                                                                                                             |
| --------------------------------------------- | ------------------------------------------------ | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| static Init                                   | int32 InGameID                                   | UPlanetaryProcessing\*         | Initializes the Planetary Processing singleton with the given game ID. If called again once already initialized, the existing Planetary Processing instance will be restored to its initial state and returned.                                                                                         |
| static GetInstance                            | None                                             | UPlanetaryProcessing\*         | Gets the singleton instance of the Planetary Processing class.                                                                                                                                                                                                                                          |
| Connect                                       | const FString& Username, const FString& Password | void                           | Connects the player to the server with the provided username and password. Broadcasts **OnAuthenticated** upon successful connection.                                                                                                                                                                   |
| Update                                        | None                                             | void                           | Updates the **Entities** map by processing state updates that the client has received from the server since the last update. Intended to be called once each game tick such that the **Entities** map consists of fresh data. Broadcasts **OnEntityAdded**, **OnEntityUpdated** and **OnEntityRemoved** |
| IsAuthenticated                               | None                                             | bool                           | Returns whether the user is authenticated.                                                                                                                                                                                                                                                              |
| IsConnected                                   | None                                             | bool                           | Returns whether the socket connection to the server is active.                                                                                                                                                                                                                                          |
| SendMessage                                   | const TMap\<FString, UMessageData\*>& Message    | void                           | Sends a message to the server. See **MessageData** class for a description of how to construct message data for sending.                                                                                                                                                                                |
| SendJSONMessage                               | const FString& JsonString                        | void                           | Sends a JSON message to the server. This can be used if your data is already in JSON format and as such can be sent directly.                                                                                                                                                                           |
| Logout                                        | None                                             | void                           | Logs out and disconnects the player from the server.                                                                                                                                                                                                                                                    |
| static ConvertVectorToServerCoords            | const FVector& InVector                          | TMap\<FString, float>          | Converts an Unreal Engine Vector into the coordinate system as used in Planetary Processing                                                                                                                                                                                                             |
| static ConvertVectorToServerCoordsMessageData | const FVector& InVector                          | TMap\<FString, UMessageData\*> | Helper function to convert an Unreal Engine Vector into Message Data with x, y and z values set in the Planetary Processing coordinate system. This is helpful if you want to communicate, for example, a change in the location of the player.                                                         |

#### Delegates

| Delegate          | Parameter                   | Description                                                                                                                                                                                                                                                |
| ----------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OnAuthenticated   | None                        | Fires when player is successfully authenticated.                                                                                                                                                                                                           |
| OnEntityAdded     | UEntity\* NewEntity         | Fires when a new **Entity** is added to the **Entities** map. This can be used to spawn a new **EntityActor**.                                                                                                                                             |
| OnEntityUpdated   | UEntity\* UpdatedEntity     | Fires when an existing **Entity** in the **Entities** map is updated. If an **EntityActor** has been spawned based on this **Entity**, the **EntityActor** will re-broadcast this as an **OnUpdated** event, which will be more useful for many use cases. |
| OnEntityRemoved   | const FString& EntityID     | Fires when an **Entity** is removed from the **Entities** map. If an **EntityActor** has been spawned based on this **Entity**, the **EntityActor** will re-broadcast this as an **OnRemoved** event, which will be more useful for many use cases         |
| OnConnectionError | const FString& ErrorMessage | Currently a catch-all for connection related errors, including authentication errors, loss of connection, failure to send messages or failure to parse updates.                                                                                            |



### Entity Class

This class represents an entity in the Planetary Processing system.

#### Properties

| Name     | Type                           | Description                                                                                                         |
| -------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| ID       | FString                        | The unique identifier of the entity.                                                                                |
| X        | double                         | X coordinate of the entity.                                                                                         |
| Y        | double                         | Y coordinate of the entity.                                                                                         |
| Z        | double                         | Z coordinate of the entity.                                                                                         |
| Data     | TMap\<FString, UMessageData\*> | Additional data of the entity. See **MessageData** class documentation                                              |
| DataJSON | FString                        | JSON representation of the additional entity data. Used if you want to handle deserialization into Structs yourself |
| Type     | FString                        | Type of the entity.                                                                                                 |

#### Functions

| Name              | Parameters | Return Type | Description                                                                                                                  |
| ----------------- | ---------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| GetLocationVector | None       | FVector     | Converts the Planetary Processing X, Y and Z coordinates of this entity into a Vector in the Unreal Engine coordinate system |



### EntityActor Class

This class extends the Unreal Engine Actor class and represents an Actor in your game client that has been spawned based on an **Entity** from your game simulation. You are intended to subclass **Entity Actor** to create Actors for each of your **Entity** types. As an example, a Tree class may be an **Entity Actor** with a mesh component, while a Cat class may be an **Entity Actor** with a different mesh component and some custom behaviours.

The **EntityActor** class is intended to have an **Entity** associated with it once created, via **SetEntity**. With this set, the **EntityActor** class will automatically broadcast **OnUpdated** and **OnRemoved** events accordingly as the **Entity** it is associated with is either updated or removed.

#### Properties

| Name      | Type                  | Description                                 |
| --------- | --------------------- | ------------------------------------------- |
| Entity    | UEntity\*             | The entity associated with this actor.      |
| OnUpdated | FOnEntityUpdatedEvent | Delegate called when the entity is updated. |
| OnRemoved | FOnEntityRemovedEvent | Delegate called when the entity is removed. |

#### Functions

| Name                           | Parameters         | Return Type | Description                                                                                                                                                                         |
| ------------------------------ | ------------------ | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SetEntity                      | UEntity\* InEntity | void        | Sets the entity for this actor.                                                                                                                                                     |
| SetActorLocationToEntityCoords | None               | void        | Converts the Planetary Processing X, Y and Z coordinates of the associated Entity into a Vector in the Unreal Engine coordinate system and sets the Actor's location to this Vector |



### MessageData Class

The **MessageData** class in is designed to serve as a flexible container for arbitrary data, akin to JavaScript objects. Its primary purpose is twofold:

* **Deserialization from JSON:** It allows data received from the server in JSON format to be efficiently parsed and stored in a way that can be accessed within Unreal Engine. This ensures that arbitrary data stored on each **Entity** (strings, numbers, booleans, arrays and nested objects) can be translated into native Unreal Engine data structures.
* **Serialization to JSON:** Conversely, the class enables the construction of arbitary data structures which can be serialized and sent to the server as the body of the message in a _SendMessage_ call.

This is particularly useful when using Blueprints to both access data on **Entity** instances and also when constructing messages for **SendMessage**. Should you want to handle serialization and deserialization yourself, the original JSON of an **Entity** is available in the **DataJSON** property and raw JSON messages can be sent using the **SendJSONMessage** function of the **PlanetaryProcessing**, so **MessageData** is entirely optional.

#### Functions

| Name                              | Parameters                                  | Return Type                    | Description                                                                         |
| --------------------------------- | ------------------------------------------- | ------------------------------ | ----------------------------------------------------------------------------------- |
| static ConvertBoolToMessageData   | bool Value                                  | UMessageData\*                 | Implicit conversion of a boolean value to a MessageData instance.                   |
| static ConvertIntToMessageData    | int32 Value                                 | UMessageData\*                 | Implicit conversion of an integer value to a MessageData instance.                  |
| static ConvertFloatToMessageData  | float Value                                 | UMessageData\*                 | Implicit conversion of a float value to a MessageData instance.                     |
| static ConvertStringToMessageData | const FString& Value                        | UMessageData\*                 | Implicit conversion of a string value to a MessageData instance.                    |
| static ConvertArrayToMessageData  | const TArray\<UMessageData\*>& Value        | UMessageData\*                 | Implicit conversion of an array of MessageData instances to a MessageData instance. |
| static ConvertObjectToMessageData | const TMap\<FString, UMessageData\*>& Value | UMessageData\*                 | Implicit conversion of a map of MessageData instances to a MessageData instance.    |
| GetString                         | None                                        | FString                        | Returns the stored string value, if type is String.                                 |
| GetDouble                         | None                                        | double                         | Returns the stored double value, if type is Double.                                 |
| GetInt                            | None                                        | int32                          | Returns the stored integer value, if type is Int.                                   |
| GetBool                           | None                                        | bool                           | Returns the stored boolean value, if type is Bool.                                  |
| GetObject                         | None                                        | TMap\<FString, UMessageData\*> | Returns the stored object value, if type is Object.                                 |
| GetArray                          | None                                        | TArray\<UMessageData\*>        | Returns the stored array value, if type is Array.                                   |
| GetActiveType                     | None                                        | EVariantType                   | Returns the active variant type of the message data.                                |
| SerializeToJson                   | None                                        | FString                        | Serializes the message data to JSON format.                                         |
| static SerializeMapToJson         | const TMap\<FString, UMessageData\*>& Map   | FString                        | Serializes a map of UMessageData to JSON format.                                    |
| DeserializeFromJson               | const FString& JsonString                   | void                           | Deserializes JSON string into message data.                                         |



## Blueprints

The Planetary Processing plugin is packaged with a number of example Blueprints. These serve to show how to carry out some basic functions using the Planetary Processing plugin.

### PP\_ExamplePlayerController

This blueprint is provided to demonstrate how to initialize your Planetary Processing instance and then how to carry out the fundamental processes required to make use of it.

#### Variables

| Name                        | Type                                        | Description                                                                                                                                                                          |
| --------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| PlanetaryProcessingInstance | Planetary Processing Object Reference       | Reference to the Planetary Processing singleton.                                                                                                                                     |
| LoginWidgetInstance         | PP Example Login Object Reference           | Stores a reference to the Login Widget when it is shown.                                                                                                                             |
| LocationMessage             | Map\<string: Message Data Object Reference> | A message to be used in a call to **SendMessage**.                                                                                                                                   |
| EntityMap                   | Map\<string: Entity Actor Class Reference>  | Used to map Entity types to the Entity Actor they should spawn. The default values for this variable map both _cat_ and _tree_ to the **PP\_ExampleEntityActor** as a demonstration. |

#### Authentication

![Authentication](https://planetaryprocessing.io/static/img/unreal_authentication.png)

1. Initializes PlanetaryProcessing singleton with a Game ID (Game ID available in the [Planetary Processing Panel](https://panel.planetaryprocessing.io/))
2. Creates an instance of **PP\_ExampleLoginWidget** and adds it to the viewport
3. Binds the following to the **OnAuthenticated** event from the PlanetaryProcessing singleton 1. Short delay to ensure subsequent bindings are handled by the game thread 2. Remove the login widget from the viewport 3. Unbind all events from **OnAuthenticated** 4. Set up bindings for **OnEntityAdded** and **OnConnectionError** events from the PlanetaryProcessing singleton (see **Entity Actor Spawning** and **Connection Error Handling**) 5. Continues with game logic, in this case a custom event named StartGame

#### Entity Actor Spawning

![Entity Actor Spawning](https://planetaryprocessing.io/static/img/unreal_actor_spawning.png)

1. Looks up the **EntityActor** class reference in the **EntityMap** variable using the _type_ from the **OnEntityAddedEvent**
2. Uses the **GetLocationVector** function of the added Entity to create a Vector for the initial location of the **EntityActor**.
3. Spawns an instance of the **EntityActor**
4. Calls **SetEntity** on the **EntityActor** to associate it with the **Entity** that we are representing

If you wish, you can extend the script to remove spawning for the client player entity by adding a branch that filters entities with the same ID as the planetary processing instance's UUID. This only removes the player's own entity from the client, not other players.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

#### Connection Error Handling

![Connection Error Handling](https://planetaryprocessing.io/static/img/unreal_error_handling.png)

1. Unbinds all events from **OnEntityAdded** and **OnConnectionError**
2. Calls a custom event called **ShowLogin**, which feeds back into the authentication flow above

#### Update Entities

![Update Entities](https://planetaryprocessing.io/static/img/unreal_update_entities.png)

1. On each tick, check if we are authenticated by calling **IsAuthenticated** on the PlanetaryProcessing instance
2. If we are, call the **Update** function of the PlanetaryProcessing instance to process state changes received
3. _additonal_ - after this, we also call the **SendPlayerCoords** custom event (see **Send Player Coordinates Message**)

#### Send Player Coordinates Message

![Send Player Coordinates Message](https://planetaryprocessing.io/static/img/unreal_send_player_coords_message.png)

1. Called on each tick (see last step of **Update Entities** above)
2. Gets the Location of the Player Pawn Actor
3. Calls the **ConvertVectorToServerCoordsMessageData** static function from the PlanetaryProcessing class. This gives us a map of strings to MessageData with the **x**, **y** and **z** values set based on the player position, adjusted to match the Planetary Processing coordinate system
4. Uses the implicit conversion of a boolean to **MessageData** as the value for a new key of _threedee_ in our map
5. Calls **SendMessage** on our PlanetaryProcessing instance with our message map as an argument



### PP\_ExampleEntityActor

This blueprint shows how to bind to the **OnUpdated** and **OnRemoved** events for an **EntityActor**.

#### Event Binding

![Entity Actor Event Binding](https://planetaryprocessing.io/static/img/unreal_entiaty_actor_binding.png)

1. Binds to the **OnUpdated** event for the **EntityActor**. Calls **SetActorLocationToEntityCoords** to update the Actor's location to match the **Entity**
2. Binds to the **OnRemoved** event for the **EntityActor**. Destroys the Actor when this event is broadcast

Note: for the actor to move correctly, following the server-side location, the actor must have physics disabled.

If you wish to retrieve the location vector of an entity before it is set, you can do so with the following example. Replace the print function with functions of your choice. This is particularly useful for smoothing movement on the client side.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>



### PP\_ExampleLoginWidget

This is a simple login widget which calls the **Connect** function and handles errors.

#### Login Click

![Login Click](https://planetaryprocessing.io/static/img/unreal_login_click.png)

1. Hides the error message (effectively resetting the error state of the login form)
2. Gets the PlanetaryProcessing singleton instance
3. Calls the **Connect** function on the PlanetaryProcessing instance with the username and password from the form

#### Error Handling

![Login Error Handling](https://planetaryprocessing.io/static/img/unreal_login_error_handling.png)

1. Binds to the **OnConnectionError** event when the widget is initialized
2. Sets the error message to be visible when **OnConnectionError** is broadcast



## API&#x20;

### PlanetaryProcessing Class

### Entity Class

### EntityActor Class

### MessageData Class

### PP\_ExamplePlayerController


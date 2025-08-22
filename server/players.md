# Players

Players are a special [Type](entities.md#types-and-behaviour-scripting) of entity. They represent a game client's connection to the server and possess some unique features.



## Player Connections

A player entity is created automatically when a player joins your game server. As such, players cannot be created like other entities (using the entity [Create](../api-reference/entity-api/create.md) function in the server side code). They will exist for as long as the game client is running and connected.&#x20;

<figure><img src="../.gitbook/assets/PlayerCreation (2).png" alt="" width="563"><figcaption></figcaption></figure>



## Player Messaging

All messages sent from the client to the server will be received by the player entity on the server side, by default. These can be handled by the `player.lua` file's [`message`](entities.md#message) function. The [Client](entities.md#message) field of the message will be true.

Messages can be sent to specific entities, besides the player, using [Direct Entity Messaging](entities.md#direct-entity-messaging).

<figure><img src="../.gitbook/assets/ClientToServer.png" alt=""><figcaption></figcaption></figure>



## Username and Password Authentication

Players can either join anonymously or using Username and Password Authentication, depending on your Game Settings.

Anonymous players are not required to log in and their [Data](../api-reference/entity-api/entity/fields/data.md) is cleared when they disconnect.

Players which log in using a username and password will use the same player entity when they next reconnect, rather than creating a new one. This means their [ID](../api-reference/entity-api/entity/fields/id.md) and [Data](../api-reference/entity-api/entity/fields/data.md) will persist even after they disconnect. A field storing their `username` is also added to the Data table automatically.

```lua
-- player.lua
self.Data.username == "Catman123"
```



## Players and Chunks

As a player moves around the game world, they will load chunks in a 3x3 grid around them. This is the main way that chunks are generated in the world.

Because chunks with a player in will always be loaded regardless, changing the [Chunkloader](../api-reference/entity-api/entity/fields/chunkloader.md) or [Transient](../api-reference/entity-api/entity/fields/transient.md) entity fields will have no effect on a player entity.

<figure><img src="../.gitbook/assets/ChunkGeneration (1).png" alt="" width="563"><figcaption></figcaption></figure>

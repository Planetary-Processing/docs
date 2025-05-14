# LÖVE

The LÖVE SDK integrates our multiplayer platform with LÖVE. The library provides methods and variables to use within your game client code - these communicate changes in state from your server side simulation to the game client itself.

If you are new here, we recommend starting with the [LÖVE Quickstart Guide](../quick-start/love.md).

## Pre-requisites

* LÖVE v11.0+

## Installation

The LÖVE SDK can be installed by cloning the latest version from our [GitHub](https://github.com/planetary-processing/) into your a folder called `sdk` within your LÖVE game directory.

```sh
git clone https://github.com/planetary-processing/love2d-sdk sdk
```

You can then import the SDK like so:

```lua
local sdk = require(“sdk.sdk”)
```



## SDK Object

The SDK object (the imported SDK) can be used to call a variety of functions. These allow you to connect to and pull information from the game server.

### [sdk.init](love.md#sdk-object-1)

* Establish a connection

The first thing you need to do when your game starts is initialise the SDK. To do this, run the [`sdk.init`](love.md#sdk-object-1) function within `love.load`.  This function takes the `game_id` parameter. The Game ID of your game can be found on the [web panel](https://panel.planetaryprocessing.io/games).&#x20;

Our LÖVE SDK only supports anonymous authentication at present, so you do **not** need supply a username and password.

```lua
function love.load()
    sdk.init(123)
end
```



### [sdk.join](love.md#sdk-object-1)

* Join with a player

After a connection has been established by [`sdk.init`](love.md#sdk-object-1), the function [`sdk.join`](love.md#sdk-object-1) will spawn the player into the game world.

```lua
function love.load()
    sdk.init(123)
    sdk.join()
end
```



### [sdk.update](love.md#sdk-object-1)

* Receive server messages

The [`sdk.update`](love.md#sdk-object-1) function should be called within `love.update`. It will pass data about entities and chunks from the server to the client, then save it in the SDK object's variables.

```lua
function love.update(dt)
    sdk.update()
end
```



### [sdk.message](love.md#sdk-object-1)

* Send messages to the server

You can call the [`sdk.message`](love.md#sdk-object-1) function to send a message to the server. This message will be received by the `player.lua` file's [`message`](../server/entities.md#message) function on the server.

```lua
sdk.message({x=1, y=-1})
```



### [sdk.server\_to\_client](love.md#sdk-object-1)

* Receive manual server messages

The [`sdk.server_to_client`](love.md#sdk-object-1) variable can store a function. This function triggers when the client receives  a message manually sent from the server by [`api.client.Message()`](../api-reference/client-api/message.md).

```lua
sdk.server_to_client = function (message)
    print(message)
end
```



## Game World Objects

Game worlds are made up of [entities](love.md#entity-object), which represent objects and creatures, and [chunks](love.md#chunk-object) to represent the world space.

### Entity Object

Data about [entities](love.md#entity-object-1) is passed to the LÖVE  client using [`sdk.update`](love.md#sdk-object-1). You can access all the entities within [`sdk.entities`](love.md#sdk-object-1) (which can be iterated over) and draw them based each entity's data.&#x20;

Entities have [types](../server/entities.md#types-and-behaviour-scripting) used to categorise their behaviour on the server side backend. They can be used to access groups of entities. Individual entities can be accessed using the [`EntityID`](love.md#entity-object-1) field.

The [`EntityID`](love.md#entity-object-1) of the player entity associated with this client is also stored in [`sdk.uuid`](love.md#sdk-object-1). This makes it easier to access the player entity directly with `sdk.entities[sdk.uuid].`

```lua
-- draw cat entities and player as 'C' and 'P' respectively
function love.draw()
	local player = sdk.entities[sdk.uuid]
	love.graphics.print("P", player.X, -player.Y)

	for id,entity in pairs(sdk.entities) do
		if entity.Type == "cat" then
			-- NOTE Planetary Processing's 'y' axis is inverted from LÖVE 
			love.graphics.print("C", entity.X, -entity.Y)
		end
	end
end
```

### Chunk Object

Data about [chunks](love.md#chunk-object-1) is passed to the LÖVE  client using [`sdk.update`](love.md#sdk-object-1). You can access all the chunk data within `sdk.chunks` (which can be iterated over).&#x20;

<pre class="language-lua"><code class="lang-lua">-- draw chunk borders when the chunk  size is 64
local chunk_size = 64

<strong>function love.draw()
</strong>    local world_x 
    local world_y 
    
    for id,chunk in pairs(sdk.chunks) do
        world_x = chunk.X * chunk_size
        world_y = chunk.Y * chunk_size
    
        love.graphics.line(world_x, world_y, world_x, world_y + chunk_size)
        love.graphics.line(world_x, world_y, world_x + chunk_size, world_y)
<strong>    end	
</strong>end
</code></pre>



## API

Below are reference tables of the API available within LÖVE 's environment.

The SDK exposes various public methods which can be accessed within scripts by importing the SDK like so:

```lua
local sdk = require(“sdk.sdk”)
```

To connect to the server and join with the player, for example:

```lua
sdk.init(123)
sdk.join()
```

### SDK Object

#### **Variables**

| Variable           | Type                                                                                                                 | Description                                                                                     |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `entities`         | <p><code>entities: table({</code></p><p><code>id: string</code><br><code>entity: table</code><br><code>)}</code></p> | List of entities the client can see.                                                            |
| `chunks`           | <p><code>table: table({</code></p><p><code>id: string</code><br><code>chunk: table</code><br><code>)}</code></p>     | List of chunks the client can see.                                                              |
| `uuid`             | `string`                                                                                                             | UUID of the player associated with this client.                                                 |
| `server_to_client` | `function(table)`                                                                                                    | Function which triggers, when a message is manually sent from the server to this player client. |

#### **Methods**

| Method    | Parameters     | Description                                            |
| --------- | -------------- | ------------------------------------------------------ |
| `init`    | `game_id: int` | Called to connect to the Planetary Processing servers. |
| `join`    | None           | Spawn into the world.                                  |
| `update`  | None           | Call this method inside `love.update`.                 |
| `message` | `msg: table`   | Send a message to the player entity on the server.     |
| `leave`   | None           | Disconnect the player from the world.                  |
| `quit`    | None           | Sever the connection to the game.                      |



### Entity Object

**Fields**

| Field      | Type     | Description                  |
| ---------- | -------- | ---------------------------- |
| `EntityID` | `string` | UUID of the entity.          |
| `X`        | `float`  | X coordinate in world units. |
| `Y`        | `float`  | Y coordinate in world units. |
| `Data`     | `table`  | Data of the entity.          |
| `Type`     | `string` | Type of the entity.          |



### Chunk Object

#### Fields

| Field  | Type     | Description                  |
| ------ | -------- | ---------------------------- |
| `ID`   | `string` | UUID of the chunk.           |
| `X`    | `float`  | X coordinate in chunk units. |
| `Y`    | `float`  | Y coordinate in chunk units. |
| `Data` | `table`  | Data of the chunk.           |


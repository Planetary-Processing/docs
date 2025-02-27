# Entities

Your game is made up of entities each of which have an associated [UUID](entities.md#entity), [position](entities.md#entity), [type](entities.md#types-and-behaviour-scripting) and [data](entities.md#entity). These fields can be edited and used alongside [Entity Functions](entities.md#methods) to create gameplay.

<figure><img src="../.gitbook/assets/Entity (3).png" alt="" width="375"><figcaption></figcaption></figure>

## Types & Behaviour Scripting

[Entities](entities.md#entity) are divided into types, each of which have their own behaviour. This behaviour is determined by a script in the `entity` folder. To create a new entity type, simply create a new file in this folder, call the file `entity_type_name.lua` and add the stub code shown below:

```lua
-- entity_type_name.lua file
local function init(self)
end

local function update(self, dt)
end

local function message(self, msg)
end

return {init=init,update=update, message=message}
```

There must always be an [Entity Type](entities.md#entity) called `player` which acts as the abstraction for both the player client and other players within the simulation.

These three functions [init()](entities.md#init), [update()](entities.md#update), and [message()](entities.md#message) govern the behaviour of all entities of this type. Each must be referenced and returned in a table at the end of the file.



### Init

The [init()](entities.md#init-1) function is called when the entity is created and should be used to set initial state (for example, setting the [Chunkloader](entities.md#entity) or [Data](entities.md#entity) fields).

```lua
local function init(self)
end
```

### Update

The [update()](entities.md#update-1) function is called each tick to update the [entity](entities.md#entity), for movement and repeating processes. The parameter [`dt`](entities.md#update-1) is the time since the last [update](entities.md#update-1) in seconds.

```lua
local function update(self, dt)
end
```

### Message

The [message()](entities.md#message-1) function **receives** messages sent to this entity and [processes](entities.md#entity-messaging) them. Messages can come from [other entities](entities.md#entity-messaging) or the game client.

All messages sent from the game client are sent to the `player` entity, and received by the [message()](entities.md#message-1) function in `player.lua`. If a message is sent from the client, its message's [`Client`](entities.md#message-1) field will be true.

```lua
local function message(self, msg)
end
```

The `msg` argument supplied to this function is defined as follows:

| Field  | Type  | Description                                                                                       |
| ------ | ----- | ------------------------------------------------------------------------------------------------- |
| Data   | table | Contains the content of the message.                                                              |
| Client | bool  | Indicates if the message is from a game client (true) or from another entity on a server (false). |



## Entity Messaging

Messages are sent using either [`api.entity.message()`](entities.md#entity-api) or by the [Client](entities.md#message). They are received by an entity's [`message`](entities.md#message) function.

For example, sending a message to a particular entity each tick would look like:

```lua
local function update(self, dt)
  api.entity.Message(receiverid, {message="hello there", from=self.ID})
end
```

And be received like this:

```lua
local function message(self, msg)
  print("The id of the sender is: ", msg.Data.from) -- prints self.ID
  print("The message is: ", msg.Data.message) -- print "hello there"
end
```

Messages sent from the client will always be received by the corresponding player entity, in the [`message`](entities.md#message-1) function of the `player.lua` file.



## Example Cat Entity

Below is the example `cat.lua` script which exists in the default template repository. The comments provide further information on what each line is doing.

```lua
-- cat.lua
-- init called on creation of entity
local function init(self)
    -- set self.Chunkloader = true means this entity's chunk will always remain loaded
    self.Chunkloader = true
    -- self.Data is an arbitrary table, this is persisted, populated here with target coordinates
    self.Data.target = { x = math.random(-64, 64), y = math.random(-64, 64) }
end

-- update called each simulation step, with dt being the number of seconds since last step (float)
local function update(self, dt)
    -- position must be fetched using self:GetPosition method
    x, y, _ = self:GetPosition()
    -- calculate distance from target on x and y axis
    local dx = self.Data.target.x - x
    local dy = self.Data.target.y - y
    -- if we're close enough then consider ourselves successful
    if dx * dx < 4 and dy * dy < 4 then
        self.Data.target = { x = math.random(-chunk.Size+1, chunk.Size*2-1), y = math.random(-chunk.Size, chunk.Size*2-1) }
    else
        -- determine largest direction of motion
        if dx * dx > dy * dy then
            dx = dx < 0 and -1 or 1
            dy = 0
        elseif dy * dy > dx * dx then
            dy = dy < 0 and -1 or 1
            dx = 0
        else
            dx = dx < 0 and -1 or 1
            dy = dy < 0 and -1 or 1
        end
        -- self:Move(dx, dy, dz) moves relative to current position
        -- by multiplying by dt we move at a constant speed regardless of performance
        -- we also have self:MoveTo(x, y, z) which moves to an absolute position
        -- note that z is left as 0 as this is a 2D game
        self:Move(dx * dt, dy * dt, 0)
    end
end

-- called when this entity receives a message
local function message(self, msg)
end

-- entity file must return table of this format
return { init = init, update = update, message = message }

```



## Entity API

A number of API calls are available for certain entity-related actions such as spawning entities, they are accessed by interfacing with the `api.entity` object in the global scope within all server-side scripts.

{% include "../.gitbook/includes/entity-api-table.md" %}



***

### Entity

#### Fields

| Field       | Type   | Description                                                                                                                                        |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID          | string | UUID of the entity. Read-only.                                                                                                                     |
| Type        | string | Type of the entity. Read-only.                                                                                                                     |
| Data        | table  | Custom lua table which can be used to store arbitrary data about the entity.                                                                       |
| Chunkloader | bool   | If true, the chunk containing this entity will remain loaded even if no players are present. Note that chunkloader entities are a premium feature. |
| Transient   | bool   | If true, when the chunk containing this entity is unloaded, it will not be persisted.                                                              |

#### Methods

Each of these methods takes the entity object `self` as its first parameter. Hence you may use `self:Move(dx, dy, dz)` instead of `self.Move(self, dx, dy, dz)`.

| Method                       | Parameters                                                                                                       | Returns                                                                                                   | Description                                                                                                                                                                                                                                                                |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Move(dx, dy, dz)`           | <p><code>dx: float</code><br><code>dy: float</code><br><code>dz: float</code></p>                                | None                                                                                                      | Move the entity relative to its current position.                                                                                                                                                                                                                          |
| `MoveTo(x, y, z)`            | <p><code>x: float</code><br><code>y: float</code><br><code>z: float</code></p>                                   | None                                                                                                      | Move the entity relative to the origin (teleport).                                                                                                                                                                                                                         |
| `GetPosition()`              | None                                                                                                             | <p><code>x: float</code><br><code>y: float</code><br><code>z: float</code> coordinates of the entity.</p> | Get the current position of the entity.                                                                                                                                                                                                                                    |
| `Remove()`                   | None                                                                                                             | None                                                                                                      | Delete this entity.                                                                                                                                                                                                                                                        |
| `GetNearbyEntities(dist)`    | `dist: float`                                                                                                    | Entity\[]                                                                                                 | Get entities within the specified distance. Please note that the maximum distance that can be reliably fetched is the game's chunk size, this is because GetNearbyEntities only searches 9 chunks centred on the entity's current chunk.                                   |
| `Warp(dimension, [x, y, z])` | <p><code>dimension: string</code><br><code>x: float</code><br><code>y: float</code><br><code>z: float</code></p> | None                                                                                                      | Transport this entity to (x,y,z) in the dimension specified. The coordinate fields are optional and default to (0,0,0) if not specified. Specifying just two coordinates will also work, for example, if your game is 2D or otherwise where 0 is the desired z-axis value. |



***

### Init

| Method | Parameters     | Return Value | Description                                                          |
| ------ | -------------- | ------------ | -------------------------------------------------------------------- |
| Init   | `self: Entity` | None         | A necessary function, to initialise an entity in its default state.  |



***

### Update

| Method | Parameters                                                  | Return Value | Description                                                        |
| ------ | ----------------------------------------------------------- | ------------ | ------------------------------------------------------------------ |
| Update | <p><code>self: Entity</code><br> <code>dt: float</code></p> | None         | A necessary function, to process an entity's behaviour each tick.  |



***

### Message

| Method  | Parameters                                                                                                                                       | Return Value | Description                                             |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | ------------------------------------------------------- |
| Message | <p><code>self: Entity</code></p><p><code>msg: table({</code><br>  <code>Data: table</code><br>  <code>Client: bool</code><br><code>)}</code></p> | None         | A necessary function, to receive messages to an entity. |

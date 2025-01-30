# Entities

Your game is made up of entities each of which have an associated UUID, position, type and data.

## The Entity Object

This section details the fields and methods available on each entity object.

**Fields**

| Field       | Type   | Description                                                                                                                                        |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID          | string | UUID of the entity. Read-only.                                                                                                                     |
| Type        | string | Type of the entity. Read-only.                                                                                                                     |
| Data        | table  | Custom lua table which can be used to store arbitrary data about the entity.                                                                       |
| Chunkloader | bool   | If true, the chunk containing this entity will remain loaded even if no players are present. Note that chunkloader entities are a premium feature. |
| Transient   | bool   | If true, when the chunk containing this entity is unloaded, it will not be persisted.                                                              |

**Methods**

Each of these methods takes the entity object `self` as its first parameter. Hence you may use `self:Move(dx, dy, dz)` instead of `self.Move(self, dx, dy, dz)`.

| Method                       | Parameters                                                                                                       | Returns                                                                                                   | Description                                                                                                                                                                                                                                                                |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Move(dx, dy, dz)`           | <p><code>dx: float</code><br><code>dy: float</code><br><code>dz: float</code></p>                                | None                                                                                                      | Move the entity relative to its current position.                                                                                                                                                                                                                          |
| `MoveTo(x, y, z)`            | <p><code>x: float</code><br><code>y: float</code><br><code>z: float</code></p>                                   | None                                                                                                      | Move the entity relative to the origin (teleport).                                                                                                                                                                                                                         |
| `GetPosition()`              | None                                                                                                             | <p><code>x: float</code><br><code>y: float</code><br><code>z: float</code> coordinates of the entity.</p> | Get the current position of the entity.                                                                                                                                                                                                                                    |
| `Remove()`                   | None                                                                                                             | None                                                                                                      | Delete this entity.                                                                                                                                                                                                                                                        |
| `GetNearbyEntities(dist)`    | `dist: float`                                                                                                    | Entity\[]                                                                                                 | Get entities within the specified distance. Please note that the maximum distance that can be reliably fetched is the game's chunk size, this is because GetNearbyEntities only searches 9 chunks centred on the entity's current chunk.                                   |
| `Warp(dimension, [x, y, z])` | <p><code>dimension: string</code><br><code>x: float</code><br><code>y: float</code><br><code>z: float</code></p> | None                                                                                                      | Transport this entity to (x,y,z) in the dimension specified. The coordinate fields are optional and default to (0,0,0) if not specified. Specifying just two coordinates will also work, for example, if your game is 2D or otherwise where 0 is the desired z-axis value. |

## Types & Behaviour Scripting

Entities are divided into types, each of which have their own behaviour. This behaviour is determined by a script in the `entity` folder; to create a new entity type, simply create a new file in this folder, call the file `entitytypename.lua` and add the stub code shown below:

```lua
local function init(self)
end

local function update(self, dt)
end

local function message(self, msg)
end

return {init=init,update=update, message=message}
```

These three functions, `init`, `update` and `message` are explained in detail later. There must always be an entity type called `player` which acts as the abstraction for players within the simulation.

These three functions, `init`, `update` and `message`, govern the behaviour of all entities of this type.

**Init**

This is called when the entity is created and should be used to set initial state (for example, setting the Chunkloader or Data fields).

```lua
local function init(self)
end
```

**Update**

This is called each tick to update the entity, dt is the time since the last update in seconds.

```lua
local function update(self, dt)
end
```

**Message**

This is called when another entity sends a message to this entity (note that the entity sending the message might not be in the same chunk). The message is of type message (see types below).

On the player entity this function also acts as the handler for messages received from a game client, which follow the same format.

```lua
local function message(self, msg)
```

The msg argument supplied to this function is defined as follows:

| Field  | Type  | Description                                                                                       |
| ------ | ----- | ------------------------------------------------------------------------------------------------- |
| Data   | table | Contains the content of the message.                                                              |
| Client | bool  | Indicates if the message is from a game client (true) or from another entity on a server (false). |

## Entity API

A number of API calls are available for certain entity-related actions such as spawning entities, they are accessed by interfacing with the `api.entity` object in the global scope within all server-side scripts.

| Method  | Parameters                                                                                                                              | Return Value | Description                                                                                                                                                                                                                  |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create  | <p><code>type: string</code><br><code>x: float</code><br><code>y: float</code><br><code>z: float</code><br><code>data: table</code></p> | `Entity`     | Creates a new entity in the current dimension and returns it. This function will automatically call the `init` function for the specified entity type. It fails if there is no entity type that matches the provided `type`. |
| Message | <p><code>entityid: string</code><br><code>message: table</code></p>                                                                     | None         | Sends a message to the entity with UUID `entityid` if it exists, this entity can be anywhere in the world.                                                                                                                   |

For example, sending a message to a particular entity each tick would look like:

```lua
local function update(self, dt)
  api.entity.Message(receiverid, {message="hello there", from=self.ID})
end
```

## Example

Below is the example cat.lua script which exists in the default template repository. The comments provide further information on what each line is doing.

```lua
-- init called on creation of entity
local function init(self)
  -- set self.Chunkloader = true means this entity's chunk will always remain loaded, and any chunks it wants to move into will be loaded
  self.Chunkloader = true
  -- self.Data is an arbitrary table, this is persisted, populated here with target coordinates
  self.Data.target = {x=math.random(-64,64),y=math.random(-64,64)}
end

-- update called each simulation step, with dt being the number of seconds since last step (float)
local function update(self, dt)
  -- position must be fetched using self:GetPosition method
  x, y, _ = self:GetPosition()
  -- calculate distance from target on x and y axis
  local dx = self.Data.target.x - x
  local dy = self.Data.target.y - y
  -- if we're close enough then consider ourselves successful
  if dx*dx < 4 and dy*dy < 4 then
    self.Data.target = {x=math.random(-96,96),y=math.random(-96, 96)}
  else
    -- determine largest direction of motion
    if dx*dx > dy*dy then
      dx = dx < 0 and -1 or 1
      dy = 0
    elseif dy*dy > dx*dx then
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
    self:Move(dx*dt, dy*dt, 0)
  end
end

-- called when this entity receives a message
local function message(self, msg)
end

-- entity file must return table of this format
return {init=init,update=update,message=message}
```

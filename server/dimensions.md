# Dimensions

Planetary Processing games can have multiple separate dimensions simulated in parallel, with entities transitioning between these seemlessly. This is useful for several use cases, for example:

* Creating separate parallel, persistent, dimensions akin to the Nether or End in Minecraft;
* Creating separate dimensions for matches, lobbies, dungeons or private sessions;
* Creating pocket dimensions, for example, a player's house or a dungeon.

By default, there is only one dimension with dimension ID of the empty string (`""`), but new dimensions can be created and destroyed with API calls.

## Creating and Deleting a Dimension

Creating a new dimension is simple, you simply call:

```lua
api.dimension.Create(dimension)
```

When a dimension is first loaded, a small number of chunks around the origin will be loaded for that chunk in the same manner as when a game is started. Note that if a dimension with this ID already exists, this will do nothing at all.

Similarly, to delete a dimension one calls:

```lua
api.dimension.Delete(dimension)
```

Note that when a dimension is deleted, it might not be unloaded immediately, if a player is still in that dimension it will wait for the player to leave, but will not load any new chunks.

## Sending an Entity Between Dimensions

When an entity is created, it is created in the current dimension. To send an entity between two dimensions you call the `Warp(dimension, x, y, z)` on the entity object. The coordinate fields are optional and default to (0,0,0) if not specified. Specifying just two coordinates will also work, for example, if your game is 2D or otherwise where 0 is the desired z-axis value. See below for an example:

```lua
-- init called on creation of entity
local function init(self)
end

-- update called each simulation step, with dt being the number of seconds since last step (float)
local function update(self, dt)
end

-- called when this entity receives a message
local function message(self, msg)
  self:Warp("test")
end

-- entity file must return table of this format
return {init=init,update=update,message=message}
```

In the above example, when this entity receives a message it will transport itself to the dimension with ID test. Note that if this dimension doesn't exist, this API call will fail and a message will be printed to the console. Additionally, an entity will only transition to a dimension if the required chunk is loaded, otherwise it will wait for it to be loaded, the same way entities wait if they attempt to move into a chunk in the same dimension (players, however, automatically load the chunks they need as always).

## Generating a Dimension

The `init()` function is called as normal for chunks in all dimensions, if you wish to vary generation by dimension then you can access the dimension ID of the current dimension in the `chunk.Dimension` field.

## Monitoring a Dimension

On the control panel, within the game overview page you can see a dropdown at the top left of the map which allows you to select which dimension you are monitoring.

![Dimension dropdown](https://planetaryprocessing.io/static/img/dimensions.png)

## API Reference

The full dimensions API is shown below, these functions are accessed by interfacing with the `api.dimension` object in the global scope within all server-side scripts.

| Method | Parameters          | Return Value | Description                                                    |
| ------ | ------------------- | ------------ | -------------------------------------------------------------- |
| Create | `dimension: string` | None         | Creates a new dimension; idempotent.                           |
| Delete | `dimension: string` | None         | Delete a dimension by ID, cannot delete the default dimension. |
| List   | None                | `string[]`   | Get all dimensions for this game in a table.                   |

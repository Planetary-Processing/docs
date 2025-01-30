# Chunks

Planetary Processing works by dividing your world into chunks. Chunks are cuboid segments of the world of fixed width and depth, and of infinite height.

## Chunk Information

One can access information about the current chunk from the global variable '`chunk`'.

| Field     | Type   | Description                                                                                                                       |
| --------- | ------ | --------------------------------------------------------------------------------------------------------------------------------- |
| X         | int    | X coordinate in chunk space (world x coordinate divided by chunk size).                                                           |
| Y         | int    | Y coordinate in chunk space (world y coordinate divided by chunk size).                                                           |
| Size      | int    | Size of the chunk in world units.                                                                                                 |
| Generated | bool   | Indicates whether or not this chunk has been loaded before. This should not be used outside of the init function (see below).     |
| Dimension | string | ID of the current dimension.                                                                                                      |
| Data      | table  | Custom lua table which can be used to store arbitrary data about the chunk, such as heightmap or other terrain info, for example. |

## Initialising a Chunk

You may have noticed the existence of a file called `init.lua` in the root of your game's git repository. This should contain a function called `init` which is called whenever this chunk is loaded and is designed to initiate the state of this chunk.

Below is the example from the template that comes pre-installed in your repository. You can see that we spawn things only if `chunk.Generated` is false, this is to ensure we only spawn things on the first load. However, in a game where state is not persisted (i.e. entities have `entity.Transient = true`, this might be useful on every load).

```lua
--[[ init is called every time a chunk is loaded
if the chunk has been loaded before then chunk.Generated will be true, else it will be false ]]
function init()
  -- do this block of code if the chunk is at (0,0) in chunk space and has not been generated yet
  if chunk.X == 1 and chunk.Y == 1 and not chunk.Generated then
    for i=1,20 do
      -- type, x, y, z, data
      -- type must match the name of one of the lua files in entity folder of this repo
      -- data is an arbitrary lua table
      api.entity.Create("cat", -35, 25, 1, {name="jar jar "..i})
    end
  end
  if not chunk.Generated then
    for i=1,math.random(4) do
      --[[ create some tree entities at random locations
      'math' and other standard lua library functions (excluding io/os operations) are included ]]
      api.entity.Create("tree", chunk.X*chunk.Size + math.random()*chunk.Size, chunk.Y*chunk.Size + math.random()*chunk.Size, 1, {})
    end
  end
end
```

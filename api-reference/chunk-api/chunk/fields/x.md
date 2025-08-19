# X

`chunk.X`

The position of the chunk along the horizontal axis of the world map. This value is in [chunk coordinates](../../../../server/chunks.md#chunk-size-and-coordinates), not world units.

Multiplying a chunk's coordinate values by its chunk [Size](size.md) will give the equivalent world coordinates for the bottom left hand corner of that chunk.



| Type  | Initialised Value  | Description                                             |
| ----- | ------------------ | ------------------------------------------------------- |
| `int` | eg. `7`            | The X position of the chunk, in chunk units. Read-only. |



Example:

Use a chunk's X and Y fields to spawn a cat in the centre of the chunk.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if chunk.X == 1 and chunk.Y == 2 and not chunk.Generated then 
<strong>        local corner_x = chunk.X * chunk.Size
</strong>        local corner_y = chunk.Y * chunk.Size   
        local centre_x = corner_x + chunk.Size / 2
        local centre_y = corner_y + chunk.Size / 2
         
        api.entity.Create("cat", centre_x, centre_y, 0, {})
        
        local chunk_pos_x = math.floor(centre_x / chunk.Size)
        local chunk_pos_y = math.floor(centre_y / chunk.Size)
        
        print("The cat is at world coordinates ("..centre_x..","..centre_y.."). "..
              "It is in the centre of Chunk ("..chunk_pos_x..","..chunk_pos_y..").") 
    end
end

-- Calculate the world coordinate position for the corner of Chunk (1,2).
-- Calculate the world coordinate position for the centre of Chunk (1,2).
-- Create a cat entity at the centre of the chunk.
-- Reverse engineer the chunk coordinate position from the entity's world coordinates.
-- Print the cat's position and the chunk's position.

-- Prints:
-- The cat is at world coordinates (96,160).
-- It is in the centre of Chunk (1,2).
</code></pre>

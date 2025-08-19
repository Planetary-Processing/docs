# Size

`chunk.Size`

The uniform width and height of the square chunks which make up the game world. This value is in world units.

This value can be altered in the Game Settings of the panel map.

| Type  | Initialised Value  | Description                                        |
| ----- | ------------------ | -------------------------------------------------- |
| `int` | `64`               | The width of the chunk, in world units. Read-only. |



Example:

Use chunk size to spawn a cat in the centre of the chunk.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if chunk.X == 1 and chunk.Y == 2 and not chunk.Generated then 
<strong>        local corner_x = chunk.X * chunk.Size
</strong><strong>        local corner_y = chunk.Y * chunk.Size   
</strong>        local centre_x = corner_x + chunk.Size / 2
        local centre_y = corner_y + chunk.Size / 2
         
        api.entity.Create("cat", centre_x, centre_y, 0, {})
        
        local chunk_distance_x = centre_x / chunk.Size
        local chunk_distance_y = centre_y / chunk.Size
        
        print("The cat is at world coordinates ("..centre_x..","..centre_y.."). "..
              "With a chunk size of "..chunk.Size..", this is "..
              chunk_distance_x.." chunk(s) width and "..
              chunk_distance_y.." chunk(s) height from the origin (0,0).")
    end
end

-- Use the chunk size to calculate the world coordinate position of Chunk (1,2).
-- Half the chunk size to calculate the centre point of Chunk (1,2).
-- Create a cat entity at the centre of the chunk.
-- Calculate the entity's world coordinates in terms of chunk units.
-- Print the cat's position and its distance from the origin in terms of chunks.

-- Prints:
--[[ The cat is at world coordinates (96,160). With a chunk size of 64, 
     this is 1.5 chunk(s) width and 2.5 chunk(s) height from the origin (0,0). ]]--
</code></pre>

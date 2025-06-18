# GetPosition



`entity:GetPosition()`

Returns the world coordinate position of this entity, relative to the origin (0,0,0).



Returns:

| Type    | Description                      |
| ------- | -------------------------------- |
| `float` | The X coordinate of this entity. |
| `float` | The Y coordinate of this entity. |
| `float` | The Z coordinate of this entity. |



Example:

Get the position of the entity in world units, and calculate its position in chunk units.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function update(self, dt)
<strong>    local x,y,z = self:GetPosition()
</strong>    
    local chunk_x = math.floor(x/chunk.Size)
    local chunk_y = math.floor(y/chunk.Size)
    
    print("This entity is at world position ("..x..","..y.."), "..
            "so it is within the chunk at coordinate ("..chunk_x..","..chunk_y..").")
end

-- Each update, get the current position of the entity.
-- Calculate the chunk coordinate the entity is in, by dividng by chunk size.
-- Print the exact entity position and the chunk coordinate.

-- Prints (asssuming a chunk size of 64):
-- This entity is at world position (72,131) so it is within chunk coordinate (1,2)
</code></pre>


# MoveTo



`entity:MoveTo(x, y, z)`

Moves an entity to an absolute coordinate position, relative to the world's origin (0,0,0).

This function will instantly move this entity from its current position, to a new XYZ position, defined by its parameters.&#x20;



Parameters:

| Name | Type    | Description                                                       |
| ---- | ------- | ----------------------------------------------------------------- |
| x    | `float` | The position the entity will teleport to, in terms of the x axis. |
| y    | `float` | The position the entity will teleport to, in terms of the y axis. |
| z    | `float` | The position the entity will teleport to, in terms of the z axis. |



Example:

Move the entity to position (16,16,0) and print.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function update(self, dt)
    local recent_x, recent_y = self:GetPosition()
    
    if recent_x ~=16 or recent_y ~= 16 then
<strong>        self:MoveTo(16, 16, 0)
</strong>        x, y = self:GetPosition()
        print("Moved this entity from ("..recent_x..","..recent_y..") "..
                "to ("..x..","..y..").")
    else
        print("This entity is at ("..recent_x..","..recent_y..") "..
                 "and will not move again.")
    end
    
end

-- Each update, get the current position of the entity.
-- Check if the entity's position is (16,16,0).
-- If not, move the entity to (16,16,0) and print the change in position.
-- If the entity is already at (16,16,0), print that the entity is stationary.

-- Prints:
-- Moved this entity from (0,0,0) to (16,16,0).
-- This entity is at (16,16,0) and will not move again.
</code></pre>

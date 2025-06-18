# Move



`entity:Move(dx, dy, dz)`

Moves an entity relative to its current position.

This function will move this entity from its current position, along the x, y, and z axis for a distance defined by its parameters.



Parameters:

| Name | Type    | Description                                                |
| ---- | ------- | ---------------------------------------------------------- |
| dx   | `float` | The distance the entity will move, in terms of the x axis. |
| dy   | `float` | The distance the entity will move, in terms of the y axis. |
| dz   | `float` | The distance the entity will move, in terms of the z axis. |



Example:

Move the entity randomly, and print how far it has moved.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Data.recent = {x=0,y=0}
end

local function update(self, dt)
    local x, y = self:GetPosition()
    
    if x ~= self.Data.recent.x 
    or y ~= self.Data.recent.y then
        x_change = x - self.Data.recent.x
        y_change = y - self.Data.recent.y
        
        print("This entity has moved "..
                x_change.." units in X and "..
                y_change.." units in Y.")    
    end
    
    self.Data.recent.x = x 
    self.Data.recent.y = y
    
    local random_x = math.random(1,10)/10 - 0.5
    local random_y = math.random(1,10)/10 - 0.5
    if math.random() &#x3C; 0.1 then
<strong>        self:Move(random_x, random_y, 0)
</strong>    end
end

-- Each update, get the current position of the entity.
-- Check if the entity's position has changed.
-- If there is a change, print the distance it has moved in X and Y.
-- Record the current position as a snapshot to compare later.
-- Each tick, on a 10% chance, move a random amount in the X and Y directions.

-- Prints (if moved):
-- This entity has moved 0.3 units in X and -0.2 units in Y.
</code></pre>

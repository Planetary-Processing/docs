# update



`function update(self, dt)`

Performs the continuous processing of this entity instance, each tick.&#x20;

The `dt` parameter is the interval between each tick, (approximately 1 / Target Tick Rate). Repeating incremental processes, like movement, can be standardised by being multiplied by `dt`. This reduces the variation caused by differing hardware processing speeds.

As a necessary entity method, this function should be returned by the entity.lua file. It must be in a table under the key `update`.



Parameters:

| Name | Type     | Description                                                           |
| ---- | -------- | --------------------------------------------------------------------- |
| self | `Entity` | This entity itself.                                                   |
| dt   | `float`  | The time elapsed in seconds  between this tick and the previous one.  |



Example:

Update an entity's position each tick, to move it towards a location.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
<strong>local function update(self, dt)
</strong>    local x, y = self:GetPosition()
    local target = {x = 100, y = 100}
    local drag = 100
	
    local differential_in_x = (target.x - x)/drag
    local differential_in_y = (target.y - y)/drag

    self:Move(
        differential_in_x * dt, 
        differential_in_y * dt, 
        0)
    
    print("This entity is gradually moving towards position (100,100,0)."
end

return { init = function (self) end,
         update = update,
         message = function (self, msg) end }

-- Get the current position of the entity, each tick.
-- Set the XY location for the entity to move to.
-- Calculate the total distance left each tick, between the entity and (100,100,0).
-- Apply a drag, to only move a small percentage of the total distance at a time.
-- Move the entity by a small amount each tick.
-- Return the 'update' function as a value in a table.

-- Prints (every tick):
-- This entity is gradually moving towards position (100,100,0).
</code></pre>

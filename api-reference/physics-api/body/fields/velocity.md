# Velocity

`body.Velocity`

The speed the entity body is currently travelling in the X, Y, Z directions per second, in regards to physics. It is governed by both applied forces and collisions.&#x20;

Non-physical movement, using the [Move](../../../entity-api/entity/methods/move.md) or [MoveTo](../../../entity-api/entity/methods/moveto.md) functions, will not influence this value.

Direct changes to a body's velocity value will only take effect, while physical forces are already being processed. To alter the velocity of a stationary entity, the [ApplyForce](../methods/applyforce.md) function should be used.&#x20;

| Type                      | Initialised Value                                                                         | Description                                                                  |
| ------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| [`Vector`](../../vector/) | <p><code>{X = 0,</code> </p><p>    <code>Y = 0,</code> </p><p>    <code>Z = 0}</code></p> | The speed the entity body is currently travelling in the X, Y, Z directions. |



Example:

Use an entity body's velocity to check whether it has collided and, if so, prevent it rebounding.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local box_shape = api.physics.NewBoxShape(8, 8, 8) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box   
    self.Body:ApplyForce(api.physics.NewVector(1,1,0))
end

local function update(self, dt)
    if self.Data.last_velocity then
        local last_velocity = api.physics.NewVector(
            self.Data.last_velocity.X, 
            self.Data.last_velocity.Y, 
       	    self.Data.last_velocity.Z
        )
<strong>	local current_velocity = self.Body.Velocity
</strong>	
	local current_direction = current_velocity:Normalize()
	local last_direction = last_velocity:Normalize()
	local is_rebounded = last_direction:Dot(current_direction) &#x3C; 0
		
	if is_rebounded then
            self.Body.Velocity = last_velocity 
            print("An inelastic collision has occurred.")
	end
    end
    self.Data.last_velocity = self.Body.Velocity
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- On the first update apply a force to make the entity move in direction (1,1,0).
-- Record the previous velocity of the body at the end of each update.
-- Each update, check whether the direction has become inverted, since the last update.
-- If the entity has rebounded, set its velocity back to the value before the rebound.

-- Prints:
-- An inelastic collision has occurred.
</code></pre>

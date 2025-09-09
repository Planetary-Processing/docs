# Sub

`vector:Sub(vector)`

Subtracts the individual X, Y, and Z values of this vector from the corresponding values of another vector, to return a third vector comprised of the subtracted values.



Parameters:

| Name   | Type            | Description                      |
| ------ | --------------- | -------------------------------- |
| vector | [`Vector`](../) | The XYZ vector to be subtracted. |

Returns:

| Type            | Description                                                                                                                                                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Vector`](../) | <p>A vector which is the difference between two other vectors.<br><br>Eg. The vector which called the function (a) minus the vector in its parameter (b):<br><code>a.X - b.X</code><br><code>a.Y - b.Y</code><br><code>a.Z - b.Z</code></p> |



Example:

Subtract the entity body's previous velocity from its current one, to find the random difference in its speed.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    self.Data.mass = 1

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, self.Data.mass)   
    self.Body = box
	
    self.Data.last_velocity = api.physics.NewVector(0,0,0)
end

local function update(self, dt)	
    local vector = api.physics.NewVector(
        math.random()-0.5, 
        math.random()-0.5, 
        math.random()-0.5
    )
    local last_velocity = api.physics.NewVector(
        self.Data.last_velocity.X,
        self.Data.last_velocity.Y,
        self.Data.last_velocity.Z
    )
<strong>    local difference = self.Body.Velocity:Sub(last_velocity)
</strong>    
    self.Body:ApplyForce(vector)
 
    if self.Data.mass == 1 then
		print ("The entity body's Velocity has changed by", difference, ".")
    end
    self.Data.last_velocity = self.Body.Velocity
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Every update, apply a random force to the entity.
-- Subtract the entity's current velocity from its velocity in the previous update.
-- If the entity body's mass is 1, deduce the random force values of the previous update.

-- Prints:
-- The entity body's Velocity has changed by &#x26;{0 0 0} .
--[[ The entity body's Velocity has changed 
     by &#x26;{-0.0796297187387347 -0.20622464089421477 -0.3295008445210883} . ]]--
--[[ The entity body's Velocity has changed 
     by &#x26;{0.18629271371162615 -0.08334125314312901 -0.10271511976491188} . ]]--
-- ...
</code></pre>

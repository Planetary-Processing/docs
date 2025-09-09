# Add

`vector:Add(vector)`

Adds the individual X, Y, and Z values of this vector to the corresponding values of another vector, to return a third vector comprised of the added values.



Parameters:

| Name   | Type            | Description                 |
| ------ | --------------- | --------------------------- |
| vector | [`Vector`](../) | The XYZ vector to be added. |

Returns:

| Type            | Description                                                                                                                                                                                                                             |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Vector`](../) | <p>A vector summed from two other vectors. <br><br>Eg. The vector which called the function (a) added to the vector in its parameter (b):<br><code>a.X + b.X</code><br><code>a.Y + b.Y</code><br> <br>  <br> <code>a.Z + b.Z</code></p> |



Example:

Add vectors together to calculate the total force and velocity of an entity body.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    self.Data.mass = 1

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, self.Data.mass)   
    self.Body = box
    
    local vector_a = api.physics.NewVector(1, 1, 1)
    local vector_b = api.physics.NewVector(2, 2, 2)
    local vector_c = api.physics.NewVector(3, 3, 3)
    
    self.Body:ApplyForce(vector_a)
    self.Body:ApplyForce(vector_b)
    self.Body:ApplyForce(vector_c)
    
<strong>    local total_force = vector_a:Add(vector_b):Add(vector_c)
</strong>    print ("The entity body's Force", self.Body.Force, "is equal to",
	    "the sum of all applied forces", total_force, "in a tick.")

    self.Data.forces = {vector_a, vector_b, vector_c}
end

local function update(self, dt)
    if self.Body.Velocity:Magnitude() ~= 0 then
        local vector = api.physics.NewVector(-0.01, -0.01, -0.01)

	self.Body:ApplyForce(vector)

	local total_force = api.physics.NewVector(0,0,0)
	for index, force in ipairs(self.Data.forces) do
	    force_vector = api.physics.NewVector(force.X, force.Y, force.Z)
<strong>	    total_force = total_force:Add(force_vector)
</strong>	end
	
	if self.Data.mass == 1 then
	    print ("The entity body's Velocity", self.Body.Velocity, "is equal to",
		    "the sum of all applied forces", total_force, "over time.")
	end
	self.Data.forces = api.table.Append(self.Data.forces, vector)
    end
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Apply three different vector forces to the entity.
-- Adding these forces together is the same as the entity body's Force field.
-- Store the three vectors in a variable which will also record subsequent forces.
-- Skip the first update, since the initial forces have not yet been applied.
-- Every update, apply a lesser opposing force, which will be recorded later.
-- Add together the total forces applied across the init and all updates.
-- With a mass of 1, the total sum of forces will be the same as the entity's velocity.
-- Record the value of the update force, for use in the next update.
-- Printed value will often show floating point errors from the physics processing.

-- Prints:
--[[ The entity body's Force {6 6 6} is equal to the sum of 
     all applied forces &#x26;{6 6 6} in a tick. ]]--
--[[ The entity body's Velocity {6 6 6} is equal to the sum of 
     all applied forces &#x26;{6 6 6} over time. ]]--
--[[ The entity body's Velocity {5.99 5.99 5.99} is equal to the sum of 
     all applied forces &#x26;{5.99 5.99 5.99} over time. ]]--
--[[ The entity body's Velocity 
     {5.980000000000001 5.980000000000001 5.980000000000001} is equal to the sum of 
     all applied forces &#x26;{5.980000000000001 5.980000000000001 5.980000000000001} 
     over time. ]]--
-- ...
</code></pre>

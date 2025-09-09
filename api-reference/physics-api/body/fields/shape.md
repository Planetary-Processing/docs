# Shape

`body.Shape`

The bounding box which defines when the entity body has intersected or collided with another entity body.

A shape can be a [Box](../../shape/box/), defined by its X, Y, and Z dimensions. \
Or a [Sphere](../../shape/sphere/) defined by its radius.

| Type                    | Initialised Value  | Description                                  |
| ----------------------- | ------------------ | -------------------------------------------- |
| [`Shape`](../../shape/) | eg. `{1,2,3}`      | The shape of an axis-aligned bounding box.   |
|                         | eg. `{1}`          | The shape of a spherical collision boundary. |



Example:

Create a box-shaped entity body and turn it into a sphere-shaped one, or back to a box, whenever it collides with another physical entity.&#x20;

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local box_shape = api.physics.NewBoxShape(8, 8, 8) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    self.Data.shape = "Box"
end

local function update(self, dt)
    if self.Data.last_velocity then
        local last_velocity = api.physics.NewVector(
            self.Data.last_velocity.X, 
            self.Data.last_velocity.Y, 
       	    self.Data.last_velocity.Z
        )
	local current_velocity = self.Body.Velocity
	
	local current_direction = current_velocity:Normalize()
	local last_direction = last_velocity:Normalize()
	local is_rebounded = last_direction:Dot(current_direction) &#x3C; 0
		
	if is_rebounded then
	    if self.Data.shape == "Box" then
		local new_shape_type = "Sphere"
	        local sphere_shape = api.physics.NewSphereShape(4)
		local sphere  = api.physics.NewBody(sphere_shape, 1) 
		print("A collision has occurred. ".. 
<strong>			"This entity was a "..self.Data.shape, self.Body.Shape, ". "..
</strong><strong>			"It is now a "..new_shape_type, sphere.Shape,".")
</strong>			
		self.Body = sphere
		self.Data.shape = "Sphere"	
		self.Body:ApplyForce(current_velocity)
		
	    elseif self.Data.shape == "Sphere" then
		local new_shape_type = "Box"
		local box_shape = api.physics.NewBoxShape(8, 8, 8) 
		local box = api.physics.NewBody(box_shape, 1) 
		print("A collision has occurred. ".. 
<strong>			"This entity was a "..self.Data.shape, self.Body.Shape, ". "..
</strong><strong>			"It is now a "..new_shape_type, box.Shape,".")
</strong>			
		self.Body = box	
		self.Data.shape = new_shape_type 	
		self.Body:ApplyForce(current_velocity)
	    end 
	end
    else
 	self.Body:ApplyForce(api.physics.NewVector(1,1,0))
    end
    self.Data.last_velocity = self.Body.Velocity
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity and record the shape type.
-- On the first update apply a force to make the entity move in direction (1,1,0).
-- Record the previous velocity of the body at the end of each update.
-- Each update, check whether the direction has become inverted, since the last update.
-- If the entity has rebounded, create a new physics body of the other shape type.
-- Swap to the other physics body shape type, record the new shape type.
-- Start the new body moving, by applying the old body's velocity as a force.

-- Prints:
-- A collision has occurred. This entity was a box &#x26;{8 8 8} . It is now a sphere &#x26;{4} .

-- Or prints:
-- A collision has occurred. This entity was a sphere &#x26;{4} . It is now a box &#x26;{8 8 8} .
</code></pre>

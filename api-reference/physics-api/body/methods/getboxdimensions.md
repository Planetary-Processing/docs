# GetBoxDimensions

`body:GetBoxDimensions()`

Get the width, length, and depth dimensions of a box shaped entity body.&#x20;

This function will return an error, if the Body is not a [Box](../fields/shape.md).



Returns:

| Name   | Type     | Description                                                                                  |
| ------ | -------- | -------------------------------------------------------------------------------------------- |
| width  | `float`  | The world measurement of the box along the X axis.                                           |
| length | `float`  | The world measurement of the box along the Y axis.                                           |
| depth  | `float`  | The world measurement of the box along the Z axis.                                           |
| error  | `string` | <p>An error string, if the body is not box-shaped. <br>Otherwise it is <code>nil</code>.</p> |



Example:

Use a cube's dimensions to calculate a replacement encompassing sphere boundary.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
   
    local box_shape = api.physics.NewBoxShape(8, 8, 8) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box  
    
    self.Data.timer = 1
    self.Body:ApplyForce(api.physics.NewVector(1,1,0))
end

local function update(self, dt)
    if self.Data.timer % 300 == 0 then
<strong>        local width, length, depth, error = self.Body:GetBoxDimensions()
</strong>        if not error then
            local diagonal = math.sqrt(width^2, length^2, depth^2)
            local radius = diagonal/2
            
            local retain_velocity = self.Body.Velocity
            local sphere_shape = api.physics.NewSphereShape(radius) 
            local sphere = api.physics.NewBody(sphere_shape, 1)
            self.Body = sphere
            self.Body:ApplyForce(retain_velocity)
            print("This entity was box-shaped. It is now sphere-shaped.")
        else
            local radius = self.Body:GetRadius()
            local diameter = radius * 2
            local width = math.sqrt(diameter)/3
            
            local retain_velocity = self.Body.Velocity
            local box_shape = api.physics.NewBoxShape(width, width, width) 
            local box = api.physics.NewBody(box_shape, 1)
            self.Body = box
            self.Body:ApplyForce(retain_velocity)
            print("This entity was sphere-shaped. It is now box-shaped.")
        end
    end
    self.Data.timer = self.Data.timer + 1
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Apply an initial force and every 300 updates, start changing the entity body's shape.
-- Get the dimensions of the box.
-- Calculate the diagonal of the cube from corner to corner.
-- Halve it to get the radius of a sphere, which totally encompasses the cube.
-- With the radius, create a sphere shape and a body. 
-- Swap the box body for the sphere body, and retain its current velocity.
-- If the body is no longer a box shape, after 300 updates, reverse the process instead.
-- Get the radius of the sphere.
-- Double the radius to get the diameter of the sphere, the diagonal of the cube.
-- Calculate the width of each side of the uniform cube from the diameter.
-- With the width, create a box shape and a body.
-- Swap the sphere body for the box body, and retain its current velocity.

-- Prints:
-- This entity was box-shaped. It is now sphere-shaped.

-- Or Prints:
-- This entity was sphere-shaped. It is now box-shaped.
</code></pre>

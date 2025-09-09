# GetRadius

`body:GetRadius()`

Get the radius of a sphere shaped entity body.&#x20;

This function will return an error, if the Body is not a [Sphere](../fields/shape.md).



Returns:

| Name   | Type     | Description                                                                                     |
| ------ | -------- | ----------------------------------------------------------------------------------------------- |
| radius | `float`  | The distance between the centre of the sphere and its surface.                                  |
| error  | `string` | <p>An error string, if the body is not sphere-shaped. <br>Otherwise it is <code>nil</code>.</p> |



Example:

Use a sphere's radius to calculate a replacement encompassing box boundary.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
   
    local sphere_shape = api.physics.NewSphereShape(4) 
    local sphere = api.physics.NewBody(sphere_shape, 1)   
    self.Body = sphere
    
    self.Data.timer = 1
    self.Body:ApplyForce(api.physics.NewVector(1,1,0))
end

local function update(self, dt)
    if self.Data.timer % 300 == 0 then
<strong>        local radius, error = self.Body:GetRadius()
</strong>        if not error then
            local width = radius * 2
            
            local retain_velocity = self.Body.Velocity
            local box_shape = api.physics.NewBoxShape(width, width, width) 
            local box = api.physics.NewBody(box_shape, 1)
            self.Body = box
            self.Body:ApplyForce(retain_velocity)
            print("This entity was sphere-shaped. It is now box-shaped.")
        else
            local width = self.Body:GetBoxDimensions()
            local radius = width / 2
            
            local retain_velocity = self.Body.Velocity
            local sphere_shape = api.physics.NewSphereShape(radius) 
            local sphere = api.physics.NewBody(sphere_shape, 1)
            self.Body = sphere
            self.Body:ApplyForce(retain_velocity)
            print("This entity was box-shaped. It is now sphere-shaped.")
        end
    end
    self.Data.timer = self.Data.timer + 1
end 

-- Turn on physics calculations for this entity.
-- Create a sphere shape and a body. Assign it to the entity.
-- Apply an initial force and every 300 updates, start changing the entity body's shape.
-- Get the radius of the sphere.
-- Calculate the width of a cube, which totally encompasses the sphere.
-- With the width, create a box shape and a body.
-- Swap the sphere body for the box body, and retain its current velocity.
-- If the body is no longer a sphere shape, after 300 updates, reverse the process.
-- Get the width of the box.
-- Halve it to get the distance from centre to edge, the radius of a sphere.
-- With the radius, create a sphere shape and a body. 
-- Swap the sphere body for the box body, and retain its current velocity.

-- Prints:
-- This entity was sphere-shaped. It is now box-shaped.

-- Or Prints:
-- This entity was box-shaped. It is now sphere-shaped.
</code></pre>

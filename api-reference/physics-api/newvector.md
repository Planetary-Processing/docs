# NewVector

`api.physics.NewVector(X,Y,Z)`

Creates a new vector. The returned object stores values for 3D space, which can be used for applying a physics force or velocity in a particular direction.



Parameters:

| Name | Type    | Description                                        |
| ---- | ------- | -------------------------------------------------- |
| X    | `float` | The scale of the direction in terms of the X axis. |
| Y    | `float` | The scale of the direction in terms of the Y axis. |
| Z    | `float` | The scale of the direction in terms of the Z axis. |

Returns:

| Type                | Description                                                                    |
| ------------------- | ------------------------------------------------------------------------------ |
| [`Vector`](vector/) | A set of values in 3D space, typically used to denote movement in a direction. |



Example:

Use a vector to apply a certain magnitude of force to a sphere in a direction.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local sphere = api.physics.NewSphereShape(1) 
    self.Body = api.physics.NewBody(sphere, 10)
    
<strong>    local force = api.physics.NewVector(1,2,3)
</strong>    self.Body:ApplyForce(force)
end

-- Turn on physics calculations for this entity.
-- Create a new sphere-shaped physical body for the entity.
-- Apply a force vector to move the entity physically in the direction (1,2,3).
-- The entity will move relative to its current position.
</code></pre>

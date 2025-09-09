# Radius

`sphere.Radius`

The radius of the bounding sphere, from its centre to its surface.

| Type    | Initialised Value  | Description               |
| ------- | ------------------ | ------------------------- |
| `float` | eg. `4`            | The radius of the sphere. |



Example:

Create a spherical physics body with a radius of 4, and assign it to this entity.

```lua
-- entity.lua
local function init(self)
    self.Physics = true

    local radius = 4
    local sphere_shape = api.physics.NewSphereShape(radius) 
    local sphere = api.physics.NewBody(sphere_shape, 1)  
    self.Body = sphere
end

-- Create a sphere shape and a body. Assign it to the entity.
```

# NewSphereShape



`api.physics.NewSphereShape(radius)`

Creates a new sphere shape, to act as collision boundaries.



Parameters:

| Name   | Type    | Description                                                    |
| ------ | ------- | -------------------------------------------------------------- |
| radius | `float` | The distance between the centre of the sphere and its surface. |

Returns:

| Type    | Description                     |
| ------- | ------------------------------- |
| `Shape` | A spherical collision boundary. |



Example:

Create a physics body with collision boundaries in the shape of a sphere, and assigns it to this entity.

```lua
-- entity_type_name.lua
local function init(self)
    self.Physics = true
    
    local sphere = api.physics.NewSphereShape(1) 
    self.Body = api.physics.NewBody(sphere, 10)
end

-- This entity will occupy a spherical area, for physics calculations.
```

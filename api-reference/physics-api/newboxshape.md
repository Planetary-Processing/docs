# NewBoxShape



`api.physics.NewBoxShape(width, length, depth)`

Creates a new box shape, to act as collision boundaries.



Parameters:

| Name   | Type    | Description                                         |
| ------ | ------- | --------------------------------------------------- |
| width  | `float` | The world measurement of the box along the X axis.  |
| length | `float` | The world measurement of  the box along the Y axis. |
| depth  | `float` | The world measurement of  the box along the Z axis. |

Returns:

| Type    | Description                      |
| ------- | -------------------------------- |
| `Shape` | A box-shaped collision boundary. |



Example:

Create a physics body with collision boundaries in the shape of a box, and assigns it to this entity.

```lua
-- entity_type_name.lua
local function init(self)
    self.Physics = true
    
    local box = api.physics.NewBoxShape(1, 1, 1) 
    self.Body = api.physics.NewBody(box, 10)
end

-- this entity will occupy a box-shaped area, for physics calculations
```

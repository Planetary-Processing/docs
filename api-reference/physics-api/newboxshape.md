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

| Type              | Description                      |
| ----------------- | -------------------------------- |
| [`Shape`](shape/) | A box-shaped collision boundary. |



Example:

Create a physics body with collision boundaries in the shape of a box, and assign it to this entity.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
<strong>    local box = api.physics.NewBoxShape(1, 1, 1) 
</strong>    self.Body = api.physics.NewBody(box, 10)
end

-- This entity will occupy a box-shaped area, for physics calculations.
</code></pre>

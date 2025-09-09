# NewBody



`api.physics.NewBody(shape, mass)`

Creates a new body object for simulating physics.



Parameters:

| Name  | Type                            | Description                                                              |
| ----- | ------------------------------- | ------------------------------------------------------------------------ |
| shape | [`Shape`](body/fields/shape.md) | The shape of the body's collision boundaries.                            |
| mass  | `float`                         | Dictates the effectiveness of forces applied by the body and against it. |

Returns:

| Type            | Description                                 |
| --------------- | ------------------------------------------- |
| [`Body`](body/) | A collidable body for physics calculations. |



Example:

Create a new box-shaped body, assign it to this entity, and apply a force to it each update.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local box = api.physics.NewBoxShape(1, 1, 1) 
<strong>    self.Body = api.physics.NewBody(box, 10)
</strong>end

local function update(self, dt)
    local vector = api.physics.NewVector(0.2, 0, 0)
    self.Body:ApplyForce(vector)
end

-- Pushes the entity with a force of 0.2 in the X direction each tick.
</code></pre>


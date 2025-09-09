# D

`box.D`

The depth of the bounding box, in world units along the Z axis.

| Type    | Initialised Value  | Description            |
| ------- | ------------------ | ---------------------- |
| `float` | eg. `3.4`          | The depth of the box.  |



Example:

Increase the depth of an entity's shape and store it in the entity's Data table. The Data can be handled every update clientside, for example to scale a 3D model.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 3.4) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
end

local function update(self, dt)
    local shape = self.Body.Shape
    if shape.D &#x3C; 50 then
<strong>	shape.D = shape.D + 0.001
</strong>    end
   
    self.Data.measurements= {
        x = shape.W,
        y = shape.L,
        z = shape.D
    }
    
    print("Storing shape dimensions, to be accessible to all client connections.")
end 

-- Create a box shape and a body. Assign it to the entity.
-- Gradually increase the depth of the entity body to a maximum of 50.
-- Record the changes in the entity's Data table.
-- On the clientside, handle the data from the automatic entity update.

-- Prints:
-- Storing shape dimensions, to be accessible to all client connections.
</code></pre>

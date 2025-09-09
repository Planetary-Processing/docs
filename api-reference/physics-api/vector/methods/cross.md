# Cross

`vector:Cross(vector)`

Returns the cross product between two vectors.&#x20;

For each coordinate slot (XYZ) of this vector (eg. X), the function multiplies the value in the next slot of this vector (eg. Y) by the value in the slot after next (eg. Z) of another vector. Then does the opposite, the value in the slot after next (eg. Z) of this vector multiplied by the other vector's next slot (eg. Y). Then subtracts the former multiplication's result from the latter, to return a third vector.

The cross product is the vector perpendicular to the two initial vectors according to the right hand rule.



Parameters:

| Name   | Type            | Description                              |
| ------ | --------------- | ---------------------------------------- |
| vector | [`Vector`](../) | The numerical value to be multiplied by. |

Returns:

| Type            | Description                                                                                                                                                                                                                                                                              |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Vector`](../) | <p>A cross product vector perpendicular to both initial vectors.<br><br>Eg. The vector which called the function (a) calculating the cross product with the vector (b):</p><p><code>a.Y*b.Z - a.Z*b.Y</code> <br> <code>a.Z*b.X - a.X*b.Z</code> <br> <code>a.X*b.Y - a.Y*b.X</code></p> |



Example:

Show that the cross product of the X axis vector and the Y axis vector is the Z axis vector.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
end

local function update(self, dt)	
    local vector_x = api.physics.NewVector(1,0,0)
    local vector_y = api.physics.NewVector(0,1,0)
 
<strong>    local vector_z = vector_x:Cross(vector_y)
</strong>    print("The Z axis", vector_z, "is perpendicular to the X and Y axes.")
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Create a vector for the X and Y axes directions.
-- Generate the direction of the Z axis using the cross product of the X and Y axes.

-- Prints:
-- The Z axis &#x26;{0 0 30} is perpendicular to the X and Y axes.
-- ...
</code></pre>

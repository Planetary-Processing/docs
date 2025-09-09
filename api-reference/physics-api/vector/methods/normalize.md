# Normalize

`vector:Normalize()`

Returns the normalized form of a vector. Divides each of the X, Y, and Z values of this vector by the square root of the sum of its squared values, to return a normalised vector.

The normalized vector is the vector's direction reduced by its [Magnitude](magnitude.md) to fit within the range -1 to 1. This process retains it direction but limits its size to the unit length.



Returns:

| Type            | Description                                                                                                                                                                                                                                                                                                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Vector`](../) | <p>A vector normalized to be no greater than the unit length.<br><br>Eg. The vector which called the function (a) uses its magnitude to normalize itself:</p><p><br><code>magnitude =</code></p><p><code>math.sqrt(a.X^2 ​</code> </p><p><br>  <code>+ a.Y^2 ​</code></p><p><br><code>+ a.Z^2)</code><br><br><code>normalized =</code><br><code>a:Mul(1/magnitude)</code></p> |



Example:

Show that normalizing vectors of different sizes allows their direction to be compared.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    self.Data.vector = api.physics.NewVector(
        math.random(-1, 1),
        math.random(-2, 2),
        math.random(-3, 3)
	) 

    local scale_changes = {0.5, 1, 2}
    self.Data.scale_change = scale_changes [math.random(1, #scale_changes)]
    local scaled_vector = self.Data.vector:Mul(self.Data.scale_change)
    
    self.Body:ApplyForce(scaled_vector)
end

local function update(self, dt)	
    local vector = api.physics.NewVector(
        self.Data.vector.X,
        self.Data.vector.Y,
        self.Data.vector.Z
    )
    
    if vector:Magnitude() == 0 then
        print("The zero vector normalized will return the zero vector.")
    end
    
<strong>    local normalized_vector = vector:Normalize()
</strong><strong>    local normalized_velocity = self.Body.Velocity:Normalize()
</strong>    local difference = normalized_velocity:Sub(normalized_vector)
    
    if difference:Magnitude() == 0 then
	print("The direction of the velocity is the same as the original vector, "..
                "even after scaling.")
    end
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Generate a vector with random direction and magnitude.
-- Apply a random scaling effect to the vector's size.
-- Apply the scaled force to the entity.
-- Create a check in case the vector is (0,0,0).
-- Normalize the original vector and the entity's velocity.
-- Once normalized the vectors can be compared. And can be shown to be the same.

-- Prints:
-- The direction of the velocity is the same as the original vector, even after scaling.

-- Or Prints (if (0,0,0)):
-- The zero vector normalized will return the zero vector.
-- The direction of the velocity is the same as the original vector, even after scaling.
</code></pre>

# X

`vector.X`

A value to measure the vector's strength in the X direction.&#x20;

For physics, it is used to denote the power and direction of forces on entity bodies, or the velocity of those bodies in terms of the X axis.

| Type    | Initialised Value  | Description                               |
| ------- | ------------------ | ----------------------------------------- |
| `float` | eg. `1`            | The vector's strength in the X direction. |



Example:

Create a vector to act as an initial force on an entity object. Then use vector maths to make the entity move along a square path anticlockwise.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local box_shape = api.physics.NewBoxShape(8, 8, 8) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    local vector = api.physics.NewVector(1, 0, 0)
    self.Body:ApplyForce(vector)
    print("The applied force is a vector", self.Body.Force, ".") 
    
    self.Data.timer = 1
end

local function update(self, dt)
    if self.Data.timer % 300 == 0 then
        local angle = 90 
        local radians = angle * math.pi / 180
        local sin_value = math.sin(radians)
        local cos_value = math.cos(radians)
        local current_velocity = self.Body.Velocity
        
<strong>        local rotated_x = current_velocity.X * cos_value - 
</strong>                            current_velocity.Y * sin_value 
<strong>        local rotated_y = current_velocity.X * sin_value +
</strong>                            current_velocity.Y * cos_value
                            
        rotated_x = math.floor(rotated_x * 1000 + 0.5) / 1000
        rotated_y = math.floor(rotated_y * 1000 + 0.5) / 1000
                                           
        rotated_velocity = api.physics.NewVector(rotated_x, rotated_y, 0)
        self.Body.Velocity = rotated_velocity
        
        print("This entity rotated by "..angle.." degrees anticlockwise. "..
                "It now moves in a", self.Body.Velocity, "direction.")
    end
    self.Data.timer = self.Data.timer + 1
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- On the first update apply a force to make the entity move in direction (1,0,0).
-- Every 300 updates, start changing the velocity direction of the entity body.
-- Convert 90 degrees into radians, to obtain sine and cosine values for that angle.
-- Apply a standard anticlockwise 2D rotation to the current velocity's X and Y values.
-- Round the values to 3 decimal places to beautify floating point errors from math.pi.
-- Create a rotated vector and apply it as a new velocity.

-- Prints:
-- The applied force is a vector {1 0 0} .
-- This entity rotated by 90 degrees anticlockwise. It now moves in a {0 1 0} direction.
-- This entity rotated by 90 degrees anticlockwise. It now moves in a {-1 0 0} direction.
-- This entity rotated by 90 degrees anticlockwise. It now moves in a {0 -1 0} direction.
-- This entity rotated by 90 degrees anticlockwise. It now moves in a {1 0 0} direction.
</code></pre>

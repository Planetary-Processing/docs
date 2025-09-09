# Z

`vector.Z`

A value to measure the vector's strength in the Z direction.&#x20;

For physics, it is used to denote the power and direction of forces on entity bodies, or the velocity of those bodies in terms of the Z axis.

| Type    | Initialised Value  | Description                               |
| ------- | ------------------ | ----------------------------------------- |
| `float` | eg. `3`            | The vector's strength in the Z direction. |



Example:

Create a vector to simulate gravity in the Z direction, when an entity jumps on static terrain.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    local vector = api.physics.NewVector(0, 0, -1)  
    self.Data.gravity = vector
end

local function update()
<strong>    local gravity = api.physics.NewVector(0, 0, self.Data.gravity.Z)
</strong>    self.Body:ApplyForce(gravity)

    local x,y,z = self:GetPosition()
    is_on_ground = z &#x3C;= 0
    
    if is_on_ground then
        self:MoveTo(x,y,0)
         
        local falling_weight_force = self.Body.Velocity:Add(gravity)
        local opposing_force = falling_weight_force:Mul(-1)
        local ground_force = api.physics.NewVector(0, 0, opposing_force.Z)
        self.Body:ApplyForce(ground_force)
    else
       print(string.format("This entity is jumping. It is at a height of %.3f.", z))
    end
end

local function message(self, msg)
    if msg.Data.act == "jump" then
        local gravity = api.physics.NewVector(0, 0, self.Data.gravity.Z)
        local jump_force = gravity:Mul(-5)
        
        self.Body:ApplyForce(jump_force)
        print("Starting a jump with a force of "..jump_force.Z..".")
    end
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Every update apply a gravitational force of (0,0,-1) on the entity.
-- If the entity is at a height of 0 or below, simulate a static ground beneath it.
-- Move the entity on top of the ground at a Z coordinate of 0, in case it has sunk.
-- Apply an opposing force to the entity's weight and movement in the Z direction.
-- This opposing force will counteract gravity, to make the entity stationary in Z.
-- If the entity is higher than the ground print what height they are at.
-- Receive a message, commanding the entity to jump.
-- Apply a single jump force against the flow of gravity.
-- This will temporarily break the equilibrium between the ground and gravity forces.
-- Eventually the repeating force of gravity will return the entity to ground level.

-- Prints:
-- Starting a jump with a force of 5.
-- This entity is jumping. It is at a height of 0.166.
-- This entity is jumping. It is at a height of 0.301.
-- This entity is jumping. It is at a height of 0.402.
-- This entity is jumping. It is at a height of 0.469.
-- This entity is jumping. It is at a height of 0.502.
-- This entity is jumping. It is at a height of 0.502.
-- This entity is jumping. It is at a height of 0.469.
-- This entity is jumping. It is at a height of 0.401.
-- This entity is jumping. It is at a height of 0.303.
-- This entity is jumping. It is at a height of 0.167.
-- This entity is jumping. It is at a height of 0.003.
</code></pre>

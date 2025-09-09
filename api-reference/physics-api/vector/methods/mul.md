# Mul

`vector:Mul(vector)`

Multiplies each of the X, Y, and Z values of this vector by a float, to return another vector comprised of the multiplied values.



Parameters:

| Name  | Type    | Description                              |
| ----- | ------- | ---------------------------------------- |
| float | `float` | The numerical value to be multiplied by. |

Returns:

| Type            | Description                                                                                                                                                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Vector`](../) | <p>A vector scaled by a single numerical value.<br><br>Eg. The vector which called the function (a) multiplied with the float (b):<br> <code>a.X * b</code> </p><p><br><code>a.Y * b</code> </p><p><br><code>a.Z * b</code></p> |



Example:

Multiply or divide the force moving an entity, either increasing or decreasing its velocity the longer that it exists.

```lua
-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    local velocity_changes = {"decreasing", "increasing"}
    self.Data.velocity_change = velocity_changes[math.random(1, #velocity_changes)]
    self.Data.timer = 1
end

local function update(self, dt)	
    local vector = api.physics.NewVector(
        math.random()-0.5, 
        math.random()-0.5, 
        math.random()-0.5
    )
    local vector_multiplied
 
    if self.Data.velocity_change == "increasing" then
        vector_multiplied = vector:Mul(self.Data.timer)
    elseif self.Data.velocity_change == "decreasing" then
        vector_multiplied = vector:Mul(1/self.Data.timer)
    end
    
    print("Moving entity at a velocity which is "..self.Data.velocity_change,
	    "by a factor of "..self.Data.timer..".")
	
    self.Body:ApplyForce(self.Body.Velocity:Mul(-1))
    self.Body:ApplyForce(vector_multiplied)
    self.Data.timer = self.Data.timer + 1
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Randomly select whether the entity's velocity should increase or decrease.
-- Start a timer to record how long the entity has existed.
-- Every update create a randomised vector for the direction of the entity's movement.
-- Multiply or divide the vector by the time the entity has existed
-- Halt any momentum between updates, by applying a force opposing the entity's velocity.
-- Apply the multiplied force for this update to the entity body.

-- Prints:
-- Moving entity at a velocity which is decreasing by a factor of 1.
-- Moving entity at a velocity which is decreasing by a factor of 2.
-- Moving entity at a velocity which is decreasing by a factor of 3.
-- ...

-- Or Prints:
-- Moving entity at a velocity which is increasing by a factor of 1.
-- Moving entity at a velocity which is increasing by a factor of 2.
-- Moving entity at a velocity which is increasing by a factor of 3.
-- ...
```

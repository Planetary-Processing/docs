# Mass

`body.Mass`

The mass of an entity body, which dictates how effective applied forces and collisions are on an entity's physics body.&#x20;

Since there is no in-built gravity or air resistance by default, the mass will have no direct affect on an entity's Velocity, independent of applied forces.&#x20;



| Type    | Initialised Value  | Description                                             |
| ------- | ------------------ | ------------------------------------------------------- |
| `float` | eg. `5.0`          | The modifier for force calculations on the entity body. |



Example:

Push an entity into an opposing 'wind' force and see how different masses change how effective the opposing force is on the entity.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
    
    local mass = 10	
    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, mass)  
     
    self.Body = box
    self.Body:ApplyForce(api.physics.NewVector(32, 32 ,0))
	
    local mass_changes = {"decrease", "not be changed", "increase"}
    self.Data.mass_change = mass_changes[math.random(1, #mass_changes)]
    print("This entity has a mass which will "..self.Data.mass_change..".")
end

local function update(self, dt)
    local not_force_update = self.Body.Force:Dot(self.Body.Force) == 0
    if not_force_update then 
        if self.Data.mass_change == "decrease" then 
<strong>	    self.Body.Mass = 1
</strong>        end
	if self.Data.mass_change == "not be changed" then
<strong>	    self.Body.Mass = self.Body.Mass
</strong>	end
	if self.Data.mass_change == "increase" then 
<strong>	    self.Body.Mass = 100
</strong>	end
    end

    local wind_force = api.physics.NewVector(-0.1, -0.1 ,0)
    self.Body:ApplyForce(wind_force)	
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body with a mass of 10. Assign it to the entity.
-- Apply an initial force to move the entity towards the top-right of the panel map.
-- Randomly select a new mass which the entity will update to.
-- Check that the initial force has finished processing, before changing the mass.
-- Decrease (to 1), increase (to 100), or leave the entity's mass unchanged (at 10).
-- Create and apply a wind force (-0.1, -0.1, 0), to blow the entity backwards.
-- Watch the panel map to see the effect: immediately, gradually, or eventually.

-- Prints:
-- This entity has a mass which will decrease.

-- Or Prints:
-- This entity has a mass which will not be changed.

-- Or Prints:
-- This entity has a mass which will increase.
</code></pre>

# Force

`body.Force`

The combined XYZ vector of forces which have been applied to a physics body on an update. Forces from incidental factors, such as collisions, are not included in this value.

If any forces were applied within the [`init`](../../../entity-api/entity/necessary-methods/init.md) function of an entity, that Force value will also persist into the first [`update`](../../../entity-api/entity/necessary-methods/update.md) tick.



| Type                      | Initialised Value                                                                         | Description                                                                              |
| ------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| [`Vector`](../../vector/) | <p><code>{X = 0,</code> </p><p>    <code>Y = 0,</code> </p><p>    <code>Z = 0}</code></p> | The total force which will be applied to this entity body at the end of the update tick. |



Example:

Cap the total force that can be applied to an entity at once, regardless of how much force is applied over the course of the update.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local sphere_shape = api.physics.NewSphereShape(1) 
    local sphere = api.physics.NewBody(sphere_shape, 10)    
    self.Body = sphere 
    
    local randomiser = {true, false}
    self.Data.speed_buffs = {
        ["Cat's Agility"] = self.Type == "cat",
        ["Sprinting"] = randomiser[math.random(1, #randomiser)],
        ["Solution of speed"] = randomiser[math.random(1, #randomiser)],
        ["Energising Beverage"] = randomiser[math.random(1, #randomiser)],
        ["Ward Against Slowness"] = randomiser[math.random(1, #randomiser)],
        ["Positive Attitude"] = randomiser[math.random(1, #randomiser)],
    }
end

local function update(self, dt)
    local buff_string = ""
    local buff_force = api.physics.NewVector(0.1, 0.1, 0)  
    
    for buff, active in pairs(self.Data.speed_buffs) do
        if active == true then
	    self.Body:ApplyForce(buff_force)	
            buff_string = buff_string..buff..", "
        end
    end
    
<strong>    local applied_force = self.Body.Force
</strong>    local force_cap = api.physics.NewVector(0.3, 0.3, 0)
    local is_above_force_cap = applied_force:Magnitude() > force_cap:Magnitude()
	
    print ("This entity has the following speed buffs: "..buff_string.. 
            "which would apply a total force of", applied_force, "to the entity."..
            " This is "..
            (is_above_force_cap and "greater than the cap and must be reduced."
            or "less than the cap and acceptable.")
    )  

    if is_above_force_cap then
	self.Body:ApplyForce(force_cap:Sub(applied_force))
    end    
end 

-- Turn on physics calculations for this entity.
-- Create a sphere shape and a body. Assign it to the entity.
-- Randomise a table of speed modifiers, to influence the amount of force applied.
-- Check each buff every update and, if it is active, apply a force and record its name.
-- If the total amount of applied force is greater than the force cap, reduce it.

-- Prints:
--[[ This entity has the following speed buffs: Cat's Agility, Energising Beverage, 
     Positive Attitude, Sprinting, which would apply a total force of {0.4 0.4 0} 
     to the entity. This is greater than the cap and must be reduced. --]]
     
-- Or Prints:
--[[ This entity has the following speed buffs: Cat's Agility, Energising Beverage, 
     which would apply a total force of {0.2 0.2 0} to the entity. This is 
     less than the cap and acceptable. --]]
</code></pre>

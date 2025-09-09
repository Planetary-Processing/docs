# ApplyForce

`body:ApplyForce(force)`

Apply a push force to this entity body, in terms of XYZ, to alter its velocity.

The applied forces are cumulative over the course of an update tick. Changes to the body's velocity are only executed once, on the update after this function is called.

Forces applied within the [`init`](../../../entity-api/entity/necessary-methods/init.md) function are not processed immediately. Processing will occur as if this function was called from the first update tick, such that body's velocity changes on the second.

Physics calculations for the body's final [Velocity](../fields/velocity.md) will use the final applied [Force](../fields/force.md) and its [Mass](../fields/mass.md), as well as external factors like collisions.&#x20;



Parameters:

| Name  | Type                      | Description                                                         |
| ----- | ------------------------- | ------------------------------------------------------------------- |
| force | [`Vector`](../../vector/) | The direction and strength of the physical push on the entity body. |



Example:

Push the entity once, twice, or continuously.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true
   
    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box  
    
    local force_count = {"once", "twice", "continuously"}
    self.Data.force_count = force_count[math.random(1,#force_count)] 
    
    if self.Data.force_count == "once" then
<strong>        self.Body:ApplyForce(api.physics.NewVector(1,1,0))
</strong>        print("This entity will be pushed once, with a force of", self.Body.Force,".")
    end 
    if self.Data.force_count == "twice" then
<strong>        self.Body:ApplyForce(api.physics.NewVector(1,1,0))
</strong><strong>        self.Body:ApplyForce(api.physics.NewVector(1,1,0))
</strong>        print("This entity will be pushed, with a total force of", self.Body.Force,".")
    end 
end

local function update(self, dt)
    if self.Data.force_count == "continuously" then
<strong>        self.Body:ApplyForce(api.physics.NewVector(1,1,0))
</strong>        print("This entity will be pushed continuously.".. 
                "It's velocity will increase", self.Body.Velocity,".")
    end 
end 

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Randomly select a how many times a force should be applied.
-- On the first update either apply a single force or the same force twice.
-- Or, on each update, apply a force continuously.

-- Prints:
-- This entity will be pushed once, with a force of {1 1 0} .

-- Or Print:
-- This entity will be pushed, with a total force of {2 2 0} .

-- Or Prints:
-- This entity will be pushed continuously.It's velocity will increase {0 0 0} .
-- This entity will be pushed continuously.It's velocity will increase {1 1 0} .
-- This entity will be pushed continuously.It's velocity will increase {2 2 0} .
-- This entity will be pushed continuously.It's velocity will increase {3 3 0} .
-- ...
</code></pre>

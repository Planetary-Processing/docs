# W

`box.W`

The width of the bounding box, in world units along the X axis.

| Type    | Initialised Value  | Description            |
| ------- | ------------------ | ---------------------- |
| `float` | eg. `1.4`          | The width of the box.  |



Example:

Increase the physical width of two cats every time they collide.

<pre class="language-lua"><code class="lang-lua">--init.lua
function init()
    if chunk.Dimension == "" and not chunk.Generated then
	if chunk.X == 0 and chunk.Y == 0 then
	    api.entity.Create("cat", -32, 32, 0, {side = "left"})
	    api.entity.Create("cat", 96, 32, 0, {side = "right"})
	end
    end
end

-- cat.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1.4, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    	
    print("The "..self.Data.side.." cat's starting width is "..self.Body.Shape.W..".")
end

local function update(self, dt)
    local x,y,z = self:GetPosition()
    if x &#x3C; 10 and self.Data.side == "left" then
    	self.Body:ApplyForce(api.physics.NewVector(0.1, 0, 0))
    elseif x > 54 and self.Data.side == "right" then
	self.Body:ApplyForce(api.physics.NewVector(-0.1, 0, 0))
    end
		
    if self.Data.last_velocity then
        local last_velocity = api.physics.NewVector(
            self.Data.last_velocity.X, 
            self.Data.last_velocity.Y, 
       	    self.Data.last_velocity.Z
        )
	local current_velocity = self.Body.Velocity
	
	local current_direction = current_velocity:Normalize()
	local last_direction = last_velocity:Normalize()
	local is_rebounded = last_direction:Dot(current_direction) &#x3C; 0
		
	if is_rebounded then
<strong>	    self.Body.Shape.W = self.Body.Shape.W + 5
</strong>	    print("The "..self.Data.side.." cat just rebounded. ".. 
            	    "Its physical width is now "..self.Body.Shape.W..".")
	end
    end
    self.Data.last_velocity = self.Body.Velocity
end 

-- Spawn two cat's when the origin chunk is first generated, labelled left and right.
-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity and print its current width.
-- Every update, apply a force to push the cats together, if they have moved apart.
-- Check for a change in velocity to indicate that they have rebounded.
-- Increase the width of the entity body by 5, after every rebound.
-- Record the velocity for the next rebound check.

-- Prints:
-- The left cat's starting width is 1.4.
-- The right cat's starting width is 1.4.
-- The left cat just rebounded. Its physical width is now 6.4.
-- The right cat just rebounded. Its physical width is now 6.4.
-- The left cat just rebounded. Its physical width is now 11.4.
-- The right cat just rebounded. Its physical width is now 11.4.
-- ...
</code></pre>

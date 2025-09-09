# Magnitude

`vector:Magnitude()`

Returns the magnitude of a vector. Square roots the sum of the individually squared X, Y, and Z values, to return a single numerical value.

The magnitude is the length of the vector, if measured as a distance in 3D space.



Returns:

| Type    | Description                                                                                                                                                                                                                                             |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `float` | <p>The scalar magnitude of a vector.<br><br>Eg. The vector which called the function (a) finding the root of its squared values summed :</p><p><code>math.sqrt(a.X^2 ​</code> </p><p><br>  <code>+ a.Y^2 ​</code></p><p><br><code>+ a.Z^2)</code></p> |



Example:

Use the magnitudes of forces to estimate the distance an entity travels over 5 seconds.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
    
    self.Data.update_timer = 0
    self.Data.distance_timer = 0
    self.Data.magnitudes = {}
    self.Data.target_tickrate = 30
end

local function update(self, dt)	
     if self.Data.update_timer == 0 then 
        local vector = api.physics.NewVector(1,1,1)
<strong>        local magnitude = vector:Magnitude()
</strong>		
	self.Body:ApplyForce(self.Body.Velocity:Mul(-1))
        self.Body:ApplyForce(vector)
        self.Data.magnitudes = api.table.Append(self.Data.magnitudes, magnitude)
     end   
       
     if self.Data.update_timer >= self.Data.target_tickrate then
         self.Data.update_timer = 0
     else
	self.Data.update_timer = self.Data.update_timer + 1
     end
	
	
     if self.Data.distance_timer >= self.Data.target_tickrate * 5 then
	local total_distance = 0
	for index, magnitude in ipairs(self.Data.magnitudes) do
             total_distance = total_distance + magnitude
        end
        print("The total distance this entity has travelled is approximately "..
               total_distance.." from its starting position.")  
        self.Data.distance_timer = 0
    else
        self.Data.distance_timer = self.Data.distance_timer + 1
    end
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Create two timers, one to trigger an event every second and another every 5 seconds.
-- Create a variable to store a list of magnitudes.
-- Create a variable for the approximate number of ticks per second.
-- Every update, check if the update timer has begun or has been reset.
-- If so, halt the entity's momentum, apply a fresh force, and record its magnitiude.
-- If the timer reaches the tickrate, approximately a second has passed.
-- Reset the update timer approximately each second, otherwise increment it.
-- Approximately every five seconds, add together all of the force magnitudes.
-- The total distance the entity has travelled can be estimated from the magnitudes.
-- It is an estimate because of tickrate fluctuations and floating point imprecision.
-- Reset the distance timer after approximately five seconds, otherwise increment it.

-- Prints:
--[[ The total distance this entity has travelled is approximately 
        8.6602540378444 from its starting position. ]]--
--[[ The total distance this entity has travelled is approximately 
        17.320508075689 from its starting position. ]]--
--[[ The total distance this entity has travelled is approximately 
        25.980762113533 from its starting position. ]]--
</code></pre>

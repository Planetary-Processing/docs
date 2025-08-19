# init



`function init(self)`

Performs the initial processing to set up an entity instance, when it is created.&#x20;

As a necessary entity method, this function should be returned at the end of the entity.lua file. It must be in a table under the key `init`.



Parameters:

| Name | Type     | Description         |
| ---- | -------- | ------------------- |
| self | `Entity` | This entity itself. |



Example:

Initialise an entity and assign it to a team.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
<strong>local function init(self)
</strong>    self.Data.created_at = api.util.Time()
    self.Data.health = 10
    self.Data.speed = 5
         
    local x,y,z = self:GetPosition()
    self.Data.team = y > 0 and " Top " or " Bottom "    
         
    print("Entity added to the"..self.Data.team.."team, at "..
             self.Data.created_at.." Unix time.") 
end

return { init = init, 
         update = function (self, dt) end, 
         message = function (self, msg) end }

-- Record the time the entity was created and its default speed and health.
-- Assign the entity to a team based on it Y position being above or below 0.
-- Print the entity's team and moment of creation.
-- Return the 'init' function as a value in a table.

-- Prints:
-- Entity added to the Bottom team, at 1750073728 Unix time.
</code></pre>

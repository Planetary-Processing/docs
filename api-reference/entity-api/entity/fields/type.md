# Type

`entity.Type`

A template shared by entities of the same group. An entity's [Type](../../../../server/entities.md#types-and-behaviour-scripting) corresponds to the name of its Lua file in the entity folder, for example cat.lua.

On the serverside, it is used as a parameter for entity [creation](../../create.md).&#x20;

It is referenced heavily in clientside SDKs for the spawning, despawning, and management of entities.

| Type     | Initialised Value (default) | Description                                   |
| -------- | --------------------------- | --------------------------------------------- |
| `string` | eg. `cat`                   | String name of an entity.lua file. Read-only. |



Example:

Get nearby entities with a specific type, then create more, if there are less than expected.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function update(self, dt)
    if api.util.Time() % 5 == 0 then
    
        local nearby_entities = self:GetNearbyEntities(64)
        local type_count = 0
        
        for index, entity in pairs(nearby_entities) do
<strong>            if entity.Type == "cat" then
</strong>                type_count = type_count + 1
            end
        end
        
        local x,y,z = self:GetPosition()
        if type_count &#x3C; 5 then
            api.entity.Create("cat", x, y, z, {})
            print("Created a new cat. Now there are "..(type_count+1).." nearby.")
        end
        
    end
end

-- Every five seconds, find each entity within 64 units distance.
-- Count how many have the Type "cat".
-- If there are less than 5 cats nearby, create a new cat.

-- Prints (starting with no cats nearby):
-- Created a new cat. Now there are 1 nearby.
-- Created a new cat. Now there are 2 nearby.
-- Created a new cat. Now there are 3 nearby.
-- Created a new cat. Now there are 4 nearby.
-- Created a new cat. Now there are 5 nearby.
</code></pre>




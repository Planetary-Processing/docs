# GetNearbyEntities



`entity:GetNearbyEntities(dist)`

Gets an indexed list of entity objects of any entities within a fixed distance of this one.

Returns a maximum of 16 entities. Entity objects returned by this function are shallow copies. They provide information about the entity at the instant it is retrieved, but cannot be used to call entity methods.

It can be used to acquire IDs for entity [messaging](../../message.md) or for triggering non-[Physical](../../../physics-api/) collisions.



Parameters:

| Name | Type    | Description                                                                            |
| ---- | ------- | -------------------------------------------------------------------------------------- |
| dist | `float` | The range to check for nearby entities. Within a 3x3 grid of chunks around the entity. |

Returns:

| Type       | Description                                                   |
| ---------- | ------------------------------------------------------------- |
| `Entity[]` | An indexed list of Entity objects, within the given distance. |



Example:

Find which entities are nearby and remove some, using messages.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function message(self, msg)
    if msg.Data.remove_excess_entities then
        local ordinals = {"st", "nd", "rd", "th"}
<strong>        local nearby_entities = self:GetNearbyEntities(64)
</strong>        
        for index, entity in ipairs(nearby_entities) do
            local suffix = (index &#x3C;=4) and ordinals[index] or ordinals[4]     
            local ordinal = index..suffix
            print("This is the "..ordinal.." entity nearby.")
            
            if index > 5 then
                api.entity.Message(entity.ID, 
                    {remove_self = true,
                    ordinal= ordinal})
            end 
        end
    end
end


-- a_nearby_entity.lua
local function message(self, msg)
    if msg.Data.remove_self then
        print("The "..msg.Data.ordinal.." entity is being removed.")
        self:Remove()
    end
end

-- Receive a message request to remove excess entities.
-- Find every entity within 64 units distance.
-- Print what number they are in the list.
-- If there are more than 5 entities, send a message to the entitites to be removed.
-- From the nearby entity, print its number and that it is being removed.

-- Prints (for 6 entities nearby):
-- This is the 1st entity nearby.
-- This is the 5th entity nearby.
-- This is the 4th entity nearby.
-- This is the 2nd entity nearby.
-- This is the 3rd entity nearby.
-- This is the 6th entity nearby.
-- The 6th entity is being removed.
</code></pre>


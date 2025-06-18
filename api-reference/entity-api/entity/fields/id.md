# ID

`entity.ID`

The unique identifier of an entity. It is a unique name for a singular entity, not shared between entities of the same [Type](../../../../server/entities.md#types-and-behaviour-scripting). It allows the entity to be referenced directly, regardless of which chunk it is in.&#x20;

An entity's ID is a necessary parameter in certain functions such as [entity messaging](../../message.md).



IDs use a standard UUID format comprised of hyphenated groups of characters of set lengths (8-4-4-4-12).\
`eg. e2e838b4-a250-4a89-9f1d-acae063201b8` \
&#x20;`eg. 12345678-1234-1234-1234-123456789012`

| Type     | Initialised Value                          | Description                                               |
| -------- | ------------------------------------------ | --------------------------------------------------------- |
| `string` | eg. `e2e838b4-a250-4a89-9f1d-acae063201b8` | UUID formatted 36 character hyphenated string. Read-only. |



Example:

Get the IDs of nearby entities and check if they match this entity's.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    local nearby_entities = self:GetNearbyEntities(64)
    
    for index, entity in pairs(nearby_entities) do
<strong>        if self.ID ~= entity.ID then
</strong>            print("These IDs don't match, because they are unique.") 
        end
    end
end

-- Find every entity within 64 units distance
-- If their ID is not the same as this entity's ID, print a message.

-- Prints (for every entity):
-- These IDs don't match, because they are unique.
</code></pre>

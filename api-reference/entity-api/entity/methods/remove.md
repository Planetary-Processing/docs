# Remove



`entity:Remove()`

Removes this instance of the entity from the game permanently.&#x20;

New default entities of the same [Type](../../../../server/entities.md#types-and-behaviour-scripting) can still be created but any changes to this specific entity's fields will be lost, when the entity is removed.



Example:

Remove the entity if there are no players nearby.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function update(self, dt)
    local nearby_entities = self:GetNearbyEntities(20)
    local player_found = false
    
    for id, entity in pairs(nearby_entities) do
        if entity.Type == "player" then
            player_found = true
            break
        end
    end
    
    if player_found == false then
       print("No players nearby. This entity can be removed.")
<strong>       self:Remove()
</strong>    end
end

-- Get a table of nearby entities.
-- Check if that table contains any players.
-- If no players are found, remove the entity from the game completely.

-- Prints (asssuming a chunk size of 64):
-- No players nearby. This entity can be removed.
</code></pre>

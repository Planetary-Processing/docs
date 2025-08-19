# ID

`chunk.ID`

The unique identifier of a chunk. This field can be accessed directly by all entities within the chunk.&#x20;

A chunk's ID will persist when the server is stopped and restarted, or deployed, but not when the simulation is reset. For identifying chunks across resets, the X and Y chunk fields should be used.



| Type  | Initialised Value  | Description                                 |
| ----- | ------------------ | ------------------------------------------- |
| `int` | eg. `735623`       | <p>Unique integer value. <br>Read-only.</p> |



Example:

Send the chunk ID to nearby entities and check if they are within the same chunk.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    local nearby_entities = self:GetNearbyEntities(64)
    
    for index, entity in pairs(nearby_entities) do
        api.entity.Message(entity.ID, 
            {
<strong>                get_chunk_ID = chunk.ID,
</strong>                sender = self.ID
            }
        )
    end
end

local function message(self, msg)
    if msg.Data.get_chunk_ID then
        api.entity.Message(msg.Data.sender, 
            {
<strong>                chunk_ID_reply = chunk.ID,
</strong>                sender = self.ID 
            }
        )
    end
        
    if msg.Data.chunk_ID_reply then
        local is_match = msg.Data.chunk_ID_reply == chunk.ID and " is " or " is not "
        print("Entity "..msg.Data.sender..is_match.."in the same chunk as this entity.")
    end
end

-- Find every entity within 64 units distance.
-- Send the nearby entities a message, to get their chunk ID.
-- Provide the chunk ID to the original entity.
-- Compare each nearby entity's chunk ID against the current chunk and print the result.

-- Prints:
-- Entity c78c8d31-1831-48e0-a8cb-9abbc42bf191 is in the same chunk as this entity.
-- Entity fc9036be-88f9-4399-9180-8aea29f09137 is in the same chunk as this entity.
-- Entity 2d830b52-ecf9-4735-88b2-80e0641960cc is not in the same chunk as this entity.
</code></pre>

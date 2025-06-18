---
description: Premium Only
---

# Chunkloader

`entity.Chunkloader`

A boolean value, which dictates whether this entity will load chunks.&#x20;

If true, a chunkloader entity will cause the chunk it is in to remain loaded and processing. \
If false, the chunk may become unloaded and the entity's processing will cease until the chunk is reloaded.

Chunkloaders only maintain the processing of chunks. They will not load new chunks around them, unless they are a player entity.

| Type   | Initialised Value | Description                     |
| ------ | ----------------- | ------------------------------- |
| `bool` | `false`           | Default: False.                 |
|        | `true`            | Player Entity: True. Read-only. |



Example:

Set this cat to be a chunkloader, which always responds to messages.

<pre class="language-lua"><code class="lang-lua">-- cat.lua
local function init(self)
<strong>    self.Chunkloader = true
</strong>end

local function message(self, msg)
    print("This cat is guaranteed to process any messages sent to it immediately.")
    
    local tree_id = msg.Data.from
    api.entity.Message(tree_id, {
        is_loaded = "Cat has been loaded once ",
        is_chunkloader = "and it will persist until the game stops."
    })
end

-- tree.lua
local function update(self, dt)
    if api.util.Time() % 5 == 0 then
        local nearby_entities = self:GetNearbyEntities(1)
    
        for index, entity in pairs(nearby_entities) do
            if entity.Type == "cat" then
    	    api.entity.Message(entity.ID, {from = self.ID})
    	    break
            end
        end      
    end
end

local function message(self, msg)
    print(msg.Data.is_loaded..msg.Data.is_chunkloader)
end

-- Every 5 seconds, if a cat is within 1 world unit of a tree, send a message to it.
-- Print that the cat is guaranteed to respond.
-- Send a message back to the tree.
-- Print the cat's response.

-- Prints (possibly multiple times, during the second it is in range):
-- This cat is guaranteed to process any messages sent to it immediately.
-- Cat has been loaded once and it will persist until the game stops.
</code></pre>

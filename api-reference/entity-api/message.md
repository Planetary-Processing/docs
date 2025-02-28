# Message



`api.entity.Message(entityid, message)`

Sends a message to an entity.

This message can be sent to an entity anywhere in the world, if it exists.



Parameters:

| Name     | Type     | Description                                   |
| -------- | -------- | --------------------------------------------- |
| entityid | `string` | The ID of the entity, to send the message to. |
| message  | `table`  | The Data being conveyed.                      |



Example:

Send messages to nearby cat and tree entities.

```lua
-- entity_type_name.lua
local function update(self, dt)    
    local entity_list = self:GetNearbyEntities(5)
    
    for id, entity in pairs(entity_list) do
        local message = {
            words = "Hello "..entity.Type.."!",
            action = "pet",
            from = self.Type
        }
        api.entity.Message(entity.ID, message)
    end
end
    
-- cat.lua
local function message(self, msg)
    if msg.Data.from == "entity_type_name" then
        print(msg.Data.words)
        print("The cat has been "..msg.Data.action..". It goes \"Meow\".")
    end
end
    
--tree.lua
local function message(self, msg)
    if msg.Data.from == "entity_type_name" then
        print(msg.Data.words)
        print("The tree has been "..msg.Data.action..". It does not respond.")
    end
end

-- Whenever an entity_type_name comes within 5 world units of a cat or tree, 
-- send a message to all entities nearby.
-- Print the details of the message from whichever entities received it.

-- Prints:
-- Hello cat!
-- The cat has been pet. It goes Meow.

-- Or Prints:
-- Hello tree!
-- The tree has been pet. It does not respond.
```


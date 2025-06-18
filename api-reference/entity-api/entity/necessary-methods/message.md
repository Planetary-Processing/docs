# message



`function message(self, msg)`

Performs a process, whenever this entity instance receives a message. These messages can come from serverside entities or a game client.

As a necessary entity method, this function should be returned by the entity.lua file. It must be in a table under the key `message`.



Parameters:

| Name | Type                                                                                                        | Description                                 |
| ---- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| self | `Entity`                                                                                                    | This entity itself.                         |
| msg  | <p><code>table({</code><br>  <code>Data: table</code><br>  <code>Client: bool</code><br><code>)}</code></p> | A table containing the message content and  |

The `msg` table contains the following keys:

| Field  | Type    | Description                                                                                                             |
| ------ | ------- | ----------------------------------------------------------------------------------------------------------------------- |
| Data   | `table` | Contains the content of the message received.                                                                           |
| Client | `bool`  | <p>True, if the message was sent from a game client.</p><p></p><p>False, if sent from another entity on the server.</p> |



Example:



<pre class="language-lua"><code class="lang-lua">-- entity.lua
<strong>local function message(self, msg)
</strong>    if msg.Client then 
        local act = ""
        if msg.Data.action then
            act = msg.Data.action
        end
        
        if act == "move_random" then
            self:Move(math.random(-1,1), math.random(-1,1), 0)
        elseif act == "move" then
            self:Move(msg.Data.x, msg.Data.y, 0)
        elseif act == "move_to" then
            self:MoveTo(msg.Data.x*10, msg.Data.y*10, 0)
        end
        print("This entity moved based on a '"..act.."' message from the client.")
    else
        print("This message comes from an entity on the server.")
    end
end

return { init = function (self) end,
         update = function (self, dt) end,
         message = message}  

-- Check whether the message received is from a client or another entity serverside.
-- If the message has an 'action' key, record what that act is.
-- Move the entity randomly, or in a specific direction, or directly to a location.
-- Print which action was in the message.
-- Return the 'message' function as a value in a table.

-- Prints (dependant on the action):
-- This entity moved based on a 'move_random' message from the client.
-- Or:
-- This message comes from an entity on the server.
</code></pre>

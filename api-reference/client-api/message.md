# Message



`api.client.Message(playerid, message)`

Sends a message to a player client.

This sends a manual message from the server to the client of a player, via that player's serverside entity.

These messages can be received by the clientside SDK's server-to-client messaging function.



Parameters:

| Name     | Type     | Description                                                                        |
| -------- | -------- | ---------------------------------------------------------------------------------- |
| playerid | `string` | The ID of the player entity, to send the message through.                          |
| message  | `table`  | The Data being conveyed. Must be a key-value pair table, not an array-style table. |



Example:

Send a message back to the client, in response to a move message.

<pre class="language-lua"><code class="lang-lua">-- player.lua
local function message(self, msg)
    if msg.Client then
        local x, y, z = msg.Data.x, msg.Data.y, msg.Data.z
        self:Move(x, y, z)
	
	local move_message = {
		x = msg.Data.x,
		y = msg.Data.y,
		z = msg.Data.z
	}
	
<strong>	api.client.Message(self.ID, move_message)
</strong>    end
end

-- Receive a typical move message, sent from the client to the game server.
-- Reconstruct the same move message.
-- Send the move message back, from the server to the client.
</code></pre>


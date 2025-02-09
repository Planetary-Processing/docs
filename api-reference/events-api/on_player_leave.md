# on\_player\_leave



`on_player_leave(player)`

An event which runs when a player leaves the game.&#x20;



Parameters:

| Name   | Type   | Description                  |
| ------ | ------ | ---------------------------- |
| player | Entity | The leaving player's entity. |



Example:

When the player leaves, message a nearby entity.&#x20;

```lua
-- event.lua
local function on_player_leave(player)
	local nearbyEntities = player:GetNearbyEntities(64)
	local msg_data = {
		player_id = player.ID, 
		from = {X=chunk.X, Y=chunk.Y}
	}
	
	api.entity.Message(nearbyEntities["1"].ID, msg_data)
end
```

<pre class="language-lua"><code class="lang-lua">-- entity_type_name.lua
local function message(self, msg)
	print("Player "..msg.Data.player_id.." has left. "..
		"They were at chunk: ("..msg.Data.from.X..","..msg.Data.from.Y..")")
end

-- Prints:
<strong>-- Player e2e838b4-a250-4a89-9f1d-acae063201b8 has left. They were at chunk (2,3)
</strong></code></pre>

Note that the player entity will disconnect before any messages can be sent to it.&#x20;

# on\_player\_join



`on_player_join(player, first)`

An event which runs when a player joins the game.&#x20;



Parameters:

| Name   | Type      | Description                                    |
| ------ | --------- | ---------------------------------------------- |
| player | `Entity`  | The joining player's entity.                   |
| first  | `boolean` | True, if this player has never joined before.  |



Example:

When the player joins, print whether it is their first time.

<pre class="language-lua"><code class="lang-lua">-- events.lua
local function on_player_join(player, first)
	if first == true then 
		print("Player "..player.ID.." has joined for the first time! "..
			"They are in chunk: ("..chunk.X..","..chunk.Y..")")
	else
		print("Player "..player.ID.." has joined this game before. "..
			"They are in chunk: ("..chunk.X..","..chunk.Y..")")
	end
end

-- Prints:
<strong>-- Player e2e838b4-a250-4a89-9f1d-acae063201b8 has joined for the first time! They are in chunk: (0,0)
</strong><strong>
</strong><strong>-- Or prints:
</strong>-- Player e2e838b4-a250-4a89-9f1d-acae063201b8 has joined this game before. They are in chunk: (0,0)
</code></pre>

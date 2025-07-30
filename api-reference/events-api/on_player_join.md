# on\_player\_join



`on_player_join(player, first)`

An event which runs when a player joins the game.&#x20;

This function may only be called from within the `events.lua` file, and must be returned in a table at the end of the file.



Parameters:

| Name   | Type      | Description                                    |
| ------ | --------- | ---------------------------------------------- |
| player | `Entity`  | The joining player's entity.                   |
| first  | `boolean` | True, if this player has never joined before.  |



Example:

When the player joins, print whether it is their first time.

<pre class="language-lua"><code class="lang-lua">-- events.lua
<strong>local function on_player_join(player, first)
</strong>	if first == true then 
		print("Player "..player.ID.." has joined for the first time! "..
			"They are in chunk: ("..chunk.X..","..chunk.Y..")")
	else
		print("Player "..player.ID.." has joined this game before. "..
			"They are in chunk: ("..chunk.X..","..chunk.Y..")")
	end
end

-- Prints:
-- Player e2e838b4-a250-4a89-9f1d-acae063201b8 has joined for the first time! They are in chunk: (0,0)

-- Or prints:
-- Player e2e838b4-a250-4a89-9f1d-acae063201b8 has joined this game before. They are in chunk: (0,0)
</code></pre>

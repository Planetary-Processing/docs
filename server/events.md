# Events

Planetary Processing provides the ability for you to run functions when specific events occur. This is implemented in (optional) the `events.lua` file in the root directory of your game repository. The default template repository comes with the following events file:

```lua
local function on_player_join(player, first)
  print(player.ID.." has joined.")
end

local function on_player_leave(player)
  print(player.ID.." has left.")
end

return {on_player_join=on_player_join, on_player_leave=on_player_leave}
```

This simply causes the server to print out a message when a player joins or leaves. More events are being added constantly, however, currently they are limited to:

<table><thead><tr><th width="202">Event Name</th><th>Description</th><th>Arguments</th></tr></thead><tbody><tr><td><code>on_player_join</code></td><td>Called when a player joins the game.</td><td><code>player</code> - this player's entity<br><code>first</code> - boolean, true if this player has never joined before</td></tr><tr><td><code>on_player_leave</code></td><td>Called when a player leaves the game.</td><td><code>player</code> - this player's entity</td></tr></tbody></table>

Note that the Lua environment in use is that of the chunk where the event occurs, so you can still access data like the `chunk` table.

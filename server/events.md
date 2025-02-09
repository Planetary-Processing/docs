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

{% include "../.gitbook/includes/events-api-table.md" %}

Note that the Lua environment in use is that of the chunk where the event occurs, so you can still access data like the `chunk` table.

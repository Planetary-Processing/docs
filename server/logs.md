# Logging

Console logs and errors from running your server-side code will appear in the [logs](https://panel.planetaryprocessing.io/logs) section of the Planetary Processing website.

## Creating Logs

While your game is running with the latest deployed version, Planetary Processing will log any errors or `print()` functions triggered by your server-side Lua code.

The default demo server code has an example of this in the `player.lua` entity file. The `print(msg)` function will trigger on any message event to the entity, such as a move message, and log that message to the Planetary Processing website.

```lua
-- called when this entity receives a message
-- as this is the player entity it can also receive messages from the game client
local function message(self, msg)
    -- print and error output can be viewed per-chunk in the control panel
    print(msg)
    -- if this is a client message (i.e. from a game client) then look in the message's Data table
    if msg.Client then
        local x, y, z = msg.Data.x, msg.Data.y, msg.Data.z
        if msg.Data.threedee then
            self:MoveTo(x, z, y) -- PP uses 'y' for depth in 3 dimensional games, and 'z' for height
        else
            self:Move(x, y, 0)
        end
    end
end
```

Logs will also be automatically created for any uncaught errors in the server-side code.

## Log Filters

Logs and log groups can be filtered by game, dimension, time period, and world chunk.

![Log Filters Image](https://planetaryprocessing.io/static/img/pp_log_filters.png)

## Log Groups

Log groups are used to batch display logs from the same source together. The most recent log group will display at the top of the table. A log group will show the date and time of its first and last log entry. You can click on a log group's id to drill down to its individual log entries.

![Log Group Image](https://planetaryprocessing.io/static/img/pp_log_groups.png)

## Log Entries

Logs will report the time they occurred, the dimension and chunk they occurred in, and the printed message or error. The most recent log entry will appear at the bottom of the table.

While a log group is still active, the entries can be refreshed by clicking 'refresh' at the bottom of the table; or using the refresh symbol at the top right of the page.

![Log Entries Image](https://planetaryprocessing.io/static/img/pp_log_entries.png)

## Panel Map

You can also access the logs from the game dashboard, by clicking 'view logs' in the bottom right of the map. This will automatically filter the log groups by game and chunk.

![Map View Log Image](https://planetaryprocessing.io/static/img/pp_map_view_log.png)

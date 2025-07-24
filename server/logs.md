# Logging

Console logs and errors from running your server-side code will appear in the [logs](https://panel.planetaryprocessing.io/logs) section of the Planetary Processing website.

## Creating Logs

While your game is running with the latest deployed version, Planetary Processing will log any [errors](logs.md#errors) or `print()` functions triggered by your server-side Lua code.

```lua
-- called when this entity receives a message
local function message(self, msg)
    -- print the message sent to this entity to logs  
    print(msg)
end
```

Logs will also be automatically created for any uncaught [errors](logs.md#errors) in the server-side code.



## Monitoring Logs

The full list of logs is the located in the [logs](https://panel.planetaryprocessing.io/logs) section of the web panel. Log [entries](logs.md#entries) are contained in batches called [log groups](logs.md#groups). Entries and groups can be filtered by [game](https://panel.planetaryprocessing.io/games), [dimension](dimensions.md), time period, and [chunk](chunks.md). You can also use a keyword, to search for specific [entries](logs.md#entries).

![Log Filters Image](https://planetaryprocessing.io/static/img/pp_log_filters.png)

### Groups

Log groups are used to batch display logs from the same source together. The most recent log group will display at the top of the table. A log group will show the date and time of its first and last log [entry](logs.md#entries). You can click on a log group's ID to view individual log [entries](logs.md#entries).

![Log Group Image](https://planetaryprocessing.io/static/img/pp_log_groups.png)

### Entries

Logs will display information about: the time they occurred; the [dimension](dimensions.md) and [chunk](chunks.md) they occurred in; and the printed [message](logs.md#messages) or [error](logs.md#errors) itself.&#x20;

The most recent log entry will appear at the bottom of the table. While a log group is still active, the entries can be refreshed by clicking the word 'refresh' below the table.

![Log Entries Image](https://planetaryprocessing.io/static/img/pp_log_entries.png)

### Messages

Messages contain any information sent to logs using `print()` function. Because the logging process is asynchronous, messages will not always arrive in the logs in the exact order they are sent.\
\
Where possible variables will be printed in a pretty format, for example printed entities will display their individual field values, space-separated, starting with their ID; X, Y, Z positions; Type; and Data table.

Most tables nested within other tables will only show their memory address rather than being prettified. Very large numbers will be displayed using scientific 'e' notation.&#x20;

Any individual messages greater than 512B in length will have their message truncated.



## Errors

Errors and their stack traces are displayed in the [logs](https://panel.planetaryprocessing.io/logs) section of the web panel, in the same way as other entries.&#x20;

The `error()` function can be used to log custom error messages, with a stack trace. However, most API functions will send an error message automatically, if there is a problem.&#x20;

Significant errors will also be visible directly from the panel map. Chunks which are failing or contain erroring entities will be coloured red on the map. Clicking on these [chunks](chunks.md) will show an information icon 🛈 with the error causing the failure.



### Log Limit

A game has the potential to produce large quantities of log entries, since each [entity](entities.md) can be printing or sending errors [every game tick](entities.md#update). If the game detects too many logs in the same second, it will display this message:

```
Rate limit exceeded [40/s]
```


# Defold

## Installation

The Defold SDK can be installed by navigating to your `game.project` file, then in the 'Project' tab (in the Editor window) you will see the 'Dependencies' list.

You need to add the following two dependencies:

* The Planetary Processing SDK: `https://github.com/Planetary-Processing/defold-sdk/archive/master.zip`
* [Defold Websocket](https://github.com/defold/extension-websocket), which we depend on: `https://github.com/defold/extension-websocket/archive/master.zip`
* [Defold Protobuf](https://github.com/Melsoft-Games/defold-protobuf), which we depend on: `https://github.com/Melsoft-Games/defold-protobuf/archive/master.zip`

![Defold dependencies](https://planetaryprocessing.io/static/img/defold_dependencies.png)

You need then to fetch dependencies, this is done with the 'Fetch Libraries' button under 'Project' in the topbar.

![Defold fetch](https://planetaryprocessing.io/static/img/defold_fetch.png)

## Components

The Defold SDK provides two new components which can be added to GameObjects. These are master and entity.

Master represents the main connection point to PP's servers and performs the heavy lifting and orchestration of the SDK. You must have a GameObject in your game which has the master component.

You are required to, for every type of entity you wish to display in the Defold client, create a GameObject file containing the entity component. You then need create factories for these attached to the same GameObject as the master component. These factories should be named `typenamefactory` as in the example below for the types `cat`, `player` and `tree`.

![Defold master](https://planetaryprocessing.io/static/img/defold_master.png)

Please note that the `player` type is only for representing other players. For the local player, you need to create a separate GameObject in your collection (not in a GameObject file) and make sure it contains an entity component.

Within the master component you will need to set the URL of the local player GameObject and the Planetary Processing game ID.

![Defold config](https://planetaryprocessing.io/static/img/defold_config.png)

By default, entities will be moved to their server-side position, you can disable this per entity type by un-ticking 'Use Server Position' in the entity component's config window. Should you not want this, you can still keep track of the server position using messages below.

## Messages

The first thing you need to do when your game starts is initialise the SDK, to do this you send the master component (or its whole GameObject) a message with ID `hash("pp_init")`. The sender url of this message will be allocated as the 'listener' and will be sent all updates in future. If you are using authentication, you should supply username and password in a table like so:

```lua
{
    username="Xx_CoolDude69420_xX",
    password="P455W0RD"
}
```

The PP SDK sends and receives several different messages and accepts several too. Each time an entity updates its position or data, a message with ID `hash("pp_update")` is sent to that entity's GameObject and the listener.

This message is of the following format, the same format is used for `pp_spawn` and `pp_delete` messages also:

| Field | Type   | Description                  |
| ----- | ------ | ---------------------------- |
| uuid  | string | UUID of the entity.          |
| x     | float  | X coordinate in world units. |
| y     | float  | Y coordinate in world units. |
| z     | float  | Z coordinate in world units. |
| data  | table  | Data of the entity.          |
| type  | string | Type of the entity.          |

An example is below, showing an update message being handled by simply printing out all the key-value pairs in the data table.

![Defold update msg](https://planetaryprocessing.io/static/img/defold_update.png)

By default, the player will not have joined the world, in order to do this, you should send an empty message with ID `hash("pp_join")` to the master component (or its GameObject).

![Defold join msg](https://planetaryprocessing.io/static/img/defold_join.png)

Finally, if you wish to send a message to the player entity on the server, to be handled by the `message` function on the server side API, simply send a message with ID `hash("pp_message")` where the message content is the Lua table to be sent as a message.

![Defold message msg](https://planetaryprocessing.io/static/img/defold_post.png)

### Message Directory

| Message Hash              | Description                                                                            | Fields                                                                                 |
| ------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `pp_init`                 | Send this to the master component to connect to the PP servers.                        | For anonymous games, nothing, otherwise: `username` and `password`.                    |
| `pp_join`                 | Send this to the master component to spawn your player into the world.                 | None.                                                                                  |
| `pp_message`              | Send this to the master component to send a message to your server-side player script. | Arbitrary table.                                                                       |
| `pp_disconnect`           | Send this to the master component to disconnect from PP's servers.                     | None.                                                                                  |
| `pp_update`               | Sent to an entity (and the listener) when it changes its server-side state.            | See above table for `pp_update` message format.                                        |
| `pp_spawn`                | Sent to the listener when a new entity spawns.                                         | Same as `pp_update`.                                                                   |
| `pp_delete`               | Sent to the listener when an entity is removed.                                        | Same as `pp_update`.                                                                   |
| `pp_authentication_error` | Sent to the listener when PP fails to authenticate.                                    | Short error string (`error`), an error code (`code`) and a longer message (`message`). |
| `pp_authentication_error` | Sent to the listener when PP fails to authenticate.                                    | Short error string (`error`), an error code (`code`) and a longer message (`message`). |
| `pp_connected`            | Sent to the listener when the client successfully connects and authenticates.          | The connected player's `uuid`.                                                         |
| `pp_disconnected`         | Sent to the listener when the client disconnects from PP's servers.                    | A `message` field explaining cause of disconnection.                                   |
| `pp_connection_error`     | Send to the listener when there is an error during initial connection.                 | The `error` string.                                                                    |

## Example Project

You can find an example project in the same repo as the library, on our [GitHub](https://github.com/planetary-processing/defold-sdk).

## Compatibility

Please note that currently the Defold SDK is incompatible with the HTML build targets due to the lack of LuaJIT (see [here](https://defold.com/manuals/lua/)).

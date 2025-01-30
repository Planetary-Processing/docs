# LÖVE

## Introduction

The LÖVE SDK can be installed by cloning the latest version from our [GitHub](https://github.com/planetary-processing/) into your a folder called `sdk` within your LÖVE game directory.

```sh
git clone https://github.com/planetary-processing/love2d-sdk sdk
```

You can then import the sdk like so:

```lua
local sdk = require(“sdk.sdk”)
```

The SDK object returned will have functions:

| Method              | Parameters     | Description                                                   |
| ------------------- | -------------- | ------------------------------------------------------------- |
| `sdk.init(game_id)` | `game_id: int` | Called to connect to the Planetary Processing servers.        |
| `sdk.update()`      | None           | Call this method inside `love.update`.                        |
| `sdk.message(msg)`  | `msg: table`   | Send a message to the player entity on the server.            |
| `sdk.join()`        | None           | Spawn into the world.                                         |
| `sdk.leave()`       | None           | De-spawn from the world.                                      |
| `sdk.quit()`        | None           | Disconnect and de-auth from the Planetary Processing servers. |

And variables:

| Variable       | Description                                     |
| -------------- | ----------------------------------------------- |
| `sdk.entities` | List of entities the client can see.            |
| `sdk.chunks`   | List of chunks the client can see.              |
| `sdk.uuid`     | UUID of the player associated with this client. |

Within `love.load` you need to run `sdk.init(game_id)` where `game_id` is found on your control panel, our LÖVE SDK only supports anonymous authentication at present, so you need not supply a username and password.

Within `love.update` you need to run `sdk.update`.

You can then access all the entities within `sdk.entities` (which can be iterated over) and the entity ID of the player associated with this client as `sdk.uuid`. Note that this means you can access the player entity with `sdk.entities[sdk.uuid].`

You can call `sdk.message(msg)` to send a message to the player entity on the server (e.g. to command it to perform an action).

You can access any chunk's data by going to `sdk.chunks` each chunk has its ID, X, Y and Data fields as in the server side environment.

The entity table within the LÖVE SDK is different to that on the server, it is defined as such:

#### Entity

**Fields**

| Field    | Type   | Description                  |
| -------- | ------ | ---------------------------- |
| EntityID | string | UUID of the entity.          |
| X        | float  | X coordinate in world units. |
| Y        | float  | Y coordinate in world units. |
| Data     | table  | Data of the entity.          |
| Type     | string | Type of the entity.          |

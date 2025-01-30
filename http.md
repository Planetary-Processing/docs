---
hidden: true
---

# HTTP API

## <mark style="color:red;background-color:red;">**DRAFT DOCUMENTATION - NOT RELEASED AND NOT FINAL**</mark>

Planetary Processing has a simple HTTP API which can be used to manage your games. This is only accessible to games on our premium tier. All endpoints should be prefixed with `https://api.planetaryprocessing.io/v1`

## Authentication

Your API access token can be generated in the game administration section of the control panel. It should be supplied as a bearer token in an 'Authorization' header to all requests like so: `Authorization: Bearer <token>`

## Endpoints

### Game Info and Control

#### **`GET /game`**

Returns a the game associated with this access token in the following format:

```json
{
  "id": "42",
  "name": "My Game",
  "chunk_size": 32,
  "tps": 10,
  "repo_url": "https://git.planetaryprocessing.io/abc/xyz.git",
  "running": true,
  "anonymous_auth": false,
  "tier": "PREMIUM",
}
```

#### **`POST /game/start`**

Starts your game, will return a `200 OK` if successful, or if the game is already started.

#### **`POST /game/stop`**

Stop your game, will return a `200 OK` if successful, or if the game is already stopped.

#### **`POST /game/update`**

This will deploy the latest version of your game's server-side code from your git repository. `200 OK` response if successful, note that this will still be a `200 OK` response if your game is in an error state after the update.

### Game State Extraction / Importing

These endpoints are used to examine or update the state of a game that is currently switched off, they will fail if the game in question is running.

#### **`GET /game/chunk/{x}/{y}?dimension=`**

This allows you to export a chunk in the below format, specify chunk coordinates (in chunk space) and optionally specify dimension (defaults to the default dimension). If this chunk is loaded, it will fetch the live version of the chunk, else it will be retrieved from the backing store.

```json
{
  "id":"123",
  "x":"1",
  "y":"-1",
  "dimension": "",
  "data": {},
  "entities": [
    {
      "uuid":"2baa4ccc-d84f-4396-b110-15bc05ad2517",
      "x":1.5,
      "y":5.1,
      "z":0,
      "data": {},
      "type":"type",
      "chunkloader":false,
    },
    ...
  ],
}
```

#### **`PUT /game/chunk`**

This allows you to explicitly import chunk data into the world, this will result in the chunk bypassing its init.lua call and instead being pre-populated as per your instructions. The same data structure as GET is used as the request body. Note that the chunk'd ID and all entity UUIDs will be automatically replaced. This can only be called while the game is off.

````json
{
  "id":"123",
  "x":"1",
  "y":"-1",
  "dimension": "",
  "data": {},
  "entities": [
    {
      "uuid":"2baa4ccc-d84f-4396-b110-15bc05ad2517",
      "x":1.5,
      "y":5.1,
      "z":0,
      "data": {},
      "type":"type",
      "chunkloader":false,
    },
    ...
  ],
}

#### `GET /game/entity/{uuid}` & `PUT /game/entity/{uuid}`

Fetch an entity by its UUID and return the state as below, you may also update an entity in this way by using PUT and supplying the modified entity in the body. May be called on a running game, if the entity is loaded, it will be deleted and replaced with the updated version.

```json
{
  "uuid":"2baa4ccc-d84f-4396-b110-15bc05ad2517",
  "x":1.5,
  "y":5.1,
  "z":0,
  "data": {},
  "type":"type",
  "chunkloader":false,
}
````

#### **`POST /game/entity/{uuid}`**

Sends a message to the specified entity, the request body should be the JSON representation of the Lua table to send.

#### Player Management

These endpoints allow creating, deleting and listing players.

#### **`GET /game/players`**

Will return a list of players for the given game in the following format:

```json
[
  {
    "uuid": "deadbeef-dead-beef-dead-beefdeadbeef",
    "username": "deadbeef@deadbeef.deadbeef"
  },
  ...
]
```

#### **`PUT /game/players`**

This endpoint creates a new player, it must include a body of the following JSON format:

```json
{
  "username": "username",
  "password": "password"
}
```

#### **`DELETE /game/players/{uuid}`**

Deletes the player with a given UUID from game {id}.

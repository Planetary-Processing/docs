---
description: Premium Only
coverY: 0
layout:
  cover:
    visible: false
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Player API

We provide an API for the creation, deletion and listing of players associated with a game.

## Create New Player

To create a new player, you must send a `POST` request to `https://api.planetaryprocessing.io/api/players` with a JSON body containing the username and password of the new player.\
\
Your request must also have your game's personal [API Key](authentication.md) in the `X-API-KEY` header.&#x20;

```json
{
    "username": "sam",
    "password": "agoodpassword"
}
```

This will return the Player ID in a JSON response:

```json
{
    "player_id": "8a319bfb-5a71-41ed-a67c-5fd44b40fb11"
}
```

#### cURL Example

```bash
curl -X POST https://api.planetaryprocessing.io/api/players/ \
-H "X-API-KEY: yourapikeyhere" \
--data '{"username": "sam", "password": "agoodpassword"}'
```



## Delete Player

To delete a player, you must send a `DELETE` request to  `https://api.planetaryprocessing.io/api/players` with a JSON body containing the UUID of the player,.

Your request must also have your game's personal [API Key](authentication.md) in the `X-API-KEY` header.&#x20;

```json
{
    "player_id": "8a319bfb-5a71-41ed-a67c-5fd44b40fb11"
}
```

#### cURL Example

```bash
curl -X DELETE https://api.planetaryprocessing.io/api/players/ \
-H "X-API-KEY: yourapikeyhere" \
--data '{"player_id": "8a319bfb-5a71-41ed-a67c-5fd44b40fb11"}'
```



## List Players

To list players, simply call `GET` on the `https://api.planetaryprocessing.io/api/players/` endpoint with your [API key](authentication.md) in the `X-API-KEY` header. It will return a JSON array of players like so:

```json
[
    {"username":"bill","player_id":"4fef6e6a-6d2a-446e-b13f-d67e02d1924a"},
    {"username":"stan","player_id":"57563f34-f536-4932-a7dc-3a351b6c63c3"},
]
```

#### cURL Example

```bash
curl -X GET https://api.planetaryprocessing.io/api/players/ \
-H "X-API-KEY: yourapikeyhere" 
```


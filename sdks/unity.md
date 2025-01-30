# Unity

## Installation

The Unity SDK can be installed by opening the Unity Package Manager (Window->Package Manager) and clicking the + on the top left and selecting 'Add package from git URL...'

![](https://planetaryprocessing.io/static/img/unity_install.png)

You then enter the GitHub URL of the PP Unity SDK repository, which is: `https://github.com/Planetary-Processing/unity-sdk.git`

And click 'Add' to add the package.

![](https://planetaryprocessing.io/static/img/unity_git.png)

## Components

The Unity SDK provides three new components which can be added to GameObjects. These are PPMaster, PPEntity and PPChunk. These can be accessed in the 'Add Component' menu of any Gameobject under the PP heading.

![Unity component](../.gitbook/assets/Untitled.png)

PPMaster represents the main connection point to PP's servers and performs the heavy lifting and orchestration of the SDK. You must have a GameObject in your game which has the master component.

PPEntity represents a server-side entity in the game world. You are required to, for every type of entity you wish to display in the Unity client, create a Prefab containing the PPEntity component.

![Unity entity](https://planetaryprocessing.io/static/img/unity_entity.png)

By default, entities' GameObjects will be moved to their server-side position, you can disable this per entity type by un-ticking 'Use Server Position' in the entity component's inspector window. Here you also need to set the entity's type, which must match the server-side type that this Prefab represents.

Please note that the `player` type is only for players other than the one connecting with this client. You need to create a separate GameObject (not in a prefab) for the local player which also contains a PPEntity component.

The final component, PPChunk, is to represent each chunk in the game world. As detailed here: [chunks.md](../server/chunks.md "mention") each chunk has a data table, which is synced down to the client with the PPChunk component. To use this, you must create a Prefab with the PPChunk component on it, which will be instantiated at the corner of each chunk and will hold its data.

You need to add the entity Prefabs to the Prefabs field in the PPMaster inspector window. Here you will also need to set a reference to the local player GameObject, the Chunk Prefab and also, the Planetary Processing game ID and chunk size.

![Unity config](<../.gitbook/assets/Capture d’écran 2025-01-28 à 15.17.33.png>)

## API

Below is a reference of the API available within Unity's C# environment.

### Components

The PPEntity and PPMaster components expose various public methods which can be accessed within MonoBehaviour scripts by getting a reference to the component like so:

```csharp
PPMaster master = GetComponent<PPMaster>();
PPEntity entity = GetComponent<PPEntity>();
PPChunk chunk = GetComponent<PPChunk>();
```

#### PPMaster

PPMaster exposes the following methods:

| Method                     | Parameters                                                            | Return Type    | Description                                                                                                        |
| -------------------------- | --------------------------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------ |
| `Init(username, password)` | <p><code>username: string</code><br><code>password: string</code></p> | None           | Connect to and authenticate with the Planetary Processing servers.                                                 |
| `Join()`                   | None                                                                  | None           | Spawn the player into the world.                                                                                   |
| `Message(msg)`             | `msg: Dictionary<string, object>`                                     | None           | Send a message to the player entity on the server, to be handled by the `message` function on the server side API. |
| `GetEntity(uuid)`          | `uuid: string`                                                        | `Entity`       | Get Entity details by UUID.                                                                                        |
| `GetEntities()`            | None                                                                  | `List<Entity>` | Get a list of all Entities that this client can see.                                                               |

#### PPEntity

PPEntity exposes the following methods:

| Method                | Parameters | Return Type                  | Description                                                                                                                                                                                                                                                     |
| --------------------- | ---------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GetServerPosition()` | None       | `Vector3`                    | Get the server-side position of this entity. Particularly useful if you are not using the 'Use Server Position' feature. **NOTE: Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height, the Unity SDK automatically swaps these.** |
| `GetServerData()`     | None       | `Dictionary<string, object>` | Get the server-side data of this entity.                                                                                                                                                                                                                        |
| `GetUUID()`           | None       | `string`                     | Get this entity's UUID.                                                                                                                                                                                                                                         |

#### PPChunk

PPChunk exposes the following methods:

| Method            | Parameters | Return Type                  | Description                             |
| ----------------- | ---------- | ---------------------------- | --------------------------------------- |
| `GetServerData()` | None       | `Dictionary<string, object>` | Get the server-side data of this chunk. |

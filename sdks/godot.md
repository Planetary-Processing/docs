# Godot

The Godot SDK integrates our multiplayer platform with the Godot Engine. The plugin provides nodes and signals to use within your game client code - these communicate changes in state from your server side simulation to the game client itself.

If you are new here, we recommend starting with the [Godot Quickstart Guide](../quick-start/godot.md).

## Pre-requisites

* Godot Engine v4.2.1+ **.NET version required**
* .NET SDK 6.0+ (Desktop target)
* .NET SDK 7.0+ (Android target)
* .NET SDK 8.0+ (iOS target)

## Installation

* Clone the Planetary Processing Godot plugin from GitHub.

```sh
git clone https://github.com/planetary-processing/godot-plugin planetary_processing
```

* Create the 'addons' directory in your project if it does not exist.
* Place the 'planetary\_processing' directory in the 'addons' directory
* Enable the Planetary Processing plugin via the 'Plugins' tab in 'Project Settings'

![Plugin Menu](https://planetaryprocessing.io/static/img/godot_plugin_menu.png)

* If not already setup on your project, you must trigger the creation of the C# solution via the Godot toolbar menu. This creates a _.csproj_ and a _.sln_ file in the root of your project.

![Csharp Menu](https://planetaryprocessing.io/static/img/godot_csharp1.png)

* Finally, we need to add a reference to the Planetary Processing C# SDK DLL to our _.csproj_ file. This can either be done manually, or via the button in the [PPRootNode ](godot.md#pprootnode)inspector. To add manually, please ensure the following reference is added to your ItemGroup in your _.csproj_ file

```xml
<Reference Include="Planetary">
  <HintPath>addons/planetary_processing/sdk/csharp-sdk.dll</HintPath>
</Reference>
```



## Nodes

Once the Planetary Processing plugin is enabled, three custom nodes are made available to your project: [PPRootNode](godot.md#pprootnode), [PPEntityNode](godot.md#ppentitynode), and [PPChunkNode](godot.md#ppchunknode).

### PPRootNode

The Planetary Processing Root Node ([PPRootNode](godot.md#pprootnode-1)) handles connections to your game backend, when running your game, while also enabling some additional functionality in the Godot editor.

A [PPRootNode ](godot.md#pprootnode-1)should be attached as a direct child of your main scene's root node. It is intended to be attached to your main scene so as to be accessible to all child scene instances, although this may vary dependent on your project structure. Due to the way the node handles connections to your game simulation, you should only have one [PPRootNode ](godot.md#pprootnode-1)in your project.&#x20;

<figure><img src="../.gitbook/assets/sdk_godot_main_scene_root_node (1).png" alt=""><figcaption></figcaption></figure>

To configure your game, you must fill in the fields in the inspector:

* **Chunk Size** - The size of chunks in your game, defined in the [web panel](https://panel.planetaryprocessing.io/games) game settings.
* **Player Scene** - The scene representing your main player.
* **Chunk Scene** - (Optional) The scene representing chunk storage in the world.
* **Scenes** - An array of scenes with each element representing a different entity in your game.
* **Game ID** - The ID of your game from the [web panel](https://panel.planetaryprocessing.io/games), for Godot to connect to.
* **Server to Client Node** - A Node which receives manual messages from the game [server to the client](godot.md#server-to-client-messaging).
* **Add Csproj Reference** - If you have already added the "Planetary" reference to your ._csproj_ file, then the **Add Csproj Reference** button will not be visible. If it is present, you can click the button to have the plugin automatically add the reference to your _.csproj_ file.

<figure><img src="../.gitbook/assets/PPRootNode (2).png" alt="" width="296"><figcaption></figcaption></figure>



### PPEntityNode

The Planetary Processing Entity Node ([PPEntityNode](godot.md#ppentitynode-1)) describes a [Planetary Processing Entity](https://panel.planetaryprocessing.io/docs#server-side-api-documentation/types/entity). This node is designed to be attached directly to the root node of a scene in your Godot project. This scene will then be instantiated as a child of your main scene. For example, a cat scene will correlate with a `cat.lua` entity in your Planetary Processing game.

With the [PPEntityNode ](godot.md#ppentitynode-1)selected, you will be able to edit the following in your inspector:&#x20;

* **Type** - The type field should correlate with whichever server side [Entity Type](../server/entities.md#types-and-behaviour-scripting) this scene is intended to represent, for example 'cat' or 'player'.

<figure><img src="../.gitbook/assets/sdk_godot_ppentity (3).png" alt=""><figcaption></figcaption></figure>



### PPChunkNode

The Planetary Processing Chunk Node ([PPChunkNode](godot.md#ppchunknode-1)) describes a [Planetary Processing Chunk](../server/chunks.md). This node is designed to be attached directly to the root node of a scene in your Godot project. This scene will then be instantiated as a child of your main scene for each chunk in the world. Data passed to this node will come from the `init.lua` file on your Planetary Processing game server.

With the [PPChunkNode ](godot.md#ppchunknode-1)selected, you will be able to edit the following in your inspector:&#x20;

* **Type** - The type field should be 'chunk'.

<figure><img src="../.gitbook/assets/sdk_godot_ppchunk (2).png" alt=""><figcaption></figcaption></figure>



## Signals and Messaging

Messages can be sent to your game server along a connection established by the [PPRootNode](godot.md#pprootnode-1). Signals from the server are sent to each of the [PP nodes](godot.md#api) on the clientside. The Planetary Processing Plugin uses these signals to manage you game and entities, but you can also connect to these signals in your scripts.



### [authenticate\_player](godot.md#pprootnode-1)

* Message to establish a connection

You may log a player in using the [`authenticate_player`](godot.md#pprootnode-1) method on the [PPRootNode](godot.md#pprootnode-1). After logging a player in, the custom nodes will begin emitting signals.

<pre class="language-gdscript"><code class="lang-gdscript"># scene_root_node_script.gd
<strong>var pp_root_node
</strong><strong>
</strong><strong># access the PPRootNode from the scene's node tree
</strong>pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
assert(pp_root_node, "PPRootNode not found") 

# authorise your connection to the game server 
#(Anonymous Auth uses empty strings as parameters)
pp_root_node.authenticate_player("", "")
</code></pre>



### [new\_player\_entity](godot.md#pprootnode-1)

* Signal when player has joined

For the player who has just logged in, the [PPRootNode ](godot.md#pprootnode-1)will emit a [`new_player_entity`](godot.md#pprootnode-1) signal. This signal is specific to the player running this instance of the game client. You can connect to this signal and extend the instantiation of your player scene as you see fit.

```gdscript
func _on_new_player_entity(entity_id, state):
    # your code
```



### [new\_entity](godot.md#pprootnode-1)

* Signal when entities spawn

The [PPRootNode ](godot.md#pprootnode-1)will emit a [`new_entity` ](godot.md#pprootnode-1)signal for any entities that have not yet been seen by the game client. After first logging in, this signal will trigger for all entities in the game world, including other players. These scenes will then be instantiated and added to the scene's node tree. You can also connect to the signal.

```gdscript
func _on_new_entity(entity_id, state):
    # your code
```



### [remove\_entity](godot.md#pprootnode-1)

* Signal when entities despawn

If any entities have left the simulation, the [PPRootNode ](godot.md#pprootnode-1)will emit a [`remove_entity` ](godot.md#pprootnode-1)signal.&#x20;

```gdscript
# scene_root_node_script.gd
func _on_remove_entity(entity_id):
  # your code
```



### [**state\_changed**](godot.md#ppentitynode-1)

* Signal when entities update

Each [PPEntityNode ](godot.md#ppentitynode-1)will emit a [`state_changed` ](godot.md#ppentitynode-1)signal each frame. You can use these signals to interpret movement from the server or trigger changes to your scenes as necessary. This script could be applied to the root node of an entity scene, for moving players or entities around the world.

```gdscript
# entity_scene_script.gd
extends Node3D

func _ready():
    var pp_entity_node = get_node("PPEntityNode")
    # connect to the state_changed signal from PPEntityNode
    pp_entity_node.state_changed.connect(_on_state_changed)

func _on_state_changed(state):
    # NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height. The depth axis is also inverted.
    # To convert coordinates, set Godot's 'y' to negative, then swap 'y' and 'z'.
    global_transform.origin = Vector3(state.x, state.z, state.y) 
```



### [message](godot.md#pprootnode-1)

* Message to send data to the server

You can send regular updates from the player to the game server using the [`message()`](godot.md#pprootnode-1) function on the [PPRootNode](godot.md#pprootnode-1). These messages allow the player to interact with the game simulation. It is up to you what data you want to send and how to process it in your server side code. All messages from the Godot client are received by the `player.lua` file on the server by default, unless you use [direct messaging](godot.md#direct_message).

Depending on your game, you might choose to send updates every frame, on keyboard press inputs, or on a fixed time delta, for example every 33ms.

```gdscript
# local_player_script.gd
var pp_root_node

func _ready() -> void:
  pp_root_node = get_tree().current_scene.get_node('PPRootNode')
  assert(pp_root_node, "PPRootNode not found")

func _process(delta: float) -> void:
  # ... your existing player movement code ...

  # send the player's current position and their health
  # NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height. The depth axis is also inverted.
  # To convert coordinates, set Godot's 'y' to negative, then swap 'y' and 'z'.
  var current_position = global_transform.origin
  pp_root_node.message({
    "x": current_position[0],
    "y": -current_position[2],
    "z": current_position[1],
    "health": 7
  })
```

Your corresponding `player.lua` file would receive it like so:

```lua
-- player.lua
local function message(self, msg)
  local x, y, z = msg.Data.x, msg.Data.y, msg.Data.z
  if msg.Client then
      self:MoveTo(x,y,z) 
      print("The player's health is".. msg.Data.health)
  end
end
```



### [direct\_message](godot.md#pprootnode-1)

* Message to send data to a specific entity on the server

You can send a message to a specific entity instance on the game server using the [`direct_message()`](godot.md#pprootnode-1) function on the [PPRootNode](godot.md#pprootnode-1). A direct message works much like a regular message. Except rather than being received by `player.lua`, it is handled in the `message()` function of the relevant entity. The [entity's ID](../server/entities.md#entity) on the server will match the PPEntityNode's [`entity_id`](godot.md#ppentitynode-1) variable.

```gdscript
var pp_entity_node = get_node_or_null("PPEntityNode")
var id = pp_entity_node.entity_id
# eg. pp_entity_node.entity_id == "e77c661b-1604-4e05-8e34-4a4cd6ac2178"

pp_root_node.direct_message( id , {"health": 6 })
```



## Server To Client Messaging

The [PPEntityNode](godot.md#ppentitynode-1) automatically receives and syncs all [Entity](../server/entities.md) data from the game world. Any client can access Entity data passed from the server, using the `state_changed` signal.

Alternatively, messages can be manually sent to a specific client using [`api.client.Message()`](../api-reference/client-api/message.md). These message can be received by a designated [Server To Client Node](godot.md#pprootnode). This Node needs a custom script, with a function named [`server_to_client`](godot.md#custom-scripts), which receives the server message as a parameter.

```gdscript
func server_to_client(message):
    print(message)
```



## API

Below are reference tables of the API available within Godot's environment.

The [PPRootNode](godot.md#pprootnode-1), [PPEntityNode](godot.md#ppentitynode-1), and [PPChunkNode ](godot.md#ppchunknode-1)each expose various public methods which can be accessed within scripts by getting a reference to the node like so:

```gdscript
# scene_root_node_script.gd
var pp_root_node = get_node_or_null("PPRootNode")
var pp_entity_node = get_node_or_null("PPEntityNode")
var pp_chunk_node = get_node_or_null("PPChunkNode")
```

To connect to the server and join with the player, for example:

```csharp
var pp_root_node = get_node_or_null("PPRootNode")
pp_root_node.authenticate_player("", "")
```

### PPRootNode

#### **Variables**

| Name                  | Type             | Description                                                                                                                 |
| --------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `player_uuid`         | `string or null` | Will be null when the player is not authenticated. Once a player authenticates, this will be set to their unique identifier |
| `player_is_connected` | `bool`           | True if the connection is established.                                                                                      |

#### **Methods**

| Method                | Parameters                                                                           | Return                                                                               | Description                                                                                                                            |
| --------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `authenticate_player` | <p><code>username: string</code> </p><p><code>password: string</code></p>            | boolean: true if authenticated, will throw assertion error with error message if not | Authenticates a player with Planetary Processing. If the game is using anonymous auth, can be called with empty strings for arguments. |
| `message`             | `msg: Dictionary(String, Variant)`                                                   | null                                                                                 | Sends a message to the player entity on the server, which can then be processed by your `player.lua` code.                             |
| `direct_message`      | <p><code>uuid: string</code></p><p><code>msg: Dictionary(String, Variant)</code></p> | null                                                                                 | Sends a message directly to the entity with the specified UUID on the server, which can then be handled in its `message()` function.   |

#### **Signals**

| Name                | Parameters                                                          | Description                                                                                                               |
| ------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `new_player_entity` | <p><code>id: string</code></p><p><code>data: dictionary</code></p>  | Fired when the player entity representing the player using this game client is spawned in the server side game simulation |
| `new_entity`        | <p><code>id: string</code>,</p><p><code>data: dictionary</code></p> | Fired when a new entity is spawned in the server side game simulation                                                     |
| `remove_entity`     | `id: string`                                                        | Fired when an entity is removed from the server side game simulation                                                      |
| `new_chunk`         | <p><code>id: string</code></p><p><code>data: dictionary</code></p>  | Fired when a new chunk is spawned in the server side game simulation                                                      |
| `remove_chunk`      | `id: string`                                                        | Fired when a chunk is removed from the server side game simulation                                                        |

***



### PPEntityNode

#### Variables

<table><thead><tr><th>Name</th><th valign="middle">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>entity_id</code></td><td valign="middle"><code>string</code></td><td>A unique identifier for the entity instance in the game world</td></tr><tr><td><code>type</code></td><td valign="middle"><code>string</code></td><td>The entity type. The value for this field is initially set based on the name of the root node of the current scene.</td></tr></tbody></table>

#### **Signals**

| Name            | Parameters                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `state_changed` | <p><code>state: dictionary({</code><br>    <code>x: number</code><br>    <code>y: number</code><br>    <code>z: number</code><br>    <code>data: dictionary</code></p><p>    <code>type: string</code><br><code>})</code></p> | <p>Fired each frame when the entity is still present in the simulation. Exposes the entity's coordinates, data table, and type. <br><br><strong>NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height, so this will need translating if your are running a 3D game</strong></p> |

***



### PPChunkNode

#### **Variables**

<table><thead><tr><th>Name</th><th valign="middle">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>chunk_id</code></td><td valign="middle"><code>string</code></td><td>A unique identifier for the chunk instance in the game world</td></tr><tr><td><code>type</code></td><td valign="middle"><code>string</code></td><td>This should be 'chunk'. The value for this field is initially set based on the name of the root node of the current scene.</td></tr></tbody></table>

***



### Custom Scripts

#### **Methods**

<table><thead><tr><th>Name</th><th valign="middle">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>server_to_client</code></td><td valign="middle"><code>Dictionary(String, Variant)</code></td><td>Receives manual messages from the server. Must be assigned to a Node in the PPRootNode inspector.</td></tr></tbody></table>

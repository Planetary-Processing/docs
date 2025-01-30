# Godot

## Introduction

Planetary processing is a platform for the creation and operation of multiplayer games; we provide a plugin for integrating our platform with the Godot Engine. The plugin provides nodes and signals to use within your game client code - these communicate changes in state from your server side simulation to the game client itself.

If you are new here, we recommend starting with the Godot Quickstart Guide.

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

* Create the _addons_ directory in your project if it does not exist
* Place the _planetary\_processing_ directory in the _addons_ directory
* Enable the Planetary Processing plugin via the Godot project settings

![Plugin Menu](https://planetaryprocessing.io/static/img/godot_plugin_menu.png)

* If not already setup on your project, you must trigger the creation of the C# solution via the Godot toolbar menu. This creates a _.csproj_ and a _.sln_ file in the root of your project.

![Csharp Menu](https://planetaryprocessing.io/static/img/godot_csharp1.png)

* Finally, we need to add a reference to the Planetary Processing C# SDK DLL to our _.csproj_ file. This can either be done manually, or via the button in the PPRootNode inspector. To add manually, please ensure the following reference is added to your ItemGroup in your _.csproj_ file

```xml
<Reference Include="Planetary">
  <HintPath>addons/planetary_processing/sdk/csharp-sdk.dll</HintPath>
</Reference>
```



## Custom Nodes

Once the Planetary Processing plugin is enabled, two custom nodes are made available to your project:

### PPRootNode

The Planetary Processing Root Node (PPRootNode) handles connections to your game backend, when running your game, while also enabling some additional functionality in the Godot editor.

A PPRootNode should be attached as a direct child of your main scene's root node. It is intended to be attached to your main scene so as to be accessible to all child scene instances, although this may vary dependent on your project structure. Due to the way the node handles connections to your game simulation, you should only have one PPRootNode in your project.&#x20;

![PPRootNode Scene Menu](https://planetaryprocessing.io/static/img/godot_root_scene_menu.png)

With the PPRootNode selected, you will be presented with the following in your inspector: ![PPRootNode Inspector Menu 1](<../.gitbook/assets/Capture d’écran 2024-09-25 à 12.58.14.png>)&#x20;

If you have already added the "Planetary" reference to your _csproj_ file, then the **Add Csproj Reference** button will not be visible. If it is present, you can click the button to have the plugin automatically add the reference to your _.csproj_ file.

To configure your game, you must fill in the fields in the inspector:

* Game ID - available in the [Planetary Processing Panel](https://panel.planetaryprocessing.io/)

#### **Variables**

The root node exposes the following useful variables

| name         | type           | description                                                                                                                 |
| ------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------- |
| player\_uuid | string or null | Will be null when the player is not authenticated. Once a player authenticates, this will be set to their unique identifier |

#### **Methods**

The root node exposes functions you can use to communicate with the server side game simulation.

| method               | parameters                         | return value                                                                         | description                                                                                                                            |
| -------------------- | ---------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| authenticate\_player | username: string, password: string | boolean: true if authenticated, will throw assertion error with error message if not | Authenticates a player with Planetary Processing. If the game is using anonymous auth, can be called with empty strings for arguments. |
| message              | msg: Dictionary(String, Variant)   | null                                                                                 | sends a message to the player entity on the server, which can then be processed by your player lua code                                |

#### **Signals**

The root node emits signals which you can use to manage the lifecycle of entities.

| name                | parameters                   | description                                                                                                               |
| ------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| new\_player\_entity | id: string, data: dictionary | fired when the player entity representing the player using this game client is spawned in the server side game simulation |
| new\_entity         | id: string, data: dictionary | fired when a new entity is spawned in the server side game simulation                                                     |
| remove\_entity      | id: string                   | fired when an entity is removed from the server side game simulation                                                      |
| new\_chunk          | id: string, data: dictionary | fired when a new chunk is spawned in the server side game simulation                                                      |
| remove\_chunk       | id: string                   | fired when a chunk is removed from the server side game simulation                                                        |



### PPEntityNode

The Planetary Processing Entity Node (PPEntityNode) describes a [Planetary Processing Entity](https://panel.planetaryprocessing.io/docs#server-side-api-documentation/types/entity). This node is designed to be attached directly to the root node of a scene in your Godot project, this scene can then be instantiated as a child of your main scene. For example, a tree scene will correlate with a _tree_ entity in your Planetary Processing game, and can have instances placed within your main scene.

With the PPEntityNode selected, you will be presented with the following in your inspector: ![PPEntityNode Inspector Menu 1](<../.gitbook/assets/Capture d’écran 2024-09-25 à 12.59.06.png>)

The type field correlate with the [server side Entity type](https://panel.planetaryprocessing.io/docs#server-side-api-documentation/types/entity).

[#variables](godot.md#variables "mention")

| name | description                                                                                                         |
| ---- | ------------------------------------------------------------------------------------------------------------------- |
| Data | A dictionary, representing the server-side data from the entity.                                                    |
| Type | The entity type. The value for this field is initially set based on the name of the root node of the current scene. |

#### **Signals**

The entity node emits signals which you can used to react to changes in the state of an entity.

| name        | parameters                                                                                            | description                                                                                                                                                                                                                                                                         |
| ----------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| new\_entity | <p>state: dictionary({<br>  x: number,<br>  y: number,<br>  z: number<br>  data: dictionary<br>})</p> | Fired each frame when the entity is still present in the simulation. Exposes the coordinates and data table of the entity. **NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height, so this will need translating if your are running a 3D game** |



## Usage

The Planetary Processing Plugin has an intended pattern of usage:

![Planetary Processing Usage Diagram](https://planetaryprocessing.io/static/img/godot_plugin_usage_diagram.png)

You may log a player in using the **authenticate\_player** method on the **PPRootNode**. After logging a player in, the custom nodes will begin emitting signals.

For the player who has just logged in, the **PPRootNode** will emit a **new\_player\_entity** signal. This signal is specific to the player running this instance of the game client. You can hook into this signal and instantiate your player scene accordingly. An example handler for this signal would be:

```python
# root.gd
var player_scene = preload("res://scenes/player.tscn") # load the player scene

func _on_new_player_entity(entity_id, state):
  var player_instance = player_scene.instantiate()
  player_instance.global_transform.origin = Vector3(state.x, state.z, state.y) # NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height
  var pp_entity_node = player_instance.get_node("PPEntityNode")
  if pp_entity_node:
    pp_entity_node.entity_id = entity_id
  else:
    print("PPEntityNode not found in the player instance")
  add_child(player_instance)
```

**Note in the above that a PPEntityNode requires its entity\_id to be set after instantiation, so that the node knows which signals to filter out and re-emit.**

The **PPRootNode** will emit a **new\_entity** signal for any entities that have not yet been seen by the game client. As an example, after logging in the PPRootNode will emit a new\_entity signal for other players in the game world, as well as tree entities. These scenes can then be instantiated and added to the tree, representing other players and trees.

```python
# root.gd
var player_scene = preload("res://data/player/other_player.tscn")
var tree_scene = preload("res://data/environment/tree_scene.tscn")

var scene_map = {
  "player": player_scene,
  "tree": tree_scene
}

func _on_new_entity(entity_id, state):
  var entity_scene = scene_map.get(state.type)
  if not entity_scene:
    print("matching scene not found: " + state.type)
  var entity_instance = entity_scene.instantiate()
  entity_instance.global_transform.origin = Vector3(state.x, state.z, state.y)
  var pp_entity_node = entity_instance.get_node("PPEntityNode")
  if pp_entity_node:
    pp_entity_node.entity_id = entity_id
  else:
    print("PPEntityNode not found in the instance")
  add_child(entity_instance)
```

If any entities have left the simulation, the **PPRootNode** will emit a **remove\_entity** signal. This signal can be used as a trigger to remove any scenes that are no longer needed from your tree. An example handler for this signal would be:

```python
# root.gd
func _on_remove_entity(entity_id):
  for child in get_children():
    var pp_entity_node = child.get_node("PPEntityNode")
    if pp_entity_node and pp_entity_node.entity_id == entity_id:
      remove_child(child)
      child.queue_free()
      print('Entity ' + entity_id + ' removed')
      return
  print('Entity ' + entity_id + ' not found to remove')
```

Each **PPEntityNode** will emit a **state\_changed** signal each frame, provided they are still part of the simulation. You can use these signals to trigger changes to your scenes as necessary, for example, moving player characters around the world. A simple example of how this might be handled in a script attached to the scene representing other players:

```python
# other_player.gd
extends CharacterBody3D

func _ready():
  var pp_entity_node = get_node("PPEntityNode")
  # connect to the state_changed signal from PPEntityNode
  pp_entity_node.state_changed.connect(_on_state_changed)

func _on_state_changed(state):
  global_transform.origin = Vector3(state.x, state.z, state.y) # NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height
```

You can send regular updates from the player to the simulation using the **message** function on the **PPRootNode**. These messages allow the player to interact with the simulation. It is up to you what data you want to send and how to process it in your server side lua code. Depending on your game, you might choose to send updates every frame, or on a fixed time delta, for example every 33ms.

An example sending the updated player position every frame

```python
# player.gd
var pp_root_node

func _ready() -> void:
  # ... your existing _ready code ...

  pp_root_node = get_tree().current_scene.get_node('PPRootNode')
  assert(pp_root_node, "PPRootNode not found")

func _process(delta: float) -> void:
  # ... your existing player movement code ...

  var current_position = global_transform.origin
  pp_root_node.message({
    "x": current_position[0],
    "y": current_position[1],
    "z": current_position[2],
    "threedee": true
  })
```

Your corresponding **player.lua** file may look like the following

```lua
--- player.lua
local function init(self)
end

local function update(self, dt)
end

local function message(self, msg)
  local x, y, z = msg.Data.x, msg.Data.y, msg.Data.z
  if msg.Client then
    if msg.Data.threedee then
      self:MoveTo(x,z,y) -- NOTE Planetary Processing uses 'y' for depth in 3 dimensional games, and 'z' for height
    else
      self:Move(x,y,0)
    end
  end
end

return {init=init,update=update,message=message}
```

**It is important to note that the coordinate systems for 3 dimensional Godot games uses Y for height and Z for depth. Planetary Processing uses Y for depth in 3 dimensional games, and Z for height. As a result, coordinate values for 3 dimensional games need translating as seen in the above examples.**

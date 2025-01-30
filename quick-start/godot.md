# Godot

Follow these steps to quickly set up and start using Planetary Processing with your Godot game. For more detailed information on both the Godot SDK and the server-side API, please visit our [documentation](https://docs.planetaryprocessing.io/).

## Godot Version

We recommend using Godot's most recent release version. If you are using an older version, we recommend a minimum of v4.2.1 for a successful integration.

You will also need the most recent compatible version of the .NET SDK.

* Godot Engine v4.2.1+ **.net version required**
* .NET SDK 6.0+ (Desktop target)
* .NET SDK 7.0+ (Android target)
* .NET SDK 8.0+ (iOS target)

## Create a Planetary Processing Game

1. Navigate to the [games section of our web panel](https://panel.planetaryprocessing.io/games)
2. Click **Create Game** in the top right
3. Provide the details of your game. Upon its creation, you will be taken to your Game Dashboard.
4. For this quick start, we will be using **Anonymous Auth**, which allows players to connect without a username and password. To enable this, navigate to the settings section of your Game Dashboard and enable Anonymous Auth as the Player Authentication Type within Game Settings.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Clone your Game Repository

1. Clone the [git](../server/git.md) repository as listed in your game dashboard - this is your Planetary Processing backend code.

```sh
git clone https://git.planetaryprocessing.io/git/aBcDE/my-planetary-processing-game.git
```

## Setting Up the Planetary Processing Plugin

1. Create a new Godot project, from the Project Manager.
2. In the FileSystem panel, create a new folder within the `res://` folder. Name it `addons`.
3. Right click the `addons` folder and select "Open in Terminal".
4. In the terminal, clone the Planetary Processing Godot SDK.

```sh
git clone https://github.com/planetary-processing/godot-plugin planetary_processing
```

5. Select the ‘Project Settings’ from the ‘Project’ tab in the topbar.&#x20;
6. Navigate to the 'Plugins' tab and enable the Planetary Processing plugin.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Setting Up the PP Root Node

1. Create a Node3D root node in the Scene panel, and save it as 'main'.
2. Add a PPRootNode as child node to your scene's root.
3. In the Inspector panel of the PPRootNode, enter the Game ID of your game. This is a number which can be found on your [game dashboard](https://panel.planetaryprocessing.io/games), next to your game’s name and repo link.

<figure><img src="../.gitbook/assets/image (15).png" alt="" width="341"><figcaption></figcaption></figure>

4. Select 'Tools > C# > Create C# Solution' from the ‘Project’ tab in the topbar.
5. This creates a _.csproj_ and a _.sln_ file in the root folder of your project. (Tabbing in and out of the Godot Editor will refresh the FileSystem Panel to show the new .csproj file).
6. In the Inspector panel of the PPRootNode, press the "Add Csproj Reference" button.

<figure><img src="../.gitbook/assets/image (32).png" alt="" width="315"><figcaption></figcaption></figure>

## Setting Up your Starting Scene

1. Select the ‘Project Settings’ from the ‘Project’ tab in the topbar.&#x20;
2. In the 'General' tab, find the 'Application > Run' section.
3. Set the Main Scene to be 'main.tscn'.

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="509"><figcaption></figcaption></figure>

## Creating a player character

1. Create a new scene to represent your player, and save it as 'player'.
2. Add the PPEntityNode as a direct child of the root node of your scene.
3. In the Inspector for the PPEntityNode, input `player` as the Type.
4. Add a MeshInstance to the root node of your scene.&#x20;
5. In the Inspector assign the Mesh parameter a 'New BoxMesh' to make it visible.
6. Add a Camera3D to the root node of your scene and position it to look at the player.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

## Creating entities

Every Entity has a ‘Type’. These Entity Types are defined in the backend code downloaded from your game repository.

1. Navigate back to your [cloned game repository](godot.md#clone-your-game-repository).
2. Locate the ‘entity’ folder.
3. Make note of the names of the .lua files inside the ‘entity’ folder. These are your Entity Types.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

For the demo game repository, the Entity Types: cat, tree, and player are used. Entities in Godot are formed from scenes containing the PPEntityNode and a valid Type parameter.&#x20;

4. Create a scene to represent one of your entities, such as a cat, and save it as 'cat'.
5. Add the PPEntityNode as a direct child of the root node of your scene.
6. In the Inspector for the PPEntityNode, input a Type. Entering ‘cat’ will sync this prefab with the ‘cat.lua’ entity in the server-side code.
7. Add a MeshInstance to the root node of your scene.&#x20;
8. In the Inspector assign the Mesh parameter a 'New SphereMesh', to make it visible.
9. Repeat the process for trees and give them a CylinderMesh.

<figure><img src="../.gitbook/assets/image (24).png" alt="" width="480"><figcaption></figcaption></figure>

## Creating other players

Other players are created in the same way as most entities, however they share their type 'player' with the player character.&#x20;

1. Create a scene to represent other players, and save it as 'other\_player'.
2. Add the PPEntityNode as a direct child of the root node of your scene.
3. In the Inspector for the PPEntityNode, input the Type 'player'.
4. Add a MeshInstance to the root node of your scene.&#x20;
5. In the Inspector assign the Mesh parameter a 'New CapsuleMesh', to make it visible.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## Moving entities

The PPRootNode emits a signal when entities change position or state on the server. These signals can be used to update an entity's position with new x, y, and z values.

1. Attach a new script called 'entity\_movement.gd' to the root node parameter (not PPEnityNode) of each of your entity scenes (except the player character scene), with the following code.

<pre class="language-gdscript"><code class="lang-gdscript"># entity_movement.gd script

# extend the functionality of your root node (here Node3D)
extends Node3D

# when the scene is loaded
<strong>func _ready():
</strong>    # connect to the state_changed signal from pp_entity_node
    var pp_entity_node= get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")

func _on_state_changed(state):
    # set the entity's position, using the server's values
    # NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
    # To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
    global_transform.origin = Vector3(state.x, state.z, -state.y) 
</code></pre>

<details>

<summary>entity_movement.gd script (no comments)</summary>

```gdscript
extends Node3D

func _ready():
    var ppEntityNode = get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")

func _on_state_changed(state):
    global_transform.origin = Vector3(state.x, state.z, -state.y) 
```

</details>

<details>

<summary>entity_movement.gd script (2D)</summary>

```gdscript
extends Node2D

func _ready():
    var pp_entity_node= get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")

func _on_state_changed(state):
    global_transform.origin = Vector2(state.x, -state.y) 

```

</details>

## Spawning entities

The PPRootNode emits a signal when new entities are added and removed on the server. These signals can be used to create or destroy entities in game.

1. Attach a new script called 'root.gd' to the root node parameter (not PPRootNode) of your main scene, with the following code.
2. Use this code to extend the root node's functionality and prepare the entity scenes for use in the main scene.

<pre class="language-gdscript"><code class="lang-gdscript"># root.gd script

# extend the functionality of your root node (here Node3D)
extends Node3D

<strong># create a variable to store the PPRootNode
</strong>var pp_root_node

# preload all the scenes for use by this script
var player_scene = preload("res://player.tscn")

var cat_scene = preload("res:///cat.tscn")
var tree_scene = preload("res://tree.tscn")
var other_player_scene = preload("res://other_player.tscn")

# define all the scenes by their entity type, except the player character
var scene_map = {
    "cat": cat_scene,
    "tree": tree_scene,
    "player": other_player_scene,
}
</code></pre>

3. Create a \_ready() function and start listening for trigger events sent from the server to the PPRootNode.

```gdscript
# when the scene is loaded
func _ready():
    # access the PPRootNode from the scene's node tree
    pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
    assert(pp_root_node, "PPRootNode not found") 

    # using signals from the PPRootNode,
    # trigger functions for entity spawning/despawning/positioning 
    pp_root_node.new_player_entity.connect(_on_new_player_entity)
    pp_root_node.new_entity.connect(_on_new_entity)
    pp_root_node.remove_entity.connect(_on_remove_entity)

# create a new player instance, and add it as a child node
func _on_new_player_entity(entity_id, state):
...

# create an entity instance matching its type, and add it as a child node
func _on_new_entity(entity_id, state):
...

# remove an entity instance, from the current child nodes
func _on_remove_entity(entity_id):
...
```

4. Create a function to handle the player character's spawning.

```gdscript
# create a new player instance, and add it as a child node
func _on_new_player_entity(entity_id, state):
    #check if the player instance already exists, and exit function if so  
    for instance in get_children():
        var pp_entity = instance.get_node_or_null('PPEntityNode')
        if pp_entity and "entity_id" in pp_entity:
            if pp_entity.entity_id == entity_id:
                return
                
    # create the player instance
    var player_instance = player_scene.instantiate()

    # validate that the player scene has a PPEntityNode
    var pp_entity_node = player_instance.get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.entity_id = entity_id
    else:
        print("PPEntityNode not found in the player instance")

    # add the player as a child of the root node
    add_child(player_instance)
    # position the player based on its server location
    # NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
    # To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
    player_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
```

5. Create a function to handle entity spawning.

```gdscript
# create an entity instance matching its type, and add it as a child node
func _on_new_entity(entity_id, state):
    # get the entity scene based on entity type
    var entity_scene = scene_map.get(state.type)
    # validate that the entity type has a matching scene
    if not entity_scene:
        print("matching scene not found: " + state.type)

    # create an entity instance
    var entity_instance = entity_scene.instantiate()

    # validate that the entity scene has a PPEntityNode
    var pp_entity_node = entity_instance.get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.entity_id = entity_id
    else:
        print("PPEntityNode not found in the instance")

    # add the entity as a child of the root node  
    add_child(entity_instance)
    # position the entity based on its server location
    # NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
    # To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
    entity_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
```

6. Create a function to handle entity removal.

<pre class="language-gdscript"><code class="lang-gdscript"><strong># remove an entity instance, from the current child nodes
</strong>func _on_remove_entity(entity_id):
    for child in get_children():
        # check if the child is an entity 
        var pp_entity_node = child.get_node_or_null("PPEntityNode")

        # check if it matches the entity_id to be removed
        if pp_entity_node and pp_entity_node.entity_id == entity_id:
            # delink the child from the parent
            remove_child(child)
            # remove the child from processing
            child.queue_free()
            print('Entity ' + entity_id + ' removed')
            return
      
    print('Entity ' + entity_id + ' not found to remove')
</code></pre>

## Creating Chunk Scenes

If you wish to store data in game world chunks, you can add chunk objects to your game as scenes. These chunks will be spawned into the world as invisible nodes in their proper location, and will load and unload as the player moves around the world.&#x20;

Chunk scenes are optional and you can create a game without them if you wish.

1. Create a scene and save it as 'chunk'.
2. Add the PPChunkNode as a direct child of the root node of your scene.
3. Attach a new script called 'chunk\_update.gd' to the root node parameter (not PPChunkNode) with the following code:

<pre class="language-gdscript"><code class="lang-gdscript"><strong># chunk_update.gd script
</strong><strong>
</strong><strong># extend the functionality of your root node (here Node3D)
</strong>extends Node3D

var pp_root_node
# when the scene is loaded
func _ready():
	# connect to the state_changed signal from pp_chunk_node
	var pp_chunk_node= get_node_or_null("PPChunkNode")
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	if pp_chunk_node:
		pp_chunk_node.state_changed.connect(_on_state_changed)
	else:
		print("PPChunkNode not found")

func _on_state_changed(state):
	pass
	# Add code to act if chunk data changes, if necessary

</code></pre>

<details>

<summary>chunk_update.gd script (no comments)</summary>

```gdscript
extends Node3D

var pp_root_node
func _ready():
	var pp_chunk_node= get_node_or_null("PPChunkNode")
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	if pp_chunk_node:
		pp_chunk_node.state_changed.connect(_on_state_changed)
	else:
		print("PPChunkNode not found")

func _on_state_changed(state):
	pass
```

</details>

## Spawning Chunks

The PPRootNode emits a signal when new chunks are added and removed on the server. These signals can be used to create or destroy chunks in game.

1. Go to your main scene and click on PPRootNode. Find ChunkSize in the inspector and set it to the same value as your game's chunk size (which can be found in the settings of your game on the Planetary Processing panel).

<figure><img src="../.gitbook/assets/image (30).png" alt="" width="315"><figcaption></figcaption></figure>

2. Go to 'root.gd', connected to the root node parameter (not PPRootNode) of your main scene, and update it with the following code. New lines are commented.
3. Use this code to extend the root node's functionality and prepare the chunk scenes for use in the main scene.

```gdscript
# preload all the scenes for use by this script
var player_scene = preload("res://player.tscn")

var cat_scene = preload("res:///cat.tscn")
var tree_scene = preload("res://tree.tscn")
var other_player_scene = preload("res://other_player.tscn")
### New Line ###
var chunk_scene = preload("res://chunk.tscn")

# define all the scenes by their entity type, except the player character
var scene_map = {
	"cat": cat_scene,
	"tree": tree_scene,
	"player": other_player_scene,
	### New Line ###
	"chunk": chunk_scene
}
```

4. Change the \_ready() function to include chunk events:

```gdscript
func _ready():
	# access the PPRootNode from the scene's node tree
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	assert(pp_root_node, "PPRootNode not found") 

	# using signals from the PPRootNode,
	# trigger functions for entity spawning/despawning/positioning 
	pp_root_node.new_player_entity.connect(_on_new_player_entity)
	pp_root_node.new_entity.connect(_on_new_entity)
	pp_root_node.remove_entity.connect(_on_remove_entity)
	### New Lines ###
	pp_root_node.new_chunk.connect(_on_new_chunk)
	pp_root_node.remove_chunk.connect(_on_remove_chunk)
	
	# authorise your connection to the game server 
	#(Anonymous Auth uses empty strings as parameters)
	pp_root_node.authenticate_player("", "")
	print("authenticated and starting")
```

5. Create a function to handle chunk spawning:

```gdscript
#create an chunk instance matching its type, and add it as a child node
func _on_new_chunk(chunk_id, state):
	if not chunk_scene:
		print("matching scene not found: chunk_scene" )

	# create an chunk instance
	var chunk_instance = chunk_scene.instantiate()

	# validate that the entity scene has a PPEntityNode
	var pp_chunk_node = chunk_instance.get_node_or_null("PPChunkNode")
	if pp_chunk_node:
		pp_chunk_node.chunk_id = chunk_id
	else:
		print("PPChunkNode not found in the instance")

	# add the chunk as a child of the root node  
	add_child(chunk_instance)
	# position the entity based on its server location
	# NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
	# To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
	chunk_instance.global_transform.origin = Vector3((state.x * pp_root_node.Chunk_Size), 0, -(state.y *  pp_root_node.Chunk_Size))
```

6. Create a function to handle chunk removal:

```gdscript
# remove an chunk instance, from the current child nodes
func _on_remove_chunk(chunk_id):
	for child in get_children():
		# check if the child is a chunk
		var pp_chunk_node = child.get_node_or_null("PPChunkNode")

		# check if it matches the chunk_id to be removed
		if pp_chunk_node and pp_chunk_node.chunk_id == chunk_id:
			# delink the child from the parent
			remove_child(child)
			# remove the child from processing
			child.queue_free()
			print('Chunk ' + chunk_id + ' removed')
			return
	  
	print('Chunk ' + chunk_id + ' not found to remove')
```

## Connecting the player

1. Add the following line of code to the end of your \_ready() function in 'root.gd'. This will allow the player to join and interact with the game world.

```gdscript
    # authorise your connection to the game server 
    #(Anonymous Auth uses empty strings as parameters)
    pp_root_node.authenticate_player("", "")
```

<details>

<summary>Full root.gd script code (commented)</summary>

```gdscript
# root.gd script

# extend the functionality of your root node (here Node3D)
extends Node3D

# create a variable to store the PPRootNode
var pp_root_node

# preload all the scenes for use by this script
var player_scene = preload("res://player.tscn")

var cat_scene = preload("res:///cat.tscn")
var tree_scene = preload("res://tree.tscn")
var other_player_scene = preload("res://other_player.tscn")
var chunk_scene = preload("res://chunk.tscn")

# define all the scenes by their entity type, except the player character
var scene_map = {
	"cat": cat_scene,
	"tree": tree_scene,
	"player": other_player_scene,
	"chunk": chunk_scene
}
# when the scene is loaded
func _ready():
	# access the PPRootNode from the scene's node tree
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	assert(pp_root_node, "PPRootNode not found") 

	# using signals from the PPRootNode,
	# trigger functions for entity spawning/despawning/positioning 
	pp_root_node.new_player_entity.connect(_on_new_player_entity)
	pp_root_node.new_entity.connect(_on_new_entity)
	pp_root_node.remove_entity.connect(_on_remove_entity)
	pp_root_node.new_chunk.connect(_on_new_chunk)
	pp_root_node.remove_chunk.connect(_on_remove_chunk)
	
	# authorise your connection to the game server 
	#(Anonymous Auth uses empty strings as parameters)
	pp_root_node.authenticate_player("", "")
	print("authenticated and starting")

# create a new player instance, and add it as a child node
func _on_new_player_entity(entity_id, state):
	print("MAKING PLAYER")
	# create the player instance
	var player_instance = player_scene.instantiate()

	# validate that the player scene has a PPEntityNode
	var pp_entity_node = player_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
		print("making new player entity")
	else:
		print("PPEntityNode not found in the player instance")

	# add the player as a child of the root node
	add_child(player_instance)
	# position the player based on its server location
	# NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
	# To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
	player_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
	
#create an entity instance matching its type, and add it as a child node
func _on_new_entity(entity_id, state):
	# get the entity scene based on entity type
	var entity_scene = scene_map.get(state.type)
	# validate that the entity type has a matching scene
	if not entity_scene:
		print("matching scene not found: " + state.type)

	# create an entity instance
	var entity_instance = entity_scene.instantiate()

	# validate that the entity scene has a PPEntityNode
	var pp_entity_node = entity_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
	else:
		print("PPEntityNode not found in the instance")

	# add the entity as a child of the root node  
	add_child(entity_instance)
	# position the entity based on its server location
	# NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
	# To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
	entity_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
	
# remove an entity instance, from the current child nodes
func _on_remove_entity(entity_id):
	for child in get_children():
		# check if the child is an entity 
		var pp_entity_node = child.get_node_or_null("PPEntityNode")

		# check if it matches the entity_id to be removed
		if pp_entity_node and pp_entity_node.entity_id == entity_id:
			# delink the child from the parent
			remove_child(child)
			# remove the child from processing
			child.queue_free()
			print('Entity ' + entity_id + ' removed')
			return
	  
	print('Entity ' + entity_id + ' not found to remove')
	
	
#create an chunk instance matching its type, and add it as a child node
func _on_new_chunk(chunk_id, state):
	if not chunk_scene:
		print("matching scene not found: chunk_scene" )

	# create an chunk instance
	var chunk_instance = chunk_scene.instantiate()

	# validate that the entity scene has a PPEntityNode
	var pp_chunk_node = chunk_instance.get_node_or_null("PPChunkNode")
	if pp_chunk_node:
		pp_chunk_node.chunk_id = chunk_id
	else:
		print("PPChunkNode not found in the instance")

	# add the chunk as a child of the root node  
	add_child(chunk_instance)
	# position the entity based on its server location
	# NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
	# To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
	chunk_instance.global_transform.origin = Vector3((state.x * pp_root_node.Chunk_Size), 0, -(state.y *  pp_root_node.Chunk_Size))
	
# remove an chunk instance, from the current child nodes
func _on_remove_chunk(chunk_id):
	for child in get_children():
		# check if the child is a chunk
		var pp_chunk_node = child.get_node_or_null("PPChunkNode")

		# check if it matches the chunk_id to be removed
		if pp_chunk_node and pp_chunk_node.chunk_id == chunk_id:
			# delink the child from the parent
			remove_child(child)
			# remove the child from processing
			child.queue_free()
			print('Chunk ' + chunk_id + ' removed')
			return
	  
	print('Chunk ' + chunk_id + ' not found to remove')
	

```

</details>

<details>

<summary>Full root.gd script code (no comments)</summary>

<pre class="language-gdscript"><code class="lang-gdscript">extends Node3D

var pp_root_node

var player_scene = preload("res://player.tscn")

var cat_scene = preload("res:///cat.tscn")
var tree_scene = preload("res://tree.tscn")
var other_player_scene = preload("res://other_player.tscn")
var chunk_scene = preload("res://chunk.tscn")

var scene_map = {
	"cat": cat_scene,
	"tree": tree_scene,
	"player": other_player_scene,
	"chunk": chunk_scene
}
<strong>
</strong>func _ready():
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	assert(pp_root_node, "PPRootNode not found") 
	
	pp_root_node.new_player_entity.connect(_on_new_player_entity)
	pp_root_node.new_entity.connect(_on_new_entity)
	pp_root_node.remove_entity.connect(_on_remove_entity)
	pp_root_node.new_chunk.connect(_on_new_chunk)
	pp_root_node.remove_chunk.connect(_on_remove_chunk)
	
	pp_root_node.authenticate_player("", "")
	print("authenticated and starting")

func _on_new_player_entity(entity_id, state):
	print("MAKING PLAYER")
	var player_instance = player_scene.instantiate()

	var pp_entity_node = player_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
		print("making new player entity")
	else:
		print("PPEntityNode not found in the player instance")
		
	add_child(player_instance)
	player_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
	
func _on_new_entity(entity_id, state):
	var entity_scene = scene_map.get(state.type)
	if not entity_scene:
		print("matching scene not found: " + state.type)

	var entity_instance = entity_scene.instantiate()

	var pp_entity_node = entity_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
	else:
		print("PPEntityNode not found in the instance")

	add_child(entity_instance)
	entity_instance.global_transform.origin = Vector3(state.x, state.z, -state.y)
	
func _on_remove_entity(entity_id):
	for child in get_children():
		var pp_entity_node = child.get_node_or_null("PPEntityNode")
		if pp_entity_node and pp_entity_node.entity_id == entity_id:
			remove_child(child)
			child.queue_free()
			print('Entity ' + entity_id + ' removed')
			return
	  
	print('Entity ' + entity_id + ' not found to remove')
	
func _on_new_chunk(chunk_id, state):
	if not chunk_scene:
		print("matching scene not found: chunk_scene" )
	var chunk_instance = chunk_scene.instantiate()
	
	var pp_chunk_node = chunk_instance.get_node_or_null("PPChunkNode")
	if pp_chunk_node:
		pp_chunk_node.chunk_id = chunk_id
	else:
		print("PPChunkNode not found in the instance")

	add_child(chunk_instance)
	chunk_instance.global_transform.origin = Vector3((state.x * pp_root_node.Chunk_Size), 0, -(state.y *  pp_root_node.Chunk_Size))
	
func _on_remove_chunk(chunk_id):
	for child in get_children():
		var pp_chunk_node = child.get_node_or_null("PPChunkNode")

		if pp_chunk_node and pp_chunk_node.chunk_id == chunk_id:
			remove_child(child)
			child.queue_free()
			print('Chunk ' + chunk_id + ' removed')
			return
	  
	print('Chunk ' + chunk_id + ' not found to remove')
	

</code></pre>

</details>

<details>

<summary>Full root.gd script code (2D)</summary>

```gdscript
extends Node2D

var pp_root_node

var player_scene = preload("res://player.tscn")

var cat_scene = preload("res:///cat.tscn")
var tree_scene = preload("res://tree.tscn")
var other_player_scene = preload("res://other_player.tscn")
var chunk_scene = preload("res://chunk.tscn")

var scene_map = {
	"cat": cat_scene,
	"tree": tree_scene,
	"player": other_player_scene,
	"chunk": chunk_scene
}

func _ready():
	pp_root_node = get_tree().current_scene.get_node_or_null('PPRootNode')
	assert(pp_root_node, "PPRootNode not found") 
	
	pp_root_node.new_player_entity.connect(_on_new_player_entity)
	pp_root_node.new_entity.connect(_on_new_entity)
	pp_root_node.remove_entity.connect(_on_remove_entity)
	pp_root_node.new_chunk.connect(_on_new_chunk)
	pp_root_node.remove_chunk.connect(_on_remove_chunk)
	
	pp_root_node.authenticate_player("", "")
	print("authenticated and starting")

func _on_new_player_entity(entity_id, state):
	print("MAKING PLAYER")
	var player_instance = player_scene.instantiate()

	var pp_entity_node = player_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
		print("making new player entity")
	else:
		print("PPEntityNode not found in the player instance")
		
	add_child(player_instance)
	player_instance.global_transform.origin = Vector2(state.x, -state.y)
	
func _on_new_entity(entity_id, state):
	var entity_scene = scene_map.get(state.type)
	if not entity_scene:
		print("matching scene not found: " + state.type)

	var entity_instance = entity_scene.instantiate()

	var pp_entity_node = entity_instance.get_node_or_null("PPEntityNode")
	if pp_entity_node:
		pp_entity_node.entity_id = entity_id
	else:
		print("PPEntityNode not found in the instance")

	add_child(entity_instance)
	entity_instance.global_transform.origin = Vector2(state.x, -state.y)
	
func _on_remove_entity(entity_id):
	for child in get_children():
		var pp_entity_node = child.get_node_or_null("PPEntityNode")
		if pp_entity_node and pp_entity_node.entity_id == entity_id:
			remove_child(child)
			child.queue_free()
			print('Entity ' + entity_id + ' removed')
			return
	  
	print('Entity ' + entity_id + ' not found to remove')
	
func _on_new_chunk(chunk_id, state):
	if not chunk_scene:
		print("matching scene not found: chunk_scene" )
	var chunk_instance = chunk_scene.instantiate()
	
	var pp_chunk_node = chunk_instance.get_node_or_null("PPChunkNode")
	if pp_chunk_node:
		pp_chunk_node.chunk_id = chunk_id
	else:
		print("PPChunkNode not found in the instance")

	add_child(chunk_instance)
	chunk_instance.global_transform.origin = Vector2((state.x * pp_root_node.Chunk_Size), -(state.y *  pp_root_node.Chunk_Size))
	
func _on_remove_chunk(chunk_id):
	for child in get_children():
		var pp_chunk_node = child.get_node_or_null("PPChunkNode")

		if pp_chunk_node and pp_chunk_node.chunk_id == chunk_id:
			remove_child(child)
			child.queue_free()
			print('Chunk ' + chunk_id + ' removed')
			return
	  
	print('Chunk ' + chunk_id + ' not found to remove')
	

```

</details>

## Moving the player

Player inputs need to be passed to the server.

1. Select the ‘Project Settings’ from the ‘Project’ tab in the topbar.&#x20;
2. Navigate to the 'Input Map' tab and add four new actions: 'move\_forward', 'move\_left', 'move\_backward', and 'move\_right'.&#x20;
3. Use the plus symbol to add an input event to each of them.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

3. Attach a new script called 'player.gd' to the root node parameter (not PPEntityNode) of your player scene, with the following code.

```gdscript
# player.gd script

# extend the functionality of your root node (Node3D if 3D)
extends Node3D

# create a variable to store the PPRootNode
var pp_root_node
# create a variable to handle movement speed
var speed = 5.0

# when the scene is loaded
func _ready() -> void:
    # access the PPRootNode from the scene's node tree 
    pp_root_node = get_tree().current_scene.get_node('PPRootNode')
    assert(pp_root_node, "PPRootNode not found") 
    
    # connect to the state_changed signal from pp_entity_node
    var pp_entity_node= get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")
        
func _on_state_changed(state):
    # sync the player's position, using the server's values
    # NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
    # To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
    var diff_in_position = (global_transform.origin - Vector3(state.x, state.z, -state.y)).abs() 
    if diff_in_position > Vector3(1,1,1):
        global_transform.origin = Vector3(state.x, state.z, -state.y)

func _process(delta: float) -> void:
    # get the raw input values
    var input_direction = Vector3(
        Input.get_action_strength("move_right") - Input.get_action_strength("move_left"),
        0,
        Input.get_action_strength("move_backward") - Input.get_action_strength("move_forward") 
    )
    # calculate the input direction
    input_direction = input_direction.normalized()

    # move the player
    var movement = input_direction * speed * delta
    translate(movement)

    # message the server to update the player's x and y positions
    # NOTE: Planetary Processing uses 'y' for depth in 3D games, and 'z' for height. The depth axis is also inverted.
    # To convert, set Godot's 'y' to negative, then swap 'y' and 'z'.
    pp_root_node.message({
        "x": movement[0],
        "y": -movement[2], 
        "z": 0,
    })
```

<details>

<summary>player.gd script (no comments)</summary>

```gdscript
extends Node3D

var pp_root_node
var speed = 5.0

func _ready() -> void: 
    pp_root_node = get_tree().current_scene.get_node('PPRootNode')
    assert(pp_root_node, "PPRootNode not found") 
    
    var pp_entity_node= get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")
        
func _on_state_changed(state):
    var diff_in_position = (global_transform.origin - Vector3(state.x, state.z, -state.y)).abs() 
    if diff_in_position > Vector3(1,1,1):
        global_transform.origin = Vector3(state.x, state.z, -state.y)

func _process(delta: float) -> void:
    var input_direction = Vector3(
        Input.get_action_strength("move_right") - Input.get_action_strength("move_left"),
        0,
        Input.get_action_strength("move_backward") - Input.get_action_strength("move_forward") 
    )
    input_direction = input_direction.normalized()

    var movement = input_direction * speed * delta
    translate(movement)

    pp_root_node.message({
        "x": movement[0],
        "y": -movement[2], 
        "z": 0,
    })
```

</details>

<details>

<summary>player.gd script (2D)</summary>

```gdscript
extends Node2D

var pp_root_node
var speed = 5.0

func _ready() -> void:
    pp_root_node = get_tree().current_scene.get_node('PPRootNode')
    assert(pp_root_node, "PPRootNode not found") 
    
    var pp_entity_node= get_node_or_null("PPEntityNode")
    if pp_entity_node:
        pp_entity_node.state_changed.connect(_on_state_changed)
    else:
        print("PPEntityNode not found")
        
func _on_state_changed(state):
    var diff_in_position = (global_transform.origin - Vector2(state.x, -state.y)).abs() 
    if diff_in_position > Vector2(1,1):
        global_transform.origin = Vector2(state.x, -state.y) 

func _process(delta: float) -> void:
    var input_direction = Vector2(
        Input.get_action_strength("move_right") - Input.get_action_strength("move_left"),
        Input.get_action_strength("move_backward") - Input.get_action_strength("move_forward") 
    )
    input_direction = input_direction.normalized()

    # move the player
    var movement = input_direction * speed * delta
    translate(movement)

    pp_root_node.message({
        "x": movement[0],
        "y": -movement[1], 
        "z": 0,
    })

```

</details>

## Test your connection

You now have everything you need to establish a basic connection between Godot and the server-side demo code.

1. On the Planetary Processing [games web panel](https://panel.planetaryprocessing.io/games), select your game to enter its dashboard.
2. Click 'Actions>Start Game' to start the server-side simulation.
3. Launch your game from Godot.

As your project runs, you should now be able to see the game world and all its entities. Your Planetary Processing game dashboard map should also show that a player has joined!

<figure><img src="../.gitbook/assets/image (17).png" alt="" width="563"><figcaption></figcaption></figure>

## Editing your backend code

[Using the repo we cloned earlier](godot.md#clone-your-game-repository) you can edit the behaviour of entities by changing their Lua file within the ‘entity’ directory.

You can also change how many and what entities are spawned in the init.lua file.

We recommend [experimenting ](../server/entities.md)here to get a sense of what you can do with Planetary Processing. When you add or change entities, make sure your server-side changes match up with your game engine client.

## Push your Planetary Processing backend code to the game repository

After configuring your game entities and logic, [push your changes](../server/git.md) to the game repository:

```sh
git add .
git commit -m "Configure game entities and logic for Planetary Processing"
git push
```

## Deploy Latest Version in the Web UI

1. Go back to your game dashboard in our web panel
2. From the actions menu in the top right, stop the game if it's running.
3. Select "Deploy Latest Version" - this will roll out your updated server-side code.

## Play and update your game

1. Start up your game again in Godot and in the web panel, to see the changes you have made!

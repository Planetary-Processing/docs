# Unity

Follow these steps to quickly set up and start using Planetary Processing with your Unity game. For more detailed information on both the SDK and the server-side API, please visit our [documentation](https://docs.planetaryprocessing.io/).

## Unity Version

We recommend using Unity’s most recent LTS release version. If you are using an older version, we recommend a minimum of 2022.3.23f1 for a successful integration.

## Create a Planetary Processing Game

1. Navigate to the [games section of our web panel](https://panel.planetaryprocessing.io/games)
2. Click **Create Game** in the top right
3. Provide the details of your game. Upon its creation, you will be taken to your Game Dashboard.
4. For this quick start, we will be using **Anonymous Auth**, which allows players to connect without a username and password. To enable this, navigate to the settings section of your Game Dashboard and enable Anonymous Auth as the Player Authentication Type within Game Settings.

![PP New Game](https://planetaryprocessing.io/static/img/pp_new_game_settings.png)

## Clone your Game Repository

1. Clone the [git](../server/git.md) repository as listed in your game dashboard - this is your Planetary Processing backend code.

```sh
git clone https://git.planetaryprocessing.io/git/aBcDE/my-planetary-processing-game.git
```

## Setting Up Your Unity Project

1. Create a new Unity project, from the Unity Hub.
2. Select the ‘Package Manager’ from the ‘Window’ tab in the topbar.
3. In the Package Manager, add the Planetary Processing package using the ‘Add package from git URL…’ option.
4. Input the following link, to install the Planetary Processing Unity SDK:

```sh
https://github.com/Planetary-Processing/unity-sdk.git
```

![Unity master](https://planetaryprocessing.io/static/img/unity_install.png) ![Unity master](https://planetaryprocessing.io/static/img/unity_git.png)

## Creating a server connection object

The Unity SDK provides two new components which can be added to GameObjects. These are PPMaster and PPEntity. These can be accessed in the 'Add Component' menu of any GameObject under the PP heading.

1. Create an empty GameObject. This will control which game server you connect to and sync the entities with the server.
2. Add the PPMaster component to this GameObject.

The PPMaster component will need to know three things: which object is the player, which prefabs represent the other players/entities, and the Game ID of the server.

![Unity Server Connection Object](https://planetaryprocessing.io/static/img/unity_qs_server_connection_object.png)

## Creating a player object:

1. Create a 3D GameObject to act as the player, such as a cube.
2. Add the PPEntity component to this GameObject.

The PPEntity component identifies entities in the game world. The main player character is the only entity which uses the PPEntity component but does not use a specific Entity Type.

![Unity Player Object](https://planetaryprocessing.io/static/img/unity_qs_player_object.png)

## Creating entities:

Entities populate your game world. Every Entity has a ‘Type’. These Entity Types are defined in the backend code downloaded from your game repository.

1. Navigate back to your cloned game repository.
2. Locate the ‘entity’ folder.
3. Make note of the names of the .lua files inside the ‘entity’ folder. These are your Entity Types.

![Lua Entity Files](https://planetaryprocessing.io/static/img/lua_entity_files.png)

For the demo game repository, the Entity Types: cat, tree, and player are used. The ‘player’ type is for representing other players in the game. It is also used for the main character's server backend, but remember the main character's PPEntity Type must remain empty.

Entities in Unity are formed from prefabs with the PPEntity component and a Type parameter. To make an Entity, download or create a prefab, then edit it to have the PPEntity component and a Type matching a Lua file.

To quickly make a prefab from scratch:

1. Create a 3D GameObject, such as a sphere.
2. Add the PPEntity component and input its Type. Entering ‘cat’ will sync this prefab with the ‘cat.lua’ entity in the server-side code.
3. Select this GameObject in the hierarchy window, then drag and drop it into the assets window to automatically convert it into a prefab.
4. Delete any instances of the prefab from your scene.

Create a prefab for every Lua file in your game repo’s entity folder.

![Cat Prefab Entity](https://planetaryprocessing.io/static/img/unity_qs_cat_entity.png)

## Configuring your server connection object

1. Return to the empty GameObject you created first, with the PP Master component, and start filling in its parameters.
2. Connect your player GameObject (eg. a cube) into the ‘Player’ input.
3. Connect each of your prefabs to separate elements in the ‘Prefabs’ input list.
4. Enter the Game ID of your game. This is a number which can be found on your [game dashboard](https://panel.planetaryprocessing.io/games), next to your game’s name and repo link.

![Unity Configured Server Connection Object](https://planetaryprocessing.io/static/img/unity_qs_config_server_connection_object.png)

## Connecting to your game server

Your server connection object now has all the info it needs to pass to the server. Now we just need a script to connect/login.

1. Add a script to your server connection object.
2. Use the example function below to establish a server connection on start.

```cs
void Start()
    {
        // get entity, player, and game info from the PPMaster component
        PPMaster master = GetComponent<PPMaster>(); 
        // authorise your connection to the game server (Anonymous Auth uses empty strings as parameters)
        master.Init("", "");   
        // spawn the player into the world
        master.Join(); 
    }
```

## Test your connection

You now have everything you need to establish a basic connection between Unity and the server-side demo code.

1. On the Planetary Processing [games web panel](https://panel.planetaryprocessing.io/games), select your game to enter its dashboard.
2. Click Actions>Start Game to start the server-side simulation.
3. Launch your game from Unity.

In your Unity preview window, you should now be able to see the game world and all its entities. Your Planetary Processing game dashboard map should also show that a player has joined!

![Unity Connected](https://planetaryprocessing.io/static/img/unity_qs_connected_to_game_server.png)

## Moving your player

Basic player movement can use the regular Unity input system. However, rather than change the position of the player GameObject, instead send a message to the server-side entity, to update its position.&#x20;

1. Make a movement script and add to it the player GameObject, with the code below as a guide.
2. Create a 'PPMasterTag' tag for the server connection object, for ease of reference.
3. Make sure the player object's PPEntity node has its 'Use server Position' value ticked (it is by default).&#x20;

```cs
using System.Collections;
using System.Collections.Generic;
using Planetary;
using UnityEngine;

public class Move : MonoBehaviour
{   
    // prepare variables for the PPMaster component and for movement speed
    private PPMaster PPMasterComponent;
    public float speed = 10.0f;

    void Start()
    {
        // get the PPMaster component from  the server connection object
        PPMasterComponent = GameObject.FindWithTag("PPMasterTag").GetComponent<PPMaster>(); 
    }

    void Update()
    {
        // get inputs
        float x = Input.GetAxis("Horizontal");
        float y = Input.GetAxis("Vertical");

        // tweak inputs to use speed
        x *= Time.deltaTime * speed;
        y *= Time.deltaTime * speed;

        // message the server to update the x and y positions
        PPMasterComponent.Message(new Dictionary<string, object>(){
            {"x", x},
            {"y", y}
        });
    }
}
```

## Editing your backend code

Using the [repo](unity.md#clone-your-game-repository) we cloned earlier you can edit the behaviour of entities by changing their Lua file within the ‘entity’ directory.

You can also change how many and what entities are spawned in the init.lua file.

We recommend [experimenting](../server/entities.md) here to get a sense of what you can do with Planetary Processing. When you add or change entities, make sure your server-side changes match up with your game engine client.

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

1. Start up your game again in Unity and in the web panel, to see the changes you have made!

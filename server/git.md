# Git Primer

We use a git repository to store and run your server-side code. So your backend code will be stored separately from the engine/client-side project.&#x20;

You can [clone](git.md#cloning), [edit](git.md#updating-your-game-repository), and [push](git.md#deploying) to your game like a regular git repository. If you are unfamiliar with git, use this page to learn. Or use the web editor in the [panel](https://panel.planetaryprocessing.io/games) to alter your server code directly.



## Accessing your Game Repository

Your game git repository will be created with a demo project, when you create a Planetary Processing game. To access this repo and start editing it, navigate to your [game’s dashboard](https://panel.planetaryprocessing.io/games) on the Planetary Processing website and find your repo link. It will something like this:

```sh
https://git.planetaryprocessing.io/git/aBcDE/my-planetary-processing-game.git
```

### Cloning

1. Using your computer's searchbar, search for 'terminal', or a similar app.
2. Open a terminal.
3. Use the command cd followed by the path to the directory where you want to store your backend code. Eg.

```sh
cd ‘C:\\Users\\JohnSmith\\’
```

3. Clone the git repository to that directory, by replacing the repo link below with your own.

```sh
git clone https://git.planetaryprocessing.io/git/aBcDE/my-planetary-processing-game.git
```

This will download the Lua files currently in your repo to your computer and give you version control over them.

4. Enter login credentials.

These will be the same email and password you use to login to the Planetary Processing website. For security, some terminals will not show the password at all, as you type it. If so, simply type the password as normal, then press enter.



## Updating your Game Repository

You can edit the behaviour of [entities](entities.md) by changing their Lua file within the `entity` directory.

You can also change how many and what entities are spawned in the [`init.lua`](chunks.md) file.

Start with something simple, like [creating](../api-reference/entity-api/create.md) an [entity](entities.md). Or if you have your game engine setup, try altering `entity/player.lua` to receive a [message](entities.md#entity-messaging) from your client, telling it how to move or act.&#x20;

### Tracking changes

The remote game repository cannot be viewed directly on development platforms like Github. Use either the git command line, your preferred IDE, or the web editor in the [panel](https://panel.planetaryprocessing.io/games) to keep track of edits.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to see the changed files and the changes themselves.

```sh
git status
git diff
```



## Deploying your Game Repository

After configuring your game [entities](entities.md) and logic, push your changes to the game repository.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to stage the files for upload, label the changes with a message of what was updated, and push the files to the online repo.

```sh
git add .
git commit -m "Configure game entities and logic for Planetary Processing"
git push
```

Your backend code will now be stored in your online repo, but will not be running live yet.

### Deploying

1. Go back to your game dashboard in our [web panel](https://panel.planetaryprocessing.io/games).
2. From the actions menu in the top right, select "Deploy Latest Version" - this will roll out your updated server-side code.

### Running

1. Click "Start Game" to begin running the server-side code from your git repository.



## Collaborating on your Game Repository

[Premium Only](../sdks/feature-comparison.md)

Make sure your local backend code stays up to date with your remote game repository, by regularly checking for changes by your team.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to get the changes and synchronise your local game repository.

```sh
git fetch
git pull
```


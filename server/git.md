# Git Primer

We use a git repository to store and run your server-side code. Your backend code is stored separately from the engine/client-side project.

## Locating your Game Repository

Your game git repository will be created with a demo project, when you create a Planetary Processing game. To access this repo and start editing it, navigate to your [game’s dashboard](https://panel.planetaryprocessing.io/games) on the Planetary Processing website and find your repo link. It will something like this:

```sh
https://git.planetaryprocessing.io/git/aBcDE/my-planetary-processing-game.git
```

## Clone your Game Repository

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

These will be the same email and password you use to login to the Planetary Processing website. For security, some terminals will not show the password at all as you type it. If so, simply type the password as normal, then press enter.

## Editing Your Backend Code

You can edit the behaviour of entities by changing their Lua file within the `entity` directory.

You can also change how many and what entities are spawned in the `init.lua` file.

As a suggestion, you could change `entity/player.lua` to receive a message from your client, to move or act. To learn more about messaging, check out our [Server Side Api documentation](https://docs.planetaryprocessing.io/server).

## Keep track of your changes

The remote game repository cannot be viewed directly on development platforms like Github. Use either the git command line or your preferred IDE to keep track of edits.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to see the changed files and the changes themselves.

```sh
git status
git diff
```

## Push Your Planetary Processing Backend Code to the Game Repository

After configuring your game entities and logic, push your changes to the game repository.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to stage the files for upload, label the changes with a message of what was updated, and push the files to the online repo.

```sh
git add .
git commit -m "Configure game entities and logic for Planetary Processing"
git push
```

Your backend code will now be stored in your online repo, but will not be running live yet.

## Deploy Latest Version in the Web UI

1. Go back to your game dashboard in our [web panel](https://panel.planetaryprocessing.io/games)
2. From the actions menu in the top right, select "Deploy Latest Version" - this will roll out your updated server-side code.

## Start Game in the Web UI

1. Click "Start Game" to begin running the server-side code from your git repository.

## Collaborating on the Game Repository

Make sure your local backend code stays up to date with your remote game repository, by regularly checking for changes by your team.

1. Open your terminal.
2. Make sure you are in your repo’s local directory, using the cd command to navigate to it.
3. Use the git commands below to get the changes and synchronise your local game repository.

```sh
git fetch
git pull
```

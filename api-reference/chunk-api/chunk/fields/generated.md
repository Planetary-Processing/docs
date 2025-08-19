# Generated

`chunk.Generated`

A boolean value, which dictates whether this chunk had been loaded before.&#x20;

If true, the chunk has been initialised previously in this game world.\
If false, the chunk has not yet been created.

This value is only accurate during chunk initialisation and should not be used outside of the `init` function of the `init.lua` file. Elsewhere it should be assumed `true`.

| Type   | Initialised Value  | Description                                                                                    |
| ------ | ------------------ | ---------------------------------------------------------------------------------------------- |
| `bool` | `false`            | Indicates whether or not this chunk has been loaded before. Read-only. Limited-scope (`init)`. |



Example:

When a chunk has not been generated before, create some trees, otherwise spawn a cat.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
<strong>    if not chunk.Generated then
</strong>    	for i=1,10 do
            local tree_x = chunk.X * chunk.Size + math.random() * chunk.Size
            local tree_y = chunk.Y * chunk.Size + math.random() * chunk.Size
            api.entity.Create("tree", tree_x, tree_y, 0, {})
        end
        print("A fresh chunk has been generated. Terrain features have been added.")
    else
        local cat_x = chunk.X * chunk.Size + math.random() * chunk.Size
        local cat_y = chunk.Y * chunk.Size + math.random() * chunk.Size
        api.entity.Create("cat", cat_x, cat_y, 0, {})
        print("An existing chunk has been loaded. Creatures have been spawned.")
    end
end

-- When a chunk is loaded for the first time, spawn 10 trees.
-- Create the trees in random positions within this chunk.
-- Whenever the chunk is loaded subsequently by a player, or a reload, spawn a cat.
-- Create the cat in a random position within this chunk.

-- Prints (on first load):
-- A fresh chunk has been generated. Terrain features have been added.

-- Prints (on subsequent loads):
-- An existing chunk has been loaded. Creatures have been spawned.
</code></pre>

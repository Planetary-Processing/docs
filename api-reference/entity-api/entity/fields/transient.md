# Transient

`entity.Transient`

A boolean value, which dictates whether this entity will be removed entirely when a chunk becomes unloaded.

If true, a transient entity will despawn whenever the chunk it is in is unloaded, and it will not persist when the chunk is loaded again.&#x20;

If false, the entity will stop processing when the chunk is unloaded and continue when the chunk is loaded again.

This field can be useful for cleaning up temporary entities like projectiles, or ensuring fresh entities are spawned each time a player loads the chunk.

| Type   | Initialised Value | Description     |
| ------ | ----------------- | --------------- |
| `bool` | `false`           | Default: False. |



Example:

Set this cat to be a transient, so that it respawns whenever its chunk is loaded by a player.

<pre class="language-lua"><code class="lang-lua">-- cat.lua
local function init(self)
<strong>    self.Transient = true
</strong>    print("Cat Boss will always be at full strength, every time you come here!")
end

-- init.lua
function init()
    if chunk.X == 3 and chunk.Y == 3 then
        local position_x = (chunk.X * chunk.Size) + chunk.Size/2
        local position_y = (chunk.Y * chunk.Size) + chunk.Size/2
    
        api.entity.Create("cat", position_x, position_y, 0, {
            name = "Cat Boss",
            health = 100,
            energy = 100,
            mana = 100,
        })
    end
end

-- When a player loads the chunk at (4,4), a Cat Boss will spawn.
-- If the player moves far enough away the chunk will unload despawning the Cat Boss.
-- When the player approaches the chunk again, a fresh Cat Boss will spawn, 
-- with full health, energy, and mana.

-- Prints:
-- Cat Boss will always be at full strength, every time you come here!
</code></pre>

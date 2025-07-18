# Data

`entity.Data`

A custom table which can be used to store additional data about a specific entity. The Data table is the primary method of customising an entity, by adding key-value pairs to the table.&#x20;

For instance, an entity might be assigned a 'health' key, with a value of 10.

`entity.Data.health = 10`

The keys must be strings, but the values can be of any type. The Data table cannot hold more than 4MB of data. It is empty by default, unless Username + Password Authentication is enabled.&#x20;

Player entities with a username will automatically have a 'username' key with that value. A Data table with a username will persist and be available whenever the player next reconnects (unless the game simulation is reset).

| Type    | Initialised Value             | Description                                                                                                                                                                   |
| ------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `table` | `{}`                          | Default: An empty table.                                                                                                                                                      |
|         | eg.`{username = "CatMan123"}` | <p>If using Username and Password Authentication, Player Entities have additional Data :<br>A table in which the key <code>username</code> matches the player's username.</p> |



Example:

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
<strong>    self.Data.health = 10
</strong><strong>    self.Data.inventory = {
</strong><strong>        wood = 5,
</strong><strong>        leaves = 8,
</strong><strong>        cat_claws = 0,
</strong><strong>        rocks = 11
</strong><strong>    }
</strong>end


local function message(self, msg)
    local healthiness = ""
    
    if self.Data.health > 5 then
        healthiness = "healthy"
    else
        healthiness = "unhealthy"
    end

    print("I am the player "..self.Data.username.." and I am "..healthiness..".")
    print("I have "..self.Data.inventory.wood.." wood.")
end


-- This player receives any message.
-- Print some information about their name, health, and inventory.

-- Prints (when using User + Password authentication):
-- I am the player CatMan123 and I am healthy.
-- I have 5 wood.
</code></pre>

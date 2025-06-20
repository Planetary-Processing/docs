# Data

`entity.Data`

A custom table which can be used to store additional data about a specific entity. The Data table is the primary method of customising an entity, by adding key-value pairs to the table. The keys must be strings.

For instance, an entity might be assigned a 'health' key, with a value of 10.

`entity.Data.health = 10`

The Data table cannot hold more than 4MB of data. It is empty by default, unless Username + Password Authentication is enabled. Player entities with a username will automatically have that value assigned to a 'username' key in the table. A Data table with a username will persist and be available whenever the player next reconnects.

| Type    | Initialised Value             | Description                                                                                                                                                                   |
| ------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `table` | `{}`                          | Default: An empty table.                                                                                                                                                      |
|         | eg.`{username = "CatMan123"}` | <p>If using Username and Password Authentication, Player Entities have additional Data :<br>A table in which the key <code>username</code> matches the player's username.</p> |



Example:

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
<strong>    self.Data.health = 10
</strong>    self.Data.inventory = {
        wood = 5,
        leaves = 8,
        cat_claws = 0,
        rocks = 11
    }
end


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

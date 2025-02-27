# Create



`api.entity.Create(type, x, y, z, data)`

Creates a new entity.&#x20;

This function will automatically call the `init` function for the specified entity type. It fails if there is no entity type that matches the provided `type`.



Parameters:

| Name | Type     | Description                                                                          |
| ---- | -------- | ------------------------------------------------------------------------------------ |
| type | `string` | Defines the type of entity to spawn.                                                 |
| x    | `float`  | The position to spawn the entity, in terms of the x axis.                            |
| y    | `float`  | The position to spawn the entity, in terms of the y axis.                            |
| z    | `float`  | The position to spawn the entity, in terms of the z axis.                            |
| data | `table`  | Custom lua table which can be used to store arbitrary data about the created entity. |

Returns:

| Type     | Description                                                     |
| -------- | --------------------------------------------------------------- |
| `Entity` | The entity which has been created, according to the parameters. |



Example:

Create a new cat entity with custom data, whenever a chunk is loaded or reloaded.

<pre class="language-lua"><code class="lang-lua">-- init.lua
local function init()
    local cat_x = chunk.X * chunk.Size
    local cat_y = chunk.Y * chunk.Size   
    
    local name_starts = {"Snow", "Fluff", "Fuzz"}
    local name_ends = {"ball", "ikins", "y"}

    local cat_name_start = name_starts[math.abs(chunk.X)%3 +1]
    local cat_name_end = name_ends[math.abs(chunk.Y)%3 +1]

    local cat_data = {
        name = cat_name_start..cat_name_end, 
        health = math.random(10)
    }
    
    local cat = api.entity.Create("cat", cat_x , cat_y, 1, cat_data)
       
    if cat.Data.health > 8 then
        print("My name is "..cat.Data.name.." and I am a strong cat!")
    end
end

-- Whenever a chunk is loaded, create a cat in the bottom left corner of the chunk.
-- The cat has two custom fields: 'name' and 'health'.
-- Its 'name' is generated based on this chunk's XY coordinates.
-- Its 'health' is a random value between 1 and 10.

-- Prints (subject to randomness):
-- My name is Snowball and I am a strong cat!
<strong>-- My name is Fluffy and I am a strong cat!
</strong>-- My name is Fuzzball and I am a strong cat!
-- My name is Fuzzy and I am a strong cat!
-- My name is Fluffikins and I am a strong cat!
</code></pre>


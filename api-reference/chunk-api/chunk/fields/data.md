# Data

`chunk.Data`

A custom table which can be used to store additional data about a specific chunk, such as a heightmap or other terrain information. The Data table is the primary method of customising a chunk, by adding key-value pairs to the table.&#x20;

For instance, a chunk might be assigned a 'biome' key, with a value of "desert".

`chunk.Data.biome = "desert"`&#x20;

The keys must be strings, but the values can be of any type. The Data table cannot hold more than 4MB of data. It is empty by default.

The Data table will persist if a chunk is reloaded or if the game is stopped, but not when the chunk is regenerated (when the game simulation is reset).



| Type    | Initialised Value | Description              |
| ------- | ----------------- | ------------------------ |
| `table` | `{}`              | Default: An empty table. |



Example:

Define some boundaries in chunk data, which prevent an entity from leaving the chunk.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    local threshold = 4    
    local world_position_x = chunk.X * chunk.Size
    local world_position_y = chunk.Y * chunk.Size
<strong>    chunk.Data.bounds = {
</strong><strong>	upper_x = world_position_x + chunk.Size - threshold, 
</strong><strong>	upper_y = world_position_y + chunk.Size - threshold, 
</strong><strong>	lower_x = world_position_x + threshold,
</strong><strong>	lower_y = world_position_y + threshold
</strong><strong>    }
</strong>end

-- entity.lua
local function SetDirection(self)
    x, y, z = self:GetPosition()
    local target_point = api.physics.NewVector(
        math.random(x-chunk.Size*2, x+chunk.Size*2),
        math.random(y-chunk.Size*2, y+chunk.Size*2),
        0
    )
    self.Data.direction = {
        x = target_point:Normalize().X,
        y = target_point:Normalize().Y
    }
end

local function MoveInDirection(self, dt)
    self:Move(
        self.Data.direction.x * dt, 
        self.Data.direction.y * dt, 
        0
    )
end

local function init(self)
    SetDirection(self)
end

local function update(self, dt)
    x, y, z = self:GetPosition()
    if x > chunk.Data.bounds.upper_x or y > chunk.Data.bounds.upper_y
    or x &#x3C; chunk.Data.bounds.lower_x or y &#x3C; chunk.Data.bounds.lower_y then
        
        self.Data.direction.x = self.Data.direction.x * -1
        self.Data.direction.y = self.Data.direction.y * -1
        MoveInDirection(self, dt)
        
        SetDirection(self)
        print("An entity is near the edge of its chunk. Trying a new direction now.")
    else
        MoveInDirection(self, dt)
    end
end

-- Set boundaries for the chunk and a threshold for how close an entity can approach.
-- Create a function to set the random direction an entity should move in.
-- Randomise a target point for the entity and calculate a normalised direction to it.
-- Create a function to move the entity in that direction.
-- Set the entity's initial direction.
-- If the entity gets too close to the edge of the chunk, reverse and change direction.
-- If the entity is not close to the edge, continue moving in the same direction.

-- Prints:
-- An entity is near the edge of its chunk. Trying a new direction now.
</code></pre>

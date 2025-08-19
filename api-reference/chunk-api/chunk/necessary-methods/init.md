# init

`function init()`

Performs the initial processing to set up a chunk instance, when it is created or loaded.

The function will trigger every time a chunk loads. This occurs for the 5x5 chunks around the origin when the game world is first generated, or when a nearby player approaches.

Initialisation may trigger more than once on a single chunk load. It is recommended that code within the function should be idempotent.



Example:

Initialise chunks in the default dimension and define a biome for them.

<pre class="language-lua"><code class="lang-lua">-- init.lua
local function GenerateDefaultChunk()   
    if chunk.Dimension == "" and not chunk.Generated then
        local tree_density = math.random() * 10
        
        if tree_density > 5 then
            chunk.Data.biome = "forest"   
        else
	    chunk.Data.biome = "urban"
	end
    
        for i = 1, tree_density do
            local tree_x = (chunk.X + math.random()) * chunk.Size
            local tree_y = (chunk.Y + math.random()) * chunk.Size
            api.entity.Create("tree", tree_x, tree_y, 0, {})
        end
    end
end

local function SpawnDefaultCats()
    if chunk.Dimension == "" then
        local cat_breeds = {
            forest = "bobcat", 
            urban = "tabby"
        }
        local cat_breed = cat_breeds[chunk.Data.biome]
    
        for i = 1,5 do
            local cat_x = (chunk.X + math.random()) * chunk.Size
            local cat_y = (chunk.Y + math.random()) * chunk.Size
            api.entity.Create("cat", cat_x, cat_y, 0, {breed = cat_breed})
        end
    end
end

<strong>function init()
</strong>    GenerateDefaultChunk()
    SpawnDefaultCats()
    print("Finished loading Chunk ("..chunk.X..","..chunk.Y.."). ".. 
            "It's biome is '"..chunk.Data.biome.."'.") 
end

-- Create a function defining how each chunk is generated in the default dimension.
-- Based on the quantity of trees, define the chunk's biome.
-- Spawn that many trees in random positions around the chunk.
-- Create a function to spawn cats whenever a chunk loads in the default dimension.
-- Use the chunk's biome to define the cats' breed.
-- Spawn five cats in random positions around the chunk.
-- Call both functions.

-- Prints:
-- Finished loading Chunk (2,-1).
-- Finished loading Chunk (-1,1).
-- Finished loading Chunk (0,-2).
-- Finished loading Chunk (1,1).
-- Finished loading Chunk (-2,-2).
-- ...
</code></pre>

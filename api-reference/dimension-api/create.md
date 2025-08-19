# Create



`api.dimension.Create(dimension)`

Creates a new dimension. When a dimension is first loaded, a small number of chunks around the origin will be loaded.&#x20;

The function will do nothing if there is already a dimension with the given ID. The default dimension already holds the ID of an empty string (`""`).



Parameters:

| Name      | Type     | Description                            |
| --------- | -------- | -------------------------------------- |
| dimension | `string` | The ID of the dimension to be created. |



Example:

Create a new dimension from the origin chunk of the default dimension.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
        if chunk.X == 0 and chunk.Y == 0 then
       
            if chunk.Dimension == "" then
<strong>                api.dimension.Create("Catland")
</strong>            else
                api.entity.Create("cat",0,0,0,{})
                print("This chunk is in "..chunk.Dimension.." and there is a cat!")
            end
            
        end
    end
end

-- When the chunk at the origin of the default dimension loads for the first time,
-- create a dimension called Catland.
-- When the chunk at the origin of Catland loads for the first time,
-- create a cat and print.

-- Prints:
-- This chunk is in Catland and there is a cat!
</code></pre>


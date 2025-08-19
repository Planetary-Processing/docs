# Dimension

`chunk.Dimension`

The unique identifier of the dimension this chunk is in.

This is the name of the [Dimension](../../../../server/dimensions.md) as specified upon its [creation](../../../dimension-api/create.md). Chunks within the default dimension use an empty string (`""`) as their ID.



| Type     | Initialised Value (default) | Description                                                              |
| -------- | --------------------------- | ------------------------------------------------------------------------ |
| `string` | `""`                        | <p>Default Dimension: <br>Empty String. Read-only.</p>                   |
|          | eg. `House Interior 4`      | <p>String name of the dimension containing the chunk. <br>Read-only.</p> |



Example:

If the chunk is at the origin of the default dimension, create a new dimension.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
        if chunk.X == 0 and chunk.Y == 0 then        
<strong>            if chunk.Dimension == "" then
</strong>                api.dimension.Create("Catland")
            else
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

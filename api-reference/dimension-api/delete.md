# Delete



`api.dimension.Delete(dimension)`

Deletes a dimension.

The default dimension cannot be deleted. It holds the ID of an empty string (`""`).



Parameters:

| Name       | Type     | Description                            |
| ---------- | -------- | -------------------------------------- |
| dimension  | `string` | The ID of the dimension to be deleted. |



Example:

Delete a dimension, from the origin chunk of that dimension.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
        if chunk.X == 0 and chunk.Y == 0 then
        
            if chunk.Dimension == "Catland" then
                print("This chunk is in "..chunk.Dimension.." which will be deleted.")
<strong>                api.dimension.Delete("Catland")
</strong>            else
                api.dimension.Create("Catland")
            end
            
        end
    end
end

-- When the chunk at the origin of a dimension loads for the first time,
-- create a dimension called Catland.
-- When the chunk at the origin of Catland loads for the first time,
-- print a message, then delete the dimension.

-- Prints:
-- This chunk is in Catland which will be deleted.
</code></pre>


# List



`api.dimension.List()`

Get all dimension IDs created for this game in a table.&#x20;

The list does not include the default dimension, which has the ID of an empty string (`""`).



Returns:

| Type       | Description                                                           |
| ---------- | --------------------------------------------------------------------- |
| `string[]` | An indexed string array of all dimension IDs which have been created. |



Example:

List five dimensions which have been created and delete them according to their name.

```lua
-- init.lua
function init()
    if not chunk.Generated then
        if chunk.X == 0 and chunk.Y == 0 then
            local dimension_descriptions = {"Fun", 
                                    "Sleepy", 
                                    "Fluffy", 
                                    "Curious", 
                                    "Cuddly"}
            local first_letter
            local first_letter_compare
            local dimension_list 
            local remove_list
        
            if chunk.Dimension == "" then                                  
                for index, description in pairs(dimension_descriptions) do 
                    api.dimension.Create(description.." Catland")
                end    
            else
                first_letter = string.sub(chunk.Dimension,1,1)
                dimension_list = api.dimension.List()
                remove_list = {}
                
                for index, dimension_id in pairs(dimension_list) do
                    first_letter_compare = string.sub(dimension_id, 1, 1)                	
                    if first_letter == first_letter_compare then
                        remove_list = api.table.Append(remove_list, dimension_id)
                    end
                end
                          
                for index, dimension_id in pairs(remove_list) do 
                    api.dimension.Delete(dimension_id)
                end               
            end         
            print("The remaining dimensions are", api.dimension.List())               
        end
        
    end
end

-- When the chunk at the origin of the default dimension loads for the first time,
-- create a five dimensions, each named with a description of Catland.
-- When the chunk at the origin of a created dimension loads for the first time,
-- delete this dimension and any others starting with the same letter.

-- Prints (order varies):
-- The remaining dimensions are [Fun Catland Sleepy Catland Fluffy Catland Curious Catland Cuddly Catland]
-- The remaining dimensions are [Fun Catland Sleepy Catland Fluffy Catland]
-- The remaining dimensions are [Sleepy Catland]
-- The remaining dimensions are []
```


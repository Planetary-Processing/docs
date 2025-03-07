# Append



`api.physics.Append(table, item)`

Appends an item to a table, without altering the original table. The modified table is returned by the function.



Parameters:

| Name  | Type    | Description                        |
| ----- | ------- | ---------------------------------- |
| table | `table` | The original table to be added to. |
| item  | `Any`   | The item to add to the table.      |

Returns:

| Type    | Descriptioon                                                               |
| ------- | -------------------------------------------------------------------------- |
| `table` | A new table, modified to include both the original table and the new item. |



Example:

Add a string to an array-style table of descriptive attributes, with a 50% chance of not adding it.

```lua
-- cat.lua
local function init(self)
    local attributes = {"strong", "fast", "smart"}
    local has_armour = math.random() > 0.5
    local description = "This cat is "
    
    if has_armour then
        attributes = api.table.Append(attributes, "armoured") 
    end
    
    for index, value in ipairs(attributes) do
	description = description..value

        if index < #attributes - 1 then
            description = description..", " 
        elseif index == #attributes - 1 then
            description = description..", and " 
        else 
	    description = description.."."
	end    
    end
    
    print(description)
end

-- When the cat entity loads, randomly decide whether it has armour.
-- If it does, add this to the cat's list of descriptive attributes.
-- Loop through the descriptive attributes to construct a sentence about the cat.

-- Prints
-- This cat is strong, fast, and smart.

-- Or Prints
-- This cat is strong, fast, smart, and armoured.
```

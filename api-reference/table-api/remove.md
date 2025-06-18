# Remove



`api.physics.Remove(table, index)`

Removes an item from a table at a specific index, without altering the original table. The modified table is returned by the function.



Parameters:

| Name  | Type    | Description                                         |
| ----- | ------- | --------------------------------------------------- |
| table | `table` | The original table for the item to be removed from. |
| index | `int`   | The index of the item to be removed from the table. |

Returns:

| Type    | Descriptioon                                                                 |
| ------- | ---------------------------------------------------------------------------- |
| `table` | A new table, modified from the original table to be without a specific item. |



Example:

Remove a string from an array-style table of descriptive attributes, with a 50% chance of not removing it.

<pre class="language-lua"><code class="lang-lua">-- cat.lua
local function init(self)
    local attributes = {"strong", "fast", "smart", "armoured"}
    local remove_armour = math.random() > 0.5
    local description = "This cat is "
    
    if remove_armour then
<strong>        attributes = api.table.Remove(attributes, #attributes) 
</strong>    end
    
    for index, value in ipairs(attributes) do
	description = description..value

        if index &#x3C; #attributes - 1 then
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
-- If it does not, remove it from the cat's list of descriptive attributes.
-- Loop through the descriptive attributes to construct a sentence about the cat.

-- Prints
-- This cat is strong, fast, smart, and armoured.

-- Or Prints
-- This cat is strong, fast, and smart.
</code></pre>

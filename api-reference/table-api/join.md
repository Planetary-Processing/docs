# Join



`api.physics.Join(table, table)`

Joins one table with another, without altering the original tables. The modified table is returned by the function.



Parameters:

| Name  | Type    | Description                         |
| ----- | ------- | ----------------------------------- |
| table | `table` | The original table to be joined to. |
| table | `table` | Another table to join to the first. |

Returns:

| Type    | Descriptioon                                                               |
| ------- | -------------------------------------------------------------------------- |
| `table` | A new table, modified from the original tables to contain items from both. |



Example:

Join a table to another array-style table of descriptive attributes, with a 50% chance of not joining them.

<pre class="language-lua"><code class="lang-lua">-- cat.lua
local function init(self)
    local attributes = {"strong", "fast", "smart"}
    local has_armour = math.random() > 0.5
    local armoured = {"armoured", "durable"}
    local description = "This cat is "

    if has_armour then
<strong>        attributes = api.table.Join(attributes, armoured) 
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
-- If it does, join the table of armour traits to the cat's list of attributes.
-- Loop through the descriptive attributes to construct a sentence about the cat.

-- Prints
-- This cat is strong, fast, and smart.

-- Or Prints
-- This cat is strong, fast, smart, armoured, and durable.
</code></pre>

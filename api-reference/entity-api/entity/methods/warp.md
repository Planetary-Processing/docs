# Warp



`entity:Warp(dimension, x, y, z)`

Moves an entity to a different [Dimension](../../../dimension-api/), at an absolute coordinate position relative to that dimension's origin (0,0,0).

If the chunk in the Dimension is loaded, this function will instantly move this entity from its current position, to the new XYZ position in that Dimension, defined by its parameters.

The coordinate fields are optional and default to (0,0,0) if not specified.&#x20;



Parameters:

| Name      | Type     | Description                                                                                         |
| --------- | -------- | --------------------------------------------------------------------------------------------------- |
| dimension | `string` | The name of the dimension to transfer this entity to.                                               |
| x         | `float`  | <p>The position the entity will teleport to, in terms of the x axis. <br>(Optional) Default: 0.</p> |
| y         | `float`  | <p>The position the entity will teleport to, in terms of the y axis. <br>(Optional) Default: 0.</p> |
| z         | `float`  | <p>The position the entity will teleport to, in terms of the z axis. <br>(Optional) Default: 0.</p> |



Example:

Move a cat entity to a new dimension.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
	if not chunk.Generated and chunk.X == 1 and chunk.Y == 1 then 
		if chunk.Dimension == "" then
			api.dimension.Create("Dimension Two")	
		end	
	end
	
	if not chunk.Generated and chunk.X == 1 and chunk.Y == 1 then
		if chunk.Dimension == "" then 
			api.entity.Create("cat", 0, 0, 0, {})
			api.entity.Create("tree", 0, 0, 0, {})
		end
	end
end

-- cat.lua
local function init() 
<strong>	self:Warp("Dimension Two", 0, 0, 0)
</strong>	print("This cat is now in "..api.dimension.List()[1]..".")
end

-- Create a second dimension when your world first loads.
-- Create a cat and a tree in your default dimension.
-- Transfer the cat to the second dimension, when it is initially created.
-- Print that the cat is in the first non-default dimension.

-- Prints:
-- This cat is now in Dimension Two.
</code></pre>



Known Bugs:

* Entities which are not actively moving during an update will delay the transfer to another dimension until an arbitrary Move() or MoveTo() command is executed. \
  Applying a Move(0,0,0) will initiate the warp without offsetting the entity's final position.

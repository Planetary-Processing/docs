# Dot

`vector:Dot(vector)`

Returns the dot product between two vectors.&#x20;

This function multiplies the individual X, Y, and Z values of this vector with the corresponding values of another vector, then sums the results into a single numerical value.&#x20;

The dot product is a float, which quantifies the degree of directional similarity between two vectors.

It can be used as part of common geometric calculations, such as the projection of one vector onto another or finding the angle between two vectors.



Parameters:

| Name   | Type            | Description                                            |
| ------ | --------------- | ------------------------------------------------------ |
| vector | [`Vector`](../) | The XYZ vector to be used in dot product calculations. |

Returns:

| Type    | Description                                                                                                                                                                                                                                            |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `float` | <p>The dot product of two other vectors.<br><br>Eg. The vector which called the function (a) multiplied with the vector in its parameter (b), then summed together:<br> <code>a.X*b.X</code> <br><code>+ a.Y*b.Y</code> <br><code>+ a.Z*b.Z</code></p> |



Example:

Use the dot product to calculate the angle between two vectors.

<pre class="language-lua"><code class="lang-lua">-- entity.lua
local function init(self)
    self.Physics = true

    local box_shape = api.physics.NewBoxShape(1, 1, 1) 
    local box = api.physics.NewBody(box_shape, 1)   
    self.Body = box
	
    self.Data.direction = { 
        {cardinal = "North", minimum = 315, maximum = 45},
        {cardinal = "East",  minimum = 45,  maximum = 135},
        {cardinal = "South", minimum = 135, maximum = 225},
        {cardinal = "West",  minimum = 225, maximum = 315}
    }
end

local function update(self, dt)	
    local north = api.physics.NewVector(0,1,0)
    local east = api.physics.NewVector(1,0,0)
    local west = api.physics.NewVector(-1,0,0)
    local vector = api.physics.NewVector(
        math.random()-0.5, 
        math.random()-0.5, 
        math.random()-0.5
    )
	
<strong>    local dot_product = north:Dot(vector)
</strong>    local combined_magnitudes = north:Magnitude() * vector:Magnitude()
    local radians = math.acos(dot_product / combined_magnitudes)
    local degrees = radians * 180 / math.pi 
    
<strong>    local is_reflex_angle = east:Dot(vector) &#x3C;= west:Dot(vector)
</strong>    if is_reflex_angle then
        degrees = 360 - degrees  
    end

    local cardinal = ""
    for index, direction in ipairs(self.Data.direction) do 
        if direction.cardinal == "North" then
            if direction.minimum &#x3C;= degrees or direction.maximum > degrees then
                cardinal = direction.cardinal
                break
            end
        else
            if direction.minimum &#x3C;= degrees and direction.maximum > degrees then
                cardinal = direction.cardinal
                break
            end
        end
    end
	
    local degrees_rounded = math.floor(degrees)
    print("This entity is moving "..degrees_rounded.." degrees from "..
	    "the north direction. Its orientation is "..cardinal..".")
    self.Body:ApplyForce(vector)
end

-- Turn on physics calculations for this entity.
-- Create a box shape and a body. Assign it to the entity.
-- Define ranges for all the cardinal directions.
-- Every update, define vectors for the north, east, and west directions.
-- Create a randomised vector for the direction of the entity's movement.
-- Use the dot product to calculate the angle between the random vector and north.
-- The angle will always be between 0 and 180 degrees.
-- Compare the similarity of the east and west directions with the vector.
-- If more westerly, calculate the clockwise reflex angle from the anticlockwise angle.
-- Check if the angle fits between a minimum and maximum for each cardinal range.
-- Adjust the check for the North range, since it wraps around from 315 to 45. 
-- Identify and print the cardinal direction of the entity's movement.

-- Prints:
-- This entity is moving 326 degrees from the north direction. Its orientation is North.
-- This entity is moving 253 degrees from the north direction. Its orientation is West.
-- This entity is moving 120 degrees from the north direction. Its orientation is East.
-- This entity is moving 219 degrees from the north direction. Its orientation is South.
-- This entity is moving 223 degrees from the north direction. Its orientation is South.
-- ...
</code></pre>


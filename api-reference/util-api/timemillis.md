# TimeMillis



`api.util.TimeMillis()`

Gives a Unix timestamp. This is the time in milliseconds which has elapsed since: `00:00:00 1st January 1970`.



Returns:

| Type  | Description                                              |
| ----- | -------------------------------------------------------- |
| `int` | The amount of time in milliseconds since the Unix epoch. |



Example:

Use the Unix timestamps to find the time taken between functions.

<pre class="language-lua"><code class="lang-lua">-- entity_type_name.lua
local function init(self)
    self.Data.previous_time = 0
end

local function update(self, dt)
	current_time = api.util.TimeMillis()
	
	if self.Data.previous_time == 0 then
		self.Data.previous_time = current_time 
	end
	
	millisecond_difference = current_time - self.Data.previous_time
	print("The time since the last update is "..millisecond_difference..
	      " milliseconds. "..
	      "The standard delta time is "..(dt*1000).." milliseconds.")
	self.Data.previous_time = current_time 
end

-- Get the time every update tick and compare it with the time at the previous tick.
-- This is approximately the same as the update functions delta time (dt) parameter.
-- The time between each update function is dependent on a game's tick rate.

-- Prints (each update, at target tick rate 60):
<strong>-- The time since the last update is 16 milliseconds. 
</strong><strong>-- The standard delta time is 16.719955 milliseconds.
</strong></code></pre>


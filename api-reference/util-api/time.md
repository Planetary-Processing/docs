# Time



`api.util.Time()`

Gives a Unix timestamp. This is the time in seconds which has elapsed since: `00:00:00 1st January 1970`.



Returns:

| Type  | Description                                         |
| ----- | --------------------------------------------------- |
| `int` | The amount of time in seconds since the Unix epoch. |



Example:

Compare the difference in Unix timestamps to make a timer.

<pre class="language-lua"><code class="lang-lua">-- entity_type_name.lua
local function init(self)
    self.Data.timer = 0
end

local function update(self, dt)
	if self.Data.timer ~= 0 then
<strong>		local current_time = api.util.Time()
</strong>        	if current_time - self.Data.timer_start > self.Data.timer-1 then 
        		print("A "..self.Data.timer.." second timer has finished. "..
            	              "At Unix timestamp "..current_time..".")
            		self.Data.timer = 0
        	end
    	end
end

local function message(self, msg)
	self.Data.timer = 5
	self.Data.timer_start = api.util.Time()
	print("A "..self.Data.timer.." second timer has started. "..
	      "At Unix timestamp "..self.Data.timer_start..".")
end

return {init=init, update=update, message=message}

-- Get the time when an entity receives a message.
-- Every update tick, check the current time.
-- If it has been 5 seconds since the start time, print a message.

-- Prints:
-- A 5 second timer has started. At Unix timestamp 1741190612.
-- A 5 second timer has finished. At Unix timestamp 1741190617.
</code></pre>


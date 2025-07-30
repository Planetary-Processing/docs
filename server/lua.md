# Lua Environment

Planetary Processing's server-side Lua environment runs using [LuaJIT](https://luajit.org/luajit.html), this means it is effectively the same as Lua 5.1 but with some tweaks and significantly improved performance.

Please note that some of Lua and LuaJIT's libraries are not available within the PP environment. The following libraries are not available: `os`, `io`, `jit`, `table`, `package` and `coroutine`.

The `math` library remains, as do various libraries added by PP. Additionally, `require`, `dofile` and similar will still work for loading Lua files from within your game repository.



## Additional Libraries

The following libraries are unique to the Planetary Processing server-side Lua environment.

### Tables

On our backend, we implement tables in a slightly different way to the normal Lua virtual machine.&#x20;

Array-style tables (e.g. `{1, 2, 3}`) are handled differently and as such we have provided a library for manipulating them that provides some of the functionality in Lua's `table` library.

The library works by taking an array-style table, manipulating it, and returning the result. This is distinct from Lua's built in tables, as the modifications are not made in place.

We provide three functions: `api.table.Append(table, item)`, `api.table.Remove(table, index)` and `api.table.Join(table, table)`.

```lua
x = {1, 2, 3}
print(x) -- Prints: [1, 2, 3]

x = api.table.Append(x, 4) -- append 4
print(x) -- Prints: [1, 2, 3, 4]

x = api.table.Remove(x, 1) -- remove the first item
print(x) -- Prints: [2, 3, 4]

x = api.table.Join(x, {5, 6, 7}) -- join the two tables
print(x) -- Prints: [2, 3, 4, 5, 6, 7]
```

Note that `Append` can be used with multiple item parameters like so:

```lua
x = {1, 2, 3}
print(x) -- Prints: [1, 2, 3, 4]

x = api.table.Append(x, 5, 6) -- add both 5 and 6 to the end of the table
print(x) -- Prints: [1, 2, 3, 4, 5, 6]
```

Note that modifying tables in-place by index is still supported as normal, and length operations will work as normal. However, you cannot index outside of the current length of the table.

```lua
x = {1, 2, 3}
print(x) -- Prints: [1, 2, 3]

x[1] = 0 -- modify the first element
print(x) -- Prints: [0, 2, 3]
print(#x) -- Prints the length of the table: 3

x[#x+1] = 42 -- this will fail, instead you should use api.table.Append
```



### Utils

We provide utilities through the `api.util` section of the API.&#x20;

The Unix timestamps used in time functions will provide the amount of time elapsed since: `00:00:00 1st January 1970`. Since this is a large integer, it will display in logs using scientific e notation. For example `1.741084186e+09` will be printed for a functional value of `1741083281` seconds.

```lua
api.util.Time() -- returns the unix timestamp in seconds
api.util.TimeMillis() -- returns the unix timestamp in milliseconds
```



### HTTP Requests

Premium Only

_<mark style="color:yellow;">**\[This feature is still in Open Beta and may have some bugs.**</mark>_ \
_<mark style="color:yellow;">**Please direct any feedback to the Planetary Processing Team on**</mark>_ [_<mark style="color:yellow;">**Discord**</mark>_](https://pp.vg/discord)_<mark style="color:yellow;">**]**</mark>_

We provide a simple HTTP API through the `api.http` section of the API. This API is for making HTTP Requests from within your game. For external requests to your game please use our [HTTP REST API](../http-api/authentication.md).&#x20;

These requests are rate limited to a maximum of 5 HTTP calls per update tick. The API is as follows:

```lua
api.http.Get(url, headers)
api.http.Post(url, headers, body)
api.http.Put(url, headers, body)
api.http.Delete(url, headers)
```

Where the url and body are strings and the headers are a table of the form:

```lua
{["header_name"]=value}
```

Every HTTP API endpoint returns the status code and the body of the response, as a string. Currently this process is sequential and blocking, so the code will not progress until there is a return.

An example of an HTTP Get request is below:

```lua
local code, body = api.http.Get("https://www.ismycomputeronfire.com/", {})
if code == 200 then
  print(body)
else
  print("something wrong, http code:", code)
end
```



### JSON

We have incorporated an open source JSON encoding/decoding library. It is imported automatically and can be accessed using the `json` object.&#x20;

Its documentation is here: [https://github.com/rxi/json.lua](https://github.com/rxi/json.lua)



## Environment Limitations

The following libraries are not available within the Planetary Processing environment: `os`, `io`, `jit`, `table`, `package` and `coroutine`. Additionally certain other functions are disabled.

### Tables

Functions, which use Lua's default `table` library such as `table.insert()`, will not work within the server-side environment. Instead, our [Table API](lua.md#table) should be used to edit array-style tables.

Tables in the standard dictionary style work similar to regular Lua. You may instantiate and edit them as normal. However, non-serialisable objects (i.e. functions, etc.) are not guaranteed to be persisted.&#x20;

```lua
local table_example = { 
    type = "cat", 
    health = 10 
}
table_example.health = 8
if table_example["health"] < 10 then
    table_example.damaged = true
end
print (table_example) -- Prints: map[damaged:true health:8 type:cat]
```



### Metatables and Metamethods

Runtime metatables and metamethods are currently not supported.&#x20;



### Error Handling

Most API functions will send error messages automatically if there is a problem. Additional error checking can be done using the `assert()` and `error()` functions, which will print the relevant stack traces to the [logs](logs.md).

Error handling using `pcall()` and `xpcall()` is not supported.



### Class and Instance Variables

<mark style="color:$primary;">This system may become deprecated in the future.</mark>

All top level variables in an entity file are accessible to all entities of that type, via entity methods, within a given chunk. As a result, entity methods which use top level variables will affect all entities of the same type within the chunk, when used within an entity's [`update()`](../api-reference/entity-api/entity/necessary-methods/update.md) function.

For creating instance variables, the entity's [Data](../api-reference/entity-api/entity/fields/data.md) table should be used. This table is isolated to only a single instance of the entity, via its `self` parameter. Alternatively, the top level variables can be used with entity methods within an instance scoped environment, such as the [`message()`](../api-reference/entity-api/entity/necessary-methods/message.md) function.

```lua
local top_level = {x = 1, y = 1, z = 1}

-- Move command affects all entities of the same type in the chunk
local function update(self, dt)
    self:Move(top_level.x, top_level.y, top_level.z)
end

-- Move command affects this entity instance
local function message(self, msg)
    self:Move(top_level.x, top_level.y, top_level.z)
end
```



### Common Functions to Avoid

These Lua functions will not work as intended in the Planetary Processing Environment. This list is not exhaustive but indicates common pitfalls to avoid in your server side code.

```lua
table.insert()
table.remove()
table.pack()
table.unpack()
table.concat()
table.sort()
table.move()

os.date()
os.time()
os.execute()
os.exit()

io.open()
io.close()
io.read()
io.write()

package.path()

coroutine.create()

setmetatable()

pcall()
xpcall()

```



## API

The global environment API is shown below, these functions are accessed by interfacing with the `api.table`, `api.util`, and `api.http` objects from any server-side script.

### Table

These functions are accessed by interfacing with the `api.table` object.

{% include "../.gitbook/includes/table-api-table.md" %}



***

### Util

These functions are accessed by interfacing with the `api.util` object.

{% include "../.gitbook/includes/util-api-table.md" %}



***

### HTTP Requests

These functions are accessed by interfacing with the `api.http` object.

{% include "../.gitbook/includes/http-request-api-table.md" %}



### JSON

These functions are accessed by interfacing with the `json` object (from [rxi's json.lua library](https://github.com/rxi/json.lua)).

| Method  | Parameters                                  | Return Value                          | Description                                                          |
| ------- | ------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------- |
| encode  | `nil`, `string`, `number`, or ~~`table`~~\* | `string` (JSON formatted)             | Converts data into a JSON formatted string.                          |
| decode  | `string` (JSON formatted)                   | `nil`, `string`, `number`, or `table` | Converts data from a JSON formatted string into an appropriate type. |

\* A bug currently prevents table to JSON encoding.

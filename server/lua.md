# Lua Environment

Planetary Processing's server-side Lua environment runs using [LuaJIT](https://luajit.org/luajit.html), this means it is effectively the same as Lua 5.1 but with some tweaks and significantly improved performance.

Please note that some of Lua and LuaJIT's libraries are not available within the PP environment. The following libaries are not available: `os`, `io`, `jit`, `table`, `package` and `coroutine`.

The `math` library remains, as do various libraries added by PP. Additionally, `require`, `dofile` and similar will still work for loading Lua files from within your game repository.

Runtime metatables and metamethods are not currently supported.

Additionally, we have incorporated an open source JSON encoding/decoding library, documentation here: [https://github.com/rxi/json.lua](https://github.com/rxi/json.lua)

## Tables

On our backend, we implement tables in a slightly different way to the normal Lua virtual machine. Tables used as a standard dictionary are similar to regular Lua, however, non-serialisable objects (i.e. functions, etc.) are not guaranteed to be persisted.

Array-style tables (e.g. `{1, 2, 3}`) are handled differently and as such we have provided a library for manipulating them that provides some of the functionality in Lua's `table` library.

The library works by taking an array-style table, manipulating it, and returning the result. This is distinct from Lua's built in tables, as the modifications are not made in place.

We provide three functions: `api.table.Append(table, item)`, `api.table.Remove(table, index)` and `api.table.Join(table, table)`.

```lua
x = {1, 2, 3}
print(x) -- prints [1, 2, 3]
x = api.table.Append(x, 4) -- append 4
print(x) -- prints [1, 2, 3, 4]
x = api.table.Remove(x, 1) -- remove the first item
print(x) -- prints [2, 3, 4]
x = api.table.Join(x, {5, 6, 7}) -- join the two tables
print(x) -- prints [2, 3, 4, 5, 6, 7]
```

Note that `Append` can be used with multiple item parameters like so:

```lua
x = {1, 2, 3}
print(x) -- prints [1, 2, 3, 4]
x = api.table.Append(x, 5, 6) -- add both 5 and 6 to the end of the table
print(x) -- prints [1, 2, 3, 4, 5, 6]
```

Note that modifying tables in-place by index is still supported as normal, and length operations will work as normal. However, you cannot index outside of the current length of the table.

```lua
x = {1, 2, 3}
print(x) -- prints [1, 2, 3]
x[1] = 0 -- modify the first element
print(x) -- prints [0, 2, 3]
print(#x) -- prints 3, the length of the table
x[#x+1] = 42 -- this will fail, instead you should use api.table.Append
```



## Utils

We provide utilities through the `api.util` section of the API.&#x20;

The Unix timestamps used in time functions will provide the amount of time elapsed since: `00:00:00 1st January 1970`. Since this is a large integer, it will display in logs using scientific e notation. For example `1.741084186e+09` will be printed for a functional value of `1741083281` seconds.

```lua
api.util.Time() -- returns the unix timestamp in seconds
api.util.TimeMillis() -- returns the unix timestamp in milliseconds
```



## HTTP Requests

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



## API

The global environment API is shown below, these functions are accessed by interfacing with the `api.table`, `api.util`, and `api.http`objects from any server-side script.

### Table

These functions are accessed by interfacing with the `api.table`object.

{% include "../.gitbook/includes/table-api-table.md" %}



***

### Utils

These functions are accessed by interfacing with the `api.util`object.

{% include "../.gitbook/includes/util-api-table.md" %}



***

### HTTP Requests

These functions are accessed by interfacing with the `api.http` object.

{% include "../.gitbook/includes/http-request-api-table.md" %}


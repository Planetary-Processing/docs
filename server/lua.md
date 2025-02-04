# Lua Environment

Planetary Processing's server-side Lua environment runs using [LuaJIT](https://luajit.org/luajit.html), this means it is effectively the same as Lua 5.1 but with some tweaks and significantly improved performance.

Please note that some of Lua and LuaJIT's libraries are not available within the PP environment. The following libaries are not available: `os`, `io`, `jit`, `table`, `package` and `coroutine`.

The `math` library remains, as do various libraries added by PP. Additionally, `require`, `dofile` and similar will still work for loading Lua files from within your game repository.

Please note that runtime metatables and metamethods are not currently supported.

## Tables

On our backend, we implement tables in a slightly different way to the normal Lua virtual machine. Tables used like a dictionary are roughly the same as in regular Lua, however, non-serialisable objects (i.e. functions, etc.) are not guaranteed to be persisted.

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

Note that `append` can be used with multiple item parameters like so:

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

**Methods**

| Method | Parameters                                                      | Return Value                          | Description                                             |
| ------ | --------------------------------------------------------------- | ------------------------------------- | ------------------------------------------------------- |
| Append | <p><code>table: table</code><br><code>item: Any</code></p>      | <p><code>table</code><br>modified</p> | Adds the item to the end of the table.                  |
| Remove | <p><code>table: table</code><br><code>index: integer</code></p> | <p><code>table</code><br>modified</p> | Removes an item from the table at the selected index.   |
| Join   | <p><code>table: table</code><br><code>table: table</code></p>   | <p><code>table</code><br>modified</p> | Combine one table with another, to form a single table. |

## Utils

We provide utilities through the `api.util` section of the API, currently this is limited to:

```lua
api.util.Time() -- returns the unix timestamp in seconds
```

## HTTP \[BETA]

We provide a simple HTTP API (for premium games only) through the `api.http` section of the API. This is rate limited to a maximum of 5 HTTP calls per tick. The API is as follows:

```lua
api.http.Get(url, headers)
api.http.Delete(url, headers)
api.http.Post(url, headers, body)
api.http.Put(url, headers, body)
```

Where the url and body are strings and the headers are a table of the form:

```lua
{["header_name"]=value}
```

Every HTTP API endpoint returns the status code and the body of the response (as a string).

An example of an HTTP Get request is below:

<pre class="language-lua"><code class="lang-lua">local code, body = api.http.Get("https://www.ismycomputeronfire.com/", {})
<strong>if code == 200 then
</strong><strong>  print(body)
</strong><strong>else
</strong><strong>  print("something wrong, http code:", code)
</strong><strong>end
</strong></code></pre>

# Put



`api.http.Put(url, headers, body)`

Sends an HTTP request to an endpoint. It uses the standard HTTP method PUT, to indicate that the request wishes to create or make an idempotent update to information.



Parameters:

| Name    | Type     | Description                                                                                  |
| ------- | -------- | -------------------------------------------------------------------------------------------- |
| url     | `string` | The address of the endpoint to send the request to.                                          |
| headers | `table`  | Provides additional information to the endpoint, on how it should respond to the request.    |
| body    | `string` | The message to be sent to the endpoint, with information to create or update its data store. |

Returns:

| Type     | Description                                                     |
| -------- | --------------------------------------------------------------- |
| `int`    | The HTTP code relaying the success of the request or otherwise. |
| `string` | The body of the HTTP response to the request.                   |



Example:

Send a PUT request to an endpoint, updating its data.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
<strong>        local code, body = api.http.POST("https://eg.mydatabase.com/chunks", 
</strong><strong>                            {
</strong><strong>                                ["Content-Type"]="text/plain",
</strong><strong>                                ["Authorisation"]="Basic YWxhZGRpbjp"},
</strong><strong>                            "cool!")
</strong>        
        if code == 200 then
          print("Response returned successfully: "..body)
          print("Chunks have been updated.")
        else
          print("Response returned HTTP code: "..code)
        end
    end
end

-- Send a PUT HTTP request to the url https://eg.mydatabase.com/chunks.
-- The endpoint url updates its data based on the request's body: "cool!"
-- The endpoint responds with some updated data about chunks.
-- The string body of the response is printed.

-- Prints (dependent on endpoint response):
-- Response returned successfully: Chunks were lame. Chunks are cool!
-- Chunks have been updated.

-- Or Prints (exact code number varies):
-- Response returned HTTP code: 404
</code></pre>


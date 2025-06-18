# Delete



`api.http.Delete(url, headers)`

Sends an HTTP request to an endpoint. It uses the standard HTTP method DELETE, to indicate that the request wishes to delete information.



Parameters:

| Name    | Type     | Description                                                                               |
| ------- | -------- | ----------------------------------------------------------------------------------------- |
| url     | `string` | The address of the endpoint to send the request to.                                       |
| headers | `table`  | Provides additional information to the endpoint, on how it should respond to the request. |

Returns:

| Type     | Description                                                     |
| -------- | --------------------------------------------------------------- |
| `int`    | The HTTP code relaying the success of the request or otherwise. |
| `string` | The body of the HTTP response to the request.                   |



Example:

Send a DELETE request to an endpoint, removing some data.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
<strong>        local code, body = api.http.DELETE("https://eg.mydatabase.com/chunks", 
</strong><strong>                            {
</strong><strong>                                ["Content-Type"]="text/plain",
</strong><strong>                                ["Authorisation"]="Basic YWxhZGRpbjp"})
</strong>        
        if code == 200 then
          print("Response returned successfully: "..body)
          print("Chunk data has been deleted.")
        else
          print("Response returned HTTP code: "..code)
        end
    end
end

-- Send a DELETE HTTP request to the url https://eg.mydatabase.com/chunks.
-- The endpoint url removes some existing data.
-- The endpoint responds with a message about the data removed.
-- The string body of the response is printed.

-- Prints (dependent on endpoint response):
-- Response returned successfully: Chunks were lame. Chunks are
-- Chunk data has been deleted.

-- Or Prints (exact code number varies):
-- Response returned HTTP code: 404
</code></pre>


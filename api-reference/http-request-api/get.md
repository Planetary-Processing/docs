# Get



`api.http.Get(url, headers)`

Sends an HTTP request to an endpoint. It uses the standard HTTP method GET, to indicate that the request wishes to retrieve information.



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

Send a GET request to an endpoint, requesting data.

<pre class="language-lua"><code class="lang-lua">-- init.lua
function init()
    if not chunk.Generated then
<strong>        local code, body = api.http.Get("https://eg.mydatabase.com/chunks", 
</strong><strong>                            {["Content-Type"]="text/plain",
</strong><strong>                            ["Authorisation"]="Basic YWxhZGRpbjp"})
</strong>        
        if code == 200 then
          print("Response returned successfully: "..body)
        else
          print("Response returned HTTP code: "..code)
        end
    end
end

-- Send a GET HTTP request to the url https://eg.mydatabase.com/chunks.
-- The endpoint url responds with some data about chunks.
-- The string body of the response is printed.

-- Prints (dependent on endpoint response):
-- Response returned successfully: Chunks are cool!

-- Or Prints (exact code number varies):
-- Response returned HTTP code: 404
</code></pre>


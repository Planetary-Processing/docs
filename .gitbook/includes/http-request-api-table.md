---
title: HTTP Request API Table
---

| Method                                                   | Parameters                                                                                     | Return Value                                                 | Description                                 |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| [Get](../../api-reference/http-request-api/get.md)       | <p><code>url: string</code><br><code>headers: table</code></p>                                 | <p><code>code: int</code> <br> <code>body: string</code></p> | Sends a GET request to an HTTP endpoint.    |
| [Post](../../api-reference/http-request-api/post.md)     | <p><code>url: string</code><br><code>headers: table</code></p><p><code>body: string</code></p> | <p><code>code: int</code> <br> <code>body: string</code></p> | Sends a POST request to an HTTP endpoint.   |
| [Put](../../api-reference/http-request-api/put.md)       | <p><code>url: string</code><br><code>headers: table</code><br> <code>body: string</code></p>   | <p><code>code: int</code> <br> <code>body: string</code></p> | Sends a PUT request to an HTTP endpoint.    |
| [Delete](../../api-reference/http-request-api/delete.md) | <p><code>url: string</code><br><code>headers: table</code></p>                                 | <p><code>code: int</code> <br> <code>body: string</code></p> | Sends a DELETE request to an HTTP endpoint. |

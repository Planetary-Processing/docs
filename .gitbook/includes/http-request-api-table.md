---
title: HTTP Request API Table
---

| Method | Parameters                                                                                  | Return Value                                               | Description                                 |
| ------ | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------- |
| Get    | <p><code>url: string</code><br><code>headers: table</code></p>                              | <p><code>code: int</code><br><code>body: string</code></p> | Sends a GET request to an HTTP endpoint.    |
| Post   | <p><code>url: string</code><br><code>headers: table</code><br><code>body: string</code></p> | <p><code>code: int</code><br><code>body: string</code></p> | Sends a POST request to an HTTP endpoint.   |
| Put    | <p><code>url: string</code><br><code>headers: table</code><br><code>body: string</code></p> | <p><code>code: int</code><br><code>body: string</code></p> | Sends a PUT request to an HTTP endpoint.    |
| Delete | <p><code>url: string</code><br><code>headers: table</code></p>                              | <p><code>code: int</code><br><code>body: string</code></p> | Sends a DELETE request to an HTTP endpoint. |

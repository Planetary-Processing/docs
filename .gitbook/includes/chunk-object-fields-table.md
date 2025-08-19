---
title: Chunk Object Fields Table
---

| Field                                                                | Type     | Description                                                             |
| -------------------------------------------------------------------- | -------- | ----------------------------------------------------------------------- |
| [ID](../../api-reference/chunk-api/chunk/fields/id.md)               | `int`    | ID of the chunk                                                         |
| [X](../../api-reference/chunk-api/chunk/fields/x.md)                 | `int`    | X coordinate in chunk space.                                            |
| [Y](../../api-reference/chunk-api/chunk/fields/y.md)                 | `int`    | Y coordinate in chunk space.                                            |
| [Size](../../api-reference/chunk-api/chunk/fields/size.md)           | `int`    | Size of the chunk in world units.                                       |
| [Generated](../../api-reference/chunk-api/chunk/fields/generated.md) | `bool`   | True, if this chunk has been loaded before.                             |
| [Dimension](../../api-reference/chunk-api/chunk/fields/dimension.md) | `string` | ID of the current dimension.                                            |
| [Data](../../api-reference/chunk-api/chunk/fields/data.md)           | `table`  | A custom Lua table which can be used to store any data about the chunk. |

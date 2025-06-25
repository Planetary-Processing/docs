---
title: Chunk Object Fields Table
---

| Field     | Type     | Description                                                                                                                       |
| --------- | -------- | --------------------------------------------------------------------------------------------------------------------------------- |
| ID        | `int`    | ID of the chunk                                                                                                                   |
| X         | `int`    | X coordinate in chunk space.                                                                                                      |
| Y         | `int`    | Y coordinate in chunk space.                                                                                                      |
| Size      | `int`    | Size of the chunk in world units.                                                                                                 |
| Generated | `bool`   | Indicates whether or not this chunk has been loaded before. This should not be used outside of the `init` function.               |
| Dimension | `string` | ID of the current dimension.                                                                                                      |
| Data      | `table`  | Custom lua table which can be used to store arbitrary data about the chunk, such as heightmap or other terrain info, for example. |

---
title: Entity Object Fields Table
---

| Field                                                                                                                                                         | Type     | Description                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------- |
| [ID](../../api-reference/entity-api/entity/fields/id.md)                                                                                                      | `string` | UUID of the entity. Read-only.                                                                |
| [Type](../../api-reference/entity-api/entity/fields/type.md)                                                                                                  | `string` | Type of the entity. Read-only.                                                                |
| [Data](../../api-reference/entity-api/entity/fields/data.md)                                                                                                  | `table`  | A custom Lua table which can be used to store any data about the entity.                      |
| <p><a href="../../api-reference/entity-api/entity/fields/chunkloader.md">Chunkloader</a><br>(<a href="../../sdks/feature-comparison.md">Premium Only</a>)</p> | `bool`   | If true, the chunk containing this entity will remain loaded even if no players are present.  |
| [Transient](../../api-reference/entity-api/entity/fields/transient.md)                                                                                        | `bool`   | If true, when the chunk containing this entity is unloaded, the entity will be removed.       |

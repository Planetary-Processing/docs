---
title: Entity Object Fields Table
---

| Field                                                                                                          | Type     | Description                                                                                   |
| -------------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------- |
| [ID](../../api-reference/entity-api/entity/fields/id.md)                                                       | `string` | UUID of the entity. Read-only.                                                                |
| [Type](../../api-reference/entity-api/entity/fields/type.md)                                                   | `string` | Type of the entity. Read-only.                                                                |
| [Data](../../api-reference/entity-api/entity/fields/data.md)                                                   | `table`  | Custom lua table which can be used to store any data about the entity.                        |
| <p><a href="../../api-reference/entity-api/entity/fields/chunkloader.md">Chunkloader</a><br>(Premium Only)</p> | `bool`   | If true, the chunk containing this entity will remain loaded even if no players are present.  |
| [Transient](../../api-reference/entity-api/entity/fields/transient.md)                                         | `bool`   | If true, when the chunk containing this entity is unloaded, it will not be persisted.         |

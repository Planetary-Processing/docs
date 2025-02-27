---
title: Entity Fields Table
---



| Field       | Type   | Description                                                                                                                                        |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID          | string | UUID of the entity. Read-only.                                                                                                                     |
| Type        | string | Type of the entity. Read-only.                                                                                                                     |
| Data        | table  | Custom lua table which can be used to store arbitrary data about the entity.                                                                       |
| Chunkloader | bool   | If true, the chunk containing this entity will remain loaded even if no players are present. Note that chunkloader entities are a premium feature. |
| Transient   | bool   | If true, when the chunk containing this entity is unloaded, it will not be persisted.                                                              |


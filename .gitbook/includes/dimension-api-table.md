---
title: Dimension API Table
---

| Method                                                | Parameters          | Returns    | Description                                                    |
| ----------------------------------------------------- | ------------------- | ---------- | -------------------------------------------------------------- |
| [Create](../../api-reference/dimension-api/create.md) | `dimension: string` | None       | Creates a new dimension; idempotent.                           |
| [Delete](../../api-reference/dimension-api/delete.md) | `dimension: string` | None       | Delete a dimension by ID. Cannot delete the default dimension. |
| [List](../../api-reference/dimension-api/list.md)     | None                | `string[]` | Get all dimension IDs for this game in a table.                |

---
title: Dimensions API Table
---

| Method | Parameters          | Return Value | Description                                                    |
| ------ | ------------------- | ------------ | -------------------------------------------------------------- |
| Create | `dimension: string` | None         | Creates a new dimension; idempotent.                           |
| Delete | `dimension: string` | None         | Delete a dimension by ID. Cannot delete the default dimension. |
| List   | None                | `string[]`   | Get all dimension IDs for this game in a table.                |

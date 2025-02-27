---
title: Entity API Table
---

| Method  | Parameters                                                                                                                              | Return Value | Description                                                    |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------------------------------------------------------- |
| Create  | <p><code>type: string</code><br><code>x: float</code><br><code>y: float</code><br><code>z: float</code><br><code>data: table</code></p> | `Entity`     | Creates a new entity in the current dimension and returns it.  |
| Message | <p><code>entityid: string</code><br><code>message: table</code></p>                                                                     | None         | Sends a message to the entity with UUID `entityid.`            |

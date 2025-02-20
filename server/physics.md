---
description: Open Beta
---

# Physics

_<mark style="color:yellow;">**\[This feature is still in Open Beta and may have some bugs.**</mark>_ \
_<mark style="color:yellow;">**Please direct any feedback to the Planetary Processing Team on**</mark>_ [_<mark style="color:yellow;">**Discord**</mark>_](https://pp.vg/discord)_<mark style="color:yellow;">**]**</mark>_

Physics is an optional server-side setting that can be enabled for entities. When physics is applied to an entity, it is assigned a mass, a body (defining its shape), and a velocity. Forces can be applied to modify an entity’s velocity, and collisions between entities are simulated server-side.

## Using Physics

Physics must be enabled on an entity for it to have a [velocity](physics.md#body-1) and interact in [collisions](physics.md#collisions). An entity with physics enabled will not collide with another entity that does not have Physics enabled.

1. #### Enable Physics:

```lua
self.Physics = true
```

2. #### Assign a [Body](physics.md#body-1) & [Mass](physics.md#body-1):

The body can be either a sphere or a box.&#x20;

```lua
local box = api.physics.NewBoxShape(1, 1, 1) -- width=1, length=1, depth=1
self.Body = api.physics.NewBody(box, 10) --shape=box, mass=10
```

3. #### Set [Velocity ](physics.md#body-1)or apply [Forces](physics.md#force):

<pre class="language-lua"><code class="lang-lua"><strong>--1: Set velocity
</strong><strong>self.Body.Velocity = api.physics.NewVector(0, -20, 0)
</strong>--2: Apply a one-time force 
local vector = api.physics.NewVector(0, .2, 0)
self.Body:ApplyForce(vector)
</code></pre>

#### Force

The function [ApplyForce()](physics.md#body-1) applies a force to an entity once during a physics update and is then reset immediately afterward. To apply a continuous [force](physics.md#body-1), you must reapply it each update.

Multiple [forces](physics.md#body-1) can be applied within a single update cycle. These [forces](physics.md#body-1) are combined and applied together in the next physics update.

```lua
self.Body:ApplyForce(vector)
```



## Physics Objects

### Shape

The Planetary Processing physics engine currently supports two kinds of 3D [shapes](physics.md#shape-1): a box or a sphere. [Shapes](physics.md#shape-1) are used to define the [collision](physics.md#collisions) boundaries for an entity's [Body](physics.md#body).

<figure><img src="../.gitbook/assets/Shape.png" alt="" width="188"><figcaption></figcaption></figure>

### Body

The [Body](physics.md#body-1) represents an entity for the game's server-side physics. It defines the entity’s [shape](physics.md#shape-1) and mass. These attributes determine how the entity interacts with [forces](physics.md#force), [collisions](physics.md#collisions), and other physics-based calculations.

<figure><img src="../.gitbook/assets/Body.png" alt="" width="188"><figcaption></figcaption></figure>

### Vector

For physics interactions in 3D space, motion occurs across the X, Y, and Z axes. So it helps to group XYZ values into [vectors](physics.md#vector-1) to represent a single direction, for fields like velocities and [forces](physics.md#force).

<figure><img src="../.gitbook/assets/Vector.png" alt="" width="375"><figcaption></figcaption></figure>

## Collisions

When two entities with [Physics](physics.md#using-physics) enabled and a [Body](physics.md#body-1) intersect, they collide. The physics engine calculates a **3D elastic collision**, determining the resulting motion [vectors](physics.md#vector-1).

* **Collisions are Elastic:** No energy is lost during collisions.
* **Collision Detection:** Collisions are only detected if entities are overlapping during an update.

<figure><img src="../.gitbook/assets/ElasticCollisions.gif" alt="" width="375"><figcaption><p>Elastic Collision</p></figcaption></figure>

### Fixed Body Collisions

To simulate a fixed body, such as a wall, assign it a very high [mass](physics.md#body-1) (e.g., 999999). The [mass](physics.md#body-1) should be several orders of magnitude larger than other entities that might collide with it. This ensures the [body’s velocity](physics.md#body-1) remains effectively unchanged during collisions, with all energy transferred to the colliding object.&#x20;

For absolute certainty that it won’t move, manually set its [velocity](physics.md#body-1) to 0 every update.

<figure><img src="../.gitbook/assets/FixedBodyCollision.gif" alt="" width="375"><figcaption><p>Collision between two box-shaped bodies of the same size/shape. <br>One depicted with a very high mass. </p></figcaption></figure>

### Inelastic Collisions

To create an inelastic collision (in which rebound momentum is not conserved), try applying an additional [force ](physics.md#body-1)to an object when its [velocity](physics.md#body-1) changes.

<figure><img src="../.gitbook/assets/output.gif" alt="" width="375"><figcaption><p>Inelastic collision</p></figcaption></figure>

<figure><img src="../.gitbook/assets/InelasticFasterCube.gif" alt="" width="375"><figcaption><p>Inelastic collision between a faster and slower cube.</p></figcaption></figure>

### **Troubleshooting Collisions**

If the game's **ticks per second (TPS)** is low and an entity's speed is high, collisions may be missed. To prevent this, ensure:

$$
\frac{Entity Max Speed }{Entity Min Diameter} > TPS
$$

A higher TPS reduces the risk of undetected collisions and unintended behaviour.

<figure><img src="../.gitbook/assets/output (1).gif" alt="" width="375"><figcaption><p>Collision fails because of a very low TPS.</p></figcaption></figure>

## Physics API

A number of API calls are available for certain physics-related actions, such as creating a physics Body. They are accessed using the `api.physics` object available to all server-side scripts.

Methods

{% include "../.gitbook/includes/physics-api-table.md" %}



***

### Shape

```lua
api.physics.NewBoxShape(1, 1, 1)
api.physics.NewSphereShape(1)
```

| Shape   | Parameters                                                                                  | Description                   |
| ------- | ------------------------------------------------------------------------------------------- | ----------------------------- |
| Box     | <p><code>width: float</code><br><code>length: float</code><br><code>depth: float</code></p> | An axis-aligned bounding box. |
| Sphere  | `radius: float`                                                                             | It's just a sphere.           |



***

### Body

```lua
api.physics.NewBody(shape, mass)
```

#### Fields

| Field    | Type     | Description                                                                              |
| -------- | -------- | ---------------------------------------------------------------------------------------- |
| Shape    | `Shape`  | The size and shape of the Body's collision boundaries.                                   |
| Mass     | `float`  | The mass of the physics body, which dictates how effective forces are upon it.           |
| Force    | `Vector` | The XYZ vector of force being applied to the physics body on the current update.         |
| Velocity | `Vector` | The current velocity of the physics body, for how fast it is moving in an XYZ direction. |

#### Methods

Each of these methods takes the Body object `self.Body` as its first parameter. Hence you may use `self.Body:ApplyForce(force)` instead of `self.Body.ApplyForce(self.Body, force)`.

<table><thead><tr><th width="230">Method</th><th width="196">Parameters</th><th>Returns</th><th>Description</th></tr></thead><tbody><tr><td>ApplyForce</td><td><code>force: vector</code><br></td><td>None</td><td>Apply a one time XYZ force to a physics body. <br>It is applied on the next update.</td></tr><tr><td>GetBoxDimensions</td><td>None</td><td><code>width: float</code><br><code>length: float</code><br><code>depth: float</code></td><td>Get the dimensions of a box shaped Body. Errors if the Body is not a Box.</td></tr><tr><td>GetRadius</td><td>None</td><td><code>radius: float</code></td><td>Get the radius of a Body. Errors if the Body is not a Sphere.</td></tr></tbody></table>



***

### Vector

```lua
api.physics.NewVector(1,1,0)
```

#### Fields

| Field | Type  |
| ----- | ----- |
| X     | float |
| Y     | float |
| Z     | float |

#### Methods

Each of these methods takes the `Vector` object as its first parameter. Hence you may use `vector_a:Add(vector_b)` instead of `vector_a.Add(vector_a,vector_b)`.

```lua
v1 = api.physics.NewVector(1,1,0)
v2 = api.physics.NewVector(0,2,2)

sum_v1_v2 = v1:Add(v2)
-- sum_v1_2 is now &{1 3 2}
```

<table><thead><tr><th width="230">Method</th><th width="196">Parameters</th><th>Returns</th><th>Description</th></tr></thead><tbody><tr><td>Add</td><td><code>b: Vector</code></td><td><code>Vector</code></td><td>Add the corresponding vector value:<br><code>a.X + b.X</code> <br> <code>a.Y + b.Y</code> <br> <code>a.Z + b.Z</code></td></tr><tr><td>Sub</td><td><code>b: Vector</code></td><td><code>Vector</code></td><td>Subtract the corresponding vector value:<br><code>a.X - b.X</code> <br> <code>a.Y - b.Y</code> <br> <code>a.Z - b.Z</code></td></tr><tr><td>Dot</td><td><code>b: Vector</code></td><td><code>float</code></td><td>Measure the degree of directional similarity between the parameters:<br> <code>a.X*b.X</code> <br><code>+ a.Y*b.Y</code> <br><code>+ a.Z*b.Z</code></td></tr><tr><td>Mul</td><td><code>b: float</code></td><td><code>Vector</code></td><td><p>Multiply each vector value by a number:</p><p><code>a.X * b</code> <br> <code>a.Y * b</code> <br> <code>a.Z * b</code> </p></td></tr><tr><td>Cross</td><td><code>b: Vector</code></td><td><code>Vector</code></td><td><p>Create a third vector, perpendicular  to the parameters:</p><p><code>a.Y*b.Z - a.Z*b.Y</code> <br> <code>a.Z*b.X - a.X*b.Z</code> <br> <code>a.X*b.Y - a.Y*b.X</code></p></td></tr><tr><td>Normalise</td><td>None</td><td><code>Vector</code></td><td><p>Make a vector unit length:</p><p></p><p><code>mag =</code> <br> <code>Sqrt(v.X^2  ​</code><br>     <code>+v.Y^2  ​</code><br>     <code>+v.Z^2)</code> <br></p><p><code>norm =</code> <br> <code>Mul(v, 1/mag)</code> </p></td></tr></tbody></table>



## Notes

* Currently rotation is not supported.
* Planetary Processing Physics is in continuous development. Please contact us on our [Discord](https://pp.vg/discord) if you would like to request specific functionality.
* Due to synchronization between multiplayer elements, physics is **not deterministic**. This means that you may experience different physics interactions each time you run a simulation.&#x20;

# Table of contents

* [Introduction](README.md)
* [HTTP API](http.md)

## sdks

* [Feature Comparison](sdks/feature-comparison.md)
* [Unity](sdks/unity.md)
* [Godot](sdks/godot.md)
* [Defold](sdks/defold.md)
* [LÖVE](sdks/love.md)
* [Unreal](sdks/unreal.md)

## Quick Start

* [Unity](quick-start/unity.md)
* [Godot](quick-start/godot.md)
* [Defold](quick-start/defold.md)
* [LÖVE](quick-start/love.md)
* [Unreal](quick-start/unreal.md)

## Server Side <a href="#server" id="server"></a>

* [Entities](server/entities.md)
* [Chunks](server/chunks.md)
* [Dimensions](server/dimensions.md)
* [Events](server/events.md)
* [Logging](server/logs.md)
* [Lua Environment](server/lua.md)
* [Git Primer](server/git.md)
* [Physics](server/physics.md)

## HTTP API

* [Authentication](http-api/authentication.md)
* [Player API](http-api/player-api.md)

## Api Reference

* [Entity API](api-reference/entity-api/README.md)
  * [Create](api-reference/entity-api/create.md)
  * [Message](api-reference/entity-api/message.md)
  * [Entity](api-reference/entity-api/entity/README.md)
    * [Fields](api-reference/entity-api/entity/fields/README.md)
      * [ID](api-reference/entity-api/entity/fields/id.md)
      * [Type](api-reference/entity-api/entity/fields/type.md)
      * [Data](api-reference/entity-api/entity/fields/data.md)
      * [Chunkloader](api-reference/entity-api/entity/fields/chunkloader.md)
      * [Transient](api-reference/entity-api/entity/fields/transient.md)
    * [Methods](api-reference/entity-api/entity/methods/README.md)
      * [Move](api-reference/entity-api/entity/methods/move.md)
      * [MoveTo](api-reference/entity-api/entity/methods/moveto.md)
      * [GetPosition](api-reference/entity-api/entity/methods/getposition.md)
      * [Remove](api-reference/entity-api/entity/methods/remove.md)
      * [GetNearbyEntities](api-reference/entity-api/entity/methods/getnearbyentities.md)
      * [Warp](api-reference/entity-api/entity/methods/warp.md)
    * [Necessary Methods](api-reference/entity-api/entity/necessary-methods/README.md)
      * [init](api-reference/entity-api/entity/necessary-methods/init.md)
      * [update](api-reference/entity-api/entity/necessary-methods/update.md)
      * [message](api-reference/entity-api/entity/necessary-methods/message.md)
* [Chunk API](api-reference/chunk-api/README.md)
  * [Chunk](api-reference/chunk-api/chunk/README.md)
    * [Fields](api-reference/chunk-api/chunk/fields/README.md)
      * [ID](api-reference/chunk-api/chunk/fields/id.md)
      * [X](api-reference/chunk-api/chunk/fields/x.md)
      * [Y](api-reference/chunk-api/chunk/fields/y.md)
      * [Size](api-reference/chunk-api/chunk/fields/size.md)
      * [Generated](api-reference/chunk-api/chunk/fields/generated.md)
      * [Dimension](api-reference/chunk-api/chunk/fields/dimension.md)
      * [Data](api-reference/chunk-api/chunk/fields/data.md)
    * [Necessary Methods](api-reference/chunk-api/chunk/necessary-methods/README.md)
      * [init](api-reference/chunk-api/chunk/necessary-methods/init.md)
* [Client API](api-reference/client-api/README.md)
  * [Message](api-reference/client-api/message.md)
* [Dimension API](api-reference/dimension-api/README.md)
  * [Create](api-reference/dimension-api/create.md)
  * [Delete](api-reference/dimension-api/delete.md)
  * [List](api-reference/dimension-api/list.md)
* [Events API](api-reference/events-api/README.md)
  * [on\_player\_join](api-reference/events-api/on_player_join.md)
  * [on\_player\_leave](api-reference/events-api/on_player_leave.md)
* [Table API](api-reference/table-api/README.md)
  * [Append](api-reference/table-api/append.md)
  * [Remove](api-reference/table-api/remove.md)
  * [Join](api-reference/table-api/join.md)
* [Util API](api-reference/util-api/README.md)
  * [Time](api-reference/util-api/time.md)
  * [TimeMillis](api-reference/util-api/timemillis.md)
* [HTTP Request API](api-reference/http-request-api/README.md)
  * [Get](api-reference/http-request-api/get.md)
  * [Post](api-reference/http-request-api/post.md)
  * [Put](api-reference/http-request-api/put.md)
  * [Delete](api-reference/http-request-api/delete.md)
* [Physics API](api-reference/physics-api/README.md)
  * [NewBody](api-reference/physics-api/newbody.md)
  * [NewBoxShape](api-reference/physics-api/newboxshape.md)
  * [NewSphereShape](api-reference/physics-api/newsphereshape.md)

## Template Projects

* [Rust Realm (MMO by BiteMe Games)](template-projects/rust-realm-mmo-by-biteme-games.md)
* [Cat & Trees (Fundamentals Template)](template-projects/cat-and-trees-fundamentals-template.md)
* [Team Tag (Entity communication)](template-projects/team-tag-entity-communication.md)

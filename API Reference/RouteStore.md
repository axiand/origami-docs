# `new RouteStore()`
This is where all the routes of your app live. This class can be used to get and set routes, and is used internally for this purpose.

## ⚠ This is an internal class.
Interacting with it directly is probably not the best idea unless you absolutely *must*, and you should just let the framework itself handle things.

# Example use
```js
var rstore = new RouteStore()
```

# Children

### Object routeTree
The route tree of this store.

# Functions

## `.resolveUrlPart(String part)`
Resolves a URL part into an `{include, typeName}` construct. 

### `String part`
The URL part to be resolved.

### Example
```js
rstore.resolveUrlPart(':profile') // --> 
//{
//   'includes': 'profile',
//   'typeName': null, 
//}
```

## `.mountRoute(Route route)`
Abstract interface for the `routeTreeAdd` API, to be used internally by the Mounter.

### `Route route`
The route to mount.

### Example use
```js
rstore.mountRoute(new Route(...))
```

## `.routeTreeAdd(pathArr, idx, object, route)`
Highly specific internal recursive function for handling the process of adding routes to the tree. **There should be absolutely no case ever where you need to use this manually, so it will not be documented here. Use .mountRoute(...) instead.**

## `.getRoute(String path, String method)`
Abstract interface for the `routeTreeGet` API, to be used internally by the Server.

### `String path`
The URL to be resolved.

### `String method`
The HTTP method.

### Example use
```js
let {route, includes} = rstore.getRoute('/users/1/profile', 'PUT')
// route --> Standard Route class || null
// includes --> Object of all applicable includes
```

## `.routeTreeGet(pathArr, idx, object, includes)`
Recursive function used internally by `.getRoute()`. **There should be absolutely no case ever where you need to use this manually, so it will not be documented here. Use .getRoute(...) instead.**
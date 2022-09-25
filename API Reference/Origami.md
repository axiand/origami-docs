# `new Origami(port = 3000, settings = {})`
This class is the root class for the entire Origami framework. Usually when instantiating this class you are creating a new app with a server to go along with it.

# Example use
```js
var app = new Origami(3000, {})
```

# Children

### Number port
The port this app is designated to run on.

### Object settings
The configuration of this app.

### [internal OrigamiServer](./OrigamiServer.md) server
The server of this app. Usually an OrigamiServer, started using `.listen()`

### [internal OrigamiMounter](./OrigamiMounter.md) mounter
The mounter of this app. Handles tasks related to mounting objects onto this app.

### [internal RouteStore](./RouteStore.md) routes
The route storage of this app. Stores the route tree of the app.

### [internal ComponentStore](./ComponentStore.md) components
The component storage of the app. Stores all components.

# Functions

## `.listen(Function cback)`

### `Function cback`
Callback to be fired when the server is live.

### Example use
```js
app.listen(() => {
    console.log("And, we're live!")
})
```

***

## `.Mount(Any thing)`

### `Any thing`
The thing you want to mount to the app.

### Example use
```js
app.Mount(new Component('MyMountedComponent'))
```

***

## `.Route(String method, String path, Function resolver)`

Shorthand for instantiating a route and mounting it to the app.

### `String method`
The method of the route.

### `String path`
The path of the route.

### `Function resolver`
The resolver of the route, to be fired when a request hits this route.

### Example use
```js
app.Route('POST', 'users/:user/posts', {} => (
    //** omitted logic... **//
))
```

***

## `.Component(String name, Function getRecipe = {} => ())`
Shorthand for instantiating a component and mounting it to the app.

### `String name`
The name of the component.

### `Function getRecipe`
The get recipe of the component, optional.

### Example use
```js
app.Component('Post', {} => (
    //** omitted logic... **//
))
```
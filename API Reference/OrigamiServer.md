# `new OrigamiServer(port, parent)`
This class is where the Origami HTTP server lives.

## ⚠ This is an internal class.
Interacting with it directly is probably not the best idea unless you absolutely *must*, and you should just let the framework itself handle things.

# Example use
```js
var serv = new OrigamiServer(3000, --Instance of origami base--)
```

# Children

### Number port
The port this server is designated to run on.

### Origami parent
The base instance of Origami this server belongs to.

### internal RequestHandler handler
The request handler of this server.

### server
The Node HTTP server.

# Functions

## `.listen()`

### Example use
```js
serv.listen()
```
# `new OrigamiMounter(server)`
This class is used to handle mounting things onto the app.

## ⚠ This is an internal class.
Interacting with it directly is probably not the best idea unless you absolutely *must*, and you should just let the framework itself handle things.

# Example use
```js
var mounter = new OrigamiMonter(--Instance of origami base--)
```

# Children

### Origami server
The Origami app this instance belongs to.

### Array allowedMounts
An array of allowed classes to be mounted.

# Functions

## `.mount(Any thing)`
Mount a thing to the app.

### `Any thing`
The thing to be mounted.

### Example use
```js
mounter.mount(new Route(...))
```
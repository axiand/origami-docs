# `new ComponentStore()`
The components of your app live here.

## ⚠ This is an internal class.
Interacting with it directly is probably not the best idea unless you absolutely *must*, and you should just let the framework itself handle things.

# Example use
```js
var cstore = new ComponentStore()
```

# Children

### Object store
The component store.

```json
{
    'Comment': Component {...},
    'User': Component {...},
    ...
}
```

# Functions

## `.mountComponent(Component comp)`
Mount a component to this store.

### `Component comp`
The component to be mounted.

### Example use
```js
var c = cstore.mountComponent(new Component(...))
```
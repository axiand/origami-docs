# `new RequestHandler()`
Class for handling request resolver responses. Takes in resolver responses, spits out metadata for HTTP payloads.

## ⚠ This is an internal class.
Interacting with it directly is probably not the best idea unless you absolutely *must*, and you should just let the framework itself handle things.

# Example use
```js
var rh = new RequestHandler()
```

# Children

### Array AllowedClasses
Array which stores a list of allowed class names that can be handled.


# Functions

## `.proc(Any resolverResponse)`
Process a resolver response.

### Any resolverResponse
The response - constructor name must be equal to one of the allowed class names.

### Example use
```js
let {head, status, write} = rh.proc({'response': 'data...'})
```
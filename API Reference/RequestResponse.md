# `new RequestResponse()`
A request response. This should be `return`ed from the route's resolver.

### Example use
```js
let res = new RequestResponse()
```

# Children

### Number status
The HTTP status of the response. Default is `200 OK`.

### Any body
The body of the response.

### String mime
The MIME type of the response.

### Object headers
The headers to be sent with the response.

# Functions

## `.write(Any body, Number status)`
Write a value to the response body. This is typically an `Object`, but theoretically this should play nice with just about anything.

### Any body
The body of the response.

### Number status
The status of the response, optional.

### Example use 
```js
res.write({...}, 201)

res.write({...})

res.write(instance of Buffer)
```

***

## `.setType(String mime)`
Set the MIME type of the response.

### String mime
The type to be set.

### Example use
```js
res.setType('text/plain')
```

***

## `.setHead(String k, String v)`
Set a header. If the header already exists, this function will *overwrite* it.

### String k
The header's name.

### String v
The header's value.

### Example use
```js
res.setHead('Content-Length', 256)
```

***

## `.setStatus(Number status)`
Set the response's HTTP status.

### Number status
The status.

### Example use
```js
res.setStatus(201)
```

***

## `.setHeadMany(Object headers)`
Convenience function to set many headers at once.

### Object headers
The headers to be set / overwritten.

### Example use
```js
res.setHeadMany(
    {
        'header': 'value',
        'other-header': 'other-value',
        ...
    }
)
```

***

## `.error(String code, String message, Number status)`
Returns a request error object. Useful if you wish to reject a request because of a client error.

### String code
The code of the error. Usually a machine-readable code. Default is `BAD_REQUEST`.

### String message
A message. This can be used to serve a user-facing message with the error, or to just provide extra info. Default is `Bad Request`.

### Number status
The status of the response. Default is `400`.

### Example use
```js
return res.error() // --> Standard error with all default values.

return res.error('EMAIL_TAKEN', 'The E-Mail address is already taken') // --> A more specific error message.

return res.error('PAYMENT_REQUIRED', 'Payment is required.', 402) // --> A message utilizing the HTTP 402 Payment Required status.
```
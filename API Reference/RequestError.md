# `new RequestError(code = 'BAD_REQUEST', message = '', status = 400)`
A request error, typically used to serve client errors.

# Example use
```js
var error = new RequestError()

//** inside request resolver code block... **//
    if(errorCondition) {
        return new RequestError(...)
    } else {
        return ...
    }
```

# Children

### String code
The code of the error.

### String message
The message of the error. Typically used for user-facing messages or to provide more info.

### Number status
The status of the error. Typically a 4xx status.
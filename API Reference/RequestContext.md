# `new RequestContext(server, context)`
The context of a request's resolver. Contains various data about the request such as headers, etc...

# Example use
```js
var ctx = new RequestContext(--Instance of Origami server--, --Context data--)
```

# Children

### Origami ServerContext
The server this request originates from.

### Object cache
The cache of this context object.

### Object includes
The includes of this request.

### Buffer body
The body of the request, sent by the client.

### Object headers
The headers received from the client.

### String method
The HTTP method of the request.

### String queryString
The query string of this request, if any.

# Functions

## `.GetServer()`
Gets the server of this request.

***

## `.GetApp()`
Gets the app of this request.

***

## `.getHead(String k)`
Get a header.

### String k
The name of the header to be retrieved.

### Example use
```js
let ctype = ctx.getHead('Content-Type')
```

***

## `.bake(String include, Any meta)`
Bake a component from the includes. Automatically serves the right recipe based on the request's HTTP method.

### String include
The name of the include.

### Any meta
Metadata to be passed on to the recipe. Optional.

### Example use
```js
let post = ctx.bake('Post', {...})
```

***

## `.parseQueryString(String qs)`
Parse a raw query string. Note that this function expects the `?` to be omitted.

### String qs
The query string.

### Example use
```js
let query = ctx.parseQueryString(ctx.queryString)
```

***

## `.getQuery(String k)`
Get a value from the query string. Preferred over `.parseQueryString()` as the query is cached for future use.

### String k
The value to get.

### Example use
```js
let page = ctx.getQuery('page')
```
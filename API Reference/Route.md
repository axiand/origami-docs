# `new Route(String method, String path, Function resolver)`
A route in Origami, the most important building block of an app.

# Example use
```js
var myRoute = new Route('GET', '/books/:book', (ctx, res) => {
    //** omitted logic... **//
})
```

# Children

### String path
The path of the route.

### String method
The HTTP method of the route.

### Function resolver
The route's resolver.

# Functions

## `.resolver(RequestContext, RequestResponse)`
Resolve the route with a context and a response.

### RequestContext
The context.

### RequestResponse
The response.

### Example use
```js
myRoute.resolver(new RequestContext(...), new RequestResponse(...))
```
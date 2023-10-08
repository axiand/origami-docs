# Intro to Middlewares

Previous: [More Advanced Routes](More%20Advanced%20Routes.md)

In the previous section, we left off on spicing up our routes with Origami's tools. Now that that's done, there is one more thing left to discuss, something that is present in every great web framework. That's right: we're talking about middlewares.

Origami has two kinds of middlewares: those that run *before* the route resolver, and those that run *after*. As is often the case, middlewares run sequentially, one after another.

## 1. Adding Middlewares

Let's consider this sample route for a moment:

```js
let secret_rt = app.Route('GET', '/secret', (ctx, res) => {
    return res
           .write(
                {'secrets': 'Shh!'}
           )
})
```

Very plain and simple. Now, let's pretend this route hides something *veeeery secret*, something where we need to be sure that only those authorized can access it. Something like, let's say, a secret passphrase in a header.

This is where middlewares come in. Specifically, *before middlewares*.

Here is our middleware function:

```js
const { RequestError } = require("@axiand/origami")

function requirePassword(ctx) {
    if(ctx.getHead("pwd") == "hunter2") {
        return;
    } else {
        throw new RequestError('NO_SECRETS_FOR_YOU')
    }
}
```

Now, this function is obviously useless if we don't bind it to the route. So:

```js
secret_rt.before(requirePassword)
```

Now, we've bound the `requirePassword` middleware to the route. It will always run its code before the route resolver, checking for the password, keeping our secrets safe! Also note how Origami passes the request context to our middleware.

Let's see what happens now:

If we request our secret route with no additional headers, we get an HTTP 400 with a body of: 

```json
{"code":"NO_SECRETS_FOR_YOU","message":""}
```

If we add a header `pwd` with a value of `hunter2`, we get an HTTP 200:

```json
{
    "secrets": "Shh!"
}
```

So, our middleware is working! Yay!

## 2. More Middleware Shenanigans

So, what if we wanted to do something else with middlewares? Like logging a request after it's done, or adding a header.

I'll show you how to do both of those things:

```js
function injectHeader(ctx, res) {
    if(ctx.getHead("ping") == "0") { res.setHead("pong", "1") }

    return ctx, res
}

function logDone() {
    console.log("Request hit!")
}
```

And let's bind these to a route:

```js
app.Route("GET", "/logged", (ctx, res) => {
    return res.write({"message": "OK!"})
})
.before(injectHeader)
.after(logDone)
```

Now, if you send a GET to /logged, you should notice two things:

1. If you set a header `ping` equal to `0`, you'll get a new header back: `pong: 1`
2. After the resolver, a message appears in your console: `Request hit!`

This means that both of our middlewares are working as intended! Notice how before middlewares can access and modify the request response even before it hits the main resolver. This is useful for when you want to tack on additional headers or do some other pre-processing action.

## 3. Understanding middleware behaviour

One thing you'll want to know about middlewares in Origami is that 1. they do not apply globally unless you bind a middleware to a route with a path of `/` and 2. middlewares will *cascade* down the route tree.

Now, what do I mean by *cascading*? To keep it brief, it means that when you apply a middleware to a route, it will also apply to all descendants of that route. To help you understand, here's a chart for reference:

```js
function requiresAuth(...) {...}
function requiresDashboardPrivilege(...) {...}

app.route("GET", "/private", () => {})
.before(requiresAuth) // requiresAuth applies, since it was bound

app.route("GET", "/private/payments/:id", () => {}) // requiresAuth cascades, since it is a descendant of /private

app.route("GET", "/private/dashboard", () => {})
.before(requiresDashboardPrivilege) // requiresAuth cascades, requiresDashboardPrivilege was bound

app.route("GET", "/private/dashboard/:pageId", () => {}) // requiresAuth and requiresDashboardPrivilege both cascade since it is a descendant of /private/dashboard

app.route("GET", "/public/users/:id", () => {}) // neither cascade, since it is not a descendant of /private nor /private/dashboard

```

Thank you for reading, and stick around for the next lesson where we will learn how to work with Components.

Next: [Intro to Components](./Intro%20to%20Components.md)
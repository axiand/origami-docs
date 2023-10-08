# First Steps with Origami

Previous: [Intro](./Intro.md)

## 1. The index.js File

Picking up where we left off, open up your text editor / IDE of choice and create an `index.js` file - this will be the entry point of our app.

Fill in this file with the following code, which we'll explain shortly:
```js
// Import Origami
const { Origami } = require('@axiand/origami')

// Create an app running on port 3000
var app = new Origami(3000)

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

In this file, we are importing the base `Origami` class from the package, which we then create an instance of. This instance contains our entire app, and serves as the base of everything else you'll be doing with Origami.

We tell the app to listen to port `3000` in the `app.listen()` function. The callback to this function can be anything you want - here, we are simply outputting a message to the console informing us that the server is live.

Let's navigate to [localhost:3000](http://localhost:3000) and see what we get.

You should be seeing a `404 Not Found` message, and that's totally expected, because right now, our app is dead weight; it has no content. So let's add some!

## 2. Our First Route
Let's bring a route to the table. Nothing crazy yet, just a simple route that echoes "Hello, World!" will suffice for now.

Ideally, you'll wanna put the `app.listen()` call as the very last thing in your file, so any routes we add will live between where we initialized our `app` variable and `app.listen()`.

Routes in Origami are easy to create. You just need to provide a *method*, a *path*, and a *resolver*.

- The **Method** must be one of these HTTP methods: GET, POST, PUT, DELETE, PATCH
- The **Path** must be a valid URL path. For your own convenience, trailing slashes at the start and end will be removed automatically.
- The **Resolver** must be a valid function that `return`s something.

With this in mind, let's craft a new route:
```js
app.Route('GET', '/helloworld', (ctx, res) => {})
```

Let's navigate to [localhost:3000/helloworld](http://localhost:3000/helloworld) now and see what we get. So, you should be seeing an empty object `{}` as the response. This is because we're not actually returning anything in our function. Let's add a `Hello, World!` message to our response.

```js
app.Route('GET', '/helloworld', (ctx, res) => {
    return {'greeting': 'Hello, World!'}
})
```

Excellent! Our app is now returning a greeting to the user. This example works for very simple use cases, but let's dive into how you would do this the proper way.

```js
app.Route('GET', '/helloworld', (ctx, res) => {
    return res
           .write(
                {'greeting': 'Hello, World!'}
           )
})
```

Here we are using the `res` object to write to the request response. The `res` and `ctx` objects are very powerful and include all the tooling necessary to manipulate the request's response and fetch data about its context, respectively.

Next: [More Advanced Routes](More%20Advanced%20Routes.md)
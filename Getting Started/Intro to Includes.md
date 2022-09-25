# Intro to Includes

Previous: [More Advanced Routes](./More%20Advanced%20Routes.md)

Includes allow you to make dynamic routes that serve dynamic content! In this article we'll be going over how to make a route that utilizes includes.

Starter code:

```js
const { Origami } = require(...)

// Create an app running on port 3000
var app = new Origami(3000)

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

## 1. Fetching Blog Posts

Here we'll be fetching a blog post based on an unique ID provided in the URL. Fortunately for us, includes make this process easy. Take a look at this code:

```js
app.Route('GET', '/articles/:artId', (ctx, res) => {
    return {}
})
```

See that `:artId`? That's our article ID that the user can provide to fetch any article they want. To verify that this works, navigate to [/articles/1](http://localhost:3000/articles/1). You should be seeing an empty object as the response. `{}`

Now try replacing the `1` in the URL with anything. No, really, absolutely anything! Be it `1`, `tacos` or `65476265476548`, it should work.

This is because includes work like a wildcard in your URL. Anything the user puts there, it'll be routed to the same resolver.

But how do we *use* these includes to our advantage? Don't worry, all of these includes live in our request context object `ctx`. I'll show you how to use it now:

```js
app.Route('GET', '/articles/:artId', (ctx, res) => {
    return {'include': ctx.includes.artId}
})
```

Here's what this should return as a response:

```json
{"include":
    {
        "key":"1",
        "includes":"artId",
        "typeName":null
    }
}
```

As you can see, an include has three properties:

- **key** which is what the user provided in the request URL. Here, it's `1`, because we made a request to `/articles/1`.
- **includes** which is the variable name of the include. This is `artId` because we named our route `/articles/:artId`.
- **typeName** which is the name of the component associated with this include. We'll be talking about components in the next article.

Now that you know how to wrap your head around includes, let's construct a sample response to see roughly how this would work in the real world.

```js
app.Route('GET', '/articles/:artId', (ctx, res) => {
    let article = ctx.includes.artId

    /** Your database logic would probably go here. **/

    return res.write(
        {
            'id': article.key,
            'author': 'Journalistdude Lastname',
            'title': 'How I Made A Million Dollars In 2 Seconds (Number 8 will shock you!)',
            'body': 'Article body goes here.'
        }
    )
})
```

Now, since we're not adding any database logic right now, every one of these articles will be the same, no matter the `key`. But you can see how as you modify the key in the request URL, you get different keys in the response. And that's the magic of includes!

Here's our full code for the sake of keeping your memory fresh:

```js
const { Origami } = require(...)

// Create an app running on port 3000
var app = new Origami(3000)

app.Route('GET', '/articles/:artId', (ctx, res) => {
    let article = ctx.includes.artId

    return res.write(
        {
            'id': article.key,
            'author': 'Journalistdude Lastname',
            'title': 'How I Made A Million Dollars In 2 Seconds (Number 8 will shock you!)',
            'body': 'Article body goes here.'
        }
    )
})

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

Next up we'll be taking includes and turning them up to 11 with Components. So follow the link below and join me in the next article!

Next: [Basic Usage of Components](Basic%20Usage%20of%20Components.md)
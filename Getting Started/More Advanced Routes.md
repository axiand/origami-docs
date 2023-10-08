# More Advanced Routes

Previous: [First Steps with Origami](First%20Steps%20with%20Origami.md)

In this section of the tutorial we'll be going over how to create more advanced routes for our app.

To refresh our memory, here's how our code looks at this point:

```js
const { Origami } = require(...)

// Create an app running on port 3000
var app = new Origami(3000)

app.Route('GET', '/helloworld', (ctx, res) => {
    return res
           .write(
                {'greeting': 'Hello, World!'}
           )
})

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

Now, let us build something a bit more advanced, shall we?

## 1. File Download Example

In this example, let's build a route that fetches a static file from our local storage and serves it to the user.

First we'll need to define what file we're talkng about.

```js
const EXAMPLE_FILE_PATH = './MyExampleFile.txt'
```

And let's create this file. Create a file `MyExampleFile.txt` in the same directory as your `index.js`. You can copy the example content I'm using below, or write whatever you want - I'll leave it up to you!

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum mattis nisl a odio pellentesque, sed rutrum nulla ornare. Nulla ut egestas nunc, et mattis eros. Etiam eget mauris metus. Pellentesque efficitur tempus tellus eu semper. Pellentesque ut mollis nulla. Praesent nec aliquam augue. Suspendisse ullamcorper elit nunc, eu congue neque sodales a. In euismod consectetur accumsan. Curabitur congue felis quis molestie cursus.
```

And, to handle tasks on the filesystem, Node.js provides the helpful `fs` module. Let's bring it in:

```js
const fs = require('fs')
```

Now comes the fun part: actually coding our route in! Consider this boilerplate to start off with:

```js
app.Route('GET', '/text', (ctx, res) => {
    return {}
})
```

Obviously, this route does nothing as it stands now. So let's add the functionality!

```js
app.Route('GET', '/text', (ctx, res) => {
	let contents = fs.readFileSync(EXAMPLE_FILE_PATH)
	
    return res
			.setType('text/plain')
			.write(contents)
})
```

Now navigate to [localhost:3000/text](http://localhost:3000/text) and enjoy the results! ✨

And just so we're all on the same page here, take a look at the full code:

```js
const { Origami } = require(...)

const EXAMPLE_FILE_PATH = './MyExampleFile.txt'
const fs = require('fs')

// Create an app running on port 3000
var app = new Origami(3000)

app.Route('GET', '/text', (ctx, res) => {
	let contents = fs.readFileSync(EXAMPLE_FILE_PATH)
	
    return res
			.setType('text/plain')
			.write(contents)
})

app.Route('GET', '/helloworld', (ctx, res) => {
    return res
           .write(
                {'greeting': 'Hello, World!'}
           )
})

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

In the next section we'll quickly go over what this code is actually doing.

## 2. How it Works

First we are telling `fs` to read our file:

```js
let contents = fs.readFileSync(EXAMPLE_FILE_PATH)
```

But pay attention to what happens next in our `return` statement:

```js
return res
        .setType('text/plain')
        .write(contents)
```

As I stated prior, `ctx` and `res` are powerful tools for performing operations on the request.

The Origami server is smart - it automatically extracts info from whatever your resolver returns and builds an HTTP response out of it. If you return a plain object like we did in our First Steps article, it'll convert that object into a string and send it off with no extra effort needed from you.

The same is true for `res`, except `res` includes much more metadata for the server to build a response out of.

So, here we are using `res.setType(...)` to set the MIME type of the response. For the uninformed, [this MDN article](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types) will often come in handy.

Then we call `res.write(...)` to set the body of our response. Setting the MIME type of our response appropriately is important to avoid errors and unexpected behaviour. Had we not set the MIME type in this example, we'd be getting our file, represented as a JavaScript `Buffer`, converted to JSON and stringified. Which is, to say the least, not what we wanted.

Now that you're more knowledgeable about how to use Origami, let's take your skills even further! Join me in the next article as we go over includes.

Next: [Intro to Middlewares](./Intro%20to%20Middlewares.md)
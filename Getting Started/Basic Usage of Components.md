# Basic Usage of Components

Previous: [Intro to Middlewares](./Intro%20to%20Middlewares.md)

Good to see you again! Last time, we looked at how Includes can be used to serve dynamic data through our routes. But now let's talk about Components.

Components allow you to take a type of data and share it across your entire app quickly and easily.

Consider this use case:

We have a social media platform, and with that comes fetching users. Now, we probably don't want to embed the same code all over the place for fetching users, so let's simplify this with Components.

Our starter code will be the same as the last article.

## 1. How Components Work

Going off the social media use case, let's say we want the client to send requests to `/users/:user` to fetch user profiles. Now, it's important to understand the anatomy of a Component to make sure that you do not get lost. Luckily, creating a Component is as simple as creating a JavaScript class, with a bit of extra syntax sprinkled on top.

The gist of it is that when calling your Component, Origami creates a new instance of your class. It expects your constructor to return a set of functions named after the HTTP methods, such as `Get()`, `Patch()`, etc. 

From there, Origami figures out the right function to call based on the method of the incoming request.

Components can also be provided with **meta**data, which is any info you might want to use to specify things. It's up to you.

Finally, every Component expects a **key** for its constructors, which is exactly like an Include's key.

## 2. Initializing a Component

Initializing a component is easy:

```js
class UserProfile {
	constructor() {
        // ** Any logic you would like to run across all methods would go here. ** //

		return {
			Get: function (key) {
                // ** Your DB logic for GET requests would go here. ** //

				return {
					'userId': key,
					'firstName': 'John',
					'lastName': 'Doe',
					'displayName': 'TheRealJohnDoe',
					'bio': 'Sup',
				}
			}
		}
	}

    // Since this is a class like any other, you can add more logic here.
}

// Then, we simply bind this component to our Origami app.
app.Component("UserProfile", UserProfile)
```

Now we have initialized a fresh component with:

- A **name** of UserProfile
- And a **get recipe** which takes a **key** and returns some sample data.

But we aren't actually *using* this Component yet, we'll have to initialize a route which takes it and performs some operations on it. Pay attention to the syntax for implementing Components in routes:

```js
app.Route('GET', '/users/:UserProfile profile', (ctx, res) => {
    return {}
})
```

Let's dig into how this works.

### `:UserProfile profile`
where:

- **UserProfile** is the name of our Component.
- **profile** is the name of the Include we'll be tying this Component to.

So, basically, it's:

### `:<ComponentName> <IncludeName>`

## 3. Using Components with Routes

Now for the final step, integrating the two together!

Consider this example code:

```js
app.Route('GET', '/users/:UserProfile profile', (ctx, res) => {
    return {
        'profile': ctx.bake('profile')
    }
})
```

Now navigate to [/users/1](http://localhost:3000/users/1)...

And *voilà*! We've just created a Component and a route to go along with it. The magic of it all is that Components take a lot of complexity away from implementing advanced logic. For instance, when we made a request to `/users/1`, we:

1. Baked an instance of `UserProfile` with a key of `1`
2. Had the Origami server automatically bind this request to `UserProfile`'s get recipe, since this was a `GET` request.
3. And returned this instance back to the user in a neat JSON response.

So, as you can see, Origami is an awesome tool for streamlining your developer experience.

Here's our finished code in all of its glory:

```js
const { Origami } = require(...)

// Create an app running on port 3000
var app = new Origami(3000)

class UserProfile {
	constructor() {
        // ** Any logic you would like to run across all methods would go here. ** //

		return {
			Get: function (key) {
                // ** Your DB logic for GET requests would go here. ** //

				return {
					'userId': key,
					'firstName': 'John',
					'lastName': 'Doe',
					'displayName': 'TheRealJohnDoe',
					'bio': 'Sup',
				}
			}
		}
	}

    // Since this is a class like any other, you can add more logic here.
}

// Then, we simply bind this component to our Origami app.
app.Component("UserProfile", UserProfile)

app.Route('GET', '/users/:UserProfile profile', (ctx, res) => {
    return {
        'profile': ctx.bake('profile')
    }
})

// Fire up the server
app.listen((app) => {
    console.log(`Server running on port ${app.port} - http://localhost:${app.port}/`)
})
```

## 4. Epilogue

You've just finished building your first app with Origami and have hopefully learned all the skills necessary to get started. Congratulations! Feel free to give yourself a pat on the back!

I hope you enjoyed following through with this tutorial and I hope to see you create great things with Origami. However, you might still be curious as to what other things are possible with Origami. 

## Ready to go further? 🚀

Below are resources to help you learn more about the framework.

If you want more examples of things to build, or are just looking for inspiration, check out the [Examples section of the docs.](../Examples/Examples%20Index.md)

If you're looking for help with doing specific things with Origami, [the Topics section](../Topics/Topics%20Index.md) will hopefully cover your question.
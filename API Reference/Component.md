# `new Component(name, getRecipe = () => {})`
A component in Origami.

# Example use
```js
var comp = new Component('Article', () => {
    //** omitted logic... **//
})
```

# Children

### String name
The component's name.

### Object recipe
A collection of the component's recipes.
```json
{
    'Get': ...,
    'Create': ...,
    'Update': ...,
    'Delete': ...
}
```

# Functions

## `.{{gets || creates || updates || deletes}}(Function fn)`
Set a recipe of the Component to a given resolver function.

### Function fn
The resolver function of the recipe.

### Example use
```js
comp
.gets(() => {...})
.creates(() => {...})
.deletes(() => {...})
```
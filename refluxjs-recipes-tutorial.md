React.js tutorial
=================

With RefluxJS and ReactRouter

Step 1
------

We're going to start real simple with everything contained in just one file.
No bower, no webpack, no grunt, no gulp, no minifiers. Just the minimum we
need to learn React.js, RefluxJS and ReactRouter.

Later everything will be devided up in to several files and folders, but
personally I think it's easier to follow along and understand how everything
works if you start of as simple as possible, and then build on top of that.

After that we might add gulp, bower, etc. But only after we've gone through
the juicy React.js bits!

The only prerequisite, as far as knowledge goes, is that you have completed
(and at least partially understood) the official React.js tutorial. The one
with the comment box.

This is the HTML we're going to work with:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 1</title>
</head>
<body>
    <div id="content"></div>
</body>
</html>
```

Pretty much just an empty div that we'll put React-generated html in to.

While it's possible to generate all HTML using plain JavaScript, I'm going to
be using JSX.

So we add react and JSXTransformer to our HTML file:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 1</title>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
</body>
</html>
```

And now it's finally time to add our first generated HTML

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 1</title>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
    <script type="text/jsx">
        React.render(
            <div>
                <h1>Recipes</h1>
                <ul>
                    <li>Breakfast butter eggs</li>
                    <li>Tony's avocado</li>
                    <li>Coconut porridge</li>
                </ul>
            </div>,
            document.getElementById('content')
        );
    </script>
</body>
</html>
```

So what we're doing here is calling `React.render()` with two parameters. The
first one is our JSX-formatted HTML, and the second one is the element to
render in to. As you can see the JSX in this case is identical to HTML, so
it's very easy to read and learn for anyone who already knows HTML.

However, this isn't very idiomatic React code. React is all about breaking
your HTML down in to reusable components, using `React.createClass`. So let's
do that!

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 1</title>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
    <script type="text/jsx">
        var RecipeApp = React.createClass({
            render: function() {
                return (
                    <div>
                        <h1>Recipes</h1>
                        <ul>
                            <li>Breakfast butter eggs</li>
                            <li>Tony's avocado</li>
                            <li>Coconut porridge</li>
                        </ul>
                    </div>
                );
            }
        });

        React.render(
            <RecipeApp />,
            document.getElementById('content')
        );
    </script>
</body>
</html>
```

Here we created a RecipeApp component, and then we rendered that in to the same
element as we used before. So visually there's no difference, but doing it like
this will give you much better posibilities of keeping your code organized
and code duplication at a minimum!

Another thing you might notice are the parentheses right after the `return`
key word, and right before the `;` at the end of the `render` method.  They're
needed to prevent JavaScript's automatic semicolon insertion from inserting a
semicolon right after the `return`, leaving us with an empty return value. As
you can see inside the anonymous function we used when looping through the
recipe titles to create `li` elements the parentheses are not needed when the
JSX starts on the same line as the `return`'s on.

Step 2a
-------

To stay more organized we're going to split the javascript/jsx parts out to
its own file. All that's left in the html file after doing that is this:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 2a</title>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
    <script type="text/jsx" src="js/components.jsx.js"></script>
</body>
</html>
```

`components.jsx.js` looks like this:

```javascript
(function(React) {
    var RecipeApp = React.createClass({
        render: function() {
            var recipes = [
                'Breakfast butter eggs',
                'Tony\'s avocado',
                'Coconut porridge'
            ];
            
            var recipeElements = recipes.map(function (recipe) {
                return <li>{recipe}</li>;
            });

            return (
                <div>
                    <h1>Recipes</h1>
                    <ul>
                        {recipeElements}
                    </ul>
                </div>
            );
        }
    });

    React.render(
        <RecipeApp />,
        document.getElementById('content')
    );
})(window.React);
```

As you can see I've made some changes compared to how the JSX looked before.
Let's go through it!

```javascript
var recipes = [
    'Breakfast butter eggs',
    'Tony\'s avocado',
    'Coconut porridge'
];
```

First I've put the recipe titles in an array. Imagine getting your recipes
from a database. You'd probably get them back as an array, or some other 
representation that you can iterate over to create an array of recipe titles.

```javascript
var recipeElements = recipes.map(function (recipe) {
    return <li>{recipe}</li>;
});
```

We then loop over the array, creating `li` elements for each recipe title.

```javascript
return (
    <div>
        <h1>Recipes</h1>
        <ul>
            {recipeElements}
        </ul>
    </div>
);
``` 

The newly created array of recipe title list items is then used in the return
value from the `render` method. `{}` is used to get the value of a JavaScript
expression. In this case it's the `recipesElement` array of `li` elements we
created earlier.

Step 2b
-------

For this step we'll add a form (just the form, no logic) to add/edit recipes.

We'll start with some basic CSS styling for the form. To keep it simple we'll
just add it straight to the main html file.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 2b</title>
    <style type="text/css">
        label {
            display: block;
        }

        textarea {
            display: block;
            min-width: 300px;
            min-height: 100px;
        }

        input {
            min-width: 300px;
        }
    </style>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
    <script type="text/jsx" src="js/components.jsx.js"></script>
</body>
</html>
```

Now let's move on to the more interesting JavaScript/JSX parts. This time I'll
go through the file from the bottom up. At the end of this section I'll show
the entire file.

```javascript
React.render(
    <RecipeApp />,
    document.getElementById('content')
);
```

Just like before we render our `RecipeApp` component to the `content` div.

```javascript
var RecipeApp = React.createClass({
    render: function () {
        var recipes = [
            'Breakfast butter eggs',
            'Tony\'s avocado',
            'Coconut porridge'
        ];

        return (
            <div>
                <RecipeList recipes={recipes} />
                <RecipeForm />
            </div>
        );
    }
});
```

The `RecipeApp` component is now the owner of two new components, the
`RecipeList` component and the `RecipeForm` component. The `RecipeList`
component has an attribute called `recipes` that takes our list of recipe
titles as its value. In the React world we call these attributes "props".

First let's have a look at the `RecipeForm` component.

```javascript
var RecipeForm = React.createClass({
    render: function () {
        return (
            <div>
                <h1>Add Recipe</h1>
                <form>
                    <label for="recipeName">Recipe Name</label>
                    <input type="text" id="recipeName" placeholder="Boiled water" />
                    <label for="recipeDescription">Description</label>
                    <textarea
                        id="recipeDescription"
                        placeholder="Best recipe for boiled water on the planet"
                    ></textarea>
                    <h2>Ingredients</h2>
                    <button type="button">Add</button>
                    <label for="recipeDirections">Directions</label>
                    <textarea
                        id="recipeDirections"
                        placeholder="Place all the ingredients in a pot. Put the pot on your stove. Put the stove on maximum heat. Wait."
                    ></textarea>
                    <button type="submit">Save</button>
                </form>
            </div>
        );
    }
});
```

As you can see it's a pretty stadard html form.

What was previously the main part of the `RecipeApp` render method has now
been moved to its own `RecipeList` component.

```javascript
var RecipeList = React.createClass({
    render: function () {
        var recipeElements = this.props.recipes.map(function (recipe) {
            return <RecipeListItem recipeName={recipe} />;
        });

        return (
            <div>
                <h1>Recipes</h1>
                <ul>
                    {recipeElements}
                </ul>
            </div>
        );
    }
});
```

Most of it is still the same as when it was part of `RecipeApp`. Two things to
note though. First, remember the `recipes` attribute (or prop as it's called)?
You access it by calling `this.props.recipes`. Looping through the recipe
titles we create instances of yet another new component, namely
`RecipeListItem`.

`RecipeListItem` is the last new component we'll introduce in this step of the
tutorial. It looks like this:

```javascript
var RecipeListItem = React.createClass({
    render: function () {
        var recipeName = this.props.recipeName;
        return (
            <li key="{recipeName}"><a href="#">{recipeName}</a></li>
        );
    }
});
```

For now all titles are just dummy links. Later on you'll obviously be able to
click on them to read the actual recipe!

The entire file, as promised:

```javascript
(function(React) {
    var RecipeListItem = React.createClass({
        render: function () {
            var recipeName = this.props.recipeName;
            return (
                <li key="{recipeName}"><a href="#">{recipeName}</a></li>
            );
        }
    });

    var RecipeList = React.createClass({
        render: function () {
            var recipeElements = this.props.recipes.map(function (recipe) {
                return <RecipeListItem recipeName={recipe} />;
            });

            return (
                <div>
                    <h1>Recipes</h1>
                    <ul>
                        {recipeElements}
                    </ul>
                </div>
            );
        }
    });

    var RecipeForm = React.createClass({
        render: function () {
            return (
                <div>
                    <h1>Add Recipe</h1>
                    <form>
                        <label for="recipeName">Recipe Name</label>
                        <input type="text" id="recipeName" placeholder="Boiled water" />
                        <label for="recipeDescription">Description</label>
                        <textarea
                            id="recipeDescription"
                            placeholder="Best recipe for boiled water on the planet"
                        ></textarea>
                        <h2>Ingredients</h2>
                        <button type="button">Add</button>
                        <label for="recipeDirections">Directions</label>
                        <textarea
                            id="recipeDirections"
                            placeholder="Place all the ingredients in a pot. Put the pot on your stove. Put the stove on maximum heat. Wait."
                        ></textarea>
                        <button type="submit">Save</button>
                    </form>
                </div>
            );
        }
    });

    var RecipeApp = React.createClass({
        render: function () {
            var recipes = [
                'Breakfast butter eggs',
                'Tony\'s avocado',
                'Coconut porridge'
            ];

            return (
                <div>
                    <RecipeList recipes={recipes} />
                    <RecipeForm />
                </div>
            );
        }
    });

    React.render(
        <RecipeApp />,
        document.getElementById('content')
    );
})(window.React);
```

Step 3a
-------

Now it's finally time to bring in Reflux! First thing to do is add the libary
to our html file.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>React + Reflux + ReactRouter - Recipes - Step 3a</title>
    <style type="text/css">
        label {
            display: block;
        }

        textarea {
            display: block;
            min-width: 300px;
            min-height: 100px;
        }

        input {
            min-width: 300px;
        }
    </style>
</head>
<body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.12.2/JSXTransformer.js"></script>
    <script src="https://cdn.jsdelivr.net/refluxjs/0.2.7/reflux.min.js"></script>
    <script type="text/jsx" src="js/components.jsx.js"></script>
</body>
</html>
```

See how we added a link to `reflux.min.js` there? Good! We'll now create our
first Flux Store. It'll be a store for our recipes. For now let's just put our
three familiar recipie titles in it.

```javascript
var recipesStore = Reflux.createStore({
    recipes: [
        'Breakfast butter eggs',
        'Tony\'s avocado',
        'Coconut porridge'
    ],

    getRecipes: function () {
        return this.recipes;
    }
});
```

Now the RecipeList needs to be updated to fetch the recipes from the store.

```javascript
var RecipeList = React.createClass({
    getInitialState: function () {
        return {
            recipes: recipesStore.getRecipes()
        };
    },

    render: function () {
        var recipeElements = this.state.recipes.map(function (recipe) {
            return (<RecipeListItem recipeName={recipe} />);
        });

        return (
            <div>
                <h1>Recipes</h1>
                <ul>
                    {recipeElements}
                </ul>
            </div>
        );
    }
});
```

To get this to work we need to pass in Reflux to our immediately-invoked
function. So at the very top we have `(function(React, Reflux) {`, and at the
bottom we have `})(window.React, window.Reflux);`.

That's it. We're now using a Reflux store to handle our data, and we display
it in our view components. But to get the full Reflux flow going we need to
add one last piece to the puzzle. Actions. That'll be Part 3b.

Step 3b
-------

```javascript
var addRecipe = Reflux.createAction();
```

I'm diving straight in to it here and create an `addRecipe` action. Next step
is to wire up the Store for this new action. Two new methods need to be added.

```javascript
init: function () {
    this.listenTo(addRecipe, this.onAddRecipe);
},

onAddRecipe: function (recipeName) {
    this.recipes.push(recipeName);
    this.trigger('ADD_RECIPE');
},
```

The final part of the Reflux flow is the View Components. There, code needs to
be added to subscribe and unsubscribe to/from events generated by the store.
There also needs to be code added to handle those events when they arrive.
Finally the `addRecipe` action needs to be called to actually add a new recipe.

The subscription, unsubscription and event handling is all done in the
`RecipeList` component.

```javascript
var RecipeList = React.createClass({
getInitialState: function () {
        return {
            recipes: recipesStore.getRecipes()
        };
    },

    onRecipesChange: function (action) {
        this.setState({
            recipes: recipesStore.getRecipes()
        });
    },

    componentDidMount: function() {
        this.unsubscribe = recipesStore.listen(this.onRecipesChange);
    },

    componentWillUnmount: function() {
        this.unsubscribe();
    },

    render: function () {
        var recipeElements = this.state.recipes.map(function (recipe) {
            return (<RecipeListItem recipeName={recipe} />);
        });

        return (
            <div>
                <h1>Recipes</h1>
                <ul>
                    {recipeElements}
                </ul>
            </div>
        );
    }
});
```

The action is called when submitting the form, so it's add it to the
`RecipeForm` component. The important parts in the code below is the function
we call in `onSubmit` for the form, and how we call the `addRecipe` action with
the name of the recipe as the parameter inside that `onSubmit`-function.

```javascript
var RecipeForm = React.createClass({
    handleSubmit: function(e) {
        e.preventDefault();
        var name = this.refs.recipeName.getDOMNode().value.trim();
        addRecipe(name);
    },

    render: function () {
        return (
            <div>
                <h1>Add Recipe</h1>
                <form onSubmit={this.handleSubmit}>
                    <label for="recipeName">Recipe Name</label>
                    <input type="text" id="recipeName" ref="recipeName" placeholder="Boiled water" />
                    <label for="recipeDescription">Description</label>
                    <textarea
                        id="recipeDescription"
                        placeholder="Best recipe for boiled water on the planet"
                    ></textarea>
                    <h2>Ingredients</h2>
                    <button type="button">Add</button>
                    <label for="recipeDirections">Directions</label>
                    <textarea
                        id="recipeDirections"
                        placeholder="Place all the ingredients in a pot. Put the pot on your stove. Put the stove on maximum heat. Wait."
                    ></textarea>
                    <button type="submit">Save</button>
                </form>
            </div>
        );
    }
});
```

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

`components.jsx.js` looks like this:

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

As you can see I've made some changes compared to how the JSX looked before.
Let's go through it!

    var recipes = [
        'Breakfast butter eggs',
        'Tony\'s avocado',
        'Coconut porridge'
    ];

First I've put the recipe titles in an array. Imagine getting your recipes
from a database. You'd probably get them back as an array, or some other 
representation that you can iterate over to create an array of recipe titles.

    var recipeElements = recipes.map(function (recipe) {
        return <li>{recipe}</li>;
    });

We then loop over the array, creating `li` elements for each recipe title.

    return (
        <div>
            <h1>Recipes</h1>
            <ul>
                {recipeElements}
            </ul>
        </div>
    );

The newly created array of recipe title list items is then used in the return
value from the `render` method. `{}` is used to get the value of a JavaScript
expression. In this case it's the `recipesElement` array of `li` elements we
created earlier.

Step 2b
-------

For this step we'll add a form (just the form, no logic) to add/edit recipes.

We'll start with some basic CSS styling for the form. To keep it simple we'll
just add it straight to the main html file.

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

Now let's move on to the more interesting JavaScript/JSX parts. This time I'll
go through the file from the bottom up. At the end of this section I'll show
the entire file.

    React.render(
        <RecipeApp />,
        document.getElementById('content')
    );

Just like before we render our `RecipeApp` component to the `content` div.

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

The `RecipeApp` component is now the owner of two new components, the
`RecipeList` component and the `RecipeForm` component. The `RecipeList`
component has an attribute called `recipes` that takes our list of recipe
titles as its value. In the React world we call these attributes "props".

First let's have a look at the `RecipeForm` component.

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

As you can see it's a pretty stadard html form.

What was previously the main part of the `RecipeApp` render method has now
been moved to its own `RecipeList` component.

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

Most of it is still the same as when it was part of `RecipeApp`. Two things to
note though. First, remember the `recipes` attribute (or prop as it's called)?
You access it by calling `this.props.recipes`. Looping through the recipe
titles we create instances of yet another new component, namely
`RecipeListItem`.

`RecipeListItem` is the last new component we'll introduce in this step of the
tutorial. It looks like this:

    var RecipeListItem = React.createClass({
        render: function () {
            var recipeName = this.props.recipeName;
            return (
                <li key="{recipeName}"><a href="#">{recipeName}</a></li>
            );
        }
    });

For now all titles are just dummy links. Later on you'll obviously be able to
click on them to read the actual recipe!

The entire file, as promised:

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


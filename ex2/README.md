# Exercise 2
_Exploring components_

## Components Properties
So far, you're probably thinking that this seems like a very long winded way to get a `<h1>` tag into a HTML document. You're right, it is.

The next thing we want to learn about components is that they have properties. HTML elements have attributes (e.g. `<input type='text' />`), functions have arguments (e.g. `Math.sqrt(49)`) and React components have properties:

```js
React.createElement(Greeting, { name: 'world' });
```

The second argument to `React.createElement` is always the properties for the component. In this case we are passing a `name` property and it has the value `'world'`.

Update your call to `React.render` so that it looks like that.

This won't change anything, if you refresh your page, because you haven't told your component what to do with its properties.

Update your component's render method to look like this instead:

```js
return (
  React.createElement('h1', null, this.props.name)
);
```

We access the properties of our component with `this.props`. As you can see, we still don't pass any properties to `h1`, because we don't need to give it any attributes.

Now instead of rendering `'Hello, world!'` the component will render whatever we pass as a `name` property.

Each component has a render method, which must return either another component, or some HTML. All we need is a `<h1>` tag with our greeting inside. Add the following code to the `render` function.

```js
return (
  React.createElement('h1', null, 'Hello, world!')
);
```

## Conditional Components
We can use the properties for all sorts of things. Lets make a simple clock that told you whether it before or after midday.

Add this starting code to your file, above the call to `React.render`.

```js
var Clock = React.createClass({
  render: function() {
    var time = this.props.hour < 12 ? 'Morning' : 'Afternoon';
    return (
      React.createElement('h1', null, time)
    );
  }
});
```

We use the ternary if statement here, to help keep things concise. If you aren't already familiar with it, here's a quick breakdown.

`condition ? value_if_true : value_if_false`.

Unlike an if statement, this operator actually returns a value. This is often useful when working with React components.

We also moved some of our code out of the return statement. This is completely fine with React, it doesn't mind where you calculate your values, so long as they end up in that return block.

Hopefully you can see what's going to happen here. We'll need to change our code, so that we render the Clock component rather than the Greeting component.

```js
var hour = (new Date).getHours();

React.render(
  React.createElement(Clock, { hour: hour }),
  document.body
);
```

We use the Date class to dynamically get the current hour, then we simply pass it into the component as a property.

Time for the next step.


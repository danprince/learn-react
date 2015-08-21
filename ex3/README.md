# Exercise 3
_Nested Components_

===

## Components made of Components
Components don't have to be made of just HTML, they can be made of other React components too. Lets take the clock we made last time and update it so that if it's the afternoon, we render an afternoon component, and likewise for the morning.

We'll need two new components.

```
var Morning = React.createClass({
  render: function() {
    return (
      React.createElement('h1', { className: 'morning' }, 'Morning')
    );
  }
});

var Afternoon = React.createClass({
  render: function() {
    return (
      React.createElement('h1', { className: 'afternoon }, 'Afternoon`)
    );
  }
});
```

Now we can change our clock component so that rather than just rendering the text `'afternoon'` or `'morning'` it renders a component.

```
var Clock = React.createClass({
  render: function() {
    return (
      this.props.hour < 12 ? React.createElement(Morning) : React.createElement(Afternoon) 
    );
  }
});
```

Notice that we used the `className` property on our `<h1>` tags this time. This is React's way of saying, give this element this CSS class. We can create a stylesheet and add the following rules to make our components look better.

```
.morning {
  color: yellow;
}

.afternoon {
  color: blue;
}
```

Time for the next step.


---
title: Component Properties
layout: page
category: lesson
---

So far, you're probably thinking that this seems like a very long winded way to get a `<h1>` tag into a HTML document. You're right, it is.

The next thing we want to learn about components is that they have properties.

Just like HTML elements have attributes

{% highlight html %}
<input type='text' placeholder='Enter test' required='true'/>
{% endhighlight %}

Functions have arguments

{% highlight javascript %}
Math.pow(4, 2);
{% endhighlight %}

And React components have properties:

{% highlight javascript %}
React.createElement(Greeting, { name: 'world' });
{% endhighlight %}

We can pass properties to a React component to change the way the component works. Just like we can pass attributes to a HTML element, or pass arguments to a function to change the value it returns.

## Passing Properties

The second argument to `React.createElement` is always the properties for the component. In the above example we are passing a `name` property and it has the value `'world'`.

Update your call to `React.render` so that it looks like this.

{% highlight javascript %}
React.render(
  React.createElement(Greeting, { name: 'world' }),
  document.body
);
{% endhighlight %}

This won't change anything, if you refresh your page, because you haven't told your component what to do with its new properties.

Update your component's render method to look like this instead:

{% highlight javascript %}
return (
  React.createElement('h1', null, this.props.name)
);
{% endhighlight %}

We access the properties of our component with `this.props`. As you can see, we still don't pass any properties to `h1`, because we don't need to give it any attributes.

Now instead of rendering `'Hello, world!'` the component will render whatever we pass as a `name` property.

__It's really important to understand this step. Passing properties to
React components will be an big focus of the rest of this tutorial.__

## Conditional Components
We can use the properties for all sorts of things. Lets make a simple clock that told you whether it is before or after midday.

Add this starting code to your file, above the call to `React.render`.

{% highlight javascript %}
var Clock = React.createClass({
  render: function() {
    var time = this.props.hour < 12 ? 'Morning' : 'Afternoon';
    return (
      React.createElement('h1', null, time)
    );
  }
});
{% endhighlight %}

_You don't need to delete or replace your `Greeting` component. We can define as many React components as we like in a file._

We use the [conditional operator][1] here, to help keep things concise. If you aren't already familiar with it, here's a quick breakdown.

`condition ? value_if_true : value_if_false`.

Unlike an if statement, this operator actually returns a value. This is often useful when working with React components.

We also moved some of our code out of the return statement. This is completely fine with React, it doesn't mind where you calculate your values, so long as they end up in that return block.

Hopefully you can see what's going to happen here. We'll need to change our code, so that we render the Clock component rather than the Greeting component.

{% highlight javascript %}
var hour = (new Date).getHours();

React.render(
  React.createElement(Clock, { hour: hour }),
  document.body
);
{% endhighlight %}

We use the Date class to dynamically get the current hour, then we simply pass it into the component as a property.

Remember that our component is looking for a property called hour when we used `this.props.hour`. We have to make sure that our props object has the appropriate `hour` key.

As soon as it works, it's [time for the next step.](./ex3.html)

[1]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator

---
title: Nested Components
layout: page
category: lesson
---

Components don't have to be made of just HTML, they can be made of other React components too. Lets take the clock we made last time and update it so that if it's the afternoon, we render an afternoon component, otherwise, we'll render a morning component.

We'll need two new components.

{% highlight javascript %}
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
{% endhighlight %}

Now we can change our clock component so that rather than just rendering the text `'afternoon'` or `'morning'` it renders a component.

{% highlight javascript %}
var Clock = React.createClass({
  render: function() {
    return (
      this.props.hour < 12 ? React.createElement(Morning) : React.createElement(Afternoon) 
    );
  }
});
{% endhighlight %}

Notice that we used the `className` property on our `<h1>` tags this time. This is React's way of saying, give this element this CSS class. We can create a stylesheet and add the following rules to make our components look better (well different).

{% highlight css %}
.morning {
  color: yellow;
}

.afternoon {
  color: blue;
}
{% endhighlight %}

All looking good? [Time for the next step.](./ex4.html)


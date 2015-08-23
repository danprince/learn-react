---
title: Reusable Components
layout: page
category: lesson
---

The best components are ones that we can easily reuse in other projects. We're going to make a reusable avatar component, using avatars from [adorable.io](http://avatars.adorable.io/). The service allows us to get a random avatar based on a name or email address.

Create a new React application, include React, the JSX Transfomer and your own `app.js`.

Let's start by defining our component. It can be helpful to try and work out what the components should look like before we try to implement them.

## Avatar
{% highlight javascript %}
<Avatar name={name} size={50}/>
{% endhighlight %}

We'll generate a random avatar based on the name of the commenter, by requesting an image from the url `http://api.adorable.io/avatars/:size/:name.png`.

{% highlight javascript %}
var Avatar = React.createClass({
  getDefaultProps: function() {
    return {
      size: 64,
      name: 'default'
    };
  },
  createUrl: function() {
    return 'http://api.adorable.io/avatars/' +
      this.props.size + '/' +
      this.props.name + '.png';
  },
  render: function() {
    return (
      <img src={this.createUrl()} width={size} height={size}/>
    );
  }
});
{% endhighlight %}

We use the `getDefaultProps` to specify some default properties just in case none are passed to the component. We also create a render function which renders a single `img` tag with some attributes.

The only different thing we've done here is the creation of the `createUrl` helper method. We could have put this in the render method, but it's cleaner to move it out into a standalone function. This approach means that other parts of the component could also use it too.

Render an avatar component with some example props to check that it works.

{% highlight javascript %}
React.render(
  <Avatar name='Test' size={100}/>,
  document.body
);
{% endhighlight %}

## Looping in React
Rather than render one avatar with a hardcoded name, let's render a whole list of names.

{% highlight javascript %}
var names = ['Tom', 'Dick', 'Harry'];

React.render(
  <div>
    {
      names.map(function(name) {
        return <Avatar name={Test} size={100}/>;
      })
    }
  </div>,
  document.body
);
{% endhighlight %}

This might seem a bit odd at first.

By themselves, the names are just strings, we need to turn them into components that `React.render` will understand.

The `.map` method applies a function to each item in an array and returns a new array, made up of the new items. It might be helpful to visualize this.

{% highlight javascript %}
[
  'Tom'   => <Avatar name='Tom' size='100'/>,
  'Dick'  => <Avatar name='Dick' size='100'/>,
  'Harry' => <Avatar name='Harry' size='100'/>
]
{% endhighlight %}



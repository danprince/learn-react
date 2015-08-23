---
title: React from a CDN
layout: page
category: lesson
---

We're going to start with the absolute minimum environment for developing React applications.

## Setup
Create an [`index.html`][1] file to use (_or use [a readymade one][1]_) for this exercise and add the following script tags.

{% highlight html %}
<script src='https://cdnjs.cloudflare.com/ajax/libs/react/0.13.3/react.js'></script>
{% endhighlight %}

Just like most libraries, we can use React from a CDN. Adding this script tag will add React to our project.

Once React is in and you can see that it loads, create a file called `app.js` and add it _below_ the React script tag.

{% highlight html %}
<script src='app.js'></script>
{% endhighlight %}

## Hello, World
Open your `app.js` file and add the following code.

{% highlight javascript %}
var Greeting = React.createClass({
  render: function() {

  }
});
{% endhighlight %}

When writing React, you define _components_, which you create with the `React.createClass` method. Here you're going to write a simple component which renders a greeting to the screen.

Each component has a render method, which must return either another component, or some HTML. We need is a `<h1>` tag with our greeting inside. Add the following code to the `render` function.

{% highlight javascript %}
return (
  React.createElement('h1', null, 'Hello, world!')
);
{% endhighlight %}

We use the `React.createElement` method to describe HTML within our Javascript files. At the moment, all we need to know is that the first argument (`'h1'`) is the element or _component_ you want to render and the third argument is the stuff we want to display inside. Don't worry about the second argument for now.

Once we've defined a React component, __we actually have to use it somewhere if we want it to show up__. Below your greeting component, add the following code.

{% highlight javascript %}
window.addEventListener('load', function() {
  React.render(
    React.createElement(Greeting, null),
    document.body
  );
});
{% endhighlight %}

Remember before, that we mentioned that React can render HTML or components? Here is us using a component rather than a tag name. We also have to tell React where to render that component to. In this case, we want it to be rendered directly to the body.

Notice that we wrote `Greeting` not `'Greeting'`. There's a very good reason for this. React doesn't know about any HTML tags called `<Greeting>`. Since we wrote `var Greeting = React.createClass(...)` our component lives inside a variable called `Greeting`. All we need to do is pass this variable to `React.render`.

Now open `index.html` in a browser and you should see a `<h1>` tag that says `'Hello, world!'`.

All working? Then it's [time for the next step.](./ex2.html)

[1]: https://github.com/danprince/learn-react/blob/gh-pages/resources/index.html

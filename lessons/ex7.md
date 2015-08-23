---
title: Structuring Applications
layout: page
category: lesson
---

It's not particularly nice to have to create an element with an id, then get it with `getElementById` before you can render a React component on the page. Thankfully, you can compose your components together, and render them in one container.

A component doesn't have to be a small thing, like a timer or a button. A component can be as big as the whole application itself, in turn made up of a handful of medium size components, which are made up of small components.

Let's revisit the multiple timers example. Clear the extra elements you added to the HTML and create a new component called `App`.

{% highlight javascript %}
var App = React.createClass({
  render: function() {

  }
});
{% endhighlight %}

We can replace the two calls to `React.render` with one call, using `App` and `document.body`.

{% highlight javascript %}
React.render(
  <App />,
  document.body
);
{% endhighlight %}

Obviously, this won't do anything because we haven't put anything in the render method for our `App` component. Let's change that.

{% highlight javascript %}
var App = React.createClass({
  render: function() {
    return (
      <div className='timers'>
        <Timer interval={100}/>
        <Timer interval={1000}/>
      </div>
    );
  }
});
{% endhighlight %}

We can treat this `App` component as the `<body>` tag of our HTML file now.

## Conceptual Structure
It's really important to see the power of components here. The `<App/>` is a component, made up of two `<Timer/>` components. The timer components could even be made up of more components if we wanted.

Pretty much any conventional application structure can be translated into a component hierarchy.

Here are some examples of components that you might make and then reuse across many projects.

* `<NavigationBar />`
* `<Gallery />`
* `<LoginForm />`
* `<Captcha />`
* `<Search />`

Check out [Elemental UI][1] and [Material UI][2] for projects that already include loads of reusable React components for building web applications.

[Time for the next step.](./ex8.html)

[1]: http://elemental-ui.com/ "Elemental UI"
[2]: http://material-ui.com/ "Material UI"


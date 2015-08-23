---
title: JSX
layout: page
category: lesson
---

Hopefully you will have already noticed that you end up typing `React.createElement` a lot. It's difficult to read large components back, as the different arguments all blend together visually and you end up with huge amounts of code to represent fairly simple things.

There are a few ways to alleviate this pain. We could alias `React.createElement`?

{% highlight javascript %}
var component = React.createElement;

var Clock = React.createClass({
  render: function() {
    return (
      this.props.hour < 12 ? component(Morning) : component(Afternoon) 
    );
  }
});
{% endhighlight %}

However, we still have to keep track of the different arguments and our brains are probably used to reading this kind of structured data as HTML not as Javascript. Enter the second solution, JSX.

## JSX

{% highlight javascript %}
var Clock = React.createClass({
  render: function() {
    return (
      this.props.hour < 12 ? <Morning /> : <Afternoon /> 
    );
  }
});

var hour = (new Date).getHours();
React.render(
  <Clock hour={hour} />,
  document.body
);
{% endhighlight %}

JSX is a language that is very similar to Javascript, but it lets us write React specific code that looks a bit like HTML. It's very important to remember that it isn't HTML though and that underneath the new syntax, it's exactly the same code as we would have written before.

{% highlight javascript %}
<Clock hour={hour} />
// is exactly the same as
React.createElement(Clock, { hour: hour });
{% endhighlight %}

Instead of writing `React.createElement`, we write it _as an element_. Instead of passing properties in as an argument, we pass them in as attributes.

> JSX is the hero Javascript deserves, but not the one it needs right now. So we'll hunt it. Because it can take it. Because it's not our hero. It's a syntactic guardian, a watchful protector, a dark knight.

Not everyone is keen on JSX. A lot of developers learnt the hard way that they needed to separate their logic (JS) from their view (HTML). Separation of concerns is an important thing, but modularity is more important for writing reusable code.

However, if you don't like JSX, you can of course carry on writing React without it, as we started out. Or see what it looks like to write React without JSX in languages such as [Coffeescript][1], [Clojurescript][3] and [Livescript][2].

## Into the Browser

This is all well and good, except the browser doesn't understand JSX. Fail.

But it's ok, because the people who built React also built a Javascript library, which can turn JSX into Javascript, _inside the browser!_

Time to hit the CDN for some more goodness, in the form of the JSXTransformer. Add this script tag above your `app.js` script.

{% highlight html %}
<script src='https://cdnjs.cloudflare.com/ajax/libs/react/0.13.3/JSXTransformer.js'></script>
{% endhighlight %}

However, even with the transformer loaded, our script files will throw syntax errors when they load, because they contain stuff that isn't valid Javascript. We need to let our transformer know which files to transform. We can do this by changing the `type` attribute on the necessary script tags.

{% highlight html %}
<script src='app.js' type='text/jsx'></script>
{% endhighlight %}

Remember, we only need to add the type attribute to scripts that contain JSX rather than regular Javascript. Try converting all of your calls to `React.createElement` to JSX instead.

Make sure that your components still work in the same way.

Then, it's [time for the next step.](./ex5.html)

[1]: http://blog.vjeux.com/2013/javascript/react-coffeescript.html "React in Coffeescript"
[2]: http://red-badger.com/blog/2014/05/27/using-livescript-with-react/ "React in Livescript"
[3]: https://reagent-project.github.io/ "React in Clojurescript"

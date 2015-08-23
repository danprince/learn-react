---
title: Customizable Components
layout: page
category: lesson
---

At the moment, our clock component uses state to store its time. We're not actually using props for anything at the moment. It turns out, passing props is a great way for us to configure the way our components actually work.

We are using a hardcoded interval of 1000ms within our setInterval call. It might be useful to be able to change this for a specific timer. Maybe we want two timers running at different speeds?

Let's add an `interval` prop to our timer.

{% highlight javascript %}
React.render(
  <Timer interval={100} />,
  document.body
);
{% endhighlight %}

By itself, this won't do anything, but it's trivial to change the `componentDidMount` method to use this new property.

{% highlight javascript %}
componentDidMount: function() {
  setInterval(function() {
    this.setState({ time: this.state.time + 1 });
  }.bind(this), this.props.interval);
},
{% endhighlight %}

Lets render two timers, one at 100ms and the other at 1000ms.

We can't render them both into `document.body` because the second one would replace the first. So we'll have to create two new elements for them to go in.

Inside `index.html` create the following structure.

{% highlight html %}
<div class='timers'>
  <div id='t1'></div>
  <div id='t2'></div>
</div>
{% endhighlight %}

Finally, we'll need to update our call to `React.render` to render different timers into the different elements.

{% highlight javascript %}
React.render(
  <Timer interval={100} />,
  document.getElementById('t1')
);

React.render(
  <Timer interval={1000} />,
  document.getElementById('t2')
);
{% endhighlight %}

If everything went to plan, you should see two timers, that will count at different speeds.

Then it's [time for the next step.](./ex6.html)


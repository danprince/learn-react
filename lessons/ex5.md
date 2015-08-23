---
title: Component State
layout: page
category: lesson
---

Components have two associated bits of data. The first is props, which we've already seen. Props are passed down from the parent and we can't change them inside our components.

The other important bit of data our components have is _state_. Unlike props, you can change the state of the component and each time you do, the component will be rendered again to show the changes.

Let's design a new component. We'll make a timer that counts up from 0 in seconds and resets to 0 it is clicked. We could manage time somewhere else and re-render the component with new props, every second. Or we could let the component manage itself, which makes a lot more sense.

{% highlight javascript %}
var Timer = React.createClass({
  render: function() {
    return (
      <button>{this.state.time} s</button>
    );
  }
});
{% endhighlight %}

As you can see, when we are telling a component how it should render, we use `state` in a very similar way to `props`. Now that we've told our timer how to render, now we need to tell it how to behave. Somewhere we need to update the state once a second.

## Lifecycle Hooks
React gives us a number of other methods, other than render, that will all be called at certain times in a components life. For now, we are only interested in two of them: `getInitialState` and `componentDidMount`. Don't worry about the slightly weird names, they'll make sense eventually.

Let's add the `getIntitialState` method to our timer component.

{% highlight javascript %}
var Timer = React.createClass({
  getInitialState: function() {
    return {
      time: 0
    };
  },
  render: function() {
    return (
      <button>{this.state.time} s</button>
    );
  }
});
{% endhighlight %}

This method simply returns whatever `this.state` should be set to when we render the component for the first time. Our timer is going to count up from 0, so setting `this.state.time` to `0` is what we want to do here. Fairly straightforward.

Next we want to use `setInterval` to update the `this.state.time` each second. We only want to start updating our state when we know our component has successfully ended up in the DOM. React lets us know when this has happened by calling the `componentDidMount` method.

Add the following `componentDidMount` method to the Timer component.

{% highlight javascript %}
var Timer = React.createClass({
  getInitialState: function() {
    return {
      time: 0
    };
  },
  componentDidMount: function() {
    setInterval(function() {
      this.setState({ time: this.state.time + 1 });
    }.bind(this), 1000);
  },
  render: function() {
    return (
      <button>{this.state.time} s</button>
    );
  }
});
{% endhighlight %}

_Don't worry about the `.bind(this)`, that's a quirk of [the way `setInterval` handles this][1], not something that's related to React._

Now we're starting to get something that looks and feels like a proper component. It's important to notice that we didn't do

{% highlight javascript %}
setInterval(function() {
  this.state.time += 1;
}, 1000);
{% endhighlight %}

__It's essential that we call `this.setState` because that lets React know that it needs to re-render the component.__

Open the app in the browser and you should see the time counting up.

Finally, we want to reset the timer if the button is clicked. This involves setting `this.state.time` back to 0 and we need it to happen whenever the button is clicked.

React lets us define our own custom methods for our components. Let's create one called `resetTimer`.

{% highlight javascript %}
var Timer = React.createClass({
  resetTimer: function() {
    this.setState({ time: 0 });
  },
  getInitialState: function() {
    return {
      time: 0
    };
  },
  componentDidMount: function() {
    setInterval(function() {
      this.setState({ time: this.state.time + 1 });
    }.bind(this), 1000);
  },
  render: function() {
    return (
      <button>{this.state.time} s</button>
    );
  }
});
{% endhighlight %}

Finally, we need to make sure it actually gets called. We can add event listeners to our React components in a similar way to inline HTML elements event listeners.

{% highlight javascript %}
return (
  <button onClick={this.resetTimer}>{this.state.time} s</button>
);
{% endhighlight %}

For a long time, the teaching has been that we should keep our event code free from our HTML, because it becomes difficult to work out what is calling what and where events are being fired from.

However, React components force all the behaviour to be closely tied together in small, reusable components. It's always easy to find the function that an `onClick` handler points to, _because it also lives within the component!_.

With the event handler added, the button should count up and reset when you click it.

Make sure it works, then [it's time for the next step.](ex6).

[1]: http://stackoverflow.com/questions/2749244/javascript-setinterval-and-this-solution Stack Overflow setInterval

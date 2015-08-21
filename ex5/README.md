# Exercise 5
_Component State_.

## State vs Props
Components have two associated bits of data. The first is props, which you've already seen. Props are passed down from the parent and you can't change them inside our component. They should be considered immutable.

The other important bit of data our components know about it their own state. Unlike props, you can change the state of the component and each time you do, the component will be rendered again to show the changes.

Let's design a new component. It will be a timer that counts up from 0 in seconds and resets when you click it. Now we could pass the time in as a property from outside the timer, but it makes a lot more sense for the timer to manage that itself.

```js
var Timer = React.createClass({
  render: function() {
    return (
      <button>{this.state.time} s</button>
    );
  }
});
```

As you can see, when we are telling a component how it should render, we use `state` in a very similar way to `props`. Now that we've told our timer how to render, we need to tell it how to behave. Somewhere we need to update the state once a second.

React gives us a number of other methods, other than render, that will all be called when certain things happen. We are interested in two of them: `getInitialState` and `componentDidMount`. Don't worry about the slight weird names. They will make sense eventually.

Let's add the `getIntitialState` method to our timer component.

```js
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
```

This method simply returns what `this.state` should be set to when we render the component for the first time. Our timer is going to count up from 0, so setting `this.state.time` to `0` is what we want to do here. Fairly straightforward.

Next we want to use `setInterval` to update the `this.state.time` each second. We only want to start updating our state when we know our component has successfully ended up in the DOM. React lets us know when this has happened by telling us that our component _mounted_. Time to add another method to our component.

```js
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
```

Now we're starting to get something that looks and feels like a proper component. It's important to notice that we didn't do

```js
setInterval(function() {
  this.state.time += 1;
}, 1000);
```

It's __essential__ that we call `this.setState` because that lets React know that we are changing the state and it needs to re-render.

Open the app in the browser and you should see the time counting up.

Finally, we want to reset the timer if the button is clicked. This basically involves setting `this.state.time` back to 0 and we need it to happen whenever the button is clicked.

React lets us define our own custom methods for our components. Let's create one called `resetTimer`.

```js
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
```

Finally, we need to make sure it actually gets called. We can add event listeners to our React components in a similar way we can with HTML elements.

```js
return (
  <button onClick={this.resetTimer}>{this.state.time} s</button>
);
```

For a long time, the teaching has been that we should keep our event code free from our HTML, because it becomes difficult to work out what is calling what and where events are being fired from.

However, React components force all the behaviour to be closely tied together in small, reusable components. It's always easy to find the function that a onClick handler points to, _because it also lives within the component!_.

With the event handler added, the button should count up and reset when you click it.

Now, it's time for the next step.


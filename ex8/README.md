# Exercise 8
_Reusable Components_

We're going to look at using a set of components to build a comment form. We'll store the comments using local storage and use avatars from [adorable.io](http://avatars.adorable.io/).

Create a new React application, include React, the JSX Transfomer and your own `app.js`.

Lets start by defining our components. It can be helpful to try and work out what the components should look like before we try to implement them.

## Avatar
```js
<Avatar name={name} size={50}/>
```

We'll generate a random avatar based on the name of the commenter, by requesting an image from the url `http://api.adorable.io/avatars/:size/:name.png`.

```js
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
```

We use the `getDefaultProps` to specify some default properties just in case none are passed to the component. We also create a render function which renders a single `img` tag with some attributes.

The only different thing we've done here is the creation of the `createUrl` helper method. We could have put this in the render method, but it's cleaner to move it out into a standalone function. This approach means that other parts of the component could also use it too.

Render an avatar component with some example props to check that it works.

```js
React.render(
  <Avatar name='Test' size={100}/>,
  document.body
);
```

## Comment
```js
<Comment name={name} body={body} />
```

The next component we'll need to define is a comment. A comment should show the users avatar, the name of the user and the actual body of the comment itself.

```js
var Comment = React.createClass({
  render: function() {
    return (
      <div className='comment'>
        <Avatar name={this.props.name} size={48}/>
        <strong className='name'>{this.props.name}</strong>
        <p className='body'>{this.props.body}</p>
      </div>
    );
  }
});
```

Nothing strange happening here. We pass in name and body as props, then we pass name down to avatar and render an avatar, a name and the comment itself.

Test it out to make sure it works.

## Comment List
```js
<CommentList comments={comments} />
```

This component takes an array of comments as a prop and renders them all. This is the first time we've come across rendering lists in React.

```js
var CommentList = React.createClass({
  getDefaultProps: function() {
    return {
      comments: []
    };
  },
  render: function() {
    var comments = this.props.comments.map(function(comment) {
      return <Comment name={comment.name} body={comment.body}/>;
    });
    return (
      <div>
        {comments}
      </div>
    );
  }
});
```

Our render method looks quite different to normal. This is because we have a list of comments that look like this:

```js
[
  { name: 'Tom', body: 'Hello Dick!' },
  { name: 'Dick', body: 'Hello Harry!' },
  { name: 'Harry', body: 'Hello Tom!' }
]
```

And what we need to return from our render function is React components. We can transform the above list into React components using the map method.

```js
this.props.comments.map(function(comment) {
  return <Comment name={comment.name} body={comment.body}/>
});
```

Which will turn the array above into an array that looks like this.

```js
[
  <Comment name='Tom' body='Hello Dick!'/>,
  <Comment name='Dick' body='Hello Harry!'/>,
  <Comment name='Harry' body='Hello Tom!'/>
]
```

Which when you take away the JSX sugar, is just 3 calls to `React.createElement`, which the render method understands.

Try the comment list out with a set of example comments.

```js
var App = React.createClass({
  render: function() {
    var comments = [
      { name: 'Tom', body: 'Hello Dick!' },
      { name: 'Dick', body: 'Hello Harry!' },
      { name: 'Harry', body: 'Hello Tom!' }
    ];
    return (
      <CommentList comments={comments}/>
    );
  }
});
```

## Comment Form
```js
var CommentForm = React.createClass({
  ENTER_KEY: 13,
  getDefaultProps: function() {
    return {
      name: prompt('What is your name?')
    };
  },
  getInitialState: function() {
    return {
      comments: []
    };
  },
  submitComment: function(body) {
    var comment = {
      name: this.props.name,
      body: body
    };

    var comments = this.state.comments.concat([comment]);

    this.setState({
      comments: comments
    });
  },
  handleKey: function(event) {
    if(event.which === this.ENTER_KEY) {
      this.submitComment(event.target.value);
      // reset value once we submit the comment
      event.target.value = '';
    }
  },
  render: function() {
    return (
      <div className='comment-form'>
        <CommentList comments={this.state.comments}/>
        <input type='text' onKeyDown={this.handleKey}/>
      </div>
    );
  }
});
```

This is the largest of our components and it's also where most of the work happens. You might have noticed already, but none of our other components use `this.state`. This is generally a good thing. The less responsibility each component has, the easier it will be to reuse and improve.

Our CommentForm component needs to handle the storage of our comments, getting the name of the user, and adding new comments to the list.

We use the `onKeyDown` event handler and check whether the key that was pressed was 13 (the enter key). If it was, we add the comment to the list (using `this.setState`) and finally we reset the input.

There's one final thing to add, lets make the component load any existing comments from localStorage and save them there before it unmounts.

We'll need the following handler for our CommentForm component.

```js
componentWillMount: function() {
  var json = localStorage.getItem('comments-json');

  if(typeof json === string) {
    this.setState({
      comments: JSON.parse(json)
    });
  }
}
```

Then we need to revise our submitComment method to save the comment in localStorage.

```js
var json = JSON.stringify(comments);
localStorage.setItem('comments-json', json);
```

Finally, you should be able to bring the whole lot together and render a `<CommentForm />`.

```js
var App = React.createClass({
  render: function() {
    return (
      <CommentForm />
    );
  }
});
```

By default it has no styles and using `prompt()` to get the name is never a great option, but you get the idea.


---
layout: post
title: React for absolute beginners
category: React
date: April 2, 2016
hashtags: reactjs,javascript
---

React JS is a powerful yet simple library. But when I talk to the guys just getting started with React, I often get a feeling that they have a lot of misconceptions and confusions about it. I don't blame them– React brings many radical new ideas with it.

Virtual DOM, JSX, One-way data flow all seem to be either too complicated, alien or counter-intuitive initially. But that's mostly subject to the way you begin learning React.

I feel that learning it the right way, learning its concepts in the correct order, is the key here. You would appreciate why it needs a Virtual DOM, what is does with it, and why DOM diffing is needed once you understand how it works.

In this article, we will try to understand the React way of doing things starting with bare basics. And building up to the point where we can appreciate why React does what it does.

### React is just a view library

Never ever forget that it is just a view library. It is *only* the "V" in the "MVC". Treat it like that. You create UI elements with it and they are called components.

Let us take a basic example of how a component would look like. Say a Logo component.

```html
<div>
  <img src="/images/logo.svg"/>
</div>
```

You'd say, "Hey! That's just HTML". Yep, that's how our logo markup would look if I were to write it in HTML. Now let's make it a React component.

```js
var Logo = React.createClass({
  render: function() {
    return (
      <div>
        <img src="/images/logo.svg"/>
      </div>
    );
  }
});
```

There you have it, your first React component. Now there are a few constructs worth explaining here:

1. You use `createClass` to create components (there are a few other ways like pure functions and ES6 classes). It takes an object.
2. `render` is the most important method and the one you will use always. It should return the markup for your component.
3. What's the HTML doing in JavaScript? Remember that it is *not* HTML, it is JSX. It just looks like HTML. It is usually converted into simple JavaScript calls by something like Babel. You could write the above in simple JavaScript, but JSX is cleaner and recommended.

This is how it would look in JavaScript:

```js
var Logo = React.createClass({
  render: function() {
    return (
      React.createElement('div', {},
        React.createElement('img', {
          src: '/images/logo.svg'
        })
      )
    );
  }
});
```

Not too shabby isn't it. I am sure you can see the correlation. I recommend beginners to try writing a few components without JSX. That way, you would really appreciate how much JSX does for you and even write better JSX as a result.

### How to use components

So we our component, how do we use it on a page. Okay let's do it.

Let's setup a testing page. I would never recommend using it in prodcution, but it serves well as a playground to quickly jot down components. This is the `index.html`.

```html
<!DOCTYPE html>
<html>
<head>
  <title>React Playground</title>
</head>
<body>
  <!-- application container -->
  <div id="root"></div>

  <script src="https://fb.me/react-0.14.8.js"></script>
  <script src="https://fb.me/react-dom-0.14.8.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>

  <script type="text/babel" src="app.js"></script>
</body>
</html>
```

So we include the necessary React libraries and an `app.js`. That's where we put our JavaScript and components. So create it as well. The `browser.js` from `babel-core` is used to support JSX syntax at runtime in the browser. Again, I recommend not to use it on production apps *ever*.

One important piece of the HTML is the `div` with ID `root`. We will use this element to plug in our component. Sure, we could have used `body` itself, but never do that in practice. What if some other script or a browser extension tries to append to your body? That will surely screw up your application.

Now let's focus on the `app.js`. Here is how it looks:

```js
var Logo = React.createClass({
  render: function() {
    return (
      <div>
        <img src="https://facebook.github.io/react/img/logo.svg"/>
      </div>
    );
  }
});

ReactDOM.render(
  <Logo/>,
  document.getElementById('root')
);
```

You're already familiar with the first half of the code. But the second half is what's going to make our Logo component show up on the page. So, save this file and load up the page in your browser. You should be greeted with the React logo in all its glory. A job well done compadre. Let's move on.

### Props: Making customisable components

Okay, so our logo is visible and looks good. How about making it possible to specify a size for it? We do it using `props`. We can pass props to a React component and alter its markup based on the values of those props.

Let's add two attributes to our Logo component, `width` and `height` with obvious purposes.

```js
var Logo = React.createClass({
  render: function() {
    return (
      <div>
        <img
          src="https://facebook.github.io/react/img/logo.svg"
          width={this.props.width}
          height={this.props.height}/>
      </div>
    );
  }
});

ReactDOM.render(
  <Logo width={100} height={100}/>,
  document.getElementById('root')
);
```

Pretty simple eh? Some new concepts here:

1. You can access props using `this.props.propName`.
2. `{}` are necessary if you are passing something other than a string to attributes/props.

Now we have made our Logo component customizable. Cool.

### But what about dynamic components?

You must be thinking the same, isn't it? Worry not, we'll talk about the most important class of components now– components that change over time.

But before we see an example, we must understand one important design choice made by the React engineers. In React, the data flow is one-way, i.e. from parent to child using `props`. There is no two-way data binding. Trust me, this makes components:

- much more predictable
- easier to reason about
- easier to debug
- performant

Okay so with that fact out of the way, we can now see an example. A rule of thumb, if the component you want to develop has data that changes over time– e.g. a clock component, or a tab component, then you could use `state` to model those changes in React components. Let's see with an example.

```js
var Stepper = React.createClass({
  getInitialState: function() {
    return {
      currentValue: 0
    };
  },

  increment: function() {
    this.setState({
      currentValue: this.state.currentValue + 1
    });
  },

  decrement: function() {
    this.setState({
      currentValue: this.state.currentValue - 1
    });
  },

  render: function() {
    return (
      <div>
        <button onClick={this.increment}>+</button>
        <span>{this.state.currentValue}</span>
        <button onClick={this.decrement}>-</button>
      </div>
    );
  }
});

ReactDOM.render(
  <Stepper/>,
  document.getElementById('root')
);
```

A lot of new constructs here. Nothing too complicated going on, though. We have written a simple stepper component with two buttons– one to increment and another to decrement a value. Now here, the value is what changes over time, so that's our state. Now let's understand the new constructs:

1. `getInitialState` is used to provide an initial state to the component. In our case, the initial value. We have set it to `0`.
2. `onClick` is used to provide a method to handle the click event.
3. `setState` is used to update the state. You cannot assign to `this.state.currentValue`. That wouldn't update our component. Always call `setState` to change the state. Consider `this.state.anyThing` to be just read-only.

Now you know how to write dynamic components. We didn't need any two-way binding to make it work. We did it with more explicit code. And we can easily tell how and when the state changes, just look for the `setState()` calls!

### So why Virtual DOM and Diffing?

Okay, so the moment of truth. You might want to think about what happens exactly when you change state using `setState`. You say that the counter increments or decrements. Well in reality, React changes the state and re-renders the whole component! Yes, it just goes about re-rendering it without doing anything else.

Wouldn't that be dumb and really non-performant? Yes, it would be, but in reailty no it isn't. The answer is Virtual DOM. Virtual DOM is an equivalent represenation of the DOM subtree rendered in the browser. Virtual DOM is just a JavaScript object. Whenever the props or state of a component changes, React rebuilds the virtual DOM and compares it to the old one. If they are not the same it calculates a minimal number of DOM operations required to make old one same as the new one.

Doing all of this on actual DOM would be slow as hell. But JavaScript is fast. This Virtual representation helps React build and diff the virtual DOMs quickly. You see now? Virtual DOM is just a side effect, an implementation requirement so as to accumulate for the design choice of simply re-rendering components when props or state changes. And performance is just a result of using Virtual DOM and diffing.

![Virtual DOM Diffing]({{ site.baseurl }}/images/articles/virtual-dom-diffing.svg)

In our `Stepper` example, the `span` holding the `currentValue` is the one that keeps on changing. On each increment or decrement, React just does a `setTextContent` operation on that `span`.

### But why just re-render like a dumbo?

Well because it's fucking simple. It just renders the output based on the state and props. Your components remain predictable because nothing else besides props and state could affect the output. It provides you with a simple mental model. You tell React how the component should look based on it pros and state and React manages the rest to make it possible.

The only thing React needs to manage well is the way it comes up with the minimal number of DOM operations needed to update a component. And I think the React engineers have done an amazing job with it.

### Fin.

To end I would like to advice you to remember that React is not about Virtual DOM or DOM diffing, but it is about the way it makes you think about apps as you write them. You will think of keeping them simple, composable and reusable. We will see these in action soon in a follow-up article about the philosohpy behind React.

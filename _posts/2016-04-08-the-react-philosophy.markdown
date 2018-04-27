---
layout: post
title: The React Philosophy
category: React
date: April 8, 2016
hashtags: reactjs,javascript
---

It's hard to imagine a conversation about React that would not involve terms like Virtual DOM, JSX aka inline HTML, and Server side rendering. I've seen people use these terms to differentiate React apart from other JS libraries or frameworks. But after using React for almost a year now, I can say with confidence that these aren't what make React so special.

The power of React lies in a few bland and old ideas:

- Simplicity
- Modularity
- Composition

And in some interesting design choices:

- One-way data flow
- Explicit mutation
- Static mental model

### Like Unix, like React

If you're familiar with the Unix philosophy, the first set of ideas would already sound familiar to you. The idea of simplicity, modularity, and composition is so old yet so powerful that it never goes out of fashion.

In Unix you write simple utilities and pipe them together to do more powerful things. And that's what React does too– it makes you write simple, focused components. It encourages you to keep them modular. And the result of doing so is that your components are reusable. You can now compose components to create even more powerful components.

Writing apps with large HTML templates and wiring them to the JS code via hooks (ViewModels or Controllers) feels inferior and dirty compared to the declarative way of writing components. React's declarative syntax naturally forces you to write more concise and reusable chunks of UIs and club them together to form your app. And due to all this, testability of these components naturally falls out.

### React's component model

Before diving into the other set of ideas– "The design choices", let's get one thing out of the way. A lot of people dislike JSX. Usually the argument against it is that it violates separation of concerns. It mixes HTML with JS. If technologies are your "concern" then sure it does violate. But if your app is your concern (which I hope is), then React enables you separate those concerns in the best way.

Don't get bogged down with separation of technologies, separating HTML from JS sure does make reusing the HTML easier. But once you decide to reuse it in another place, you increase the chances of bugs creeping in to your app. As now you have two parts of code to worry about and keep in sync with the HTML. Look at what technology separation got you.

React allows you to encapsulate all the technical pieces of a component in one place: Markup (JSX), behaviour (JS), and even the presentation (CSS). This is a true representation of a component. The component as a whole is now reusable rather than just a technical part of it.

Okay enough philosohpical talk, let's get down to design choices now (not to say that above wasn't one).

### Design choices

React didn't go with the Two-way data binding approach sold as a forefront feature by many of the frameworks today. Instead it embraced a simple one-way flow– Data flows down and events flow up the hierarchy. You would ask, "Why? Doesn't two-way data binding allow you to get rid of a lot of boilderplate?". Yes, it sure does. But it also makes data flow in your app complex and unpredictable.

React makes this data flow explicit. You are in control of the flow. This makes it easy to understand how your program works. The behaviour of components is more predictable. The only downside is it requires a little more typing than traditional two-way data binding.

Parent communicates with children using props, and children communicate with parents using callbacks. Callbacks are usually the places where you will mutate your state using `setState`. This keeps all the mutations explicit.

These design choices make for another excellent proprty of React components. They are idempotent:

> "They describe your UI at any point in time, just like a server-rendered app." **–Pete Hunt**

Components map the current state to DOM. And whenever the state changes, React (kinda) dumps the old DOM and just renders the component again. Similar to how you would refresh a page to update it. Dont 'worry, it remains performant thanks to Virtual DOM.

### Fin.

When React made first public appearance, it created a stir with the radical changes it brought. A lot of people steered away, including me. But as I wrote more and more React apps I realised that those radical ideas were just some implementation choices. The power of React lies in the way it makes you think to write app in a modular, predictable way. More often, the code itself tells me when I am doing something wrong or need to refactor it.

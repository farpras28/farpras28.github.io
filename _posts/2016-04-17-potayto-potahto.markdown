---
layout: post
title: Potayto, Potahto
category: Opinion
date: April 17, 2016
hashtags: reactjs,javascript
---

So these days I am mentoring my team on React. As an exercise I asked them to write a rating widget. And I was blown seeing the different approaches they took. Almost each implementation was different.

This was the expected way the widget should work. Nothing special, usual rating widget you'd see anywhere.

![Rating Widget]({{ site.baseurl }}/images/articles/rating-widget.gif)

So let's get digging into the implementations shall we?

### Implementation #1

<iframe width="100%" height="300" src="//jsfiddle.net/nayaabkhan/65e1vmgu/3/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

So, this is a very straighforward implementation. A state `current` to track the currently set rating and another `supposed` to track the rating when user is changing it. The `rating` state is used to map over each star. An unnecessary one. Could have just used a `for` loop instead. `handleSetRating` sets the `current` state when a star is clicked. `handleSupposeRating` changes the `supposed` as the user hovers around the stars. `handleRevertRating` resets the `supposed` to the last set `current` values to revert when the user just aborts hovering over the widget. Pretty simple.

All the heavy lifting is done by the `RatingList` component. While the `RatingWidget` acts more like a "Container" component to the `RatingList`. A very straight-forward implementation. But it could be improved.


### Implementation #2

<iframe width="100%" height="300" src="//jsfiddle.net/nayaabkhan/etjortLt/6/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This one has 3 set of components. It begins with a simple `Star` component that is either active or inactive depending on its `isActive` property. It is a pure components, very nice.

The `RatingWidget` is also a pure components and it just displays the ratings based on the `size` and the `rating` props. Has a very simple implementation again. It uses the `Star` component to simply a lot of the output. It also gets 3 callbacks, one for each:

1. `onRatingChange`: When the user clicks a star to pick a rating
2. `onChangeBegin`: When the user hovers over a star in the widget
3. `onChangeAbort`: When the user hovers out of a star

The `RatingWidgetContainer` is the "Container" for the `RatingWidget`. It has an internal UI state to keep a track of the hovered star. The `handleChangeBegin` and the `handleChangeAbort` are passed as the respective callbacks to the `RatingWidget`.

This implementation is better than the first one. Refactors components well and keeps stuff simple too.


### Implementation #3

<iframe width="100%" height="300" src="//jsfiddle.net/nayaabkhan/ckv51k1j/1/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This one is the most interesting one. There is just one component and it just renders the stars depending on the current rating. So nothing special here. But how does it do the hovering changes? It kind of hands of this task to CSS. The `#rating .star:hover ~ .star` and `#rating:hover .star` are of interest to us.

The `#rating .star:hover ~ .star` uses the general sibling selector to render all the following stars grey. So pretty smart here. But we also want to display all the other stars golden. So it uses the `#rating:hover .star` to accomplish it. Very cool indeed.

This is by far the best implementation. The ony thing is you need to be careful with the styles. A lot depends on the way the markup is laid out and stlyed. For instance, the `#rating` cannot be a block level element or if you hover on it where there are no stars, all the stars will get active. Also, the specificity of the `#rating .star:hover ~ .star` needs to be higher than `#rating:hover .star` or it breaks too. In short, the JS gets simpler, but the CSS get a beating.

### Fin.

It was interesting to see and critique the different implementations when I was them first. Also I am pretty sure there could be some more interesting ways to implement it. Also, I might have missed some points here. Feel free to post your thoughts below in the comments or propse an even better solution. _Cheers!_

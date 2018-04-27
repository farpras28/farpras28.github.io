---
layout: post
title: Unit Testing React Components
category: React
date: April 30, 2016
hashtags: reactjs,unittesting,javascript
---

React components are modular and composable. And Unit tests exist to tests software modules. Hence it makes React components a perfect candidate for Unit tests.


### Why Unit test?

I am a huge Unit testing advocate. Not to say other tests are not important. But Unit tests are something that every software must have. Having Unit tests not only ensure that your modules are well tested but it also acts as a litmus test to keep your software modularity in check. If while writing a Unit test for a module feels unnatural, it pretty much means that the module isn't "modular" enough and you should fix it.

When it comes to React testing, I always begin with Unit testing them and later adding tests for callbacks. I use Shallow rendering to assert comparisons of components in my Unit tests.

### What is Shallow rendering?

Shallow rendering of a component renders the component only one level deep. It doesn't render out child components. Testing components using Shallow rendering enforces you to keep your component hierachy flat and as a result, more modular.

So, if you have a component hierachy like this:

```js
const Label = ({ text }) => <label>{text}</label>;
const SubmitButton = ({ text }) => <button><Label text={text} /></button>;
```

Shallow rendering the `<SubmitButton text="Hello" />` would produce:

```js
<button><Label text="Hello!" /></button>
```

This allows your assertions to be simple and also encourages you to refactor you components into subcomponents once you feel testing a components is complex.


### What tools are we going to use?

#### Tape and Faucet

Firstly, we would need a testing framework. The most popular are Mocha, Jasmine, Jest and several others. I believe they do a lot more than what is needed, and the more tools you've got the more you'd keep thinking on how to use each one of them, even if not needed. I like my tools simple and pluggable.

I use [Tape](https://github.com/substack/tape) as my testing framework. It doesn't spoil me with too much of features so I could focus well on my tests. And the tests are fucking simple to read and understand.

You want to see an example of a test with Tape?

```js
import test from 'tape';
import { leftPad } from './utils';

test('leftPad output', (assert) => {
  const message = 'String is left padded correctly.';
  const expected = '   foo';
  const actual = leftPad('foo', 3);

  assert.equal(actual, expected, message);
  assert.end();
});
```

We shall also use [Faucet](https://github.com/substack/faucet) to prettify the output Tape produces, which is in [TAP format](https://en.wikipedia.org/wiki/Test_Anything_Protocol).

#### Test Utils

Another package we would use is [`react-addons-test-utils`](https://facebook.github.io/react/docs/test-utils.html). So far these have kept me satisfied and I haven't felt the need to use something more sophisticated like Enzyme. Though, if I change my mind in future I'll definitely do a writeup about it.

Again, the reason to use `react-addons-test-utils` is keeping my toolset simple. `react-addons-test-utils` provides ample tools you could use to test your React components. It provides a simple utility to Shallow render your component besides other utilities.

#### Extending Tape for JSX

We would also need `extend-tape` to extend the assertion API of Tape with `tape-jsx-equals`. This gives us a function to compare JSX outputs as we will see in a bit.


### The candidates we are going to test

We are going to test the `Star` and `RatingWidget` components from one of my [previous articles](http://nayaabkhan.me/opinion/potayto-potahto).

```js
// star.js
import React from 'react';

const Star = ({ isActive }) => {
  const classNames = ['star'];
  const activeClass = 'star--active';

  if (isActive) {
    classNames.push(activeClass);
  }

  return <span className={classNames.join(' ')}>★</span>;
};

export default Star;
```

```js
// rating-widget.js
import React from 'react';
import Star from './star';

const RatingWidget = ({ size, rating }) => {
  var starItems = [];
  for (let i = 0; i < size; i++) {
    starItems.push(
      <li key={i} className="star-item">
        <Star isActive={i < rating}/>
      </li>
    );
  }

  return (
    <ul className="rating-widget">{starItems}</ul>
  );
};

export default RatingWidget;
```


### Setting up

Let's begin with installing the necessary packages first.

```sh
$ npm init -y
$ npm install react --save
$ npm install babel-cli babel-preset-react babel-preset-es2015 tape faucet extend-tape tape-jsx-equals react-addons-test-utils --save-dev
```

Also create the two components we are testing in an `app` folder. Create a `test` folder in which we shall put our test files.

You might have noticed we are installing `babel-cli` and a couple of presets for supporting React and ES2015 syntax. To enable the support let's create a `.babelrc` file:

```json
{
  "presets": ["es2015", "react"]
}
```

Now are ready to write some tests.


### The Tests

Always begin writing tests for the components lower-most in the hierarchy. For us it is the `Star` component. So lets write a test to assert its output:

```js
// tests/star.test.js
import tape from 'tape';
import addAssertions from 'extend-tape';
import jsxEquals from 'tape-jsx-equals';
import React from 'react';
import { createRenderer } from 'react-addons-test-utils';

import Star from '../app/star';

const test = addAssertions(tape, { jsxEquals });
const renderer = createRenderer();


// case 1 when it is set as active
test('Testing Star component output if isActive set', (assert) => {
  renderer.render(<Star isActive={true} />);

  const message = 'Should be active';
  const expected = <span className='star star--active'>★</span>;
  const actual = renderer.getRenderOutput();

  assert.jsxEquals(actual, expected, message);
  assert.end();
});
```

I like to follow this template in every unit test I write. And I think you should follow it too. It makes the tests very readable and structured.

```js
test('Module aspect that I am testing', (assert) => {
  const message = 'Assertion message with expectation';
  const expected = 'expected value';
  const actual = 'expected' + ' ' + 'value';

  assert.equals(actual, expected, message);
  assert.end();
});
```

Besides the testing code there is not much boilerplate, except these two lines that add `jsxEquals` to the assertion API.

```js
const test = addAssertions(tape, { jsxEquals });
const renderer = createRenderer();
```

### Running the tests

Obviously we are not done with testing every aspect of our components. But let's first see how to run our test. Open up your `package.json` and find the `scripts` key. There should be a single script called `test` in it already. Right now it doesn't do anything useful but failing. Let's change it:

```json
{
  "test": "babel-node node_modules/.bin/tape tests/*.test.js | faucet"
}
```

We are using `babel-node` installed via the `babel-cli` to run our JavaScript files. We pass the `tape` script and our tests to it. Piping the output to `faucet` to make it more readable.

Okay, so now run `npm test` to run the tests and you should see something like this:

```text
> react-unit-testing@1.0.0 test /Users/nayaabkhan/Desktop/react-unit-testing
> babel-node node_modules/.bin/tape tests/*.test.js | faucet

✓ Testing Star component output if isActive set
# tests 1
# pass  1
✓ ok
```

Cool, our test is running fine and it is passing. Let's finish adding a few more test cases to the `star.test.js`.

```js
// case 2 when it is set as inactive
test('Testing Star component output if isActive not set', (assert) => {
  renderer.render(<Star isActive={false} />);

  const message = 'Should be inactive';
  const expected = <span className='star'>★</span>;
  const actual = renderer.getRenderOutput();

  assert.jsxEquals(actual, expected, message);
  assert.end();
});

// case 3 when isActive not specified
test('Testing Star component output if isActive not specified', (assert) => {
  renderer.render(<Star />);

  const message = 'Should be inactive';
  const expected = <span className='star'>★</span>;
  const actual = renderer.getRenderOutput();

  assert.jsxEquals(actual, expected, message);
  assert.end();
});
```

Let's also add a test for `RatingWidget` now.

```js
// tests/rating-widget.test.js
import tape from 'tape';
import addAssertions from 'extend-tape';
import jsxEquals from 'tape-jsx-equals';
import React from 'react';
import { createRenderer } from 'react-addons-test-utils';

import RatingWidget from '../app/rating-widget';
import Star from '../app/star';

const test = addAssertions(tape, { jsxEquals });
const renderer = createRenderer();

test('Testing RatingWidget component output', (assert) => {
  renderer.render(<RatingWidget size="5" rating="2" />);

  const message = 'Should render correctly as per size and rating specified';
  const expected = (
    <ul className="rating-widget">
      <li key={0} className="star-item"><Star isActive={true}/></li>
      <li key={1} className="star-item"><Star isActive={true}/></li>
      <li key={2} className="star-item"><Star isActive={false}/></li>
      <li key={3} className="star-item"><Star isActive={false}/></li>
      <li key={4} className="star-item"><Star isActive={false}/></li>
    </ul>
  );
  const actual = renderer.getRenderOutput();

  assert.jsxEquals(actual, expected, message);
  assert.end();
});
```

If you run the tests now, you should see all of them passing with flying colors.

```text
> react-unit-testing@1.0.0 test /Users/nayaabkhan/Desktop/react-unit-testing
> babel-node node_modules/.bin/tape tests/*.test.js | faucet

✓ Testing RatingWidget component output
✓ Testing Star component output if isActive set
✓ Testing Star component output if isActive not set
✓ Testing Star component output if isActive not specified
# tests 4
# pass  4
✓ ok
```

That is all cool, but we haven't yet seen the most common and important part of running tests yet, failing. So let's say someone comes along and changes the `Star` component's active style from `star--active` to `star--is-active`, may be because it is more BEMMY™. The tests will fail with this report:

```text
> react-unit-testing@1.0.0 test /Users/nayaabkhan/Desktop/react-unit-testing
> babel-node node_modules/.bin/tape tests/*.test.js | faucet

✓ Testing RatingWidget component output
⨯ Testing Star component output if isActive set
  not ok 2 Should be active
    ---
      operator: equal
      expected: |-
        '<span className="star star--active">\n  ★\n</span>'
      actual: |-
        '<span className="star star--is-active">\n  ★\n</span>'
      at: Test.jsxEquals (/Users/nayaabkhan/Desktop/react-unit-testing/node_modules/tape-jsx-equals/dist/index.js:15:10)
    ...
✓ Testing Star component output if isActive not set
✓ Testing Star component output if isActive not specified
# tests 4
# pass  3
⨯ fail  1
```

The output is so readable and has ample details of what has gone wrong. It prints out the expected and actual rendered outputs and now we know that either the tests need an update or the change needs to be reverted.

### Fin.

The above testing setup is pretty much what I use for my projects. We could of course improve it by moving the boilerplate out to a separate module but I'll leave it as an exercise.

I hope I was able to convince you that writing Unit tests for your React components is important. It doesn't have to be difficult or complex. With the right tools it simple, fast and natural. Tape is an extremely minimal testing utility and coupled with React's Testing Utilities it provides enough tools to get you started writing Unit tests. Plug it into Faucet to produce good error reports.

Avoid using tools that are feature bloated and don't play well with others. Focus your time and resources on testing itself, not on the distractions around the toolset.

All the source code for this article could be found [here](https://github.com/nayaabkhan/react-unit-testing).

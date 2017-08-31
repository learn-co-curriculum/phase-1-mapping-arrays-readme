# JavaScript Map

## Objectives

1. Implement a `map()` function from scratch
2. Explain how `map()` differs from `reduce()`
3. Explain how `map()` builds on `reduce()`

## Introduction
In a lot of code you'll be writing as a soon-to-be JS expert, you'll be iterating over arrays. Sometimes, you'll want to apply a transformation to the elements in the array. We can do this using `for` loops, but writing all of that stuff gets tedious after a while. Let's create our own helper method called `map()` to make things a little easier!

## Map to the unknown
![Some crummy old map](https://i.ytimg.com/vi/gKVIWyj2QnU/maxresdefault.jpg)

To start things off, let's write a `map()` function that abstracts away the `for` loop:

```js
function map(collection) {
  for (let i = 0; i < collection.length; i++) {
    const element = collection[i];
    console.log(element);
  }
}
```

This will log all of the elements in the array. Not terribly interesting. Let's add a second argument so we can pass a function to our `map()` function. This function will receive the `element` and can then optionally transform it. We'll also need a new array to store our results in, so we can return the result when we're done:

```js
function map(collection, callback) {
  const result = [];

  for (let i = 0; i < collection.length; i++) {
    const element = collection[i];
    result.push(callback(element));
  }

  return result;
}
```

Sweet! That should work for now. Let's take this baby for a spin by doubling a list of numbers:

```js
const numbers = [1, 2, 3];
const doubledNumbers = map(numbers, function (number) {
 return number * 2;
});
console.log(doubledNumbers); // prints [2, 4, 6]
```

In case we ever need the index of the item or the full list of items in our callback function, let's add these as arguments to our callback. The callback doesn't **have** to use these values, but they're there if we ever need them.

```js
function map(collection, callback) {
  const result = [];

  for (let i = 0; i < collection.length; i++) {
    const element = collection[i];
    result.push(callback(element, i, collection));
  }

  return result;
}
```

## Autobots, roll out!
![That's not how this works. That's not how any of this works.](https://media.giphy.com/media/RjBKvVNcf4xH2/giphy.gif)

Let's use our `map()` function on a trickier data structure — a list of Autobots. To start things off, we have an array of Autobots. Now, let's transform all of them to their robotic form! A transformed Autobot needs to be marked as such using the `isTransformed` boolean, as well as have its strength doubled:

```js
const autobots = [
  { name: 'Optimus Prime', strength: 5, isTransformed: false, },
  { name: 'Ironhide', strength: 3, isTransformed: false, },
  { name: 'Bumblebee', strength: 2.5, isTransformed: false, },
  { name: 'Ratchet', strength: 1.5, isTransformed: false, },
];

const transformedAutobots = map(autobots, function (autobot) {
  return Object.assign({}, autobot, {
    strength: autobot.strength * 2,
    isTransformed: true,
  });
});

console.log(transformedAutobots);
/*
 Result:

 [
   { name: 'Optimus Prime', strength: 10, isTransformed: true },
   { name: 'Ironhide', strength: 6, isTransformed: true },
   { name: 'Bumblebee', strength: 5, isTransformed: true },
   { name: 'Ratchet', strength: 3, isTransformed: true }
 ]
*/
```

We're using `Object.assign()` here to defensively copy the object and change its values. If we didn't, the objects in the original array would get modified too. Defensive copying is important to keep in mind — modifying values all over our code is often the biggest source of bugs.

## Seeing the light
![Praise the JS gods.](https://media.giphy.com/media/kkpWcU9XgFIUE/giphy.gif)

Time for a confession. We basically just implemented something that is already part of the JS standard library. Sisyphus has nothing on us! To map elements in an array, we can simply use `Array.prototype.map()`.

Much like our own `map()` function, `Array.prototype.map()` is an array method that iterates over all elements, allowing you to apply a function to each element in that array, effectively transforming them into something else. The result is then returned as a *new* array, leaving the original array intact and unmodified (but remember, **not** the elements we modify, necessitating the need for defensive copying). That last part is super important, because it either saves us from having to create a new array ourselves and copy stuff in there, **or** modifying the original elements in the array — much like what we did in our own `map()` function.

Just so you believe I'm not pulling your leg, let's see what it looks like:

```js
const transformedAutobotsWithMap = autobots.map(function (autobot) {
  return Object.assign({}, autobot, {
    strength: autobot.strength * 2,
    isTransformed: true,
  });
});

console.log(transformedAutobotsWithMap);
```

In this code snippet, we're using the native `.map()` function that is a property of `Array`'s prototype. It gives us the exact same result! Now that we know how map is implemented, it holds no more secrets for us! We can discard our own `map()` function and just use the `.map()` property on arrays. Sweet!

## Resources

- [MDN: Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

<p class='util--hide'>View <a href='https://learn.co/lessons/javascript-map'>Map</a> on Learn.co and start learning to code for free.</p>

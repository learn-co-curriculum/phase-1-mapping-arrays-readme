# Mapping Arrays

## Learning Goals

- Implement a `map()` function from scratch
- Demonstrate using `Array.prototype.map()` instead

## Introduction

| Use Case                                                       | Method                |
| -------------------------------------------------------------- | --------------------- |
| Executing a provided callback on each element                  | `forEach()`           |
| Finding a single element that meets a condition                | `find()`, `indexOf()` |
| Finding and returning a list of elements that meet a condition | `filter()`            |
| ** Modifying each element and returning the modified array     | `map()`               |
| Creating a summary or aggregation of values in an array        | `reduce()`            |

In the previous lesson, we learned about `filter()`, a built-in array method
that searches through a collection, passes each element to a provided callback
function, and returns an entirely new array comprised of elements for which the
callback returned a truthy value.

Another very common built-in array method is `map()`, which transforms every
element in an array to another value. For example, it can be used to square
every value in an array of numbers: `[1, 2, 3]` -> `[1, 4, 9]`. Like
`forEach()`, `find()` and `filter()`, `map()` accepts a callback function, and
passes each element in turn to the callback:

```js
[1, 2, 3].map(function (num) {
  return num * num;
});
// => [1, 4, 9]
```

While both `filter()` and `map()` return a new array, `filter()` returns a
subset of the original array (unless all elements meet the provided condition)
in which the elements are unchanged. `map()`, on the other hand, returns a new
array that's the same length as the original array in which the elements have
been modified.

Let's quickly run through how we could create our own version of the `map()`
method.

## Implementing `map()` From Scratch

### Abstracting the iteration

Right off the bat, we know that our function needs to accept the array from
which we'd like to _map_ values as an argument:

```js
function map(array) {
  // Map magic to follow shortly
}
```

Inside the function, we need to iterate over each element in the passed-in
array, so let's fall back on our trusty `for...of` statement:

```js
function map(array) {
  for (const element of array) {
    // Do something to each element
  }
}
```

### Callback city

As we've discussed, `map()` is used to transform each element of an array and
return a new array containing the transformed values. However, for code
organization and reusability it's best to keep the logic of the transformation
decoupled from the `map()` function itself. `map()` should really only be
concerned with iterating over the collection and passing each element to a
callback that will handle the transformations. Let's set up `map()` to take that
callback function as its second argument:

```js
function map(array, callback) {
  for (const element of array) {
    // Do something to each element
  }
}
```

And inside our iteration, we'll want to invoke the callback, passing in the
elements from `array`:

```js
function map(array, callback) {
  for (const element of array) {
    callback(element);
  }
}
```

Let's make sure this is working so far:

```js
map([1, 2, 3], function (num) {
  console.log(num * num);
});
// LOG: 1
// LOG: 4
// LOG: 9
```

### Returning a brand new collection

Logging each squared number out to the console is fun, but `map()` should really
be returning an entirely new array containing all of the squared values. First,
let's create that new array:

```js
function map(array, callback) {
  const newArr = [];

  for (const element of array) {
    callback(element);
  }
}
```

Inside the `for...of` statement, let's `.push()` the return value of each
callback invocation into `newArr`:

```js
function map(array, callback) {
  const newArr = [];

  for (const element of array) {
    newArr.push(callback(element));
  }
}
```

And at the end of our `map()` function we're going to want to return the new
array:

```js
function map(array, callback) {
  const newArr = [];

  for (const element of array) {
    newArr.push(callback(element));
  }

  return newArr;
}
```

Let's test it out!

```js
const originalNumbers = [1, 2, 3, 4, 5];

const squaredNumbers = map(originalNumbers, function (num) {
  return num * num;
});

originalNumbers;
// => [1, 2, 3, 4, 5]

squaredNumbers;
// => [1, 4, 9, 16, 25]
```

## Using `Array.prototype.map()` Instead

Now let's see what it looks like if we use the built-in `map()` method:

```js
const originalNumbers = [1, 2, 3, 4, 5];

const squaredNumbers = originalNumbers.map(function (num) {
  return num * num;
});

originalNumbers;
// => [1, 2, 3, 4, 5]

squaredNumbers;
// => [1, 4, 9, 16, 25]
```

That's it! This code works exactly the same as our version - the only difference
is we called the method on our array rather than passing the array as an
argument. And, of course, we didn't have to code `map()` ourselves!

## Resources

- [MDN — `Array.prototype.map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

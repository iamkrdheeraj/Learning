## 7 More Killer One-Liners in JavaScript

This is a continuation of the previous list of JavaScript one-liners. If you haven’t checked out the article, you are highly encouraged to accomplish so.

Let’s crack on with the new list!


#### 1. Sleep Function

JavaScript has NO built-in sleep function to wait for a given duration before continuing the execution flow of the code.

Luckily, generating a function for this purpose is straightforward
```js:
const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
```
#### 2. Go Back to the Previous Page
Need to send the user back to the page they came from? history object to the rescue!
```js:
const navigateBack = () => history.back();
// Or
const navigateBack = () => history.go(-1);
```
#### 3. Compare Objects
Javascript behaves in mysterious ways when comparing objects. Comparing objects with the === operator checks only the reference of the objects, so the following code always returns false:
```js:
const obj1 = { a: 1, b: 2 };
const obj2 = { a: 1, b: 2 };
obj1 === obj2; // false
```
To solve this very issue, you can create a deep comparison function:
```js:
const isEqual = (a, b) => JSON.stringify(a) === JSON.stringify(b);
// examples
isEqual({ a: 1, b: 2 }, { a: 1, b: 2 }); // true
isEqual({ a: 1, b: 2 }, { a: 1, b: 3 }); // false
// works with arrays too
isEqual([1, 2, 3], [1, 2, 3]); // true
isEqual([1, 2, 3], [1, 2, 4]); // false
```
#### 4. Generate Random id
Need multiple unique identifiers but not looking to add the uuid package? Try out this simple utility function:

```js:
const uuid = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  );
```
NOTE: The function is a one-liner, but spread across multiple lines for readability.
#### 5. Get Selected Text

Want to have access to the text user selects? window just has a method to help you out!
```js:
const getSelectedText = () => window.getSelection().toString();
```
#### 6. Check if an Element is Focused
You can effortlessly check if an element is currently focused without setting up the focus & blur listener.

```js:
const hasFocus = (element) => element === document.activeElement;
```
#### 7. Count by the Properties of an Array of Objects
Need to count the number of items in an array that match a particular property? Using reduce on arrays can achieve the task with ease!
```js:
const countElementsByProp = (arr, prop) =>
  arr.reduce(
    (prev, curr) => ((prev[curr[prop]] = ++prev[curr[prop]] || 1), prev),
    {}
  );
// example
countElementsByProp(
  [
    { manufacturer: "audi", model: "q8", year: "2019" },
    { manufacturer: "audi", model: "rs7", year: "2020" },
    { manufacturer: "ford", model: "mustang", year: "2019" },
    { manufacturer: "ford", model: "explorer", year: "2020" },
    { manufacturer: "bmw", model: "x7", year: "2020" },
  ],
  "manufacturer"
); // { 'audi': 2, 'ford': 2, 'bmw': 1 }
```
NOTE: This too is a one-liner, but spread across multiple lines for readability.

https://tapajyoti-bose.medium.com/7-more-killer-one-liners-in-javascript-1cd180d82695

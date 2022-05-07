## Stop Using Function Parameters Now
### Use object destructuring instead

Functions in JavaScript can be many things, but first and foremost it’s a contract. When we define a function we essentially define a contract between a caller and the implementor.

The contract says “given parameters X, Y, Z” you’ll get a result “R”.

#### The thing is, contracts tend to change over time (and boy, do they).

##### The Problem
Let’s take a look at a very simple example:
```js:
// Recieves a name and returns a greeting message
// Typescript
function greet(name: string): string {
  return `Hello, ${name}`; 
}

// Javascript
function greet(name) {
  return `Hello, ${name}`; 
}
```
The contract is simple — provided a name, the function will return a greeting message. A name is all we’ve got and all we need at the moment.

In a few months though, when the function is already used all over the code base, we find ourselves in a need of adding a last name to the greeting message.

We could, of course, just use the name parameter for the last name as well, but since we’re deeply aware of the differences between Internationalization and Localization we know that in some countries the last name can come first. We want our product to be the best fit for our users around the globe, so we want to support this use case.

Since the function is already used in many places across the code base we want to make sure it’s backwards compatible.

Let’s do this, again, in a very naive way:

```js:
// Typescript
function greet(firstName: string, lastName?: string, lastNameFirst: boolean = false): string {
  if (!lastName) {
    return `Hello, ${firstName}`;
  }
  return `Hello ${lastNameFirst ? lastName + ' ' + firstName : firstName + ' ' + lastName}`;
}

// Javascript
function greet(firstName, lastName, lastNameFirst = false) {
  if (!lastName) {
    return `Hello, ${firstName}`;
  }
  return `Hello ${lastNameFirst ? lastName + ' ' + firstName : firstName + ' ' + lastName}`;
}
```
Thus, for the backwards compatibility, if the function isn’t provided with the last name it will just greet the old way, but if it is, it will look at the ordering and greet accordingly.

So far so good, let’s take a look at all the possible usages of this function:

```js:
// First name only - the old way
greet('John');
// First name and last name
greet('John', 'Doe');
// First name and last name but last name first :D 
greet('John', 'Doe', true);
```
There is one slight problem you can notice here and it is the readability. For example, we can guess by the usage that the first parameter is the name and the second is the last name. However, without knowing the implementation we can’t say or possibly know what the true stands for.

Not the biggest problem we have though, at the end of the day we have autocompletion and signature description in almost any IDE.

Let’s give it a few more months to spread across the code base and then realize that there is also a name prefix and (hell, why not!) a middle name.

And the ordering… oh boy, we better not talk about the implementation.

Anyways, back to the signature. We want to support name prefixes (Mr. , Mrs. , Ms. etc.) and middle names. The signature should be changed again, and once again it should be backwards compatible:

```js:
// Typescript
function greet(
  firstName: string,
  lastName?: string,
  lastNameFirst: boolean = false,
  middleName?: string,
  prefix?: string,
): string {
  const greet: string = '';
  // some implementation
  return greet;
}

// Javascript
function greet(firstName, lastName, lastNameFirstn = false, middleName, prefix) {
  const greet = '';
  // some implementation
  return greet;
}
```
It is backwards compatible, meaning the invocations we saw previously would still work without issues:

```js:
// First name only - the old way
greet('John');
// First name and last name
greet('John', 'Doe');
// First name and last name but last name first :D 
greet('John', 'Doe', true);
```
But what if we try to use the new API in all these use cases? Since you can’t just pass an argument without passing all the previous arguments it would look like that:

```js:
// Name with prefix
greet('John', undefined, false, undefined, 'Mr.');
// Name and last name with prefix
greet('John', 'Doe', false, undefined, 'Mr.');
// Name and last name with prefix but last name first
greet('John', 'Doe', true, undefined, 'Mr.');
// Finally just a name, a middle name and a last name
greet('John', 'Doe', false, 'Christopher', 'Mr.');
```
If you use Typescript then it’s ugly but relatively safe. If you use plain JavaScript it’s ugly and unsafe, since the chance of passing a wrong argument in a wrong place is increasing with every new parameter added to the function signature.

#### The Solution
All this could have been easily avoided if we used a single object as a function parameter and seized the great power of object destructuring.

Let’s reiterate on our example.

A function that receives a name and returns a greeting message:

```js:
// Typescript
interface GreetingParams {
  name: string;
}

function greet({ name }: GreetingParams) {
  return `Hello, ${name}`;
}

// Javascript
function greet({ name }) {
  return `Hello, ${name}`;
}

// Invocation
greet({ name: 'John' });
```
Backwards compatible function that works with name and last name for all countries:
```js:
// Typescript
interface GreetingParams {
  name?: string; // keep this for backward compatbility
  firstName?: string; // replaces name
  lastName?: string;
  lastNameFirst?: boolean;
}

function greet({
  name,
  firstName = name, // if firstName not provided, use name as firstName
  lastName,
  lastNameFirst = false,
}: GreetingParams) {
  if (!lastName) {
    return `Hello, ${firstName}`;
  }
  return `Hello ${lastNameFirst ? lastName + ' ' + firstName : firstName + ' ' + lastName}`;
}

// Javascript
function greet({
  name,
  firstName = name, // if firstName not provided, use name as firstName
  lastName,
  lastNameFirst = false,
}) {
  if (!lastName) {
    return `Hello, ${firstName}`;
  }
  return `Hello ${lastNameFirst ? lastName + ' ' + firstName : firstName + ' ' + lastName}`;
}

// Backward compatible with old signature
greet({ name: 'John' });
// New signature with first name only
greet({ firstName: 'John' });
// New signature with first name and last name
greet({ firstName: 'John', lastName: 'Doe' });
// New signature with first name and last name, last name first
greet({ firstName: 'John', lastName: 'Doe', lastNameFirst: true });
```
Backwards compatible function that works with name, last name, middle name and prefix:
```js:
// Typescript
interface GreetingParams {
  name?: string; // keep this for backward compatbility
  firstName?: string; // replaces name
  lastName?: string;
  lastNameFirst?: boolean;
  middleName?: string;
  prefix?: string;
}

function greet({
  name,
  firstName = name, // if firstName not provided, use name as firstName
  lastName,
  lastNameFirst = false,
  middleName,
  prefix,
}: GreetingParams) {
  let greeting: string = '';
  // Some implementation
  return greeting;
}

// Javascript
function greet({
  name,
  firstName = name, // if firstName not provided, use name as firstName
  lastName,
  lastNameFirst = false,
  middleName,
  prefix,
}) {
  let greeting = '';
  // Some implementation
  return greeting;
}

// Backward compatible with old signature and prefix
greet({ name: 'John', prefix: 'Mr.' });
// New signature with first name and prefix
greet({ firstName: 'John', prefix: 'Mr.' });
// New signature with first name, last name and prefix
greet({ firstName: 'John', lastName: 'Doe', prefix: 'Mr.' });
// New signature with first name, last name, middle name and prefix
greet({ firstName: 'John', lastName: 'Doe', middleName: 'Christopher', prefix: 'Mr.' });
```
Ain’t that great?!




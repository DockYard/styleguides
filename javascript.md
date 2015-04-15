# JavaScript Style Guide

## Table Of Contents

### Grammar

* [Block Statements](#block-statements)
* [Conditional Statements] (#conditional-statements)
* [Commas](#commas)
* [Semicolons](#semicolons)
* [Comments](#comments)
* [Variables](#variables)
* [Whitespace](#whitespace)

### Objects

* [Objects](#objects)
* [Properties](#properties)

### Arrays

* [Arrays](#arrays)

### Properties

* [Properties](#properties)

### Strings

* [Strings](#strings)

### Functions

* [Functions](#functions)
* [Function Arguments](#function-arguments)

### ES2015 Features

* [Declaration](#declaration)
* [Destructuring](#destructuring)
* [Destructured Parameters](#destructured-parameters)
* [Default Parameters](#default-parameters)

## Block Statements

+ Use spaces before leading brace.

```javascript
// conditional
if (notFound) {
  return 0;
} else {
  return 1;
}

switch (condition) {
  case 'yes':
    // code
    break;
}

// loops
for (var key in keys) {
  // code
}

for (var i = 0, l = keys.length; i < l; i++) {
  // code
}

while (true) {
  // code
}

try {
  // code that throws an error
} catch(e) {
  // code that handles an error
}
```

+ Opening curly brace (`{`) should be on the same line as the beginning of a statement or declaration.

```javascript
function foo() {
  var obj = {
    val: 'test'
  };

  return {
    data: obj
  };
}

if (foo === 1) {
  foo();
}

for (var key in keys) {
  bar(e);
}

while (true) {
  foo();
}
```

+ Keep `else` and its accompanying braces on the same line.

```javascript
if (foo === 1) {
  bar = 2;
} else {
  bar = '2';
}

if (foo === 1) {
  bar = 2;
} else if (foo === 2) {
  bar = 1;
} else {
  bar = 3;
}
```

## Conditional Statements

+ Use strict equality (`===` and `!==`).
+ Use curly braces for all conditional blocks.

```javascript
if (notFound) {
  // code
}
```

+ Use explicit conditions when checking for non `null`, `undefined`, `true`, `false` values.

```javascript
if (arr.length > 0) {
  // code
}

if (foo !== '') {
  // code
}
```

+ Use multiline format.

```javascript
if (foo === 'bar') {
  return;
}
```

## Commas

+ Skip trailing commas.

```javascript
var foo = [1, 2, 3];
var bar = { a: 'a' };
```

+ Skip trailing and leading commas.

```javascript
var foo = [1, 2, 3];
var bar = {
  a: 'a',
  b: 'b'
};
```

## Semicolons

+ Use semicolons.

## Comments

+ Use multiline comments with two leading asterisks for documentation.

```javascript
/**
  This is documentation for something just below.
*/
```

+ Use [YUIDoc](http://yui.github.io/yuidoc/syntax/index.html) comments for
  documenting functions.
+ Use `//` for non-documenting comments (both single and multiline).

```javascript
function foo() {
  var bar = 5;

  // multiplies `bar` by 2.
  fooBar(bar);

  console.log(bar);
}
```

+ Pad comments with a space.

## Variables

+ Put all non-assigning declarations on one line.

```javascript
var a, b;
```

+ Use a single `var` declaration for each assignment.

```javascript
var a = 1;
var b = 2;
```

+ Declare variables at the top of their block scope.

```javascript
function foo() {
  var bar;

  console.log('foo bar!');

  bar = getBar();
}

function bar() {
  var coolList;

  // code

  for (var index = 0, length = coolThing.length; index < length; index++) {
    // code
  }
}
```

## Whitespace

+ Use soft tabs set to 2 spaces.

```javascript
function() {
∙∙var name;
}
```

+ Place 1 space before a leading brace (`{`).

```javascript
obj.set('foo', {
  foo: 'bar'
});

test('foo-bar', function() {
});
```

+ No spaces before semicolons.

```javascript
var foo = {};
```

+ Keep parenthesis adjacent to the function name when declared or called.

```javascript
function foo() {
}

foo();
```

+ No trailing whitespace.

## Objects

+ Use literal form for object creation.

```javascript
var foo = {};
```

+ Pad single-line objects with white-space.

```javascript
var bar = { color: 'orange' };
```

## Arrays

+ Use literal form for array creation (unless you know the exact length).

```javascript
var foo = [];
```

+ Use new Array if you know the exact length of the array and know that its length will not change.

```javascript
var foo = new Array(16);
```

+ Use `push` to add an item to an array.

```javascript
var foo = [];
foo.push('bar');
```

+ Join single line array items with a space.

```javascript
var foo = ['a', 'b', 'c'];
```

## Properties

+ Use dot-notation when accessing properties.

```javascript
var foo = {
  bar: 'bar'
};

foo.bar;
```

+ Use `[]` when accessing properties via a variable.

```javascript
var propertyName = 'bar';
var foo = {
  bar: 'bar'
};

foo[propertyName];
```

## Strings

* Use single quotes.

```javascript
var food = 'croissant';
```

* Use double quotes to avoid escaping.

```javascript
var msg = "Let's do something.";
```

## Functions

+ Make sure to name functions when you define them.

```javascript
function fooBar() {
}

var foo = {
  bar: function foo_bar() {
    // code
  }
};

var foo = function foo() {
  // code
};
```

+ Use scope to lookup functions (not variables).

```javascript
// GOOD:
function foo() {
  function bar() {
    // code
  }

  bar();
}

// BAD:
function foo() {
  var bar = function bar() {
    // code
  }

  bar();
}
```

## Function Arguments

`arguments` object must not be passed or leaked anywhere.
See the [reference](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments).

+ Use a `for` loop with `arguments` (instead of `slice`).

```javascript
function fooBar() {
  var args = new Array(arguments.length);

  for (var i = 0; i < args.length; ++i) {
    args[i] = arguments[i];
  }

  return args;
}
```

+ Don't re-assign the arguments.

```javascript
// Don't re-assigning the arguments
function fooBar() {
  arguments = 3;
}

// Don't re-assigning the arguments
function fooBar(opt) {
  opt = 3;
}

// Use a new variable if you need to assign the value of an argument
function fooBar(_opt) {
  var opt = _opt;

  opt = 3;
}
```

+ Use a new variable if you need to re-assign an argument.

```javascript
function fooBar(opt) {
  var options = opt;

  options = 3;
}
```

## Declaration

Prefer `let` to `var` for variable declarations.

```javascript
if (condition) {
  let color = 'blue'; // block-scope, no hoisting
}

for (let i = 0; i < items.length; i++) {
  process(items[i]);
}
```

Use `const` to declare constants.

```javascript
const MAX_ITEMS = 30;
```

_Both are block-scoped._

## Destructuring

When decomposing simple arrays or objects, prefer [destructuring](http://babeljs.io/docs/learn-es6/#destructuring).

```javascript
// array destructuring
var fullName = 'component:foo-bar';
var [
  first,
  last
] = fullName.split(':');
```

```javascript
// object destructuring
var person = {
  firstName: 'Stefan',
  lastName: 'Penner'
}

var {
  firstName,
  lastName
} = person;
```

## Destructured Parameters

When using destructured parameters, always provide a default value to avoid an error being thrown when the argument is not provided.

```javascript
// Good
function setCookie(name, value, { secure, path, domain, expires } = {}) {
}

// Will not throw an error
setCookie('type', 'js');

// Bad
function setCookie(name, value, { secure, path, domain, expires }) {
}

// Will throw an error
setCookie('type', 'js');
```

## Default Parameters

Use whitespace around `=` when specifying default parameters.

```javascript
// Good
function makeRequest(url, timeout = 2000, callback = function() {}) {
}

// Bad
function makeRequest(url, timeout=2000, callback=function() {}) {
}
```

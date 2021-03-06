# Node.js Style Guide

This is a guide for writing consistent and aesthetically pleasing node.js code.
Please go through this and you will get better idea how to write better code.

## Table of contents

### Formatting
* [4 Spaces for indentation](#4-spaces-for-indentation)
* [Newlines](#newlines)
* [No trailing whitespace](#no-trailing-whitespace)
* [Use Semicolons](#use-semicolons)
* [80 characters per line](#80-characters-per-line)
* [Use single quotes](#use-single-quotes)
* [Opening braces go on the same line](#opening-braces-go-on-the-same-line)
* [Declare one variable per var statement](#declare-one-variable-per-var-statement)

### Naming Conventions
* [Use lowerCamelCase for variables, properties and function names](#use-lowercamelcase-for-variables-properties-and-function-names)
* [Use UpperCamelCase for class names](#use-uppercamelcase-for-class-names)
* [Use UPPERCASE for Constants](#use-uppercase-for-constants)

### Variables
* [Object / Array creation](#object--array-creation)

### Conditionals
* [Use the === operator](#use-the--operator)
* [Use multi-line ternary operator](#use-multi-line-ternary-operator)
* [Use descriptive conditions](#use-descriptive-conditions)

### Functions
* [Write small functions](#write-small-functions)
* [Return early from functions](#return-early-from-functions)
* [Name your closures](#name-your-closures)
* [No nested closures](#no-nested-closures)
* [Method chaining](#method-chaining)

### Comments
* [Use slashes for comments](#use-slashes-for-comments)

### Miscellaneous
* [Object.freeze, Object.preventExtensions, Object.seal, with, eval](#objectfreeze-objectpreventextensions-objectseal-with-eval)
* [Requires At Top](#requires-at-top)
* [Tips and Tricks](#tips-and-tricks)
* [Further Reading](#further-reading)

## Formatting


### 4 Spaces for indentation

Use 4 spaces for indenting your code and never mix tabs and
spaces. Tab don't have any standard, in some IDE it will take 4 space and in other will take 8.
So don't mess code with tab and space and always use 4 space for indentation.

### Newlines

Use UNIX-style newlines (`\n`), and a newline character as the last character
of a file. Windows-style newlines (`\r\n`) are forbidden inside any repository.

### No trailing whitespace

Clean up any trailing whitespace in your JS files before committing.

### Use Semicolons

According to [scientific research][hnsemicolons], the usage of semicolons is
a core value of our community. Consider the points of [the opposition][], but
be a traditionalist when it comes to abusing error correction mechanisms for
cheap syntactic pleasures.

[the opposition]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hnsemicolons]: http://news.ycombinator.com/item?id=1547647

### 80 characters per line

Limit your lines to 80 characters. Yes, screens have gotten much bigger over the
last few years, but our brain has not. JSHINT and JSLINT will give you warning if any line has more than 80 charecters.

### Use single quotes

Use single quotes, unless you are writing JSON.

*Right:*

```js
var foo = 'bar';
```

*Wrong:*

```js
var foo = "bar";
```

### Opening braces go on the same line

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
    console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
    console.log('losing');
}
```

Also, notice the use of whitespace before and after the condition statement.

### Declare one variable per var statement

Declare one variable per var statement, it makes it easier to re-order the
lines. However, ignore [Crockford][crockfordconvention] when it comes to
declaring variables deeper inside a function, just put the declarations wherever
they make sense.

*Right:*

```js
var keys   = ['foo', 'bar'];
var values = [23, 42];

var object = {};
while (keys.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (keys.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

[crockfordconvention]: http://javascript.crockford.com/code.html

### Naming Conventions

### Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use `lowerCamelCase`.  They
should also be descriptive. Single character variables and uncommon
abbreviations should generally be avoided.

*Right:*

```js
var adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
var admin_user = db.query('SELECT * FROM users ...');
```

### Use UpperCamelCase for class names

Class names should be capitalized using `UpperCamelCase`.

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties,
using all uppercase letters.

*Right:*

```js
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Variables

### Object / Array creation

Use trailing commas and put *short* declarations on a single line. Only quote
keys when your interpreter complains:

*Right:*

```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```

*Wrong:*

```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## Conditionals

### Use the === operator

Programming is not about remembering [stupid rules][comparisonoperators]. Use
the triple equality operator as it will work just as expected.

*Right:*

```js
var a = 0;
if (a !== '') {
  console.log('winning');
}

```

*Wrong:*

```js
var a = 0;
if (a == '') {
  console.log('losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

### Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

*Right:*

```js
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
  console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  console.log('losing');
}
```

## Functions

### Write small functions

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

### Return early from functions

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

*Right:*

```js
function isPercentage(val) {
    if (val < 0) {
        return false;
    }
    if (val > 100) {
        return false;
    }
    return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
    if (val >= 0) {
        if (val < 100) {
            return true;
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
function isPercentage(val) {
    var isInRange = (val >= 0 && val <= 100);
    return isInRange;
}
```

### Name your closures

Feel free to give your closures a name. It shows that you care about them, and
will produce better stack traces, heap and cpu profiles.

*Right:*

```js
req.on('end', function onEnd() {
    console.log('winning');
});
```

*Wrong:*

```js
req.on('end', function() {
    console.log('losing');
});
```

### No nested closures

Use closures, but don't nest them. Otherwise code will become a mess.

*Right:*

```js
setTimeout(function() {
    client.connect(afterConnect);
}, 1000);

function afterConnect() {
    console.log('winning');
}
```

*Wrong:*

```js
setTimeout(function() {
    client.connect(function() {
        console.log('losing');
    });
}, 1000);
```

### Method chaining

One method per line should be used if you want to chain methods.

You should also indent these methods so it's easier to tell they are part of the same chain.

*Right:*

```js
User.findOne({ name: 'foo' })
  .populate('bar')
  .exec(function(err, user) {
    return true;
  });
````

*Wrong:*

```js
User
.findOne({ name: 'foo' })
.populate('bar')
.exec(function(err, user) {
  return true;
});


User.findOne({ name: 'foo' }).populate('bar')
.exec(function(err, user) {
  return true;
});

User.findOne({ name: 'foo' }).populate('bar')
  .exec(function(err, user) {
    return true;
  });
````

## Comments

### Use slashes for comments

Use slashes for both single line and multi line comments. Try to write
comments that explain higher level mechanisms or clarify difficult
segments of your code. use block comments for formal documentation / method documentation.
before writing please have the complete idea what is the difference between comment and documentation.

*Right:*

```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

*Wrong:*

```js
// Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/);

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```

## Miscellaneous

### Object.freeze, Object.preventExtensions, Object.seal, with, eval

Stay away from these kinds of stuffs..

### Requires At Top

Always put requires at top of file to clearly illustrate a file's dependencies. Besides giving an overview for others at a quick glance of dependencies and possible memory impact, it allows one to determine if they need a package.json file should they choose to use the file elsewhere.

### Tips and Tricks

*True and False Boolean Expressions*

The following are all false in boolean expressions:

* null
* undefined
* '' the empty string
* 0 the number

But be careful, because these are all true:

* '0' the string
* [] the empty array
* {} the empty object

This means that instead of this:

```js
while (x != null) {
```

you can write this shorter code (as long as you don't expect x to be 0, or the empty string, or false):

```js
while (x) {
```

And if you want to check a string to see if it is null or empty, you could do this:

```js
if (y != null && y != '') {
```

But this is shorter and nicer:

```js
if (y) {
```

*Conditional (Ternary) Operator (?:)*

Instead of this:

```js
if (val) {
  return foo();
} else {
  return bar();
}
```

you can write this:

```js
return val ? foo() : bar();
```
*&& and ||*

These binary boolean operators are short-circuited, and evaluate to the last evaluated term.

"||" has been called the 'default' operator, because instead of writing this:

```js
function foo(opt_win) {
  var win;
  if (opt_win) {
    win = opt_win;
  } else {
    win = window;
  }
  // ...
}
```

you can write this:

```js
function foo(opt_win) {
  var win = opt_win || window;
  // ...
}
```

"&&" is also useful for shortening code. For instance, instead of this:

```js
if (node) {
  if (node.kids) {
    if (node.kids[index]) {
      foo(node.kids[index]);
    }
  }
}
```

you could do this:

```js
if (node && node.kids && node.kids[index]) {
  foo(node.kids[index]);
}
```

or this:

```js
var kid = node && node.kids && node.kids[index];
if (kid) {
  foo(kid);
}
```

### Further Reading

* [Best Practices part1](https://blog.risingstack.com/node-js-best-practices/)
* [Best Practices part2](https://blog.risingstack.com/node-js-best-practices-part-2/)




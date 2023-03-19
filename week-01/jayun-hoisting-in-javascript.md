# Hoisting in JavaScript

> In JavaScript, hoisting allows you to use functions and variables **before they’re declared**.

## What is hoisting?

Take a look at the code below and guess what happens when it runs:

```tsx
console.log(foo);
var foo = 'foo';
```

- Result: `undefined`
  - It might surprise you that this code outputs `undefined` and doesn’t fail or throw an error.
  - Even though `foo` gets assigned after we `console.log` it.

### JavaScript interpreter splits the declaration and assignment of functions and variables.

- It “hoists” your declarations **to the top of their containing scope** before execution.
  - This process is called hoisting, and it allows us to use `foo` before its declaration in our example above.

## Variable hoisting in JavaScript

- We declare a variable with the `var`, `let`, and `const` statements.
- We assign a variable a value using the assignment operator.

```tsx
// Declaration
var foo;
let bar;

// Assignment
foo = 'foo';
bar = 'bar';
```

- In many cases, we can combine declaration and assignment into one step.

```tsx
var foo = 'foo';
let bar = 'bar';
const baz = 'baz';
```

- Variable hoisting acts differently depending on how the variable is declared.

## Variable hoisting with `var`

- When the interpreter hoists a variable declared with `var`, it initializes its value to `undefined`.

```tsx
console.log(foo); // undefined

var foo = 'bar';

console.log(foo); // "bar"
```

- As we defined earlier, hoisting comes from the interpreter splitting variable declaration and assignment.
- We can achieve this same behavior manually by splitting the declaration and assignment into two steps.

```tsx
var foo;

console.log(foo); // undefined

foo = 'foo';

console.log(foo); // "foo"
```

- The first console.log(foo) outputs undefined because foo is hoisted and given a default value.
- Using an undeclared variable will throw a `ReferenceError` instead.

```tsx
console.log(foo); // Uncaught ReferenceError: foo is not defined
```

- Using an undeclared variable before its assignment will also throw a `ReferenceError` because no declaration was hoisted.

```tsx
console.log(foo); // Uncaught ReferenceError: foo is not defined
foo = 'foo'; // Assigning a variable that's not declared is valid
```

- By now, you may be thinking,
  - “It’s kind of weird that JavaScript lets us access variables before they’re declared.”
- This behavior is an unusual part of JavaScript and can lead to errors.
- Using a variable before its declaration is usually not desirable.
- Thankfully the `let` and `const` variables, introduced in ECMAScript 2015, behave differently.

## Variable hoisting with `let` and `const`

- Variables declared with `let` and `const` are hoisted **but not initialized with a default value**.
- Accessing a `let` or `const` variable before it's declared will result in a `ReferenceError` .

```tsx
console.log(foo); // Uncaught ReferenceError: Cannot access 'foo' before initialization

let foo = 'bar'; // Same behavior for variables declared with const
```

Notice that the interpreter still hoists `foo`: the error message tells us the variable is initialized somewhere.

## The temporal dead zone

> A **temporal dead zone (TDZ)**
>  is the area of a block where a variable is inaccessible until the moment the computer completely initializes it with a value.

- The reason that we get a reference error when we try to access a `let` or `const` variable before its declaration is because of the temporal dead zone(TDZ).
- **The TDZ starts at the beginning of the variable's enclosing scope and ends when it is declared.**
  - A block’s temporal dead zone starts at the beginning of the block’s local scope. It ends when the computer fully initializes your variable with a value.
  - Accessing the variable in this TDZ throws a `ReferenceError.`
- Here's an example with an explicit block that shows the start and end of `foo`'s TDZ:

```tsx
// A block is a pair of braces ({...}) used to group multiple statements.
{
  // Start of foo's TDZ
  let bar = 'bar';
  console.log(bar); // "bar"

  console.log(foo); // ReferenceError because we're in the TDZ

  // Initialization occurs when you assign an initial value to a variable.
  let foo = 'foo'; // End of foo's TDZ
}
```

```tsx
{
  // bestFood’s TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  console.log(bestFood); // returns ReferenceError because bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood = 'Vegetable Fried Rice'; // bestFood’s TDZ ends here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

- The TDZ is also present in **default function parameters**, which are evaluated left-to right.
- In the following example, bar is in the TDZ until its default value is set:

```tsx
function foobar(foo = bar, bar = 'bar') {
  console.log(foo);
}
foobar(); // Uncaught ReferenceError: Cannot access 'bar' before initialization
```

- But this code works because we can access `foo` outside of its TDZ:

```tsx
function foobar(foo = 'foo', bar = foo) {
  console.log(bar);
}
foobar(); // "foo"
```

> So, to prevent JavaScript from throwing such an error, you’ve got to remember to access your variables from outside the temporal dead zone.

---

### Reference

- https://www.freecodecamp.org/news/javascript-temporal-dead-zone-and-hoisting-explained/
- https://www.freecodecamp.org/news/what-is-hoisting-in-javascript/
- https://developer.mozilla.org/en-US/docs/Glossary/Hoisting

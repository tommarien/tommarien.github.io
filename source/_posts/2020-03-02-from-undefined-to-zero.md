---
title: From undefined to zero
tags:
  - javascript
date: 2020-03-02 14:53:15
updated: 2020-03-02 14:53:15
---


As **Javascript** continues to be one of the most misunderstood languages on the planet, of which you could find evidence all over the web. One of my personal favorites is the [lightning Wat talk by Gary Bernhardt](https://www.destroyallsoftware.com/talks/wat).

Lately i've begun to realize the underlying beauty of the language and felt the need to blog about it.

We'll start with two [primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive), `undefined` and `null`. Why do they exist both and what differentiates them?

## `undefined`

The value that gets assigned to unassigned declared variables or to `function` parameters that are missing.

```js
// 1. Unassigned variable
let variable;
console.log(variable); // => undefined

// 2. Missing function parameter
function greet(name) {
  console.log(name);
}

greet(); // => undefined
```

## `null`

Represents the **intentional** absence of a value, it's a semantical difference.

```js
const notDotted = 'googlebe';
console.log(notDotted.match(/\w+\.\w+/)); // => null
```

## Are they equal?

```js
console.log(undefined === null); // => false
```

So strictly speaking they are different, which we could have expected. But they do have **the same meaning**:

```js
console.log(undefined == null); // => true
```

## Falsy

Luckily they both evaluate to false, otherwise we would have to write the following code:

```js
let user;

// 👎 If they didn't evaluate to false
if (user !== null && user !== undefined) user.rename('John', 'Doe');

// 👍 Thank god
if (user) user.rename('John', 'Doe');
```

## Conclusion

Concepts like [falsy](https://developer.mozilla.org/nl/docs/Glossary/Falsy), [type coercion](https://developer.mozilla.org/nl/docs/Glossary/Type_coercion) start to make a whole lot of sense, if you try to understand the problems they fix.
---
title: From undefined to zero
description: One of the most misunderstood topics in Javascript
tags:
  - javascript
date: 2020-03-02
lastmod: 2020-03-03
---

As **Javascript** continues to be one of the most misunderstood languages on the planet, of which you could find evidence all over the web. One of my personal favorites is the [lightning Wat talk by Gary Bernhardt](https://www.destroyallsoftware.com/talks/wat).

Lately i've begun to realize the underlying beauty of the language and felt the need to blog about it.

We'll start with two [primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive), `undefined` and `null`. Why do they exist both and what differentiates them?

### undefined

The value that gets assigned to unassigned declared variables or to `function` parameters that are missing.

```javascript
// 1. Unassigned variable
let variable;
console.log(variable); // => undefined

// 2. Missing function parameter
function greet(name) {
  console.log(name);
}

greet(); // => undefined
```

### null

Represents the **intentional** absence of a value, it's a semantical difference.

```javascript
const notDotted = "googlebe";
console.log(notDotted.match(/\w+\.\w+/)); // => null
```

### Are they equal?

```js
console.log(undefined === null); // => false
```

So strictly speaking they are different, which we could have expected. But they do have **the same meaning**:

```javascript
console.log(undefined == null); // => true
```

### Falsy

Luckily they both evaluate to false, otherwise we would have to write the following code:

```javascript
let user;

// 💩 If they didn't evaluate to false and without ==
if (user !== null && user !== undefined) user.rename("John", "Doe");

// 💪 With == (type coercion)
if (user != null) user.rename("John", "Doe");

// 🙏 Thanks to falsy
if (user) user.rename("John", "Doe");
```

### Conclusion

Concepts like [type coercion](https://developer.mozilla.org/nl/docs/Glossary/Type_coercion) and [falsy](https://developer.mozilla.org/nl/docs/Glossary/Falsy) start to make a whole lot of sense if you look at the problems they fix.

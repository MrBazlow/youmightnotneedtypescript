---
transition: fade
---

# Enums

````md magic-move
```ts
enum Foo {
  A = 1,
  B = 2,
  C = 3
}
```

```js
/**
 * Foo enum
 *
 * @enum {typeof Foo[keyof typeof Foo]}
 */
const Foo = /** @type {const} */ ({
  A: 1,
  B: 2,
  C: 3,
})
```
````

<!--

While enums are not a thing in JavaScript and TypeScript enums are still not really enums either. 

If you like the dot notation of enums, you can still get that in JSDoc.

-->

---
transition: fade
---

# Conforming to a type

````md magic-move
```ts
// TypeScript

interface Foo {
  /**
   * The bar string or number
   */
  bar: string | number;
  /**
   * The literal string
   */
  baz: "literal";
}

const bar = {
  bar: "bar",
  baz: "literal",
} satisfies Foo;

bar.bar.toExponential();
// Property 'toExponential' does not exist on type 'string'.
```

```js
// JavaScript

/**
 * @typedef {object} Foo
 * @property {string | number} bar - The bar string or number
 * @property {"literal"} baz - The literal string
 */

/** 
 * @satisfies {Foo}
 */ 
const bar = {
  bar: "bar",
  baz: "literal",
}

bar.bar.toExponential();
// Property 'toExponential' does not exist on type 'string'.
```
````

<!--

TypeScript has the satisfies keyword, this lets you conform to a type without excessive widening of the type. Just like TypeScript, you can use the `@satisfies` tag in JSDoc to conform to a type.

-->

---
transition: fade
---

# Defining a Tuple

````md magic-move
```ts
// TypeScript

const foobar: readonly [string, string] = ["foo", "bar"];
const names: [string, string, string] = ["Alice", "Bob", "Eve"];
```

```js
// JavaScript

/**
 * @type {[string, string]}
 * @readonly
 */
const foobar = ["foo", "bar"];

/**
 * @type {[string, string, string]}
 */
const names = ["Alice", "Bob", "Eve"];
```
````

<!--

If you ever use `noUncheckedIndexAccess` in your TypeScript config, any indexed access to an array will be treated as potentially undefined. If you use a Tuple however, you get type safety for the indexed access in that array

and it works in JSDoc too.

-->

---
transition: fade
---

# Generic base type

````md magic-move
```ts
type Bar = "foo" | "bar" | "buzz";

type Foo<T extends Bar> = {
  /**
   * The value
   */
  value: T;
}
```

```js
/**
 * @typedef {"foo" | "bar" | "buzz"} Bar
 */

/**
 * @template {Bar} T
 * @typedef {object} Foo
 * @property {T} value - The value
 */
```
````

---
transition: fade
---

# Generic function

````md magic-move
```ts
function foo<T>(a: T): T {
  return a;
}
```

```js
/**
 * @template T
 * @param {T} a - The value
 * @returns {T} The value
 */
function foo(a) {
  return a;
}
```
````

---
transiton: fade
---

# Function with optional parameter

````md magic-move
```ts
function increment(a?: number): number {
  if (a !== undefined) {
    return a++
  }

  return 1;
}
```

```js
/**
 * @param {number} [a]
 * @returns {number}
 */
function increment(a) {
  if (a !== undefined) {
    return a++
  }

  return 1;
}
```

```js
/**
 * @param {number=} a
 * @returns {number}
 */
function increment(a) {
  if (a !== undefined) {
    return a++
  }

  return 1;
}
```
````

---
transition: fade
---

# Class with Generic

````md magic-move
```ts
class Inject<T extends UnknownContext = UnknownContext> {
  
  private eventTarget: EventTarget;

  private context: T;

  constructor(eventTarget: EventTarget, context: T) {
    this.eventTarget = eventTarget;
    this.context = context;
  }
}
```

```js
/**
 * @template {UnknownContext} [T = UnknownContext]
 */
class Inject {
  /**
   * @type {EventTarget}
   * @private
   */
  eventTarget;

  /**
   * @type {T}
   * @private
   */
  context;

  /**
   * @param {EventTarget} eventTarget
   * @param {T} context
   */
  constructor(eventTarget, context) {
    this.eventTarget = eventTarget;
    this.context = context;
  }
}
```
````
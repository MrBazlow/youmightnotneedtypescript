---
theme: default
background: /assets/bg.jpg
title: You might not need TypeScript
info: |
  Notes for talk
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# You might not need TypeScript

### Oliver Mckee

<!--

Before I start, I just want to mention there will be a link and a QR code at the end of this talk to the slides so if I rush through something too quickly and you want a screenshot, you can grab it at the end.

I'd like to start this afternoon with a scenario, perhaps something familiar to you. You are tasked with updating the frontend of a web application, this is not a living project or a green field one. Instead it's a legacy project, one that has been around the block a few times, I think it's PHP 5, the version of Gulp don't work with the latest version of node.js, the last commit was in 2014 and your task is to add a new button to a form that will auto populate it with data from an API. As you setup your environment, you look over the frontend scripting, it's thankfully all JavaScript, no CoffeeScript in here but also, no TypeScript either.

You'd prefer it to be TypeScript, but it's not. And your task is not to refactor the entire projects build process just so you can use TypeScript, you're just going to add a button to a form.

And that's the point I want to make today, there is a lot of JavaScript out there, and I mean a lot of JavaScript. Now, if you attended this conference last year, there was a wonderful talk by Eamonn Boyle from Gearset about how they incrementally added TypeScript to their codebase, how they took on that challenge and how long it took them to get there and yes, it took a fair bit of time to get there. For a living project, I think that's the right choice but think back to that scenario I just gave you, maybe you might know of some projects like that, are they worth the time to migrate to TypeScript?

-->

---
layout: image
image: /assets/2023_most_wanted.png
---

<!--

And why would you migrate to TypeScript? Because developers love the types that TypeScript provides, they love the squiggly lines that tell them when they've made a mistake, they love the autocomplete that tells them what properties are available on an object, they love that the types gives them documentation on the codebase and they love that the static analysis that can catch bugs before they happen.

-->

---
layout: image
image: /assets/2023_build_tool_libraries.png
---

<!--

What they might not love however is the build process. In workshopping this talk, it was suggested to me to call it "Friends don't let friends use build tools" but I think that's a little extreme.

What I want to show you today is how to get the types, the squiggly lines, the autocomplete, the documentation and the static analysis without the build process. I want to show you how to get TypeScript without TypeScript.

And I can do that through the use of comments in your existing JavaScript codebase, even one that's a decade old but first, a little trip through history.

-->

---
layout: image
image: /assets/1999.png
--- 

<!--

The year is 1999, the third edition of the ECMAScript standard is being finalised and the committee is already looking ahead to the fourth edition. In this memo, they outline some of the features that they would like to see in the fourth edition and one of those features in the first bullet point there ... is types.

-->

---
layout: image
image: /assets/ecma.png
---

<!--

That all sounds promising but if we look up all the specification versions and scroll down to the fourth edition, it's missing. 

-->

---
layout: image
image: /assets/js20.png
backgroundSize: 60rem
---

<!--

Now, if I wanted to discuss what happened here, i'd need more time than I have today but the abridged history can be summed up as Microsoft said no.

If you are interested and maybe want to read a bit more of the history, there is a white paper online called "JavaScript: The First 20 Years" that you can read through in your own time, it's rare that a white paper is a page turner but if you don't know the history, without spoilers, there's more action in there than a Korean K-Drama.

Now you know, types are not a new idea for JavaScript and somehow, Python, PHP and even plain old CSS got types before JavaScript.

-->

---

# What did ES4 types look like?

````md magic-move
```ts
// Interface
interface I {
  function f(): String;
}

// Class
class C implements I {
  function f() {
    return "foo";
  }
}

// Function
function add(a: int, b: int): int {
  return a + b;
}

// Union
type StringOrInt = (String,Int)
```

```ts
// Interface
interface I {
  f(): string;
}

// Class
class C implements I {
  f() {
    return "foo";
  }
}

// Function
function add(a: number, b: number): number {
  return a + b;
}

// Union
type StringOrNumber = string | number
```
````

Find out more at https://evertpot.com/ecmascript-4-the-missing-version

<!--

And what did those types look like? Well, for a rejected standard, they're not as crazy as you may think 

and if we adjust them slightly, we can see how close they were to TypeScript too.

There's a brief blog post detailing some of these features in the link below if you want to know more.

We didn't get this future from 1999 and the nature of the JavaScript language means we might never get it as implemented in the fourth edition but change is on the horizon.

-->

---
layout: image
image: /assets/github.com_tc39_proposal-type-annotations.png
---

<!--

Fast forward to today and the team behind TypeScript have submitted a proposal to have *most* of the type annotations in TypeScript work in JavaScript natively, the process takes time, this is not going to be something we'll see in the near future but it is *the* future that we were promised more than 20 years ago.

-->

---
layout: image
image: /assets/gogenerics.png
backgroundSize: 60rem
---

<!--

The proposal also can't be a carbon copy of TypeScript, it needs some minor changes to syntax such as with generics as they use the angle brackets to capture the generic value, that's a problem for JavaScript since those symbols are already in use. Obstacles like these need to be resolved first, unless you want to let developers come up with their own solutions such as this creative example for Golang.

-->

---
layout: section
---

# What can we do today?

<!--

That's the past and the future, what about in the meantime and in between time, what can we do today?

That's where JSDocs comes into the picture

-->

---
layout: image
image: /assets/jsdoc.png
backgroundSize: 40rem
---

<!--

Inspired by Javadoc and tracing its heritage back to the 90s, JSDoc lets you document your code. If you've ever used Javadoc, it will feel just like a dialect of it.

-->

---

# JSDoc relatives

## JSDoc for JavaScript

```javascript
/**
 * This is a simple JSDoc comment
 * @param {string} name - The name of the person
 * @returns {string} - The greeting
 */
```

## Javadoc for Java

```java
/**
 * This is a simple JavaDoc comment
 * @param {string} name - The name of the person
 * @returns {string} - The greeting
 */
```

## PHPDoc for PHP

```php
/**
 * This is a simple PHPDoc comment
 * @param string $name - The name of the person
 * @return string - The greeting
 */
```

<!--

Another child of Javadoc is PHPDoc for the PHP language, before there was a language level solution to type declarations, PHPDoc filled that void along with DocBlocks.

So if you are familiar with PHPDoc, this is the direction that JSDoc and JavaScript is heading. Comments today, language features tomorrow.

-->

---
layout: image
image: /assets/typescript.svg
backgroundSize: 20rem
---

<!--

As for how we're going to go from comments to IDE integration, that's where TypeScript enters the picture.

The TypeScript language server that provides all the features you know and love about TypeScript has also...

-->

---
layout: image
image: /assets/typescript_jsdoc.png
backgroundSize: 60rem
---

<!--

...quietly supported a limited subset of JSDoc, specifically some of the Google Closure syntax. This has allowed TypeScript to also check JavaScript files and even attempt to infer the types to maintain that developer experience.

This is important because of two events that have happened in the last few years.

-->

---
layout: image
image: /assets/closurelibrary.png
---

<!--

First up, Google finally archived the closure library after announcing it last year

The Google Closure *compiler* does live on internally at Google but for those outside of Google, this left the TypeScript language server as the only JSDoc type checker that is widely supported today. 

Which brings us to the second event.

-->

---
layout: image
image: /assets/typescript.svg
backgroundSize: 20rem
---

<!--

Acknowledging this, the TypeScript development team have started to add new features to TypeScript that are JSDoc specific, which you can see in the release notes for TypeScript over the last two years. Buried in the release notes are new features like...

-->

---
layout: image
image: /assets/importtag.png
backgroundSize: 60rem
---

<!--

The new JSDoc @import tag

-->

---
layout: image
image: /assets/satisfies.png
backgroundSize: 60rem
---

<!--

The new @satisfies tag. There's more examples but we're on the clock.

-->

---
layout: section
---

# JSDoc in action

<!--

Now that you're caught up, let's see some code examples of what you can do with JSDoc in your JavaScript codebase.

-->

---

# Enabling JSDoc type checking

1) Enable via comment

```js
// @ts-check
```

2) Enable via tsconfig.json

```jsonc
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "noEmit": true,
  }
}
```

3) Enable globally in VS Code

```jsonc
{
  "js/ts.implicitProjectConfig.checkJs": true,
}
```

<!--

By default, TypeScript will not evaluate the JSDocs since that's the default behavior you expect in your IDE. To enable it, you need to use a directive such as `@ts-check` and that will enable type checking in that file.

Alternatively if you want to enable it for every file in the project, you can throw a tsconfig.json into your working directory.

And the last option, enable it globally in your VS Code settings.

With that done, we can get type checking with JSDocs

-->

---
transition: fade
---

# Importing

````md magic-move
```ts
// TypeScript

import type { MyObject } from "my-types"
```

```js
// JavaScript

/** @import { MyObject } from "my-types" */
```
````

<!--

If you have type signatures in another file, you can import them like this in TypeScript 

and just like this in JSDoc. It's simple yet familiar.

-->

---
transition: fade
---

# Defining an Object

````md magic-move
```ts
// TypeScript

/**
 * A user object
 */
interface User {
  /**
   * The name of the user
   */
  name: string;
  /**
   * The age of the user
   */
  age: number;
  /**
   * Whether the user has full access
   */
  fullAccess: boolean;
}

let myUser: User;
```

```js
// JavaScript

/**
 * A user object
 * 
 * @typedef {object} User
 * @property {string} name - The name of the user
 * @property {number} age - The age of the user
 * @property {boolean} fullAccess - Whether the user has full access
 */

/**
 * @type {User}
 */
let myUser;
```
````

<!--

To define an object with TypeScript you can use an interface or a type

and with JSDoc you can use a typedef, like that.

-->

---
transition: fade
---

# Extending an Object

````md magic-move
```ts
// TypeScript

interface Foo {
  /**
   * The A string
   */
  a: string;
}

interface Bar extends Foo {
  b: number;
}
```

```js
// JavaScript

/**
 * @typedef {object} Foo
 * @property {string} a - The A string
 */

/**
 * @typedef {object} Bar
 * @property {number} b - The B number
 */

/**
 * @typedef {Foo & Bar} FooBar
 */
```
````

<!--

Type definitions in TypeScript can't be extended but interfaces can. 

To extend an object in JSDoc, you use the same syntax as you would for merging two types defined using the "type" keyword in TypeScript through the use of an intersection ampersand, like that.

-->

---
transition: fade
---

# Typing a variable

````md magic-move
```ts
// TypeScript

const names: string[] = ["Alice", "Bob", "Eve"];
```

```js
// JavaScript

/**
 * @type {string[]}
 */
const names = ["Alice", "Bob", "Eve"];
```
````

<!--

In TypeScript you set the type of a variable by putting the type after the variable name

And in JSDoc you can do the same by moving that type to the comment before it.

-->

---
transition: fade
---

# Defining a constant

````md magic-move
```ts
// TypeScript

const myObject = {
  foo: true,
  bar: false
} as const;
```

```js
// JavaScript

const myObject = /** @type {const} */ ({
  foo: true,
  bar: false
});
```
````

<!--

Sometimes you want a deeply immutable object, if you want to do that, you can use the `as const` syntax in TypeScript

and the `@type {const}` in JSDoc.

-->

--- #19
transition: fade
---

# Function

````md magic-move
```ts
// TypeScript

/**
 * A function that adds two numbers
 *
 * @param a The first number
 * @param b The second number
 * @returns The sum of the two numbers
 */
export function add(a: number, b: number): number {
  return a + b;
}
```

```js
// JavaScript

/**
 * A function that adds two numbers
 *
 * @param {number} a The first number
 * @param {number} b The second number
 * @returns {number} The sum of the two numbers
 */
export function add(a, b) {
  return a + b;
}
```
````

<!--

The syntax for Functions is super simple,

you just move the type annotations up into the comments.

-->

---
transition: fade
---

# Function Overloading

````md magic-move
```ts
// TypeScript

function foo(a: string): void;
function foo(a: number): void;
function foo(a: string | number): void {}
```

```js
// JavaScript

/**
 * @overload
 * @param {string} a - The string value
 * @returns {string} string return value
 */

/**
 * @overload
 * @param {number} a - The number value
 * @returns {number} number return value
 */

/**
 * @param {string | number} a - The string or number value
 * @returns {string | number} string or number return value
 */
function foo(a) {
  return a;
}
```
````

<!--

JavaScript doesn't have function overloading but TypeScript does, 

and with the `@overload` signature in JSDoc, you can provide multiple function signatures for a single function.

-->

---
transition: fade
---

# Defining a callback

````md magic-move
```ts
// TypeScript

/**
 * The function foo
 */
type Foo = (a: string, b: boolean) => void
```

```js
// JavaScript

/**
 * The function foo
 * 
 * @callback Foo
 * @param {string} a - The string value
 * @param {boolean} b - The boolean value
 * @returns {void} No return value
 */
```
````

<!--

Sometimes you don't need a function declaration but a function definition,

in those cases you can use the `@callback` tag in JSDoc to create a type of a function to use elsewhere.

-->

---
transition: fade
---

# Assigning `this`

````md magic-move
```ts
// TypeScript

interface Foo {
  /**
   * The value
   */
  value: string;
  /**
   * The method
   */
  method(): void;
}

function bar(this: Foo) {
  this.method();
}
```

```js
// JavaScript

/**
 * @typedef {object} Foo
 * @property {string} value - The value
 * @property {() => void} method - The method
 */

/**
 * @this {Foo}
 */
function bar() {
  this.method();
}
```
````

<!--

If you ever need to bind a function to another object and want type safety for that, 

you can use the `@this` tag in JSDoc. Although you're more likely to use...

-->

---
transition: fade
---

# Classes

````md magic-move
```ts
class Foo {
  protected readonly a: string;

  private b: number;

  public constructor(a: string, b: number) {
    this.a = a;
    this.b = b;
  }
}
```

```js
class Foo {
  /**
   * The A string
   * @type {string}
   * @readonly
   * @protected
   */
  a;

  /**
   * The B number
   * @type {number}
   * @private
   */
  b;

  /**
   * The constructor
   * @param {string} a - The A string
   * @param {number} b - The B number
   */
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}
```
````

<!--

Classes and every access modifier prefix in TypeScript

is also available to use in JSDoc.

-->

---
transition: fade
---

# Implements

````md magic-move
```ts
// TypeScript

interface Bar {
  /**
   * The fuzz method
   */
  fuzz(): void;
}

class Foo implements Bar {
  fuzz() {}
}
```

```js
// JavaScript

/**
 * @typedef {object} Bar
 * @property {() => void} fuzz - The fuzz method
 */

/**
 * @implements {Bar}
 */
class Foo {
  fuzz() {}
}
```
````

<!--

Classes in TypeScript can implement interfaces, 

and you can too in JSDoc with the `@implements` tag.

That's some of the basics, now lets see some more intermediate examples.

-->

---
transition: fade
---

# Casting

````md magic-move
```ts
// TypeScript

const mystery = undefined as unknown

const notMystery = mystery as string
```

```js
// JavaScript

/**
 * @type {unknown}
 */
const mystery = undefined

const notMystery = /** @type {string} */ (mystery)
```
````

<!--

We can cast types in TypeScript with the `as` keyword and likewise,

if we wrap a variable in a type cast in JSDoc, we can do that too.

-->

---
transition: fade
---

# Modifiers

````md magic-move
```ts
// TypeScript

type Bar = {
  stringKey: string
  optionalNumber?: number
  optionalNumber2: number | undefined
  nullableString: string | null
  nullableOptionalString: string | null | undefined
  union: string | number
  nested: {
    stringKey: string
  }
}
```

```js
// JavaScript

/**
 * The bar object
 *
 * @typedef {object} Bar
 * @property {string} stringKey - The string key
 * @property {number=} optionalNumber - The optional number
 * @property {number} [optionalNumber2] - The optional number
 * @property {?string} nullableString - The nullable string
 * @property {?string=} nullableOptionalString - The nullable string
 * @property {string | number} union - The union type
 * @property {object} nested - The nested object
 * @property {string} nested.stringKey - The nested string key
 */
```
````

<!--

Quite a few modifiers here to run through, have a quick look at this definiton. We have two optional numbers defined in two ways, a nullable string, a nullable optional string, a union type and a nested object.

Now let's see how that looks in JSDoc. The optionalNumber can be defined with a shorthand `=` syntax from closure or with square bracket syntax familiar to C#. The nullable string can also be defined with a closure shorthand `?`. The union works just like TypeScript and the nested object allows dot notation to define the nested properties.

-->

---
transition: fade
---

# Type Guards

````md magic-move
```ts
type LiteralString = "bar" | "foo"

function guard(value: LiteralString): value is LiteralString {
  return value === "foo" || value === "bar";
}

function asserts(value: LiteralString): asserts value is LiteralString {
  if (!guard(value)) {
    throw new Error("Invalid value");
  }
}
```

```js
/**
 * @typedef {"bar" | "foo"} LiteralString
 */

/**
 * 
 * @param {unknown} value 
 * @returns {value is LiteralString}
 */
function guard(value) {
  return value === "foo" || value === "bar";
}

/**
 * @param {unknown} value
 * @returns {asserts value is LiteralString} 
 */
function asserts(value) {
  if (!guard(value)) {
    throw new Error("Invalid value");
  }
}
```
````

<!--

Now this is a little advanced, type guards let you narrow a type, these work great in conditional statements, the syntax works just as well with JSDoc as it does in TypeScript and it looks like this.

I also included the asserts type guard because if type guards were esoteric, asserts are even more so and yet they are here working in JSDoc too.

-->

---
transition: fade
---

# Generics

````md magic-move
```ts
// TypeScript

type Foo<T> = {
  /**
   * The value
   */
  value: T;
  /**
   * The promise
   */
  promise: Promise<T>;
}
```

```js
// JavaScript

/**
 * @template T
 * @typedef {object} Foo
 * @property {T} value - The value
 * @property {Promise<T>} promise - The promise
 */
```
````

<!--

Generics are the secret super power of TypeScript and JSDoc have them too. Here we have a generic that takes anything.

And just like that, it's now a generic in JSDoc too.

-->

---
transition: fade
---

# Generic with base type and default

````md magic-move
```ts
type Bar = {
  foo: string
};

type FooBar = {
  foo: string
  bar: string
}

type Foo<T extends Bar = FooBar> = {
  value: T
}
```

```js
/**
 * @typedef {object} Bar
 * @property {string} foo 
 */

/**
 * @typedef {object} FooBar
 * @property {string} foo
 * @property {string} bar
 */

/**
 * @template {Bar} [T = FooBar]
 * @typedef {object} Foo
 * @property {T} value - The value
 */
```
````

<!--

Additionally, we can provide a base type and default type to a generic. If we do not provide a type when we use it, it will use the default and any type we provie must be a subset of the base type.

And in JSDoc, it works just like that.

-->

---
transition: fade
---

# Comment Syntax

```js {monaco}
/**
 * Logs the amount to the console, throws a {@link RangeError} if the amount is less than 0
 * 
 * @param {number} amount - The amount to log
 * @returns {void}
 * @throws {RangeError} If the amount is less than 0, a {@link RangeError} is thrown
 */
function logAmount(amount) {
  if (amount > 0) {
    console.info(`The amount is ${amount}`);
  } else {
    throw new RangeError("amount cannot be less than 0");
  }
}
```

<!--

Lastly I want to show you some comment syntax and what you can do with it, this syntax works in TypeScript and JavaScript comments.

You can use the link syntax to link to a specific type in the comment for easier navigation.

And there's a `@throws` statement. While TypeScript will not warn you about not catching this error, you can include it to help document your code and there's linter rules that can enforce this.

-->

---
transition: fade
---

# Comment Syntax

```js {monaco}
const STRING_CONSTANT = "Hello, World!";

/**
 * @typedef {object} Foo
 * @property {string} bar - [A string constant](#STRING_CONSTANT)
 * @property {string} baz - ![Fine](http://localhost:3030/assets/fine.webp)
 */

/**
 * @type {Foo}
 */
const foo = {};

foo.bar
foo.baz
```

<!--

Some quick additional examples also include linking to a variable in the comment and below that, we have a link to an external image. Hiding images used to be quite a fun easter egg and I feel we've forgotten how to do that especially since...

-->

---
layout: image
image: /assets/babelguy.png
transition: fade
backgroundSize: 60rem
---

<!--

...it used to show up in some unexpected places.

-->

---
layout: section
---

# What's next for JSDoc?

<!--

What about what's next? What new features are in the pipeline?

-->

---
transition: fade
---

# Coming soon to a JSDoc near you

New JSDocs tags

@nonnull

````md magic-move
```ts
// TypeScript

let items = [1, 2, 3, 4, 5];

let item = items.pop()!;
```

```js
// JavaScript

let items = [1, 2, 3, 4, 5];

let item = /** @nonnull */ (items.pop());
```
````

<!--

The `@nonnull` tag lets you omit from the type any nullish value. You can do this right now with TypeScript using a `!` after the value

right now with JSDoc you need to cast the value but coming soon it will be as straightforward as this.

-->

---

# Coming soon to a JSDoc near you

New JSDocs tags

@specialize

````md magic-move
```ts
// TypeScript

const [optionalString, setOptionalString] = useState<string | null>(null);
```

```js
// JavaScript

/**
 * @specialize {string | null}
 */
const [optionalString, setOptionalString] = useState(null);
```
````

<!--

Sometimes you want to pass generic types to a function. With TypeScript you can it like this...

but with JSDoc right now, you need to cast the parameter but soon it will look like this.

-->

---
layout: image
image: /assets/jsdocscheatsheet.png
transition: fade
backgroundSize: 50rem
---

# https://docs.joshuatz.com/cheatsheets/js/jsdoc

<!--

That's just a small tour of current JSDoc syntax, if i've intrigued you enough to give this a shot, a comprehensive guides out there is Joshua's Docs cheatsheet for exploring this world a bit more.

-->

---
transition: fade
---

# All of this to avoid...

<!--

So, there you go. You might not need TypeScript, you can get the types, the squiggly lines, the autocomplete, the documentation and the static analysis...

-->

---
layout: image
image: /assets/tsnode.png
transition: fade
backgroundSize: 40rem
---

# All of this to avoid...

<!--

...without the build process. You can get TypeScript without TypeScript.

And as JSDoc is just comments, i'll leave you with the words of German Mathmatician Karl Weirstrass whose wisdom has found its way into many comments and expresses exactly why you too should type your JavaScript.

-->

---

# All of this to avoid...

```js
                                      }
                                    }
                                  }
                                }
                              }
                            // When I wrote this, only God and I understood what I was doing
                            // Now, God only knows
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      } catch {
        return null;
      }
    }
  }
}
```

<!--

Thank you

-->
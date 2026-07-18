# Lesson 1: Getting Started with TypeScript

## What is TypeScript?

TypeScript is a programming language built on top of JavaScript. It adds **static types** to JavaScript, which means you describe the "shape" of your data (is it a number? a string? an object with certain fields?) and the TypeScript compiler checks that your code respects those shapes *before* it ever runs.

Every valid JavaScript file is already valid TypeScript — TypeScript is a superset of JavaScript. You can adopt it gradually, one file at a time.

Why use it?

- **Catch bugs early.** Typos and mismatched types are flagged while you write code, not when a user hits them in production.
- **Better editor support.** Autocomplete, inline documentation, and safe renaming all work better when the editor knows the types.
- **Self-documenting code.** Function signatures tell you exactly what's expected, without having to read the implementation.

Under the hood, TypeScript code is **compiled** (or "transpiled") into plain JavaScript, which is what actually runs in the browser or Node.js.

## Installing TypeScript

You'll need [Node.js](https://nodejs.org) installed first. Then install the TypeScript compiler globally via npm:

```bash
npm install -g typescript
```

Check that it installed correctly:

```bash
tsc --version
```

## Your First TypeScript File

Create a file called `hello.ts`:

```typescript
function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet("World"));
```

Notice the `: string` annotations — one says the `name` parameter must be a string, the other says the function returns a string.

Compile it to JavaScript:

```bash
tsc hello.ts
```

This produces a `hello.js` file. Run it with Node:

```bash
node hello.js
```

You should see:

```
Hello, World!
```

## Seeing Type Checking in Action

Now try breaking it. Change the call to pass a number instead of a string:

```typescript
console.log(greet(42));
```

Run `tsc hello.ts` again. The compiler will refuse and print an error like:

```
Argument of type 'number' is not assignable to parameter of type 'string'.
```

This is the core value of TypeScript: it stops this kind of mistake before your code ever runs.

## A Note on `tsconfig.json`

Real projects usually have a `tsconfig.json` file that configures how the compiler behaves (which JS version to target, how strict to be, where to put output files, etc.). You can generate a default one with:

```bash
tsc --init
```

We'll keep using single-file compilation for these lessons to keep things simple, but know that `tsconfig.json` is the standard way to configure a real project.

## Exercise

1. Install TypeScript and confirm `tsc --version` works.
2. Create a file `area.ts` with a function `rectangleArea(width: number, height: number): number` that returns the area of a rectangle.
3. Compile and run it, logging the result for a rectangle with width `4` and height `5`.
4. Try calling `rectangleArea("4", 5)` and observe the compiler error.

---

**Next:** [Lesson 2 — Basic Types](02-basic-types.md)

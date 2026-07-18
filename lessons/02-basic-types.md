# Lesson 2: Basic Types

In this lesson, we'll cover the core types you'll use in nearly every TypeScript file.

## Primitive Types

```typescript
let age: number = 30;
let username: string = "andrei";
let isActive: boolean = true;
```

Numbers, strings, and booleans work exactly like in JavaScript — the annotation just tells the compiler what's allowed to be assigned to the variable.

## Type Inference

You don't always need to write annotations. TypeScript can often figure out the type on its own from the initial value:

```typescript
let count = 10; // inferred as number
count = "ten";  // Error: Type 'string' is not assignable to type 'number'.
```

Explicit annotations are most useful for function parameters (which have no initial value to infer from) and for cases where you want to be extra clear about intent.

## Arrays

```typescript
let scores: number[] = [10, 20, 30];
let names: string[] = ["Ann", "Bob"];

// Equivalent generic syntax:
let scores2: Array<number> = [10, 20, 30];
```

## Tuples

A tuple is a fixed-length array where each position has its own type:

```typescript
let point: [number, number] = [10, 20];
let user: [string, number] = ["andrei", 30]; // [name, age]

user[0] = 42; // Error: Type 'number' is not assignable to type 'string'.
```

Tuples are useful when a fixed, ordered set of values has meaning by position.

## Object Types

You can describe the shape of an object inline:

```typescript
let point3D: { x: number; y: number; z: number } = { x: 1, y: 2, z: 3 };
```

This works, but it gets unwieldy fast — in Lesson 3 we'll use `interface` to give shapes like this a reusable name.

## Union Types

A variable can be allowed to hold more than one type using `|`:

```typescript
let id: string | number;
id = "abc123"; // OK
id = 123;      // OK
id = true;     // Error
```

## Literal Types

You can restrict a value to specific literal values, which is great for things like status flags:

```typescript
let status: "pending" | "shipped" | "delivered";
status = "pending";   // OK
status = "cancelled"; // Error: not one of the allowed literals
```

## `any` (and why to avoid it)

`any` opts a value out of type checking entirely:

```typescript
let anything: any = 5;
anything = "now a string"; // no error
anything = true;           // no error
```

`any` is sometimes necessary (e.g., working with untyped third-party data), but overusing it defeats the purpose of TypeScript. Prefer `unknown` when you have a value of an unknown type but still want safety — it forces you to narrow the type before using it.

```typescript
let value: unknown = getDataFromSomewhere();

if (typeof value === "string") {
  console.log(value.toUpperCase()); // OK, TypeScript knows it's a string here
}
```

## `null` and `undefined`

With strict mode enabled (recommended, and the default when you use `tsc --init`), `null` and `undefined` are only assignable to variables that explicitly allow them:

```typescript
let middleName: string | null = null;
```

## Exercise

1. Declare a `let inventory: string[]` and add three item names to it.
2. Declare a tuple `product: [string, number]` representing a product name and price.
3. Declare a union-typed variable `let result: number | string` and assign it a number, then reassign it a string.
4. Declare `let level: "low" | "medium" | "high"` and try assigning an invalid value to see the compiler catch it.

---

**Previous:** [Lesson 1 — Getting Started](01-getting-started.md)
**Next:** [Lesson 3 — Functions and Interfaces](03-functions-and-interfaces.md)

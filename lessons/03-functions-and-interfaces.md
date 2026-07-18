# Lesson 3: Functions and Interfaces

## Typing Function Parameters and Return Values

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

Here, `a` and `b` must be numbers, and the function must return a number. If you omit the return type, TypeScript infers it from the `return` statements — but writing it explicitly is good practice for public functions, since it documents intent and catches mistakes if the implementation changes.

Arrow functions work the same way:

```typescript
const multiply = (a: number, b: number): number => a * b;
```

## Optional and Default Parameters

Mark a parameter optional with `?`. Optional parameters must come after required ones:

```typescript
function greet(name: string, greeting?: string): string {
  return `${greeting ?? "Hello"}, ${name}!`;
}

greet("Ann");            // "Hello, Ann!"
greet("Ann", "Welcome"); // "Welcome, Ann!"
```

Or give a parameter a default value directly, which also makes it optional to callers:

```typescript
function greetWithDefault(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}
```

## Functions Returning Nothing

Use `void` for functions that don't return a meaningful value:

```typescript
function logMessage(message: string): void {
  console.log(message);
}
```

## Typing Functions as Values

You can describe the shape of a function itself, which is useful for callback parameters:

```typescript
function applyOperation(a: number, b: number, operation: (x: number, y: number) => number): number {
  return operation(a, b);
}

applyOperation(5, 3, add); // 8
applyOperation(5, 3, (x, y) => x - y); // 2
```

## Interfaces

An `interface` gives a name to an object shape, so you don't have to repeat inline object types everywhere:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function printUser(user: User): void {
  console.log(`${user.name} <${user.email}>`);
}

const user: User = {
  id: 1,
  name: "Andrei",
  email: "andrei@example.com",
};

printUser(user);
```

If you try to create a `User` missing a required field, or with a field of the wrong type, TypeScript will flag it.

## Optional Properties

Just like function parameters, interface properties can be optional with `?`:

```typescript
interface Product {
  name: string;
  price: number;
  description?: string; // not every product needs one
}

const product: Product = { name: "Mug", price: 9.99 };
```

## Readonly Properties

`readonly` prevents a property from being reassigned after the object is created:

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

const origin: Point = { x: 0, y: 0 };
origin.x = 5; // Error: Cannot assign to 'x' because it is a read-only property.
```

## Interfaces Describing Functions

Interfaces aren't limited to objects with data properties — they can describe a function's call signature too:

```typescript
interface MathOperation {
  (a: number, b: number): number;
}

const subtract: MathOperation = (a, b) => a - b;
```

## Extending Interfaces

Interfaces can build on other interfaces with `extends`:

```typescript
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = { name: "Rex", breed: "Labrador" };
```

## Exercise

1. Write an interface `Book` with `title: string`, `author: string`, and an optional `pages?: number`.
2. Write a function `describeBook(book: Book): string` that returns a sentence describing the book.
3. Write a function `filterBooks(books: Book[], predicate: (book: Book) => boolean): Book[]` that returns only the books matching the predicate.
4. Use `filterBooks` with an inline arrow function to find all books with more than 300 pages.

---

**Previous:** [Lesson 2 — Basic Types](02-basic-types.md)
**Next:** [Lesson 4 — Classes](04-classes.md)

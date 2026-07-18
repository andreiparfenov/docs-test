# Lesson 5: Generics

This is the final lesson — generics are one of TypeScript's most powerful features, letting you write code that works with many types while staying fully type-safe.

## The Problem Generics Solve

Suppose you want a function that returns the first element of an array:

```typescript
function firstElement(arr: any[]): any {
  return arr[0];
}
```

This works, but it loses all type information — `firstElement([1, 2, 3])` returns `any`, not `number`, so you lose autocomplete and type checking on the result.

## A Generic Function

Generics let you capture the input type and reuse it in the output type:

```typescript
function firstElement<T>(arr: T[]): T {
  return arr[0];
}

const num = firstElement([1, 2, 3]);       // T is inferred as number
const str = firstElement(["a", "b", "c"]); // T is inferred as string
```

`T` is a **type parameter** — a placeholder for "whatever type gets passed in." TypeScript infers it automatically from the argument, but you can also specify it explicitly: `firstElement<number>([1, 2, 3])`.

## Generics with Multiple Type Parameters

```typescript
function pair<A, B>(first: A, second: B): [A, B] {
  return [first, second];
}

const p = pair("age", 30); // [string, number]
```

## Generic Interfaces

Interfaces can also be generic, which is useful for describing reusable containers:

```typescript
interface Box<T> {
  contents: T;
}

const numberBox: Box<number> = { contents: 42 };
const stringBox: Box<string> = { contents: "hello" };
```

## Generic Classes

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
console.log(numberStack.pop()); // 2

const nameStack = new Stack<string>();
nameStack.push("Ann");
```

The same `Stack` class works for numbers, strings, or any other type — `T` gets fixed to a concrete type each time you create an instance.

## Constraining Generics

Sometimes you want to allow *any* type, but only if it has certain properties. Use `extends` to constrain a type parameter:

```typescript
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): T {
  console.log(`Length: ${item.length}`);
  return item;
}

logLength("hello");        // OK, strings have .length
logLength([1, 2, 3]);      // OK, arrays have .length
logLength(42);              // Error: number doesn't have .length
```

## Default Type Parameters

Like function parameters, type parameters can have defaults:

```typescript
interface ApiResponse<T = unknown> {
  data: T;
  success: boolean;
}

const response: ApiResponse<string> = { data: "ok", success: true };
const genericResponse: ApiResponse = { data: "anything", success: true }; // T defaults to unknown
```

## Putting It Together

```typescript
interface Repository<T> {
  getById(id: number): T | undefined;
  add(item: T): void;
}

class InMemoryRepository<T extends { id: number }> implements Repository<T> {
  private items: T[] = [];

  getById(id: number): T | undefined {
    return this.items.find((item) => item.id === id);
  }

  add(item: T): void {
    this.items.push(item);
  }
}

interface User {
  id: number;
  name: string;
}

const userRepo = new InMemoryRepository<User>();
userRepo.add({ id: 1, name: "Andrei" });
console.log(userRepo.getById(1)); // { id: 1, name: "Andrei" }
```

This `InMemoryRepository` class can store any type, as long as that type has an `id: number` — a nice combination of generics, constraints, and interfaces, tying together everything from this course.

## Exercise

1. Write a generic function `wrapInArray<T>(value: T): T[]` that returns a single-element array containing the value.
2. Write a generic interface `Pair<A, B>` with `first: A` and `second: B`.
3. Write a generic class `Queue<T>` with `enqueue(item: T): void` and `dequeue(): T | undefined` methods (first in, first out).
4. Write a constrained generic function `logAndReturn<T extends { toString(): string }>(item: T): T` that logs `item.toString()` and returns the item unchanged.

## Where to Go Next

Congratulations on finishing the course! From here, good next steps are:
- Read the [official TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) for deeper coverage of everything introduced here.
- Try adding TypeScript to a small existing JavaScript project.
- Explore utility types like `Partial<T>`, `Pick<T>`, and `Omit<T>`, which build on the generics you just learned.

---

**Previous:** [Lesson 4 — Classes](04-classes.md)

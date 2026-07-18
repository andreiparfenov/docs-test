# Lesson 4: Classes

TypeScript builds on JavaScript's class syntax and adds type annotations, access modifiers, and a few extra features that make object-oriented code safer.

## A Basic Class

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hi, I'm ${this.name} and I'm ${this.age} years old.`;
  }
}

const person = new Person("Andrei", 28);
console.log(person.greet());
```

Class properties are typed just like variables, and methods are typed just like functions.

## Access Modifiers

TypeScript adds `public`, `private`, and `protected` to control where properties and methods can be accessed from:

- `public` (the default) — accessible from anywhere.
- `private` — accessible only from within the class itself.
- `protected` — accessible from within the class and its subclasses.

```typescript
class BankAccount {
  private balance: number;

  constructor(initialBalance: number) {
    this.balance = initialBalance;
  }

  deposit(amount: number): void {
    this.balance += amount;
  }

  getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount(100);
account.deposit(50);
console.log(account.getBalance()); // 150
account.balance;                   // Error: Property 'balance' is private
```

## Shorthand Constructor Properties

Writing `this.name = name` for every property gets repetitive. TypeScript lets you declare and assign properties directly in the constructor signature:

```typescript
class Person {
  constructor(public name: string, private age: number) {}

  greet(): string {
    return `Hi, I'm ${this.name}.`;
  }
}
```

This is equivalent to the longer version from the first example — `public name: string` in the constructor parameters both declares the `name` property and assigns it.

## Readonly Properties

Just like with interfaces, `readonly` prevents reassignment after construction:

```typescript
class Configuration {
  readonly apiUrl: string;

  constructor(apiUrl: string) {
    this.apiUrl = apiUrl;
  }
}

const config = new Configuration("https://api.example.com");
config.apiUrl = "https://other.com"; // Error
```

## Inheritance

Classes can extend other classes with `extends`, inheriting their properties and methods:

```typescript
class Animal {
  constructor(public name: string) {}

  move(distance: number): void {
    console.log(`${this.name} moved ${distance}m.`);
  }
}

class Dog extends Animal {
  bark(): void {
    console.log(`${this.name} says woof!`);
  }
}

const dog = new Dog("Rex");
dog.move(10); // inherited from Animal
dog.bark();   // defined on Dog
```

## Overriding Methods with `super`

A subclass can override a parent method, and call the parent's version using `super`:

```typescript
class Cat extends Animal {
  move(distance: number): void {
    console.log(`${this.name} creeps...`);
    super.move(distance);
  }
}

new Cat("Whiskers").move(2);
// "Whiskers creeps..."
// "Whiskers moved 2m."
```

## Implementing Interfaces

A class can promise to match a given interface using `implements`. TypeScript checks that the class provides everything the interface requires:

```typescript
interface Greetable {
  name: string;
  greet(): string;
}

class Employee implements Greetable {
  constructor(public name: string, public role: string) {}

  greet(): string {
    return `Hi, I'm ${this.name}, working as ${this.role}.`;
  }
}
```

## Exercise

1. Write a class `Shape` with a `protected` property `name: string` set via the constructor, and a method `describe(): string`.
2. Write a class `Circle extends Shape` with a `private radius: number`, and an `area(): number` method (`Math.PI * radius ** 2`).
3. Write an interface `Describable` with a `describe(): string` signature, and make `Shape` implement it.
4. Create a `Circle` instance and call both `describe()` and `area()` on it.

---

**Previous:** [Lesson 3 — Functions and Interfaces](03-functions-and-interfaces.md)
**Next:** [Lesson 5 — Generics](05-generics.md)

# TypeScript Interview Questions (2025–2026)

### Table of Contents
1. [Advantages of using TypeScript over JavaScript?](#1-advantages-of-using-typescript-over-javascript)
2. [Difference between any, unknown, and never types?](#2-difference-between-any-unknown-and-never-types)
3. [How do you define an interface in TypeScript?](#3-how-do-you-define-an-interface-in-typescript)
4. [What is Type Inference?](#4-what-is-type-inference)
5. [Difference between interface and type](#5-difference-between-interface-and-type)
6. [What is a union type?](#6-what-is-a-union-type)
7. [What are generics in TypeScript?](#7-what-are-generics-in-typescript)

## 1. **Advantages of using TypeScript over JavaScript?**
- Static type checking

- Better tooling (e.g., autocompletion, refactoring)

- Improved code readability and maintainability

- Early error detection

- Supports modern JavaScript features and transpiles to older versions



## 2. **Difference between any, unknown, and never types?**

### ***`any` - Opt-out of type checking (unsafe)***
```jsx
let data: any;

data = 42;
data = "hello";
data = true;

console.log(data.toUpperCase()); // ❌ No error at compile time, but can fail at runtime
```

### ***`unknown` – Safe version of any (must be type-checked before use)***
```jsx
let value: unknown;

value = 100;
value = "hello";

// TypeScript error: Property 'toUpperCase' does not exist on type 'unknown'
// console.log(value.toUpperCase()); ❌

if (typeof value === "string") {
  console.log(value.toUpperCase()); ✅ // Type narrowed to string
}
```

### ***`never` – For functions that never return or unreachable code***

```jsx
// Example 1: Function that always throws
function throwError(message: string): never {
  throw new Error(message);
}

// Example 2: Function with infinite loop
function infiniteLoop(): never {
  while (true) {
    // Do something forever
  }
}

// Example 3: Exhaustiveness checking
type Shape = "circle" | "square";

function getArea(shape: Shape) {
  switch (shape) {
    case "circle":
      return "πr²";
    case "square":
      return "a²";
    default:
      const _exhaustiveCheck: never = shape; // ✅ catches unhandled cases at compile time
      return _exhaustiveCheck;
  }
}
```
<br>

## 3. **How do you define an interface in TypeScript?**
```jsx
interface User {
  name: string;
  age: number;
  isAdmin?: boolean; // optional
}
```
<br>
  
## 4. **What is Type Inference?**
Type inference is TypeScript’s ability to automatically determine the type of a variable or expression without explicitly specifying it.

When you assign a value to a variable, TypeScript infers the type based on the value.

```jsx
let message = "Hello, TypeScript";
// TypeScript infers: message is of type string

// This will throw a compile-time error:
message = 123; 
// ❌ Error: Type 'number' is not assignable to type 'string'
```


## 5. **Difference between interface and type?**
Both define object shapes.

interface is extendable and ideal for OOP-style code.

type is more flexible, can represent unions, tuples, primitives, etc.

Interfaces can be merged; types cannot.

### ***interface Example***
```jsx
interface User {
  name: string;
  age: number;
}

interface Admin extends User {
  isAdmin: boolean;
}
```

### ***type Example***
```jsx
type User = {
  name: string;
  age: number;
};

type Admin = User & {
  isAdmin: boolean;
};
```
🧠 Summary
- Use **interface** when defining object shapes, especially for public APIs and class structures.
- Use **type** when combining types (e.g., unions, intersections, primitives).
  
<br>

## 6. **What is a union type?**
Allows a variable to hold multiple types

```jsx
let value: string | number;
value = "text";
value = 10;
```
<br>

## 7. **What are generics in TypeScript?**
Generics allow you to create reusable and type-safe components, functions, or classes that work with different data types without losing type information.

Think of generics like placeholders for types.

### *** Example: Generic Function***
```jsx
function identity<T>(value: T): T {
  return value;
}

const numberResult = identity<number>(10);      // T = number
const stringResult = identity<string>("hello"); // T = string

```

📌 T is a type variable – it holds the place for the actual type passed during function call.

### ***Without Generics (less type-safe)***
```jsx
function identity(value: any): any {
  return value;
}

const result = identity("hello");
// TypeScript has no idea it's a string — no autocomplete or checks

```

### ***Generic Array Example***
```jsx
function getFirstElement<T>(arr: T[]): T {
  return arr[0];
}

const firstNumber = getFirstElement([1, 2, 3]);       // T = number
const firstString = getFirstElement(["a", "b", "c"]); // T = string

```

### ***Generic Interface Example***
```jsx
interface Box<T> {
  content: T;
}

const stringBox: Box<string> = { content: "books" };
const numberBox: Box<number> = { content: 123 };

```

<br>

## 8. **What are mapped types?**

<br>

## 9. **What is keyof and typeof in TypeScript?**

<br>

## 10. **What is declaration merging in TypeScript?**

<br>


# TypeScript Interview Questions (2025–2026)

### Table of Contents
1. [Advantages of using TypeScript over JavaScript?](#1-advantages-of-using-typescript-over-javascript)
2. [Difference between any, unknown, and never types?](#2-difference-between-any-unknown-and-never-types)
3. [How do you define an interface in TypeScript?](#3-how-do-you-define-an-interface-in-typescript)
4. [What is Type Inference?](#4-what-is-type-inference)
5. [Difference between interface and type](#5-difference-between-interface-and-type)
6. [What is a union type?](#6-what-is-a-union-type)
7. [What are generics in TypeScript?](#7-what-are-generics-in-typescript)
8. [What are mapped types?](#8-what-are-mapped-types)
9. [What is keyof and typeof in TypeScript?](#9-what-is-keyof-and-typeof-in-typescript)
10. [What is declaration merging in TypeScript?](#10-what-is-declaration-merging-in-typescript)
11. [What are union and intersection types?](#11-what-are-union-and-intersection-types)
12. [Difference between readonly and const?](#12-difference-between-readonly-and-const)
13. [What is as type assertion?](#13-what-is-as-type-assertion)
14. [Example](#14-example)
15. [Example](#15-example)

## 1. **Advantages of using TypeScript over JavaScript?**

- ***Static type checking*** : Ensures variables and functions use the correct data types at compile time.
- ***Better tooling (e.g., autocompletion, refactoring)*** : Provides powerful IDE support with smart suggestions and safe code transformations.

- ***Improved code readability and maintainability*** : Makes code easier to understand and manage by clearly defining data structures and intent.

- ***Early error detection*** : Catches bugs during development before the code runs in production.

- ***Safer refactoring*** : Rename a variable or change a function signature and TypeScript tells you everywhere it breaks. No guessing, no fear.


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


## 3. **How do you define an interface in TypeScript?**
```jsx
interface User {
  name: string;
  age: number;
  isAdmin?: boolean; // optional
}
```

  
## 4. **What is Type Inference?**

***Type inference*** is TypeScript’s ability to automatically determine the type of a variable or expression without explicitly specifying it.

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
  
- Interfaces allow ***Declaration Merging***. If you define two interfaces with the same name, TypeScript automatically merges them into one.
- ***Types*** do not allow this and will throw an error if you try to define the same name twice.

```jsx
// Interfaces merge
interface User { name: string; }
interface User { age: number; }
// User now has both name and age

// Types do not merge
type Admin = { name: string; }
type Admin = { role: string; } // Error: Duplicate identifier 'Admin'
```

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
***Generics*** are a tool for creating reusable components. They allow you to write code that works with a variety of types rather than a single one, without losing the benefits of type safety.

Think of generics like placeholders for types.

### *** Example: Generic Function***
```jsx
function identity<T>(value: T): T {
  return value
}

identity<number>(10)
identity<string>("hello")
```

1️⃣ <T> - Declares a generic type parameter named T.
2️⃣ value: T - Specifies that the parameter value must be of type T.
3️⃣ : T - Specifies that the return value will also be of type T.

***<T> defines the generic type, value: T accepts that type, and : T returns the same type.***

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

<br>

## 8. **What are mapped types?**

Mapped types let you create new types by looping over the keys of an existing type and transforming them.

***normal type***
```jsx
type User = {
  id: number
  name: string
}
```

***Mapped type***
```jsx
type ReadOnlyUser = {
  readonly [K in keyof User]: User[K]
}
```

***Read it in plain English***
- `keyof User` → "id" | "name"
- `K in keyof User` → “for each key in User”
- `User[K]` → “use the same type as original”
- `readonly` → “make it read-only”

***👉 Result***
```jsx
{
  readonly id: number
  readonly name: string
}
```

<br>

## 9. **What is keyof and typeof in TypeScript?**

`typeof` is used in a type context to extract the type of a variable or property so you can use it elsewhere.

```jsx
const point = { x: 10, y: 20 };

// Create a type that matches the shape of 'point'
type Point = typeof point; 

/* Resulting Type:
type Point = { x: number; y: number; }
*/

function move(p: Point) {
    console.log(`Moving to ${p.x}, ${p.y}`);
}
```
`keyof` operator takes an object type and produces a string or numeric literal union of its keys.

***When to use it:*** When you want to ensure a value matches one of the property names of an object.

```jsx
interface Car {
    make: string;
    model: string;
    year: number;
}

// Extract the keys of Car
type CarProperty = keyof Car; 

// Resulting Type: "make" | "model" | "year"

let myProperty: CarProperty = "make"; // Valid
// let myProperty: CarProperty = "color"; // Error! "color" doesn't exist on Car.
```

## 10. **What is declaration merging in TypeScript?**

Declaration merging means TypeScript automatically combines multiple declarations with the same name into a single definition.

```jsx
interface User {
  name: string;
}

interface User {
  age: number;
}

// Resulting merged interface:
// interface User { name: string; age: number; }

const person: User = {
  name: "Alex",
  age: 30
};
```

## 11. **What are union and intersection types?**

- Union (|) allows multiple types
- Intersection (&) combines multiple types

```jsx
type Status = "success" | "error"

type User = { name: string }
type Admin = { role: string }

type AdminUser = User & Admin
```

## 12. **Difference between readonly and const?**

- `const` prevents variable reassignment
- `readonly` prevents property modification

```jsx
const x = 10

interface User {
  readonly id: number
}
```

## 13. **What is as type assertion?**

Forces TypeScript to treat a value as a specific type.

```jsx
let value: unknown = "hello"
let str = value as string

```


## 14. **Example**

***Without TypeScript (JavaScript)***
```jsx
//User Login Function
function login(user) {
  console.log(user.email.toLowerCase())
}

login({ email: 123 }) // Runtime error

```
❌ Error happens only at runtime because email is not a string.

***With TypeScript***
```jsx
type User = {
  email: string
  password: string
}

function login(user: User) {
  console.log(user.email.toLowerCase())
}

login({ email: 123, password: "abc" }) // Compile-time error

```
✅ TypeScript catches the mistake before running the code.
## 15. **Example**

***Problem in JavaScript***
```jsx
//API Response Handling
fetch("/api/user")
  .then(res => res.json())
  .then(data => {
    console.log(data.name.toUpperCase())
  })

```
If name is missing or not a string → ❌ runtime error.

***With TypeScript***
```jsx
type ApiUser = {
  id: number
  name: string
  email: string
}

async function getUser(): Promise<ApiUser> {
  const res = await fetch("/api/user")
  return res.json()
}

getUser().then(user => {
  console.log(user.name.toUpperCase()) // safe
})

```

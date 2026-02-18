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
14. [API](#14-api)
15. [Example](#15-example)


## 1. **Advantages of using TypeScript over JavaScript?**

| Feature                  | TypeScript                                      | JavaScript                             |
| ------------------------ | ----------------------------------------------- | -------------------------------------- |
| **Type Safety**          | Static typing catches errors at compile time    | Errors found only at runtime           |
| **Error Detection**      | Detects type errors before running code         | Errors appear when code executes       |
| **Autocompletion**       | Better IntelliSense and suggestions             | Limited editor support                 |
| **Code Maintainability** | Easier to manage large codebases                | Harder to maintain as project grows    |
| **Refactoring**          | Safer refactoring with type checks              | Risky changes, may break things        |
| **Interfaces & Types**   | Supports interfaces, type aliases, generics     | No built-in type system                |
| **Scalability**          | Designed for large scale applications           | Better suited for small to medium apps |
| **Documentation**        | Types act as self documentation                 | Requires comments for clarity          |
| **OOP Features**         | Access modifiers, abstract classes, enums       | Limited OOP structure                  |
| **Modern JS Support**    | Transpiles modern features to older JS versions | Depends on browser/node support        |

<br>

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
  readonly id: number; // You cannot change id after assignment
}
```
### 🔹  Interface for Function
```jsx
interface AddFunction {
  (a: number, b: number): number;
}

const add: AddFunction = (x, y) => x + y;
```

### 🔹 Interface with React Props
```jsx
interface ButtonProps {
  label: string;
  onClick: () => void;
}

function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>;
}
```

### 🔹 Extending Interface
```jsx
interface Person {
  name: string;
}

interface Employee extends Person {
  salary: number;
}

const emp: Employee = {
  name: "Vishal",
  salary: 50000,
};
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

## typeof
`typeof` is used to get the type of an existing variable, object, or function so it can be reused as a type.

***Get type of a variable***
```jsx
const count = 10
type CountType = typeof count // number
```

***Get type of a function***
```jsx
function add(a: number, b: number) {
  return a + b
}

type AddType = typeof add
// (a: number, b: number) => number

```
***typeof*** in JavaScript gives the runtime type

## keyof
The ***keyof*** operator takes an object type and produces a union of its keys. 

Think of it as a way to say: "This variable must be one of the property names from that object."


```jsx
type User = {
  id: number
  name: string
  isActive: boolean
}
```

```jsx
function getValue(obj: User, key: keyof User) {
  return obj[key]
}

getValue(user, "name")      // valid
getValue(user, "email")    // error ❌
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

***Union***
```jsx
type Status = "success" | "error"
```

***Intersection***
```jsx
type Person = {  name: string}
type Employee = { id: number}

type PersonEmployee = Person & Employee
```
***Result***
```jsx
{
  name: string
  id: number
}
```

<br>

## 12. **Difference between readonly and const?**

- `const` prevents variable reassignment
- `readonly` prevents property modification

```jsx
const x = 10

interface User {
  readonly id: number
}
```

<br>

## 13. **What is as type assertion?**

Forces TypeScript to treat a value as a specific type.

```jsx
let value: any = "Hello"
let length = (value as string).length
```
Here we tell TypeScript:
👉 “value is a string.”

```jsx
let str = value as string
```
## 14. **API**
let’s build a simple React + TypeScript API example step by step.

1️⃣ Simple API Fetch Example (useEffect)
```jsx
import React, { useEffect, useState } from "react";

interface User {
  id: number;
  name: string;
  email: string;
  phone: string;
}

export default function Users() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data: User[] = await response.json();
        setUsers(data);
      } catch (err) {
        setError((err as Error).message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

```

2️⃣ Better Pattern: Create API Function Separately

```jsx
export async function getUsers(): Promise<User[]> {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  return response.json();
}
```

🚀 Even Better: Generic Fetch Utility
```jsx
export async function fetchData<T>(url: string): Promise<T> {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error("API Error");
  }

  return response.json();
}

```
Usage:
```jsx
const users = await fetchData<User[]>(url);
```
### 🚀 Real API Example
```jsx
interface ApiResponse {
  id: number;
  name: string;
  email: string;
}

async function fetchUser(): Promise<ApiResponse> {
  const res = await fetch("/api/user");
  return res.json();
}
```
## 15. **Example**

### ***Example 1***

***Without TypeScript (JavaScript)***
```jsx
function addNumbers(a, b) {
  return a + b;
}

console.log(addNumbers(5, 10)); // 15
console.log(addNumbers("5", 10)); // "510" (Wait, what?)
```


***With TypeScript***
```jsx
function addNumbers(a: number, b: number): number {
  return a + b;
}

console.log(addNumbers(5, 10)); // 15
// console.log(addNumbers("5", 10)); // ERROR: Argument of type 'string' is not assignable to 'number'.
```
✅ TypeScript catches the mistake before running the code.


### ***Example 2***

## 📁 Project Structure
```jsx
my-app/
│
├── src/
│   ├── types.ts
│   ├── greet.ts
│   └── index.ts
│
├── package.json
└── tsconfig.json

```

## 1️⃣ src/types.ts
```jsx
// Union Type
export type Role = "admin" | "user";

export type User = {
  name: string;
  age: number;
  role: Role;   // using union here
};
```

## 2️⃣ src/greet.ts
```jsx
import { User } from "./types";

export function greet(user: User): string {
  if (user.role === "admin") {
    return `Welcome Admin ${user.name}`;
  }

  return `Hello ${user.name}, you are ${user.age} years old.`;
}
```

## 3️⃣ src/index.ts
```jsx
import { greet } from "./greet";
import { User } from "./types";

const person: User = {
  name: "Vishal",
  age: 25,
  role: "admin"   // must be "admin" or "user"
};

console.log(greet(person));
```

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
11. [Intersection types?](#11-intersection-types)
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

Type Inference means:

- 👉 TypeScript automatically guesses the type
- 👉 You don’t have to explicitly write it

When you assign a value to a variable, TypeScript infers the type based on the value.

🔹 1️⃣ Basic Example
```jsx
let name = "Vishal";
```
TypeScript infers:
```jsx
let name: string
```
You did not write string manually.

If you now do:
```jsx
name = 10;
```
❌ Error

Because TypeScript already inferred it as string.


### 🔹 Function Return Type Inference
```jsx
function add(a: number, b: number) {
  return a + b;
}
```
TypeScript automatically infers:
```jsx
function add(a: number, b: number): number
```

## 5. **Difference between interface and type?**

### 🔹 A. Declaration Merging

### ***interface Example***
```jsx
interface User {
  name: string;
}

interface User {
  age: number;
}
```
This becomes:
```jsx
{
  name: string;
  age: number;
}
```

You cannot do this with type ❌

```jsx
type User = { name: string };
type User = { age: number }; // Error
```

### 🔹 B. Extending

### ***interface extends***
```jsx
interface Person {
  name: string;
}

interface Employee extends Person {
  salary: number;
}
```

### ***Type extends using intersection***
```jsx
type Person = {
  name: string;
};

type Employee = Person & {
  salary: number;
};
```

### 🔹 C. Union Types

```jsx
type Status = "success" | "error" | "loading";
```

You cannot do this with interface.


### 🔹 D. Tuples
Only type supports tuples cleanly:

```jsx
type Coordinates = [number, number];
```

<br>

## 6. **What is a union type?**
Allows a variable to hold multiple types

### 🔹 1️⃣ Basic Example
```jsx
let id: string | number;

id = 10;        // ✅
id = "ABC123";  // ✅
id = true;      // ❌ Error
```

### 🔹 2️⃣ String Literal Union
```jsx
type Status = "success" | "error" | "loading";

let currentStatus: Status = "success";  // ✅
let s: Status = "pending";              // ❌ Error
```

### 🔹 3️⃣ Union with Objects
```jsx
type SuccessResponse = {
  status: "success";
  data: string;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

type ApiResponse = SuccessResponse | ErrorResponse;
```

### 4️⃣ Union in Function Parameter
```jsx
function printId(id: string | number) {
  console.log(id);
}
```

### 🔥 5️⃣ Real React Example
```jsx
type Status = "loading" | "success" | "error";

interface ApiState {
  status: Status;
  data?: string;
  error?: string;
}
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

- 1️⃣ <T> - Declares a generic type parameter named T.
- 2️⃣ value: T - Specifies that the parameter value must be of type T.
- 3️⃣ : T - Specifies that the return value will also be of type T.

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

A mapped type allows you to:

- 👉 Create a new type
- 👉 By transforming properties of an existing type
- It loops over keys of a type.

### 1️⃣ Basic Example
***normal type***
```jsx
type User = {
  name: string;
  age: number;
};
```
Now suppose we want all properties optional.

Instead of rewriting:

```jsx
type OptionalUser = {
  name?: string;
  age?: number;
};
```

We can use mapped type:
***Mapped type***
```jsx
type OptionalUser = {
  [Key in keyof User]?: User[Key];
};
```
✅ Automatically makes all properties optional.


***Read it in plain English***
- `keyof User` → "name" | "age"
- `K in keyof User` → “for each key in User”
- `User[K]` → “use the same type as original”
- `?` → “make it optional”


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

## 11. **intersection types**

- 👉 Combine multiple types
- 👉 The result must satisfy ALL types
- 👉 It uses the & operator

🔹 1️⃣ Basic Example
```jsx
type Person = {
  name: string;
};

type Employee = {
  salary: number;
};

type Staff = Person & Employee;
```

Now Staff must have BOTH:
```jsx
const user: Staff = {
  name: "Vishal",
  salary: 50000
};

```
If you miss one:
```jsx
const user: Staff = {
  name: "Vishal"
};

```
❌ Error

Because salary is missing.

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

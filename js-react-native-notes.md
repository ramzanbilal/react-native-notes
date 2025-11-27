# JavaScript Study Notes for React Native - Complete Guide

## Table of Contents

1. [Introduction](#introduction)
2. [JavaScript vs TypeScript](#javascript-vs-typescript)
3. [Data Types](#data-types)
   - [Primitive Data Types](#primitive-data-types)
   - [Non-Primitive Data Types](#non-primitive-data-types)
4. [Variables: var, let, const](#variables-var-let-const)
5. [Operators](#operators)
6. [Conditional Statements](#conditional-statements)
7. [Functions](#functions)
   - [Traditional Functions](#traditional-functions)
   - [Arrow Functions](#arrow-functions)
8. [Arrays](#arrays)
9. [Array Methods](#array-methods)
10. [Loops](#loops)
11. [Sets](#sets)
12. [Optional Chaining & Nullish Coalescing](#optional-chaining--nullish-coalescing)
13. [Functional Components in React Native](#functional-components-in-react-native)
14. [Why Functions Are Better Than Classes](#why-functions-are-better-than-classes)
15. [JavaScript's Shift to Function-Based Programming](#javascripts-shift-to-function-based-programming)

---

## Introduction

This guide provides comprehensive JavaScript fundamentals specifically tailored for React Native development, with real-world examples in both JavaScript and TypeScript.

---

## JavaScript vs TypeScript

### Key Differences

| Feature             | JavaScript                | TypeScript                            |
| ------------------- | ------------------------- | ------------------------------------- |
| **Type System**     | Dynamic (runtime)         | Static (compile-time)                 |
| **Type Checking**   | No built-in type checking | Strong type checking                  |
| **Compilation**     | Interpreted directly      | Compiled to JavaScript                |
| **Error Detection** | Runtime errors            | Compile-time errors                   |
| **IDE Support**     | Basic                     | Advanced (IntelliSense, autocomplete) |
| **Learning Curve**  | Easier                    | Steeper                               |
| **Code Verbosity**  | Less verbose              | More verbose                          |
| **Refactoring**     | Risky                     | Safer                                 |

### Which One Is Better and Why?

**TypeScript is generally better for large-scale React Native projects** because:

1. **Early Error Detection**: Catches bugs during development, not in production
2. **Better IDE Support**: Enhanced autocomplete and IntelliSense
3. **Self-Documenting Code**: Types serve as inline documentation
4. **Safer Refactoring**: Compiler catches breaking changes
5. **Better Team Collaboration**: Clear contracts between code modules
6. **Scalability**: Easier to maintain large codebases

**JavaScript is better for**:

- Small projects or prototypes
- Quick experiments
- Developers new to programming
- When team doesn't know TypeScript

---

## Data Types

### Primitive Data Types

Primitive data types are immutable and stored directly in memory.

#### String, Number, Boolean Examples

**JavaScript:**

```javascript
import { Text, View } from "react-native";
import { useState } from "react";

const DataTypesDemo = () => {
  const name = "John Doe";
  const age = 30;
  const isActive = true;
  const score = 0;
  const message = `User ${name} is ${age} years old`;

  return (
    <View>
      <Text>Name: {name}</Text>
      <Text>Age: {age}</Text>
      <Text>Active: {isActive ? "Yes" : "No"}</Text>
      <Text>Score: {score}</Text>
      <Text>{message}</Text>
    </View>
  );
};
```

**TypeScript:**

```typescript
import { Text, View } from "react-native";
import { FC } from "react";

const DataTypesDemo: FC = () => {
  const name: string = "John Doe";
  const age: number = 30;
  const isActive: boolean = true;
  const score: number = 0;
  const message: string = `User ${name} is ${age} years old`;

  return (
    <View>
      <Text>Name: {name}</Text>
      <Text>Age: {age}</Text>
      <Text>Active: {isActive ? "Yes" : "No"}</Text>
      <Text>Score: {score}</Text>
      <Text>{message}</Text>
    </View>
  );
};
```

### Non-Primitive Data Types

#### Objects and Arrays

**JavaScript:**

```javascript
import { Text, View, FlatList } from "react-native";

const UserProfile = () => {
  const user = {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
    hobbies: ["Reading", "Coding", "Gaming"],
  };

  const products = [
    { id: 1, name: "Laptop", price: 999 },
    { id: 2, name: "Phone", price: 599 },
  ];

  return (
    <View>
      <Text>User: {user.name}</Text>
      <Text>Hobbies: {user.hobbies.join(", ")}</Text>
      <FlatList
        data={products}
        keyExtractor={(item) => item.id.toString()}
        renderItem={({ item }) => (
          <Text>
            {item.name}: ${item.price}
          </Text>
        )}
      />
    </View>
  );
};
```

**TypeScript:**

```typescript
import { Text, View, FlatList } from "react-native";
import { FC } from "react";

interface User {
  id: number;
  name: string;
  email: string;
  hobbies: string[];
}

interface Product {
  id: number;
  name: string;
  price: number;
}

const UserProfile: FC = () => {
  const user: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
    hobbies: ["Reading", "Coding", "Gaming"],
  };

  const products: Product[] = [
    { id: 1, name: "Laptop", price: 999 },
    { id: 2, name: "Phone", price: 599 },
  ];

  return (
    <View>
      <Text>User: {user.name}</Text>
      <Text>Hobbies: {user.hobbies.join(", ")}</Text>
      <FlatList
        data={products}
        keyExtractor={(item: Product) => item.id.toString()}
        renderItem={({ item }: { item: Product }) => (
          <Text>
            {item.name}: ${item.price}
          </Text>
        )}
      />
    </View>
  );
};
```

---

## Variables: var, let, const

### Comparison Table

| Feature              | var                 | let           | const         |
| -------------------- | ------------------- | ------------- | ------------- |
| **Scope**            | Function-scoped     | Block-scoped  | Block-scoped  |
| **Hoisting**         | Hoisted (undefined) | Hoisted (TDZ) | Hoisted (TDZ) |
| **Re-declaration**   | Allowed             | Not allowed   | Not allowed   |
| **Re-assignment**    | Allowed             | Allowed       | Not allowed   |
| **Use in Modern JS** | Avoided             | Common        | Preferred     |

**JavaScript Example:**

```javascript
import { Text, View, Button } from "react-native";
import { useState } from "react";

const VariablesDemo = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    let newCount = count;
    newCount = newCount + 1;
    setCount(newCount);
  };

  const API_URL = "https://api.example.com";
  const user = { name: "John" };
  user.name = "Jane"; // Allowed - object properties can change

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={increment} />
      <Text>API: {API_URL}</Text>
      <Text>User: {user.name}</Text>
    </View>
  );
};
```

---

## Operators

**JavaScript Example:**

```javascript
const a = 10,
  b = 5;

// Arithmetic
const sum = a + b; // 15
const product = a * b; // 50
const remainder = a % 3; // 1

// Comparison
const isEqual = a === b; // false
const isGreater = a > b; // true

// Logical
const and = true && false; // false
const or = true || false; // true

// Ternary
const status = a > b ? "Greater" : "Smaller"; // 'Greater'
```

---

## Conditional Statements

**JavaScript Example:**

```javascript
import { Text, View } from "react-native";

const WeatherApp = () => {
  const temp = 25;

  let message;
  if (temp > 30) {
    message = "It's hot!";
  } else if (temp > 20) {
    message = "It's pleasant!";
  } else {
    message = "It's cold!";
  }

  const day = "Monday";
  let schedule;
  switch (day) {
    case "Monday":
    case "Wednesday":
      schedule = "Gym Day";
      break;
    case "Saturday":
      schedule = "Rest Day";
      break;
    default:
      schedule = "Work Day";
  }

  return (
    <View>
      <Text>{message}</Text>
      <Text>
        {day}: {schedule}
      </Text>
    </View>
  );
};
```

---

## Functions

### Traditional Functions

**JavaScript:**

```javascript
function add(a, b) {
  return a + b;
}

function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
```

**TypeScript:**

```typescript
function add(a: number, b: number): number {
  return a + b;
}

function greet(name: string = "Guest"): string {
  return `Hello, ${name}!`;
}

function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}
```

### Arrow Functions

**JavaScript:**

```javascript
const square = (x) => x * x;
const multiply = (a, b) => a * b;
const calculate = (op, x, y) => op(x, y);

const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((n) => n * 2);
const evens = numbers.filter((n) => n % 2 === 0);
```

**TypeScript:**

```typescript
const square = (x: number): number => x * x;
const multiply = (a: number, b: number): number => a * b;

type Operation = (a: number, b: number) => number;
const calculate = (op: Operation, x: number, y: number): number => op(x, y);

const numbers: number[] = [1, 2, 3, 4, 5];
const doubled: number[] = numbers.map((n: number) => n * 2);
const evens: number[] = numbers.filter((n: number) => n % 2 === 0);
```

### Function vs Arrow Function

| Feature            | Traditional Function | Arrow Function          |
| ------------------ | -------------------- | ----------------------- |
| **Syntax**         | `function name() {}` | `const name = () => {}` |
| **`this` binding** | Has its own `this`   | Inherits `this`         |
| **Constructor**    | Can be constructor   | Cannot                  |
| **Best for**       | Methods              | Callbacks               |

---

## Arrays

**JavaScript:**

```javascript
const fruits = ["Apple", "Banana", "Orange"];
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "Hello", true, { name: "John" }];

const firstFruit = fruits[0];
const lastFruit = fruits[fruits.length - 1];
const moreFruits = [...fruits, "Mango"];

const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];
```

---

## Array Methods

**map() - Transform Elements:**

```javascript
import { FlatList, Text } from "react-native";

const ProductList = () => {
  const products = [
    { id: 1, name: "Laptop", price: 999 },
    { id: 2, name: "Phone", price: 599 },
  ];

  const withTax = products.map((p) => ({
    ...p,
    priceWithTax: p.price * 1.1,
  }));

  return (
    <FlatList
      data={withTax}
      keyExtractor={(item) => item.id.toString()}
      renderItem={({ item }) => (
        <Text>
          {item.name}: ${item.priceWithTax.toFixed(2)}
        </Text>
      )}
    />
  );
};
```

**filter() - Filter Elements:**

```javascript
const users = [
  { name: "John", age: 28, active: true },
  { name: "Jane", age: 34, active: false },
  { name: "Bob", age: 45, active: true },
];

const activeUsers = users.filter((u) => u.active);
const over30 = users.filter((u) => u.age > 30);
```

**reduce() - Reduce to Single Value:**

```javascript
const cartItems = [
  { name: "Shirt", price: 29.99, qty: 2 },
  { name: "Jeans", price: 59.99, qty: 1 },
];

const total = cartItems.reduce((sum, item) => sum + item.price * item.qty, 0);
```

**Other Methods:**

```javascript
const nums = [1, 2, 3, 4, 5];

nums.find((n) => n > 3); // 4
nums.findIndex((n) => n > 3); // 3
nums.some((n) => n % 2 === 0); // true
nums.every((n) => n > 0); // true
nums.includes(3); // true
nums.slice(1, 4); // [2, 3, 4]
nums.concat([6, 7]); // [1,2,3,4,5,6,7]
```

---

## Loops

### for Loop

```javascript
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

### for...of (Values)

```javascript
const fruits = ["Apple", "Banana", "Orange"];
for (const fruit of fruits) {
  console.log(fruit);
}
```

### for...in (Keys)

```javascript
const user = { name: "John", age: 30 };
for (const key in user) {
  console.log(`${key}: ${user[key]}`);
}
```

### while Loop

```javascript
let count = 5;
while (count > 0) {
  console.log(count);
  count--;
}
```

### forEach

```javascript
const tasks = ["Task 1", "Task 2", "Task 3"];
tasks.forEach((task, index) => {
  console.log(`${index + 1}. ${task}`);
});
```

---

## Sets

**JavaScript:**

```javascript
import { Text, View } from "react-native";
import { useState } from "react";

const TagsDemo = () => {
  const [tags, setTags] = useState(new Set(["React", "JavaScript"]));

  // Remove duplicates
  const nums = [1, 2, 2, 3, 4, 4, 5];
  const unique = [...new Set(nums)];

  const addTag = (tag) => {
    setTags((prev) => new Set([...prev, tag]));
  };

  const removeTag = (tag) => {
    setTags((prev) => {
      const newTags = new Set(prev);
      newTags.delete(tag);
      return newTags;
    });
  };

  return (
    <View>
      {[...tags].map((tag) => (
        <Text key={tag}>• {tag}</Text>
      ))}
      <Text>Size: {tags.size}</Text>
      <Text>Unique: {unique.join(", ")}</Text>
    </View>
  );
};
```

**Set Operations:**

```javascript
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

// Union
const union = new Set([...setA, ...setB]);

// Intersection
const intersection = new Set([...setA].filter((x) => setB.has(x)));

// Difference
const difference = new Set([...setA].filter((x) => !setB.has(x)));
```

---

## Optional Chaining & Nullish Coalescing

### Optional Chaining (?.)

Safely access nested properties without errors.

**JavaScript:**

```javascript
import { Text, View } from "react-native";

const UserCard = ({ user }) => {
  return (
    <View>
      <Text>Name: {user?.name}</Text>
      <Text>City: {user?.address?.city}</Text>
      <Text>Phone: {user?.contacts?.[0]?.phone}</Text>
      <Text>Email: {user?.getEmail?.()}</Text>
    </View>
  );
};
```

**TypeScript:**

```typescript
interface User {
  name?: string;
  address?: { city?: string };
  contacts?: Array<{ phone?: string }>;
  getEmail?: () => string;
}

const UserCard: FC<{ user?: User }> = ({ user }) => {
  return (
    <View>
      <Text>Name: {user?.name}</Text>
      <Text>City: {user?.address?.city}</Text>
      <Text>Phone: {user?.contacts?.[0]?.phone}</Text>
      <Text>Email: {user?.getEmail?.()}</Text>
    </View>
  );
};
```

### Nullish Coalescing (??)

Provide defaults only for null/undefined (not for 0, '', false).

**Comparison:**

```javascript
const score = 0;
const name = "";

// Using || (wrong for these cases)
console.log(score || 100); // 100 (wrong!)
console.log(name || "Guest"); // 'Guest' (wrong!)

// Using ?? (correct)
console.log(score ?? 100); // 0 (correct!)
console.log(name ?? "Guest"); // '' (correct!)
```

**React Native Example:**

```javascript
const GameScore = ({ player }) => {
  return (
    <View>
      <Text>Score: {player.score ?? "No score"}</Text>
      <Text>Level: {player.level ?? 1}</Text>
      <Text>Lives: {player.lives ?? 3}</Text>
    </View>
  );
};
```

### Combined Usage

```javascript
const UserProfile = ({ user }) => {
  return (
    <View>
      <Text>City: {user?.address?.city ?? "Unknown"}</Text>
      <Text>Phone: {user?.contacts?.[0] ?? "No phone"}</Text>
      <Text>Age: {user?.profile?.age ?? "Not specified"}</Text>
    </View>
  );
};
```

---

## Functional Components in React Native

### Basic Component

**JavaScript:**

```javascript
import { View, Text, StyleSheet } from "react-native";

const Welcome = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome!</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center" },
  title: { fontSize: 24, fontWeight: "bold" },
});

export default Welcome;
```

### Component with Props

**TypeScript:**

```typescript
import { View, Text } from "react-native";
import { FC } from "react";

interface UserCardProps {
  name: string;
  email: string;
  age: number;
}

const UserCard: FC<UserCardProps> = ({ name, email, age }) => {
  return (
    <View>
      <Text>{name}</Text>
      <Text>{email}</Text>
      <Text>Age: {age}</Text>
    </View>
  );
};
```

### Component with State

**JavaScript:**

```javascript
import { View, Text, Button } from "react-native";
import { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="+" onPress={() => setCount(count + 1)} />
      <Button title="-" onPress={() => setCount(count - 1)} />
    </View>
  );
};
```

### Component with Effects

**JavaScript:**

```javascript
import { View, Text, ActivityIndicator } from "react-native";
import { useState, useEffect } from "react";

const DataFetcher = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://api.example.com/data")
      .then((res) => res.json())
      .then((json) => {
        setData(json);
        setLoading(false);
      });

    return () => {
      console.log("Cleanup");
    };
  }, []);

  if (loading) return <ActivityIndicator />;
  return <Text>{JSON.stringify(data)}</Text>;
};
```

---

## Why Functions Are Better Than Classes

### Comparison

| Feature         | Classes          | Functions    |
| --------------- | ---------------- | ------------ |
| **Syntax**      | Verbose          | Concise      |
| **State**       | this.state       | useState()   |
| **Lifecycle**   | Multiple methods | useEffect()  |
| **`this`**      | Requires binding | No `this`    |
| **Reusability** | HOCs             | Custom Hooks |
| **Testing**     | Complex          | Simple       |
| **Bundle Size** | Larger           | Smaller      |

### Example Comparison

**Class Component:**

```javascript
class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
    this.increment = this.increment.bind(this);
  }

  componentDidMount() {
    console.log("Mounted");
  }

  increment() {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <View>
        <Text>{this.state.count}</Text>
        <Button title="+" onPress={this.increment} />
      </View>
    );
  }
}
```

**Functional Component:**

```javascript
const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Mounted");
  }, []);

  return (
    <View>
      <Text>{count}</Text>
      <Button title="+" onPress={() => setCount(count + 1)} />
    </View>
  );
};
```

### Advantages

1. Less boilerplate code
2. No `this` binding confusion
3. Easier to read and understand
4. Better code reusability with hooks
5. Smaller bundle size
6. Official React recommendation

---

## JavaScript's Shift to Function-Based Programming

### Why the Shift?

**1. `this` Complexity:**

```javascript
// Class - confusing 'this'
class Button {
  handleClick() {
    console.log(this); // undefined without binding!
  }
}

// Function - no 'this'
const Button = () => {
  const handleClick = () => {
    console.log("Clear!");
  };
};
```

**2. Code Reuse:**

```javascript
// Old: HOC (complex)
const withAuth = (Component) => {
  return class extends React.Component {
    render() {
      return this.props.isAuth ? <Component /> : <Login />;
    }
  };
};

// New: Hook (simple)
const useAuth = () => {
  const [isAuth, setIsAuth] = useState(false);
  return isAuth;
};

const Dashboard = () => {
  const isAuth = useAuth();
  return isAuth ? <Content /> : <Login />;
};
```

**3. Lifecycle Simplification:**

```javascript
// Class: scattered logic
class Profile extends Component {
  componentDidMount() {
    this.fetchUser();
    this.subscribe();
  }

  componentDidUpdate(prevProps) {
    if (prevProps.id !== this.props.id) {
      this.fetchUser();
    }
  }

  componentWillUnmount() {
    this.unsubscribe();
  }
}

// Function: grouped logic
const Profile = ({ id }) => {
  useEffect(() => {
    fetchUser(id);
  }, [id]);

  useEffect(() => {
    subscribe();
    return () => unsubscribe();
  }, []);
};
```

### Modern Philosophy

Functions enable:

- **Composition over inheritance**
- **Functional programming principles**
- **Better tree-shaking**
- **Concurrent mode support**
- **Improved developer experience**

### Conclusion

JavaScript shifted to functions because they provide:

- Simpler syntax
- Better code organization
- Easier testing
- More maintainable code
- Better performance potential

**Functions are now the standard for React and React Native development.**

---

## Summary

**Key Takeaways:**

1. **TypeScript** is better for large projects (type safety)
2. **Prefer `const`**, use `let` when needed, avoid `var`
3. **Arrow functions** are simpler and avoid `this` issues
4. **Master array methods**: map, filter, reduce
5. **Optional chaining (`?.`)** for safe property access
6. **Nullish coalescing (`??`)** for smart defaults
7. **Functional components** are the modern standard
8. **Hooks** (useState, useEffect) replace class methods
9. **Functions > Classes** for simplicity and maintainability

**Practice by building real React Native apps!**

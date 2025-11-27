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
   - [Function vs Arrow Function Differences](#function-vs-arrow-function-differences)
8. [Arrays](#arrays)
9. [Array Methods](#array-methods)
10. [Loops](#loops)
11. [Sets](#sets)
12. [Functional Components in React Native](#functional-components-in-react-native)
13. [Why Functions Are Better Than Classes](#why-functions-are-better-than-classes)
14. [JavaScript's Shift to Function-Based Programming](#javascripts-shift-to-function-based-programming)

---

## Introduction

This guide provides comprehensive JavaScript fundamentals specifically tailored for React Native development, with real-world examples in both JavaScript and TypeScript.

---

## JavaScript vs TypeScript

### Key Differences

| Feature | JavaScript | TypeScript |
|---------|-----------|------------|
| **Type System** | Dynamic (runtime) | Static (compile-time) |
| **Type Checking** | No built-in type checking | Strong type checking |
| **Compilation** | Interpreted directly | Compiled to JavaScript |
| **Error Detection** | Runtime errors | Compile-time errors |
| **IDE Support** | Basic | Advanced (IntelliSense, autocomplete) |
| **Learning Curve** | Easier | Steeper |
| **Code Verbosity** | Less verbose | More verbose |
| **Refactoring** | Risky | Safer |

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

#### 1. String

**JavaScript Example:**
```javascript
// React Native Component
import { Text, View } from 'react-native';

const UserGreeting = () => {
  const firstName = 'John';
  const lastName = "Doe";
  const message = `Welcome, ${firstName} ${lastName}!`; // Template literal
  
  return (
    <View>
      <Text>{message}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const UserGreeting = (): JSX.Element => {
  const firstName: string = 'John';
  const lastName: string = "Doe";
  const message: string = `Welcome, ${firstName} ${lastName}!`;
  
  return (
    <View>
      <Text>{message}</Text>
    </View>
  );
};
```

#### 2. Number

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const ProductPrice = () => {
  const price = 99.99;
  const quantity = 5;
  const total = price * quantity;
  const discount = 10; // 10%
  const finalPrice = total - (total * discount / 100);
  
  return (
    <View>
      <Text>Price: ${price}</Text>
      <Text>Quantity: {quantity}</Text>
      <Text>Total: ${finalPrice.toFixed(2)}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const ProductPrice = (): JSX.Element => {
  const price: number = 99.99;
  const quantity: number = 5;
  const total: number = price * quantity;
  const discount: number = 10;
  const finalPrice: number = total - (total * discount / 100);
  
  return (
    <View>
      <Text>Price: ${price}</Text>
      <Text>Quantity: {quantity}</Text>
      <Text>Total: ${finalPrice.toFixed(2)}</Text>
    </View>
  );
};
```

#### 3. Boolean

**JavaScript Example:**
```javascript
import { Text, View, Button } from 'react-native';
import { useState } from 'react';

const ToggleComponent = () => {
  const [isVisible, setIsVisible] = useState(true);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  
  return (
    <View>
      {isVisible && <Text>This content is visible!</Text>}
      <Button 
        title={isVisible ? "Hide" : "Show"} 
        onPress={() => setIsVisible(!isVisible)} 
      />
      <Text>User is {isLoggedIn ? "logged in" : "logged out"}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View, Button } from 'react-native';
import { useState } from 'react';

const ToggleComponent = (): JSX.Element => {
  const [isVisible, setIsVisible] = useState<boolean>(true);
  const [isLoggedIn, setIsLoggedIn] = useState<boolean>(false);
  
  return (
    <View>
      {isVisible && <Text>This content is visible!</Text>}
      <Button 
        title={isVisible ? "Hide" : "Show"} 
        onPress={() => setIsVisible(!isVisible)} 
      />
      <Text>User is {isLoggedIn ? "logged in" : "logged out"}</Text>
    </View>
  );
};
```

#### 4. Undefined

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';
import { useState } from 'react';

const UserProfile = () => {
  const [username, setUsername] = useState(); // undefined initially
  
  return (
    <View>
      <Text>
        Username: {username !== undefined ? username : 'Not set'}
      </Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';
import { useState } from 'react';

const UserProfile = (): JSX.Element => {
  const [username, setUsername] = useState<string | undefined>(undefined);
  
  return (
    <View>
      <Text>
        Username: {username !== undefined ? username : 'Not set'}
      </Text>
    </View>
  );
};
```

#### 5. Null

**JavaScript Example:**
```javascript
import { Text, View, ActivityIndicator } from 'react-native';
import { useState, useEffect } from 'react';

const DataFetcher = () => {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(json => setData(json))
      .catch(err => setError(err.message));
  }, []);
  
  if (error !== null) return <Text>Error: {error}</Text>;
  if (data === null) return <ActivityIndicator />;
  
  return <Text>Data loaded: {JSON.stringify(data)}</Text>;
};
```

**TypeScript Example:**
```typescript
import { Text, View, ActivityIndicator } from 'react-native';
import { useState, useEffect } from 'react';

interface DataType {
  id: number;
  name: string;
}

const DataFetcher = (): JSX.Element => {
  const [data, setData] = useState<DataType | null>(null);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then((json: DataType) => setData(json))
      .catch((err: Error) => setError(err.message));
  }, []);
  
  if (error !== null) return <Text>Error: {error}</Text>;
  if (data === null) return <ActivityIndicator />;
  
  return <Text>Data loaded: {JSON.stringify(data)}</Text>;
};
```

#### 6. Symbol

**JavaScript Example:**
```javascript
// Symbols create unique identifiers
const USER_ID = Symbol('userId');
const ADMIN_ID = Symbol('userId');

const user = {
  [USER_ID]: 12345,
  name: 'John'
};

console.log(user[USER_ID]); // 12345
console.log(USER_ID === ADMIN_ID); // false (symbols are always unique)
```

**TypeScript Example:**
```typescript
const USER_ID: symbol = Symbol('userId');
const ADMIN_ID: symbol = Symbol('userId');

interface User {
  [USER_ID]: number;
  name: string;
}

const user: User = {
  [USER_ID]: 12345,
  name: 'John'
};

console.log(user[USER_ID]); // 12345
console.log(USER_ID === ADMIN_ID); // false
```

#### 7. BigInt

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const LargeNumbers = () => {
  const largeNumber = 9007199254740991n; // BigInt
  const anotherBig = BigInt("9007199254740991");
  const sum = largeNumber + 1n;
  
  return (
    <View>
      <Text>Large Number: {largeNumber.toString()}</Text>
      <Text>Sum: {sum.toString()}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const LargeNumbers = (): JSX.Element => {
  const largeNumber: bigint = 9007199254740991n;
  const anotherBig: bigint = BigInt("9007199254740991");
  const sum: bigint = largeNumber + 1n;
  
  return (
    <View>
      <Text>Large Number: {largeNumber.toString()}</Text>
      <Text>Sum: {sum.toString()}</Text>
    </View>
  );
};
```

### Non-Primitive Data Types

Non-primitive data types are mutable and stored by reference.

#### 1. Object

**JavaScript Example:**
```javascript
import { Text, View, FlatList } from 'react-native';

const UserCard = () => {
  const user = {
    id: 1,
    name: 'Alice Johnson',
    email: 'alice@example.com',
    age: 28,
    address: {
      street: '123 Main St',
      city: 'New York',
      zipCode: '10001'
    },
    hobbies: ['Reading', 'Coding', 'Gaming']
  };
  
  return (
    <View>
      <Text>Name: {user.name}</Text>
      <Text>Email: {user.email}</Text>
      <Text>City: {user.address.city}</Text>
      <Text>Hobbies: {user.hobbies.join(', ')}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

interface Address {
  street: string;
  city: string;
  zipCode: string;
}

interface User {
  id: number;
  name: string;
  email: string;
  age: number;
  address: Address;
  hobbies: string[];
}

const UserCard = (): JSX.Element => {
  const user: User = {
    id: 1,
    name: 'Alice Johnson',
    email: 'alice@example.com',
    age: 28,
    address: {
      street: '123 Main St',
      city: 'New York',
      zipCode: '10001'
    },
    hobbies: ['Reading', 'Coding', 'Gaming']
  };
  
  return (
    <View>
      <Text>Name: {user.name}</Text>
      <Text>Email: {user.email}</Text>
      <Text>City: {user.address.city}</Text>
      <Text>Hobbies: {user.hobbies.join(', ')}</Text>
    </View>
  );
};
```

#### 2. Array

**JavaScript Example:**
```javascript
import { Text, View, FlatList } from 'react-native';

const TodoList = () => {
  const todos = ['Buy groceries', 'Walk the dog', 'Finish project', 'Call mom'];
  
  return (
    <View>
      <Text style={{fontSize: 20, fontWeight: 'bold'}}>My Todos:</Text>
      <FlatList
        data={todos}
        keyExtractor={(item, index) => index.toString()}
        renderItem={({item, index}) => (
          <Text>{index + 1}. {item}</Text>
        )}
      />
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View, FlatList } from 'react-native';

const TodoList = (): JSX.Element => {
  const todos: string[] = ['Buy groceries', 'Walk the dog', 'Finish project', 'Call mom'];
  
  return (
    <View>
      <Text style={{fontSize: 20, fontWeight: 'bold'}}>My Todos:</Text>
      <FlatList
        data={todos}
        keyExtractor={(item: string, index: number) => index.toString()}
        renderItem={({item, index}: {item: string; index: number}) => (
          <Text>{index + 1}. {item}</Text>
        )}
      />
    </View>
  );
};
```

#### 3. Function

**JavaScript Example:**
```javascript
import { Text, View, Button } from 'react-native';

const Calculator = () => {
  // Function as a data type
  const add = function(a, b) {
    return a + b;
  };
  
  const multiply = (a, b) => a * b;
  
  const calculate = (operation, x, y) => {
    return operation(x, y);
  };
  
  return (
    <View>
      <Text>5 + 3 = {calculate(add, 5, 3)}</Text>
      <Text>5 × 3 = {calculate(multiply, 5, 3)}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

type OperationFunction = (a: number, b: number) => number;

const Calculator = (): JSX.Element => {
  const add: OperationFunction = function(a, b) {
    return a + b;
  };
  
  const multiply: OperationFunction = (a, b) => a * b;
  
  const calculate = (operation: OperationFunction, x: number, y: number): number => {
    return operation(x, y);
  };
  
  return (
    <View>
      <Text>5 + 3 = {calculate(add, 5, 3)}</Text>
      <Text>5 × 3 = {calculate(multiply, 5, 3)}</Text>
    </View>
  );
};
```

---

## Variables: var, let, const

### Comparison Table

| Feature | var | let | const |
|---------|-----|-----|-------|
| **Scope** | Function-scoped | Block-scoped | Block-scoped |
| **Hoisting** | Hoisted (undefined) | Hoisted (TDZ) | Hoisted (TDZ) |
| **Re-declaration** | Allowed | Not allowed | Not allowed |
| **Re-assignment** | Allowed | Allowed | Not allowed |
| **Use in Modern JS** | Avoided | Common | Preferred |

### var (Avoid in Modern Code)

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const VarExample = () => {
  var count = 0;
  
  if (true) {
    var count = 10; // Same variable (function-scoped)
  }
  
  return <Text>Count: {count}</Text>; // Outputs: 10
};
```

### let (Mutable Variables)

**JavaScript Example:**
```javascript
import { Text, View, Button } from 'react-native';
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    let newCount = count; // let allows reassignment
    newCount = newCount + 1;
    setCount(newCount);
  };
  
  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={increment} />
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View, Button } from 'react-native';
import { useState } from 'react';

const Counter = (): JSX.Element => {
  const [count, setCount] = useState<number>(0);
  
  const increment = (): void => {
    let newCount: number = count;
    newCount = newCount + 1;
    setCount(newCount);
  };
  
  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={increment} />
    </View>
  );
};
```

### const (Immutable Binding - Preferred)

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const AppConfig = () => {
  const API_URL = 'https://api.example.com';
  const MAX_RETRIES = 3;
  
  // This works - object properties can change
  const user = { name: 'John', age: 30 };
  user.age = 31; // Allowed
  
  // This would throw an error
  // user = { name: 'Jane', age: 25 }; // Error!
  
  return (
    <View>
      <Text>API: {API_URL}</Text>
      <Text>Max Retries: {MAX_RETRIES}</Text>
      <Text>User: {user.name}, Age: {user.age}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

interface User {
  name: string;
  age: number;
}

const AppConfig = (): JSX.Element => {
  const API_URL: string = 'https://api.example.com';
  const MAX_RETRIES: number = 3;
  
  const user: User = { name: 'John', age: 30 };
  user.age = 31; // Allowed
  
  return (
    <View>
      <Text>API: {API_URL}</Text>
      <Text>Max Retries: {MAX_RETRIES}</Text>
      <Text>User: {user.name}, Age: {user.age}</Text>
    </View>
  );
};
```

---

## Operators

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const OperatorsDemo = () => {
  // Arithmetic Operators
  const sum = 10 + 5;         // 15
  const difference = 10 - 5;   // 5
  const product = 10 * 5;      // 50
  const quotient = 10 / 5;     // 2
  const remainder = 10 % 3;    // 1
  const power = 2 ** 3;        // 8
  
  // Comparison Operators
  const isEqual = 5 === 5;           // true (strict equality)
  const isLooseEqual = 5 == '5';     // true (loose equality)
  const isNotEqual = 5 !== 3;        // true
  const isGreater = 10 > 5;          // true
  const isLessOrEqual = 5 <= 5;      // true
  
  // Logical Operators
  const andResult = true && false;   // false
  const orResult = true || false;    // true
  const notResult = !true;           // false
  
  // Ternary Operator
  const age = 20;
  const status = age >= 18 ? 'Adult' : 'Minor';
  
  return (
    <View>
      <Text>Sum: {sum}</Text>
      <Text>Is Equal: {isEqual.toString()}</Text>
      <Text>Status: {status}</Text>
    </View>
  );
};
```

---

## Conditional Statements

### if-else

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const WeatherApp = () => {
  const temperature = 25;
  
  let message;
  if (temperature > 30) {
    message = "It's hot! 🔥";
  } else if (temperature > 20) {
    message = "It's pleasant! ☀️";
  } else {
    message = "It's cold! ❄️";
  }
  
  return (
    <View>
      <Text>Temperature: {temperature}°C</Text>
      <Text>{message}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const WeatherApp = (): JSX.Element => {
  const temperature: number = 25;
  
  let message: string;
  if (temperature > 30) {
    message = "It's hot! 🔥";
  } else if (temperature > 20) {
    message = "It's pleasant! ☀️";
  } else {
    message = "It's cold! ❄️";
  }
  
  return (
    <View>
      <Text>Temperature: {temperature}°C</Text>
      <Text>{message}</Text>
    </View>
  );
};
```

### switch-case

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const DaySchedule = () => {
  const day = 'Monday';
  
  let schedule;
  switch(day) {
    case 'Monday':
    case 'Wednesday':
    case 'Friday':
      schedule = 'Gym Day 💪';
      break;
    case 'Tuesday':
    case 'Thursday':
      schedule = 'Coding Day 💻';
      break;
    case 'Saturday':
    case 'Sunday':
      schedule = 'Rest Day 😴';
      break;
    default:
      schedule = 'Unknown Day';
  }
  
  return (
    <View>
      <Text>{day}: {schedule}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

type DayOfWeek = 'Monday' | 'Tuesday' | 'Wednesday' | 'Thursday' | 'Friday' | 'Saturday' | 'Sunday';

const DaySchedule = (): JSX.Element => {
  const day: DayOfWeek = 'Monday';
  
  let schedule: string;
  switch(day) {
    case 'Monday':
    case 'Wednesday':
    case 'Friday':
      schedule = 'Gym Day 💪';
      break;
    case 'Tuesday':
    case 'Thursday':
      schedule = 'Coding Day 💻';
      break;
    case 'Saturday':
    case 'Sunday':
      schedule = 'Rest Day 😴';
      break;
    default:
      schedule = 'Unknown Day';
  }
  
  return (
    <View>
      <Text>{day}: {schedule}</Text>
    </View>
  );
};
```

---

## Functions

### Traditional Functions

**JavaScript Example:**
```javascript
import { Text, View, Button } from 'react-native';

const MathOperations = () => {
  // Function Declaration
  function add(a, b) {
    return a + b;
  }
  
  // Function Expression
  const subtract = function(a, b) {
    return a - b;
  };
  
  // Function with default parameters
  function greet(name = 'Guest') {
    return `Hello, ${name}!`;
  }
  
  // Function with rest parameters
  function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
  }
  
  return (
    <View>
      <Text>5 + 3 = {add(5, 3)}</Text>
      <Text>5 - 3 = {subtract(5, 3)}</Text>
      <Text>{greet()}</Text>
      <Text>{greet('John')}</Text>
      <Text>Sum of 1,2,3,4,5 = {sum(1, 2, 3, 4, 5)}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const MathOperations = (): JSX.Element => {
  // Function Declaration
  function add(a: number, b: number): number {
    return a + b;
  }
  
  // Function Expression
  const subtract = function(a: number, b: number): number {
    return a - b;
  };
  
  // Function with default parameters
  function greet(name: string = 'Guest'): string {
    return `Hello, ${name}!`;
  }
  
  // Function with rest parameters
  function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
  }
  
  return (
    <View>
      <Text>5 + 3 = {add(5, 3)}</Text>
      <Text>5 - 3 = {subtract(5, 3)}</Text>
      <Text>{greet()}</Text>
      <Text>{greet('John')}</Text>
      <Text>Sum of 1,2,3,4,5 = {sum(1, 2, 3, 4, 5)}</Text>
    </View>
  );
};
```

### Arrow Functions

**JavaScript Example:**
```javascript
import { Text, View, FlatList } from 'react-native';

const ArrowFunctionExamples = () => {
  // Simple arrow function
  const square = (x) => x * x;
  
  // Arrow function with multiple parameters
  const multiply = (a, b) => a * b;
  
  // Arrow function with block body
  const calculateDiscount = (price, discount) => {
    const discountAmount = price * (discount / 100);
    return price - discountAmount;
  };
  
  // Arrow function with no parameters
  const getRandomNumber = () => Math.floor(Math.random() * 100);
  
  // Arrow functions in array methods
  const numbers = [1, 2, 3, 4, 5];
  const doubled = numbers.map(n => n * 2);
  const evens = numbers.filter(n => n % 2 === 0);
  
  return (
    <View>
      <Text>Square of 5: {square(5)}</Text>
      <Text>3 × 4 = {multiply(3, 4)}</Text>
      <Text>$100 with 20% off: ${calculateDiscount(100, 20)}</Text>
      <Text>Random: {getRandomNumber()}</Text>
      <Text>Doubled: {doubled.join(', ')}</Text>
      <Text>Evens: {evens.join(', ')}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

const ArrowFunctionExamples = (): JSX.Element => {
  const square = (x: number): number => x * x;
  
  const multiply = (a: number, b: number): number => a * b;
  
  const calculateDiscount = (price: number, discount: number): number => {
    const discountAmount: number = price * (discount / 100);
    return price - discountAmount;
  };
  
  const getRandomNumber = (): number => Math.floor(Math.random() * 100);
  
  const numbers: number[] = [1, 2, 3, 4, 5];
  const doubled: number[] = numbers.map((n: number) => n * 2);
  const evens: number[] = numbers.filter((n: number) => n % 2 === 0);
  
  return (
    <View>
      <Text>Square of 5: {square(5)}</Text>
      <Text>3 × 4 = {multiply(3, 4)}</Text>
      <Text>$100 with 20% off: ${calculateDiscount(100, 20)}</Text>
      <Text>Random: {getRandomNumber()}</Text>
      <Text>Doubled: {doubled.join(', ')}</Text>
      <Text>Evens: {evens.join(', ')}</Text>
    </View>
  );
};
```

### Function vs Arrow Function Differences

| Feature | Traditional Function | Arrow Function |
|---------|---------------------|----------------|
| **Syntax** | `function name() {}` | `const name = () => {}` |
| **`this` binding** | Has its own `this` | Inherits `this` from parent |
| **`arguments` object** | Has `arguments` object | No `arguments` object |
| **Constructor** | Can be used as constructor | Cannot be used as constructor |
| **Hoisting** | Hoisted (declarations) | Not hoisted (expressions) |
| **Method definition** | Can be used | Not recommended |
| **Best for** | Methods, constructors | Callbacks, functional code |

**Real Example Showing `this` Difference:**

```javascript
// Traditional function - 'this' changes based on caller
const person = {
  name: 'John',
  greet: function() {
    console.log(`Hello, ${this.name}`);
  }
};
person.greet(); // Works: "Hello, John"

// Arrow function - 'this' is lexically bound
const personArrow = {
  name: 'Jane',
  greet: () => {
    console.log(`Hello, ${this.name}`); // 'this' refers to outer scope
  }
};
personArrow.greet(); // Doesn't work as expected
```

---

## Array Methods

Array methods are essential for manipulating data in React Native applications.

### map() - Transform Array Elements

**JavaScript Example:**
```javascript
import { Text, View, FlatList } from 'react-native';

const ProductList = () => {
  const products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 599 },
    { id: 3, name: 'Tablet', price: 399 }
  ];
  
  // Transform prices with tax
  const productsWithTax = products.map(product => ({
    ...product,
    priceWithTax: product.price * 1.1
  }));
  
  return (
    <FlatList
      data={productsWithTax}
      keyExtractor={item => item.id.toString()}
      renderItem={({item}) => (
        <View>
          <Text>{item.name}</Text>
          <Text>Price: ${item.price}</Text>
          <Text>With Tax: ${item.priceWithTax.toFixed(2)}</Text>
        </View>
      )}
    />
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View, FlatList } from 'react-native';

interface Product {
  id: number;
  name: string;
  price: number;
}

interface ProductWithTax extends Product {
  priceWithTax: number;
}

const ProductList = (): JSX.Element => {
  const products: Product[] = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 599 },
    { id: 3, name: 'Tablet', price: 399 }
  ];
  
  const productsWithTax: ProductWithTax[] = products.map((product: Product) => ({
    ...product,
    priceWithTax: product.price * 1.1
  }));
  
  return (
    <FlatList
      data={productsWithTax}
      keyExtractor={(item: ProductWithTax) => item.id.toString()}
      renderItem={({item}: {item: ProductWithTax}) => (
        <View>
          <Text>{item.name}</Text>
          <Text>Price: ${item.price}</Text>
          <Text>With Tax: ${item.priceWithTax.toFixed(2)}</Text>
        </View>
      )}
    />
  );
};
```

### filter() - Filter Array Elements

**JavaScript Example:**
```javascript
import { Text, View, FlatList, TextInput } from 'react-native';
import { useState } from 'react';

const SearchableList = () => {
  const [searchQuery, setSearchQuery] = useState('');
  
  const users = [
    { id: 1, name: 'John Doe', age: 28, active: true },
    { id: 2, name: 'Jane Smith', age: 34, active: false },
    { id: 3, name: 'Bob Johnson', age: 45, active: true },
    { id: 4, name: 'Alice Williams', age: 23, active: true }
  ];
  
  // Filter active users
  const activeUsers = users.filter(user => user.active);
  
  // Filter by search query
  const searchResults = users.filter(user => 
    user.name.toLowerCase().includes(searchQuery.toLowerCase())
  );
  
  // Filter users over 30
  const over30 = users.filter(user => user.age > 30);
  
  return (
    <View>
      <TextInput
        placeholder="Search users..."
        value={searchQuery}
        onChangeText={setSearchQuery}
      />
      <Text>Active Users: {activeUsers.length}</Text>
      <Text>Over 30: {over30.length}</Text>
      <FlatList
        data={searchResults}
        keyExtractor={item => item.id.toString()}
        renderItem={({item}) => (
          <Text>{item.name} - {item.age}</Text>
        )}
      />
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View, FlatList, TextInput } from 'react-native';
import { useState } from 'react';

interface User {
  id: number;
  name: string;
  age: number;
  active: boolean;
}

const SearchableList = (): JSX.Element => {
  const [searchQuery, setSearchQuery] = useState<string>('');
  
  const users: User[] = [
    { id: 1, name: 'John Doe', age: 28, active: true },
    { id: 2, name: 'Jane Smith', age: 34, active: false },
    { id: 3, name: 'Bob Johnson', age: 45, active: true },
    { id: 4, name: 'Alice Williams', age: 23, active: true }
  ];
  
  const activeUsers: User[] = users.filter((user: User) => user.active);
  
  const searchResults: User[] = users.filter((user: User) => 
    user.name.toLowerCase().includes(searchQuery.toLowerCase())
  );
  
  const over30: User[] = users.filter((user: User) => user.age > 30);
  
  return (
    <View>
      <TextInput
        placeholder="Search users..."
        value={searchQuery}
        onChangeText={setSearchQuery}
      />
      <Text>Active Users: {activeUsers.length}</Text>
      <Text>Over 30: {over30.length}</Text>
      <FlatList
        data={searchResults}
        keyExtractor={(item: User) => item.id.toString()}
        renderItem={({item}: {item: User}) => (
          <Text>{item.name} - {item.age}</Text>
        )}
      />
    </View>
  );
};
```

### reduce() - Reduce Array to Single Value

**JavaScript Example:**
```javascript
import { Text, View } from 'react-native';

const OrderSummary = () => {
  const cartItems = [
    { id: 1, name: 'Shirt', price: 29.99, quantity: 2 },
    { id: 2, name: 'Jeans', price: 59.99, quantity: 1 },
    { id: 3, name: 'Shoes', price: 89.99, quantity: 1 }
  ];
  
  // Calculate total price
  const totalPrice = cartItems.reduce((total, item) => {
    return total + (item.price * item.quantity);
  }, 0);
  
  // Count total items
  const totalItems = cartItems.reduce((count, item) => {
    return count + item.quantity;
  }, 0);
  
  // Group items by price range
  const priceRanges = cartItems.reduce((acc, item) => {
    const range = item.price < 50 ? 'budget' : 'premium';
    acc[range] = (acc[range] || 0) + 1;
    return acc;
  }, {});
  
  return (
    <View>
      <Text style={{fontSize: 20, fontWeight: 'bold'}}>Order Summary</Text>
      <Text>Total Items: {totalItems}</Text>
      <Text>Total Price: ${totalPrice.toFixed(2)}</Text>
      <Text>Budget Items: {priceRanges.budget || 0}</Text>
      <Text>Premium Items: {priceRanges.premium || 0}</Text>
    </View>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, View } from 'react-native';

interface CartItem {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

interface PriceRanges {
  budget?: number;
  premium?: number;
}

const OrderSummary = (): JSX.Element => {
  const cartItems: CartItem[] = [
    { id: 1, name: 'Shirt', price: 29.99, quantity: 2 },
    { id: 2, name: 'Jeans', price: 59.99, quantity: 1 },
    { id: 3, name: 'Shoes', price: 89.99, quantity: 1 }
  ];
  
  const totalPrice: number = cartItems.reduce((total: number, item: CartItem) => {
    return total + (item.price * item.quantity);
  }, 0);
  
  const totalItems: number = cartItems.reduce((count: number, item: CartItem) => {
    return count + item.quantity;
  }, 0);
  
  const priceRanges: PriceRanges = cartItems.reduce((acc: PriceRanges, item: CartItem) => {
    const range: 'budget' | 'premium' = item.price < 50 ? 'budget' : 'premium';
    acc[range] = (acc[range] || 0) + 1;
    return acc;
  }, {});
  
  return (
    <View>
      <Text style={{fontSize: 20, fontWeight: 'bold'}}>Order Summary</Text>
      <Text>Total Items: {totalItems}</Text>
      <Text>Total Price: ${totalPrice.toFixed(2)}</Text>
      <Text>Budget Items: {priceRanges.budget || 0}</Text>
      <Text>Premium Items: {priceRanges.premium || 0}</Text>
    </View>
  );
};
```

### Other Essential Array Methods

**JavaScript Example:**
```javascript
import { Text, ScrollView } from 'react-native';

const ArrayMethodsDemo = () => {
  const numbers = [1, 2, 3, 4, 5];
  const fruits = ['apple', 'banana', 'orange'];
  
  // find() - Find first element matching condition
  const found = numbers.find(n => n > 3); // 4
  
  // findIndex() - Find index of first matching element
  const foundIndex = numbers.findIndex(n => n > 3); // 3
  
  // some() - Check if any element matches
  const hasEven = numbers.some(n => n % 2 === 0); // true
  
  // every() - Check if all elements match
  const allPositive = numbers.every(n => n > 0); // true
  
  // includes() - Check if array includes value
  const hasApple = fruits.includes('apple'); // true
  
  // indexOf() - Get index of value
  const appleIndex = fruits.indexOf('apple'); // 0
  
  // slice() - Get portion of array
  const sliced = numbers.slice(1, 4); // [2, 3, 4]
  
  // concat() - Merge arrays
  const combined = numbers.concat([6, 7, 8]);
  
  // join() - Convert to string
  const joined = fruits.join(', '); // "apple, banana, orange"
  
  // reverse() - Reverse array (mutates original)
  const reversed = [...numbers].reverse(); // [5, 4, 3, 2, 1]
  
  // sort() - Sort array (mutates original)
  const sorted = [...fruits].sort(); // ['apple', 'banana', 'orange']
  
  // flat() - Flatten nested arrays
  const nested = [1, [2, 3], [4, [5, 6]]];
  const flattened = nested.flat(2); // [1, 2, 3, 4, 5, 6]
  
  return (
    <ScrollView>
      <Text>find(): {found}</Text>
      <Text>findIndex(): {foundIndex}</Text>
      <Text>some(): {hasEven.toString()}</Text>
      <Text>every(): {allPositive.toString()}</Text>
      <Text>includes(): {hasApple.toString()}</Text>
      <Text>indexOf(): {appleIndex}</Text>
      <Text>slice(): {sliced.join(', ')}</Text>
      <Text>concat(): {combined.join(', ')}</Text>
      <Text>join(): {joined}</Text>
      <Text>reverse(): {reversed.join(', ')}</Text>
      <Text>sort(): {sorted.join(', ')}</Text>
      <Text>flat(): {flattened.join(', ')}</Text>
    </ScrollView>
  );
};
```

---

## Arrays

**JavaScript Example:**
```javascript
import { Text, View, ScrollView } from 'react-native';

const ArrayExamples = () => {
  // Array creation
  const fruits = ['Apple', 'Banana', 'Orange'];
  const numbers = [1, 2, 3, 4, 5];
  const mixed = [1, 'Hello', true, null, {name: 'John'}];
  
  // Array access
  const firstFruit = fruits[0];
  const lastFruit = fruits[fruits.length - 1];
  
  // Array modification
  const moreFruits = [...fruits, 'Mango', 'Grapes'];
  
  // Multidimensional array
  const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
  ];
  
  return (
    <ScrollView>
      <Text style={{fontSize: 18, fontWeight: 'bold'}}>Arrays Demo</Text>
      <Text>First Fruit: {firstFruit}</Text>
      <Text>Last Fruit: {lastFruit}</Text>
      <Text>All Fruits: {moreFruits.join(', ')}</Text>
      <Text>Matrix [1][1]: {matrix[1][1]}</Text>
      <Text>Array Length: {fruits.length}</Text>
    </ScrollView>
  );
};
```

**TypeScript Example:**
```typescript
import { Text, ScrollView } from 'react-native';

const ArrayExamples = (): JSX.Element => {
  const fruits: string[] = ['Apple', 'Banana', 'Orange'];
  const numbers: number[] = [1, 2, 3, 4, 5];
  const mixed: (number | string | boolean | null | object)[] = [1, 'Hello', true, null, {name: 'John'}];
  
  const firstFruit: string = fruits[0];
  const lastFruit: string = fruits[fruits.length - 1];
  
  const moreFruits: string[] = [...fruits, 'Mango', 'Grapes'];
  
  const matrix: number[][] = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
  ];
  
  return (
    <ScrollView>
      <Text style={{fontSize: 18, fontWeight: 'bold'}}>Arrays Demo</Text>
      <Text>First Fruit: {firstFruit}</Text>
      <Text>Last Fruit: {lastFruit}</Text>
      <Text>All Fruits: {moreFruits.join(', ')}</Text>
      <Text>Matrix [1][1]: {matrix[1][1]}</Text>
      <Text>Array Length: {fruits.length}</Text>
    </ScrollView>
  );
};
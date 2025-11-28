# Object-Oriented Programming in Flutter: Complete Guide

A comprehensive guide to understanding and implementing OOP concepts in Flutter development with real-world examples and performance considerations.

---

## Table of Contents

1. [Introduction to OOP](#1-introduction-to-oop)
2. [Classes and Their Structure](#2-classes-and-their-structure)
   - [What is a Class?](#what-is-a-class)
   - [Class Components](#class-components)
   - [Creating Objects](#creating-objects)
3. [Encapsulation](#3-encapsulation)
   - [What is Encapsulation?](#what-is-encapsulation)
   - [Access Modifiers in Dart](#access-modifiers-in-dart)
   - [Getters and Setters](#getters-and-setters)
4. [Inheritance](#4-inheritance)
   - [What is Inheritance?](#what-is-inheritance)
   - [Types of Inheritance](#types-of-inheritance)
   - [Method Overriding](#method-overriding)
5. [Polymorphism](#5-polymorphism)
   - [What is Polymorphism?](#what-is-polymorphism)
   - [Compile-time Polymorphism](#compile-time-polymorphism)
   - [Runtime Polymorphism](#runtime-polymorphism)
6. [Interfaces](#6-interfaces)
   - [What are Interfaces?](#what-are-interfaces)
   - [Implementing Interfaces](#implementing-interfaces)
7. [Abstract Classes](#7-abstract-classes)
   - [What are Abstract Classes?](#what-are-abstract-classes)
   - [Abstract vs Interface](#abstract-vs-interface)
8. [Singleton Pattern](#8-singleton-pattern)
   - [What is Singleton?](#what-is-singleton)
   - [When to Use Singleton](#when-to-use-singleton)
   - [Implementation Methods](#implementation-methods)
9. [OOP and Performance in Mobile Apps](#9-oop-and-performance-in-mobile-apps)
   - [Good vs Bad Practices](#good-vs-bad-practices)
   - [Memory Management](#memory-management)
   - [Widget Performance](#widget-performance)
10. [Real-Time Flutter Examples](#10-real-time-flutter-examples)
    - [User Management System](#user-management-system)
    - [Custom Form Widgets](#custom-form-widgets)
    - [API Service Layer](#api-service-layer)
11. [Performance Case Studies](#11-performance-case-studies)
    - [Bad Practice: Memory Leaks](#bad-practice-memory-leaks)
    - [Good Practice: Efficient State Management](#good-practice-efficient-state-management)
12. [Best Practices Summary](#12-best-practices-summary)

---

## 1. Introduction to OOP

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects" that contain data and code. In Flutter (using Dart), OOP helps us create scalable, maintainable, and reusable code.

**Four Pillars of OOP:**
- **Encapsulation**: Bundling data and methods together
- **Inheritance**: Creating new classes from existing ones
- **Polymorphism**: Using a single interface for different data types
- **Abstraction**: Hiding complex implementation details

**Why OOP Matters in Flutter:**
- Flutter widgets are classes
- Better code organization
- Easier testing and maintenance
- Promotes code reusability
- Enables cleaner architecture

---

## 2. Classes and Their Structure

### What is a Class?

A class is a blueprint for creating objects. It defines properties (variables) and behaviors (methods) that objects created from the class will have.

### Class Components

```dart
class Student {
  // 1. Properties (Instance Variables)
  String name;
  int age;
  String studentId;
  
  // 2. Constructor
  Student({
    required this.name,
    required this.age,
    required this.studentId,
  });
  
  // 3. Named Constructor
  Student.anonymous() 
      : name = 'Anonymous',
        age = 0,
        studentId = 'N/A';
  
  // 4. Methods
  void study() {
    print('$name is studying');
  }
  
  String getInfo() {
    return 'Name: $name, Age: $age, ID: $studentId';
  }
  
  // 5. Static Property
  static int totalStudents = 0;
  
  // 6. Static Method
  static void incrementStudents() {
    totalStudents++;
  }
}
```

### Creating Objects

```dart
// Creating an object using default constructor
Student student1 = Student(
  name: 'John Doe',
  age: 20,
  studentId: 'S001',
);

// Creating an object using named constructor
Student student2 = Student.anonymous();

// Accessing properties
print(student1.name); // Output: John Doe

// Calling methods
student1.study(); // Output: John Doe is studying

// Accessing static members
Student.incrementStudents();
print(Student.totalStudents); // Output: 1
```

**Flutter Widget Example:**

```dart
class CustomButton extends StatelessWidget {
  // Properties
  final String text;
  final VoidCallback onPressed;
  final Color backgroundColor;
  
  // Constructor
  const CustomButton({
    Key? key,
    required this.text,
    required this.onPressed,
    this.backgroundColor = Colors.blue,
  }) : super(key: key);
  
  // Method (build is overridden from StatelessWidget)
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: backgroundColor,
      ),
      child: Text(text),
    );
  }
}
```

---

## 3. Encapsulation

### What is Encapsulation?

Encapsulation is the bundling of data and methods that operate on that data within a single unit (class), while restricting direct access to some of the object's components.

**Benefits:**
- Data hiding and security
- Better control over data
- Flexibility and maintainability
- Prevents accidental data modification

### Access Modifiers in Dart

```dart
class BankAccount {
  // Public property (accessible everywhere)
  String accountHolderName;
  
  // Private property (only accessible within this library/file)
  double _balance;
  String _accountNumber;
  
  BankAccount({
    required this.accountHolderName,
    required double initialBalance,
    required String accountNumber,
  })  : _balance = initialBalance,
        _accountNumber = accountNumber;
  
  // Public getter
  double get balance => _balance;
  
  // Public getter with logic
  String get maskedAccountNumber {
    return '****${_accountNumber.substring(_accountNumber.length - 4)}';
  }
  
  // Public setter with validation
  set balance(double amount) {
    if (amount >= 0) {
      _balance = amount;
    } else {
      throw Exception('Balance cannot be negative');
    }
  }
  
  // Private method
  void _logTransaction(String type, double amount) {
    print('[$type] Amount: \$${amount.toStringAsFixed(2)}');
  }
  
  // Public method using private method
  void deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
      _logTransaction('DEPOSIT', amount);
    }
  }
  
  void withdraw(double amount) {
    if (amount > 0 && amount <= _balance) {
      _balance -= amount;
      _logTransaction('WITHDRAWAL', amount);
    }
  }
}
```

### Getters and Setters

```dart
class User {
  String _email;
  String _password;
  
  User(this._email, this._password);
  
  // Getter with validation
  String get email => _email;
  
  // Setter with validation
  set email(String value) {
    if (value.contains('@')) {
      _email = value;
    } else {
      throw Exception('Invalid email format');
    }
  }
  
  // Computed property
  bool get hasStrongPassword => _password.length >= 8;
  
  // Write-only property (no getter)
  set password(String value) {
    if (value.length >= 8) {
      _password = value;
    } else {
      throw Exception('Password must be at least 8 characters');
    }
  }
}
```

**Flutter Example with TextField:**

```dart
class EncapsulatedTextField extends StatefulWidget {
  final String label;
  final Function(String) onChanged;
  
  const EncapsulatedTextField({
    Key? key,
    required this.label,
    required this.onChanged,
  }) : super(key: key);

  @override
  State<EncapsulatedTextField> createState() => _EncapsulatedTextFieldState();
}

class _EncapsulatedTextFieldState extends State<EncapsulatedTextField> {
  // Private controller - encapsulated
  final TextEditingController _controller = TextEditingController();
  String _errorText = '';
  
  @override
  void dispose() {
    _controller.dispose(); // Clean up
    super.dispose();
  }
  
  // Private validation method
  void _validateInput(String value) {
    setState(() {
      if (value.isEmpty) {
        _errorText = '${widget.label} is required';
      } else if (value.length < 3) {
        _errorText = '${widget.label} must be at least 3 characters';
      } else {
        _errorText = '';
        widget.onChanged(value);
      }
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return TextField(
      controller: _controller,
      decoration: InputDecoration(
        labelText: widget.label,
        errorText: _errorText.isEmpty ? null : _errorText,
      ),
      onChanged: _validateInput,
    );
  }
}
```

---

## 4. Inheritance

### What is Inheritance?

Inheritance is a mechanism where a new class (child/derived class) acquires properties and behaviors from an existing class (parent/base class).

**Benefits:**
- Code reusability
- Method overriding
- Hierarchical classification
- Polymorphism support

### Types of Inheritance

```dart
// Base Class
class Vehicle {
  String brand;
  int year;
  
  Vehicle({required this.brand, required this.year});
  
  void start() {
    print('$brand vehicle is starting...');
  }
  
  void displayInfo() {
    print('Brand: $brand, Year: $year');
  }
}

// Single Inheritance
class Car extends Vehicle {
  int numberOfDoors;
  
  Car({
    required String brand,
    required int year,
    required this.numberOfDoors,
  }) : super(brand: brand, year: year);
  
  void openTrunk() {
    print('Opening trunk of $brand car');
  }
}

// Multilevel Inheritance
class ElectricCar extends Car {
  int batteryCapacity;
  
  ElectricCar({
    required String brand,
    required int year,
    required int numberOfDoors,
    required this.batteryCapacity,
  }) : super(
          brand: brand,
          year: year,
          numberOfDoors: numberOfDoors,
        );
  
  void charge() {
    print('Charging $brand electric car');
  }
}

// Usage
void main() {
  ElectricCar tesla = ElectricCar(
    brand: 'Tesla',
    year: 2024,
    numberOfDoors: 4,
    batteryCapacity: 100,
  );
  
  tesla.start();        // Inherited from Vehicle
  tesla.openTrunk();    // Inherited from Car
  tesla.charge();       // Own method
  tesla.displayInfo();  // Inherited from Vehicle
}
```

### Method Overriding

```dart
class Animal {
  String name;
  
  Animal(this.name);
  
  void makeSound() {
    print('$name makes a sound');
  }
  
  void sleep() {
    print('$name is sleeping');
  }
}

class Dog extends Animal {
  Dog(String name) : super(name);
  
  // Overriding parent method
  @override
  void makeSound() {
    print('$name barks: Woof! Woof!');
  }
  
  // Additional method
  void fetch() {
    print('$name is fetching the ball');
  }
}

class Cat extends Animal {
  Cat(String name) : super(name);
  
  @override
  void makeSound() {
    print('$name meows: Meow! Meow!');
  }
  
  // Using super to call parent method
  @override
  void sleep() {
    super.sleep();
    print('$name curls up in a ball');
  }
}
```

**Flutter Widget Inheritance Example:**

```dart
// Base Widget Class
abstract class BaseFormField extends StatelessWidget {
  final String label;
  final TextEditingController controller;
  final String? Function(String?)? validator;
  
  const BaseFormField({
    Key? key,
    required this.label,
    required this.controller,
    this.validator,
  }) : super(key: key);
  
  // Common decoration method
  InputDecoration getDecoration() {
    return InputDecoration(
      labelText: label,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(8),
      ),
      contentPadding: const EdgeInsets.symmetric(
        horizontal: 16,
        vertical: 12,
      ),
    );
  }
}

// Email Field - Inherits from BaseFormField
class EmailFormField extends BaseFormField {
  const EmailFormField({
    Key? key,
    required String label,
    required TextEditingController controller,
  }) : super(
          key: key,
          label: label,
          controller: controller,
          validator: null,
        );
  
  @override
  Widget build(BuildContext context) {
    return TextFormField(
      controller: controller,
      decoration: getDecoration().copyWith(
        prefixIcon: const Icon(Icons.email),
      ),
      keyboardType: TextInputType.emailAddress,
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Email is required';
        }
        if (!value.contains('@')) {
          return 'Invalid email format';
        }
        return null;
      },
    );
  }
}

// Password Field - Inherits from BaseFormField
class PasswordFormField extends BaseFormField {
  const PasswordFormField({
    Key? key,
    required String label,
    required TextEditingController controller,
  }) : super(
          key: key,
          label: label,
          controller: controller,
        );
  
  @override
  Widget build(BuildContext context) {
    return TextFormField(
      controller: controller,
      decoration: getDecoration().copyWith(
        prefixIcon: const Icon(Icons.lock),
      ),
      obscureText: true,
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Password is required';
        }
        if (value.length < 8) {
          return 'Password must be at least 8 characters';
        }
        return null;
      },
    );
  }
}
```

---

## 5. Polymorphism

### What is Polymorphism?

Polymorphism allows objects of different types to be treated as objects of a common parent type. It means "many forms" and enables one interface to be used for different data types.

**Benefits:**
- Code flexibility
- Easier maintenance
- Clean and readable code
- Dynamic method binding

### Compile-time Polymorphism

**Method Overloading (through optional parameters in Dart):**

```dart
class Calculator {
  // Method with optional parameters
  int add(int a, int b, [int c = 0, int d = 0]) {
    return a + b + c + d;
  }
  
  // Named parameters for different behaviors
  double calculate({
    required double num1,
    required double num2,
    String operation = 'add',
  }) {
    switch (operation) {
      case 'add':
        return num1 + num2;
      case 'subtract':
        return num1 - num2;
      case 'multiply':
        return num1 * num2;
      case 'divide':
        return num1 / num2;
      default:
        return 0;
    }
  }
}

// Usage
void main() {
  Calculator calc = Calculator();
  
  print(calc.add(5, 3));           // 8
  print(calc.add(5, 3, 2));        // 10
  print(calc.add(5, 3, 2, 1));     // 11
  
  print(calc.calculate(num1: 10, num2: 5));                    // 15
  print(calc.calculate(num1: 10, num2: 5, operation: 'multiply')); // 50
}
```

### Runtime Polymorphism

```dart
// Base class
class Shape {
  void draw() {
    print('Drawing a shape');
  }
  
  double getArea() {
    return 0;
  }
}

// Child classes
class Circle extends Shape {
  double radius;
  
  Circle(this.radius);
  
  @override
  void draw() {
    print('Drawing a circle with radius $radius');
  }
  
  @override
  double getArea() {
    return 3.14 * radius * radius;
  }
}

class Rectangle extends Shape {
  double width;
  double height;
  
  Rectangle(this.width, this.height);
  
  @override
  void draw() {
    print('Drawing a rectangle $width x $height');
  }
  
  @override
  double getArea() {
    return width * height;
  }
}

class Triangle extends Shape {
  double base;
  double height;
  
  Triangle(this.base, this.height);
  
  @override
  void draw() {
    print('Drawing a triangle');
  }
  
  @override
  double getArea() {
    return 0.5 * base * height;
  }
}

// Polymorphic function
void drawShape(Shape shape) {
  shape.draw();
  print('Area: ${shape.getArea()}');
}

// Usage
void main() {
  List<Shape> shapes = [
    Circle(5),
    Rectangle(4, 6),
    Triangle(3, 4),
  ];
  
  // Same method call, different behavior
  for (Shape shape in shapes) {
    drawShape(shape);
    print('---');
  }
}
```

**Flutter Polymorphism Example:**

```dart
// Base class for different input types
abstract class CustomInput extends StatelessWidget {
  final String label;
  final TextEditingController controller;
  
  const CustomInput({
    Key? key,
    required this.label,
    required this.controller,
  }) : super(key: key);
  
  // Common method to be overridden
  Widget buildInput();
  
  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          label,
          style: const TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.bold,
          ),
        ),
        const SizedBox(height: 8),
        buildInput(),
      ],
    );
  }
}

// Different implementations
class TextInput extends CustomInput {
  const TextInput({
    Key? key,
    required String label,
    required TextEditingController controller,
  }) : super(key: key, label: label, controller: controller);
  
  @override
  Widget buildInput() {
    return TextField(
      controller: controller,
      decoration: InputDecoration(
        hintText: 'Enter $label',
        border: const OutlineInputBorder(),
      ),
    );
  }
}

class NumberInput extends CustomInput {
  const NumberInput({
    Key? key,
    required String label,
    required TextEditingController controller,
  }) : super(key: key, label: label, controller: controller);
  
  @override
  Widget buildInput() {
    return TextField(
      controller: controller,
      keyboardType: TextInputType.number,
      decoration: InputDecoration(
        hintText: 'Enter $label',
        border: const OutlineInputBorder(),
        prefixIcon: const Icon(Icons.numbers),
      ),
    );
  }
}

class EmailInput extends CustomInput {
  const EmailInput({
    Key? key,
    required String label,
    required TextEditingController controller,
  }) : super(key: key, label: label, controller: controller);
  
  @override
  Widget buildInput() {
    return TextField(
      controller: controller,
      keyboardType: TextInputType.emailAddress,
      decoration: InputDecoration(
        hintText: 'Enter $label',
        border: const OutlineInputBorder(),
        prefixIcon: const Icon(Icons.email),
        suffixIcon: const Icon(Icons.check_circle_outline),
      ),
    );
  }
}

// Usage with polymorphism
class PolymorphicForm extends StatelessWidget {
  const PolymorphicForm({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    // List of different input types treated as CustomInput
    List<CustomInput> inputs = [
      TextInput(
        label: 'Name',
        controller: TextEditingController(),
      ),
      EmailInput(
        label: 'Email',
        controller: TextEditingController(),
      ),
      NumberInput(
        label: 'Age',
        controller: TextEditingController(),
      ),
    ];
    
    return Column(
      children: inputs.map((input) {
        return Padding(
          padding: const EdgeInsets.only(bottom: 16),
          child: input, // Polymorphic usage
        );
      }).toList(),
    );
  }
}
```

---

## 6. Interfaces

### What are Interfaces?

In Dart, every class implicitly defines an interface containing all instance members of the class. An interface is a contract that defines what methods a class must implement, without specifying how they should be implemented.

**Benefits:**
- Contract enforcement
- Multiple inheritance support
- Loose coupling
- Better testability

### Implementing Interfaces

```dart
// Class used as interface
class Drawable {
  void draw() {
    print('Drawing...');
  }
  
  void erase() {
    print('Erasing...');
  }
}

class Colorable {
  void setColor(String color) {
    print('Setting color to $color');
  }
}

// Implementing multiple interfaces
class Canvas implements Drawable, Colorable {
  @override
  void draw() {
    print('Drawing on canvas');
  }
  
  @override
  void erase() {
    print('Erasing from canvas');
  }
  
  @override
  void setColor(String color) {
    print('Canvas color set to $color');
  }
  
  // Additional methods
  void clear() {
    print('Clearing entire canvas');
  }
}

// Another implementation
class Whiteboard implements Drawable, Colorable {
  @override
  void draw() {
    print('Drawing on whiteboard');
  }
  
  @override
  void erase() {
    print('Erasing from whiteboard with eraser');
  }
  
  @override
  void setColor(String color) {
    print('Whiteboard marker color: $color');
  }
}
```

**Real-World Interface Example:**

```dart
// Repository interface
abstract class UserRepository {
  Future<User?> getUserById(String id);
  Future<List<User>> getAllUsers();
  Future<void> createUser(User user);
  Future<void> updateUser(User user);
  Future<void> deleteUser(String id);
}

// API Implementation
class ApiUserRepository implements UserRepository {
  final String baseUrl = 'https://api.example.com';
  
  @override
  Future<User?> getUserById(String id) async {
    // API call implementation
    print('Fetching user $id from API');
    // Simulate API call
    await Future.delayed(const Duration(seconds: 1));
    return User(id: id, name: 'API User');
  }
  
  @override
  Future<List<User>> getAllUsers() async {
    print('Fetching all users from API');
    await Future.delayed(const Duration(seconds: 1));
    return [
      User(id: '1', name: 'User 1'),
      User(id: '2', name: 'User 2'),
    ];
  }
  
  @override
  Future<void> createUser(User user) async {
    print('Creating user via API: ${user.name}');
  }
  
  @override
  Future<void> updateUser(User user) async {
    print('Updating user via API: ${user.name}');
  }
  
  @override
  Future<void> deleteUser(String id) async {
    print('Deleting user $id via API');
  }
}

// Local Database Implementation
class LocalUserRepository implements UserRepository {
  @override
  Future<User?> getUserById(String id) async {
    print('Fetching user $id from local database');
    await Future.delayed(const Duration(milliseconds: 100));
    return User(id: id, name: 'Local User');
  }
  
  @override
  Future<List<User>> getAllUsers() async {
    print('Fetching all users from local database');
    await Future.delayed(const Duration(milliseconds: 100));
    return [
      User(id: '1', name: 'Local User 1'),
      User(id: '2', name: 'Local User 2'),
    ];
  }
  
  @override
  Future<void> createUser(User user) async {
    print('Creating user in local database: ${user.name}');
  }
  
  @override
  Future<void> updateUser(User user) async {
    print('Updating user in local database: ${user.name}');
  }
  
  @override
  Future<void> deleteUser(String id) async {
    print('Deleting user $id from local database');
  }
}

// User model
class User {
  final String id;
  final String name;
  
  User({required this.id, required this.name});
}

// Service using interface (doesn't care about implementation)
class UserService {
  final UserRepository repository;
  
  UserService(this.repository);
  
  Future<void> displayUser(String id) async {
    User? user = await repository.getUserById(id);
    print('User: ${user?.name}');
  }
}

// Usage - can easily switch implementations
void main() {
  // Using API repository
  UserService apiService = UserService(ApiUserRepository());
  apiService.displayUser('123');
  
  // Using Local repository
  UserService localService = UserService(LocalUserRepository());
  localService.displayUser('123');
}
```

**Flutter Interface Example:**

```dart
// Data source interface
abstract class DataSource<T> {
  Future<List<T>> fetchData();
  Future<T?> fetchById(String id);
  Future<void> saveData(T item);
  Stream<List<T>> watchData();
}

// API implementation
class ApiDataSource implements DataSource<Product> {
  @override
  Future<List<Product>> fetchData() async {
    // Simulate API call
    await Future.delayed(const Duration(seconds: 2));
    return [
      Product(id: '1', name: 'Product 1', price: 29.99),
      Product(id: '2', name: 'Product 2', price: 49.99),
    ];
  }
  
  @override
  Future<Product?> fetchById(String id) async {
    await Future.delayed(const Duration(seconds: 1));
    return Product(id: id, name: 'Product $id', price: 39.99);
  }
  
  @override
  Future<void> saveData(Product item) async {
    print('Saving ${item.name} to API');
  }
  
  @override
  Stream<List<Product>> watchData() async* {
    while (true) {
      await Future.delayed(const Duration(seconds: 5));
      yield await fetchData();
    }
  }
}

class Product {
  final String id;
  final String name;
  final double price;
  
  Product({required this.id, required this.name, required this.price});
}
```

---

## 7. Abstract Classes

### What are Abstract Classes?

An abstract class is a class that cannot be instantiated and is meant to be extended by other classes. It can contain both abstract methods (without implementation) and concrete methods (with implementation).

**Benefits:**
- Enforces structure
- Provides partial implementation
- Supports code reuse
- Defines template methods

### Abstract vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Instantiation | Cannot be instantiated | Cannot be instantiated |
| Methods | Can have both abstract and concrete methods | All methods are abstract (using `implements`) |
| Inheritance | Single inheritance only | Multiple implementation allowed |
| Fields | Can have instance variables | Cannot have instance variables (when used as interface) |
| Use Case | When classes share common behavior | When defining a contract |

```dart
// Abstract class with both abstract and concrete methods
abstract class Employee {
  String name;
  String employeeId;
  
  Employee({required this.name, required this.employeeId});
  
  // Abstract method - must be implemented by child classes
  double calculateSalary();
  
  // Concrete method - can be used by all child classes
  void displayInfo() {
    print('Name: $name');
    print('Employee ID: $employeeId');
    print('Salary: \$${calculateSalary()}');
  }
  
  // Another concrete method
  void clockIn() {
    print('$name clocked in at ${DateTime.now()}');
  }
  
  // Abstract getter
  String get employeeType;
}

// Full-time employee implementation
class FullTimeEmployee extends Employee {
  double monthlySalary;
  
  FullTimeEmployee({
    required String name,
    required String employeeId,
    required this.monthlySalary,
  }) : super(name: name, employeeId: employeeId);
  
  @override
  double calculateSalary() {
    return monthlySalary;
  }
  
  @override
  String get employeeType => 'Full-Time';
}

// Part-time employee implementation
class PartTimeEmployee extends Employee {
  double hourlyRate;
  int hoursWorked;
  
  PartTimeEmployee({
    required String name,
    required String employeeId,
    required this.hourlyRate,
    required this.hoursWorked,
  }) : super(name: name, employeeId: employeeId);
  
  @override
  double calculateSalary() {
    return hourlyRate * hoursWorked;
  }
  
  @override
  String get employeeType => 'Part-Time';
}

// Contractor implementation
class Contractor extends Employee {
  double projectRate;
  int projectsCompleted;
  
  Contractor({
    required String name,
    required String employeeId,
    required this.projectRate,
    required this.projectsCompleted,
  }) : super(name: name, employeeId: employeeId);
  
  @override
  double calculateSalary() {
    return projectRate * projectsCompleted;
  }
  
  @override
  String get employeeType => 'Contractor';
}

// Usage
void main() {
  List<Employee> employees = [
    FullTimeEmployee(
      name: 'John Doe',
      employeeId: 'FT001',
      monthlySalary: 5000,
    ),
    PartTimeEmployee(
      name: 'Jane Smith',
      employeeId: 'PT001',
      hourlyRate: 25,
      hoursWorked: 80,
    ),
    Contractor(
      name: 'Bob Johnson',
      employeeId: 'CT001',
      projectRate: 1000,
      projectsCompleted: 3,
    ),
  ];
  
  for (Employee emp in employees) {
    print('\n${emp.employeeType} Employee:');
    emp.displayInfo();
  }
}
```

**Flutter Abstract Class Example:**

```dart
// Abstract base widget for list items
abstract class BaseListItem extends StatelessWidget {
  final String id;
  final String title;
  
  const BaseListItem({
    Key? key,
    required this.id,
    required this.title,
  }) : super(key: key);
  
  // Abstract method - must be implemented
  Widget buildLeading();
  Widget buildTrailing();
  void onTap(BuildContext context);
  
  // Concrete method - shared implementation
  Widget buildTitle() {
    return Text(
      title,
      style: const TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w500,
      ),
    );
  }
  
  // Template method pattern
  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      child: ListTile(
        leading: buildLeading(),
        title: buildTitle(),
        trailing: buildTrailing(),
        onTap: () => onTap(context),
      ),
    );
  }
}

// User list item
class UserListItem extends BaseListItem {
  final String email;
  final String imageUrl;
  
  const UserListItem({
    Key? key,
    required String id,
    required String title,
    required this.email,
    required this.imageUrl,
  }) : super(key: key, id: id, title: title);
  
  @override
  Widget buildLeading() {
    return CircleAvatar(
      backgroundImage: NetworkImage(imageUrl),
    );
  }
  
  @override
  Widget buildTrailing() {
    return const Icon(Icons.chevron_right);
  }
  
  @override
  void onTap(BuildContext context) {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => UserDetailPage(userId: id),
      ),
    );
  }
}

// Product list item
class ProductListItem extends BaseListItem {
  final double price;
  final bool inStock;
  
  const ProductListItem({
    Key? key,
    required String id,
    required String title,
    required this.price,
    required this.inStock,
  }) : super(key: key, id: id, title: title);
  
  @override
  Widget buildLeading() {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue.shade100,
        borderRadius: BorderRadius.circular(8),
      ),
      child: const Icon(Icons.shopping_bag, color: Colors.blue),
    );
  }
  
  @override
  Widget buildTrailing() {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      crossAxisAlignment: CrossAxisAlignment.end,
      children: [
        Text(
          '\$${price.toStringAsFixed(2)}',
          style: const TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.bold,
          ),
        ),
        Text(
          inStock ? 'In Stock' : 'Out of Stock',
          style: TextStyle(
            fontSize: 12,
            color: inStock ? Colors.green : Colors.red,
          ),
        ),
      ],
    );
  }
  
  @override
  void onTap(BuildContext context) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Product $title tapped')),
    );
  }
}

// Placeholder pages
class UserDetailPage extends StatelessWidget {
  final String userId;
  const UserDetailPage({Key? key, required this.userId}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('User Details')),
      body: Center(child: Text('User ID: $userId')),
    );
  }
}
```

---

## 8. Singleton Pattern

### What is Singleton?

Singleton is a design pattern that ensures a class has only one instance throughout the application lifecycle and provides a global point of access to that instance.

**Benefits:**
- Controlled access to single instance
- Reduced memory footprint
- Shared resource management
- Global state management

### When to Use Singleton

**Good Use Cases:**
- Database connections
- API clients
- Configuration managers
- Logger services
- Cache managers
- Authentication services

**Bad Use Cases:**
- When you need multiple instances
- When testing requires different instances
- When state should not be shared globally
- When it creates tight coupling

### Implementation Methods

**Method 1: Factory Constructor**

```dart
class DatabaseService {
  // Private constructor
  DatabaseService._privateConstructor();
  
  // Single instance
  static final DatabaseService _instance = DatabaseService._privateConstructor();
  
  // Factory constructor returns the same instance
  factory DatabaseService() {
    return _instance;
  }
  
  // Instance methods
  void connect() {
    print('Database connected');
  }
  
  void query(String sql) {
    print('Executing query: $sql');
  }
}

// Usage
void main() {
  DatabaseService db1 = DatabaseService();
  DatabaseService db2 = DatabaseService();
  
  print(identical(db1, db2)); // true - same instance
}
```

**Method 2: Static Instance with Getter**

```dart
class ApiService {
  // Private constructor
  ApiService._();
  
  // Static instance
  static final ApiService _instance = ApiService._();
  
  // Getter for instance
  static ApiService get instance => _instance;
  
  // Properties
  final String baseUrl = 'https://api.example.com';
  String? _authToken;
  
  // Methods
  void setAuthToken(String token) {
    _authToken = token;
  }
  
  Future<Map<String, dynamic>> get(String endpoint) async {
    print('GET $baseUrl/$endpoint');
    print('Auth Token: $_authToken');
    // Simulate API call
    return {'data': 'response'};
  }
}

// Usage
void main() {
  ApiService.instance.setAuthToken('abc123');
  ApiService.instance.get('users');
}
```

**Method 3: Lazy Initialization**

```dart
class ConfigurationManager {
  ConfigurationManager._();
  
  static ConfigurationManager? _instance;
  
  static ConfigurationManager get instance {
    _instance ??= ConfigurationManager._();
    return _instance!;
  }
  
  // Configuration properties
  Map<String, dynamic> _config = {};
  
  void loadConfig(Map<String, dynamic> config) {
    _config = config;
  }
  
  dynamic getConfig(String key) {
    return _config[key];
  }
}
```

**Flutter Singleton Examples:**

```dart
// Theme Manager Singleton
class ThemeManager {
  ThemeManager._();
  
  static final ThemeManager _instance = ThemeManager._();
  static ThemeManager get instance => _instance;
  
  ThemeMode _themeMode = ThemeMode.system;
  
  ThemeMode get themeMode => _themeMode;
  
  void setThemeMode(ThemeMode mode) {
    _themeMode = mode;
  }
  
  void toggleTheme() {
    _themeMode = _themeMode == ThemeMode.light 
        ? ThemeMode.dark 
        : ThemeMode.light;
  }
}

// Navigation Service Singleton
class NavigationService {
  NavigationService._();
  
  static final NavigationService _instance = NavigationService._();
  static NavigationService get instance => _instance;
  
  final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();
  
  Future<dynamic> navigateTo(String routeName, {Object? arguments}) {
    return navigatorKey.currentState!.pushNamed(
      routeName,
      arguments: arguments,
    );
  }
  
  void goBack() {
    return navigatorKey.currentState!.pop();
  }
  
  void navigateAndRemoveUntil(String routeName) {
    navigatorKey.currentState!.pushNamedAndRemoveUntil(
      routeName,
      (route) => false,
    );
  }
}

// User Session Singleton
class UserSession {
  UserSession._();
  
  static final UserSession _instance = UserSession._();
  static UserSession get instance => _instance;
  
  String? _userId;
  String? _userName;
  String? _email;
  bool _isLoggedIn = false;
  
  bool get isLoggedIn => _isLoggedIn;
  String? get userId => _userId;
  String? get userName => _userName;
  String? get email => _email;
  
  void login({
    required String userId,
    required String userName,
    required String email,
  }) {
    _userId = userId;
    _userName = userName;
    _email = email;
    _isLoggedIn = true;
  }
  
  void logout() {
    _userId = null;
    _userName = null;
    _email = null;
    _isLoggedIn = false;
  }
  
  void updateProfile({String? userName, String? email}) {
    if (userName != null) _userName = userName;
    if (email != null) _email = email;
  }
}

// Cache Manager Singleton
class CacheManager {
  CacheManager._();
  
  static final CacheManager _instance = CacheManager._();
  factory CacheManager() => _instance;
  
  final Map<String, dynamic> _cache = {};
  final Map<String, DateTime> _cacheExpiry = {};
  
  void set(String key, dynamic value, {Duration? expiry}) {
    _cache[key] = value;
    if (expiry != null) {
      _cacheExpiry[key] = DateTime.now().add(expiry);
    }
  }
  
  dynamic get(String key) {
    if (_cacheExpiry.containsKey(key)) {
      if (DateTime.now().isAfter(_cacheExpiry[key]!)) {
        _cache.remove(key);
        _cacheExpiry.remove(key);
        return null;
      }
    }
    return _cache[key];
  }
  
  void clear() {
    _cache.clear();
    _cacheExpiry.clear();
  }
  
  void remove(String key) {
    _cache.remove(key);
    _cacheExpiry.remove(key);
  }
}
```

---

## 9. OOP and Performance in Mobile Apps

### Good vs Bad Practices

Understanding how OOP decisions impact performance is crucial for building efficient Flutter applications.

**Performance Impact Factors:**
- Object creation frequency
- Memory management
- Widget rebuilds
- State management
- Resource disposal

### Memory Management

**Bad Practice: Creating Objects in Build Method**

```dart
// ❌ BAD - Creates new objects on every build
class BadPerformanceWidget extends StatelessWidget {
  const BadPerformanceWidget({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    // New TextStyle object created on every build
    final textStyle = TextStyle(
      fontSize: 16,
      fontWeight: FontWeight.bold,
      color: Colors.blue,
    );
    
    // New EdgeInsets object created on every build
    final padding = EdgeInsets.all(16);
    
    return Padding(
      padding: padding,
      child: Text('Hello World', style: textStyle),
    );
  }
}
```

**Good Practice: Const Objects and Static Fields**

```dart
// ✅ GOOD - Reuses const objects
class GoodPerformanceWidget extends StatelessWidget {
  const GoodPerformanceWidget({Key? key}) : super(key: key);
  
  // Static const - created once, reused everywhere
  static const TextStyle _textStyle = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
  );
  
  static const EdgeInsets _padding = EdgeInsets.all(16);
  
  @override
  Widget build(BuildContext context) {
    return const Padding(
      padding: _padding,
      child: Text('Hello World', style: _textStyle),
    );
  }
}
```

**Bad Practice: Not Disposing Resources**

```dart
// ❌ BAD - Controllers not disposed, memory leak
class BadControllerWidget extends StatefulWidget {
  const BadControllerWidget({Key? key}) : super(key: key);
  
  @override
  State<BadControllerWidget> createState() => _BadControllerWidgetState();
}

class _BadControllerWidgetState extends State<BadControllerWidget> {
  final TextEditingController controller = TextEditingController();
  final ScrollController scrollController = ScrollController();
  final AnimationController animationController = AnimationController(
    vsync: this,
    duration: const Duration(seconds: 1),
  );
  
  // Missing dispose method - MEMORY LEAK!
  
  @override
  Widget build(BuildContext context) {
    return TextField(controller: controller);
  }
}
```

**Good Practice: Proper Resource Disposal**

```dart
// ✅ GOOD - Properly disposed resources
class GoodControllerWidget extends StatefulWidget {
  const GoodControllerWidget({Key? key}) : super(key: key);
  
  @override
  State<GoodControllerWidget> createState() => _GoodControllerWidgetState();
}

class _GoodControllerWidgetState extends State<GoodControllerWidget> 
    with SingleTickerProviderStateMixin {
  late final TextEditingController _controller;
  late final ScrollController _scrollController;
  late final AnimationController _animationController;
  
  @override
  void initState() {
    super.initState();
    _controller = TextEditingController();
    _scrollController = ScrollController();
    _animationController = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 1),
    );
  }
  
  @override
  void dispose() {
    // Dispose in reverse order of creation
    _animationController.dispose();
    _scrollController.dispose();
    _controller.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return TextField(controller: _controller);
  }
}
```

### Widget Performance

**Bad Practice: Unnecessary Widget Rebuilds**

```dart
// ❌ BAD - Entire widget rebuilds on every change
class BadCounterWidget extends StatefulWidget {
  const BadCounterWidget({Key? key}) : super(key: key);
  
  @override
  State<BadCounterWidget> createState() => _BadCounterWidgetState();
}

class _BadCounterWidgetState extends State<BadCounterWidget> {
  int _counter = 0;
  
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    print('Entire widget rebuilt'); // This prints on every increment
    
    return Column(
      children: [
        // This expensive widget rebuilds unnecessarily
        ExpensiveWidget(),
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}

class ExpensiveWidget extends StatelessWidget {
  const ExpensiveWidget({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    print('ExpensiveWidget rebuilt'); // Unnecessary rebuild
    return Container(
      height: 200,
      color: Colors.blue,
      child: const Center(child: Text('I rebuild every time!')),
    );
  }
}
```

**Good Practice: Optimized Widget Rebuilds**

```dart
// ✅ GOOD - Only necessary parts rebuild
class GoodCounterWidget extends StatefulWidget {
  const GoodCounterWidget({Key? key}) : super(key: key);
  
  @override
  State<GoodCounterWidget> createState() => _GoodCounterWidgetState();
}

class _GoodCounterWidgetState extends State<GoodCounterWidget> {
  int _counter = 0;
  
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    print('Widget rebuilt');
    
    return Column(
      children: [
        // const prevents rebuilds
        const ExpensiveWidget(),
        // Only this part rebuilds
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}

// Or use separate widget for counter display
class CounterDisplay extends StatelessWidget {
  final int counter;
  
  const CounterDisplay({Key? key, required this.counter}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    print('Only CounterDisplay rebuilt');
    return Text('Counter: $counter');
  }
}
```

**Bad Practice: Creating Functions in Build Method**

```dart
// ❌ BAD - New function created on every build
class BadCallbackWidget extends StatelessWidget {
  const BadCallbackWidget({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      // New VoidCallback created every build
      onPressed: () {
        print('Button pressed');
      },
      child: const Text('Click Me'),
    );
  }
}
```

**Good Practice: Extracting Callbacks**

```dart
// ✅ GOOD - Callback method reused
class GoodCallbackWidget extends StatelessWidget {
  const GoodCallbackWidget({Key? key}) : super(key: key);
  
  void _handlePress() {
    print('Button pressed');
  }
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: _handlePress, // Reuses same method reference
      child: const Text('Click Me'),
    );
  }
}
```

---

## 10. Real-Time Flutter Examples

### User Management System

```dart
// User model with proper encapsulation
class User {
  final String id;
  String _name;
  String _email;
  UserRole _role;
  
  User({
    required this.id,
    required String name,
    required String email,
    required UserRole role,
  })  : _name = name,
        _email = email,
        _role = role;
  
  // Getters
  String get name => _name;
  String get email => _email;
  UserRole get role => _role;
  
  // Setters with validation
  set name(String value) {
    if (value.trim().isNotEmpty) {
      _name = value;
    } else {
      throw Exception('Name cannot be empty');
    }
  }
  
  set email(String value) {
    if (value.contains('@') && value.contains('.')) {
      _email = value;
    } else {
      throw Exception('Invalid email format');
    }
  }
  
  set role(UserRole value) {
    _role = value;
  }
  
  // Methods
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': _name,
      'email': _email,
      'role': _role.toString(),
    };
  }
  
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      role: UserRole.values.firstWhere(
        (e) => e.toString() == json['role'],
      ),
    );
  }
}

enum UserRole { admin, user, guest }

// Abstract repository
abstract class UserRepository {
  Future<List<User>> getUsers();
  Future<User?> getUserById(String id);
  Future<void> addUser(User user);
  Future<void> updateUser(User user);
  Future<void> deleteUser(String id);
}

// Implementation
class ApiUserRepository implements UserRepository {
  final ApiService _apiService = ApiService.instance;
  
  @override
  Future<List<User>> getUsers() async {
    try {
      final response = await _apiService.get('users');
      return (response['data'] as List)
          .map((json) => User.fromJson(json))
          .toList();
    } catch (e) {
      print('Error fetching users: $e');
      return [];
    }
  }
  
  @override
  Future<User?> getUserById(String id) async {
    try {
      final response = await _apiService.get('users/$id');
      return User.fromJson(response['data']);
    } catch (e) {
      print('Error fetching user: $e');
      return null;
    }
  }
  
  @override
  Future<void> addUser(User user) async {
    await _apiService.post('users', user.toJson());
  }
  
  @override
  Future<void> updateUser(User user) async {
    await _apiService.put('users/${user.id}', user.toJson());
  }
  
  @override
  Future<void> deleteUser(String id) async {
    await _apiService.delete('users/$id');
  }
}

// Service layer
class UserService {
  final UserRepository _repository;
  
  UserService(this._repository);
  
  Future<List<User>> getAllUsers() async {
    return await _repository.getUsers();
  }
  
  Future<User?> getUser(String id) async {
    return await _repository.getUserById(id);
  }
  
  Future<bool> createUser({
    required String name,
    required String email,
    required UserRole role,
  }) async {
    try {
      final user = User(
        id: DateTime.now().millisecondsSinceEpoch.toString(),
        name: name,
        email: email,
        role: role,
      );
      await _repository.addUser(user);
      return true;
    } catch (e) {
      print('Error creating user: $e');
      return false;
    }
  }
  
  Future<bool> updateUserDetails(User user) async {
    try {
      await _repository.updateUser(user);
      return true;
    } catch (e) {
      print('Error updating user: $e');
      return false;
    }
  }
  
  Future<bool> removeUser(String id) async {
    try {
      await _repository.deleteUser(id);
      return true;
    } catch (e) {
      print('Error deleting user: $e');
      return false;
    }
  }
}

// Widget implementation
class UserListWidget extends StatefulWidget {
  const UserListWidget({Key? key}) : super(key: key);
  
  @override
  State<UserListWidget> createState() => _UserListWidgetState();
}

class _UserListWidgetState extends State<UserListWidget> {
  final UserService _userService = UserService(ApiUserRepository());
  List<User> _users = [];
  bool _isLoading = false;
  
  @override
  void initState() {
    super.initState();
    _loadUsers();
  }
  
  Future<void> _loadUsers() async {
    setState(() => _isLoading = true);
    _users = await _userService.getAllUsers();
    setState(() => _isLoading = false);
  }
  
  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return const Center(child: CircularProgressIndicator());
    }
    
    return ListView.builder(
      itemCount: _users.length,
      itemBuilder: (context, index) {
        final user = _users[index];
        return UserListItem(user: user);
      },
    );
  }
}

class UserListItem extends StatelessWidget {
  final User user;
  
  const UserListItem({Key? key, required this.user}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return ListTile(
      leading: CircleAvatar(
        child: Text(user.name[0].toUpperCase()),
      ),
      title: Text(user.name),
      subtitle: Text(user.email),
      trailing: Text(user.role.toString().split('.').last),
    );
  }
}
```

### Custom Form Widgets

```dart
// Abstract form field base class
abstract class BaseFormField extends StatefulWidget {
  final String label;
  final String? initialValue;
  final String? Function(String?)? validator;
  final void Function(String?)? onSaved;
  
  const BaseFormField({
    Key? key,
    required this.label,
    this.initialValue,
    this.validator,
    this.onSaved,
  }) : super(key: key);
  
  // Template method
  InputDecoration getDecoration(String label) {
    return InputDecoration(
      labelText: label,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(8),
      ),
      contentPadding: const EdgeInsets.symmetric(
        horizontal: 16,
        vertical: 14,
      ),
    );
  }
}

// Email field implementation
class EmailFormField extends BaseFormField {
  const EmailFormField({
    Key? key,
    required String label,
    String? initialValue,
    void Function(String?)? onSaved,
  }) : super(
          key: key,
          label: label,
          initialValue: initialValue,
          onSaved: onSaved,
        );
  
  @override
  State<EmailFormField> createState() => _EmailFormFieldState();
}

class _EmailFormFieldState extends State<EmailFormField> {
  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'Email is required';
    }
    final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
    if (!emailRegex.hasMatch(value)) {
      return 'Please enter a valid email';
    }
    return null;
  }
  
  @override
  Widget build(BuildContext context) {
    return TextFormField(
      initialValue: widget.initialValue,
      decoration: widget.getDecoration(widget.label).copyWith(
        prefixIcon: const Icon(Icons.email),
      ),
      keyboardType: TextInputType.emailAddress,
      validator: _validateEmail,
      onSaved: widget.onSaved,
    );
  }
}

// Phone field implementation
class PhoneFormField extends BaseFormField {
  const PhoneFormField({
    Key? key,
    required String label,
    String? initialValue,
    void Function(String?)? onSaved,
  }) : super(
          key: key,
          label: label,
          initialValue: initialValue,
          onSaved: onSaved,
        );
  
  @override
  State<PhoneFormField> createState() => _PhoneFormFieldState();
}

class _PhoneFormFieldState extends State<PhoneFormField> {
  String? _validatePhone(String? value) {
    if (value == null || value.isEmpty) {
      return 'Phone number is required';
    }
    final phoneRegex = RegExp(r'^\+?[\d\s-]{10,}$');
    if (!phoneRegex.hasMatch(value)) {
      return 'Please enter a valid phone number';
    }
    return null;
  }
  
  @override
  Widget build(BuildContext context) {
    return TextFormField(
      initialValue: widget.initialValue,
      decoration: widget.getDecoration(widget.label).copyWith(
        prefixIcon: const Icon(Icons.phone),
      ),
      keyboardType: TextInputType.phone,
      validator: _validatePhone,
      onSaved: widget.onSaved,
    );
  }
}

// Usage in a form
class RegistrationForm extends StatefulWidget {
  const RegistrationForm({Key? key}) : super(key: key);
  
  @override
  State<RegistrationForm> createState() => _RegistrationFormState();
}

class _RegistrationFormState extends State<RegistrationForm> {
  final _formKey = GlobalKey<FormState>();
  String? _email;
  String? _phone;
  
  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      _formKey.currentState!.save();
      print('Email: $_email, Phone: $_phone');
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          EmailFormField(
            label: 'Email',
            onSaved: (value) => _email = value,
          ),
          const SizedBox(height: 16),
          PhoneFormField(
            label: 'Phone',
            onSaved: (value) => _phone = value,
          ),
          const SizedBox(height: 24),
          ElevatedButton(
            onPressed: _submitForm,
            child: const Text('Submit'),
          ),
        ],
      ),
    );
  }
}
```

### API Service Layer

```dart
// Singleton API service
class ApiService {
  ApiService._();
  
  static final ApiService _instance = ApiService._();
  static ApiService get instance => _instance;
  
  final String _baseUrl = 'https://api.example.com';
  String? _authToken;
  
  void setAuthToken(String token) {
    _authToken = token;
  }
  
  Map<String, String> get _headers => {
        'Content-Type': 'application/json',
        if (_authToken != null) 'Authorization': 'Bearer $_authToken',
      };
  
  Future<Map<String, dynamic>> get(String endpoint) async {
    print('GET $_baseUrl/$endpoint');
    await Future.delayed(const Duration(seconds: 1));
    return {'data': [], 'success': true};
  }
  
  Future<Map<String, dynamic>> post(
    String endpoint,
    Map<String, dynamic> body,
  ) async {
    print('POST $_baseUrl/$endpoint');
    await Future.delayed(const Duration(seconds: 1));
    return {'data': body, 'success': true};
  }
  
  Future<Map<String, dynamic>> put(
    String endpoint,
    Map<String, dynamic> body,
  ) async {
    print('PUT $_baseUrl/$endpoint');
    await Future.delayed(const Duration(seconds: 1));
    return {'data': body, 'success': true};
  }
  
  Future<Map<String, dynamic>> delete(String endpoint) async {
    print('DELETE $_baseUrl/$endpoint');
    await Future.delayed(const Duration(seconds: 1));
    return {'success': true};
  }
}
```

---

## 11. Performance Case Studies

### Bad Practice: Memory Leaks

```dart
// ❌ BAD - Multiple memory leaks
class BadListScreen extends StatefulWidget {
  const BadListScreen({Key? key}) : super(key: key);
  
  @override
  State<BadListScreen> createState() => _BadListScreenState();
}

class _BadListScreenState extends State<BadListScreen> {
  final ScrollController scrollController = ScrollController();
  Timer? timer;
  List<String> items = List.generate(1000, (i) => 'Item $i');
  
  @override
  void initState() {
    super.initState();
    
    // Memory leak: listener never removed
    scrollController.addListener(() {
      print('Scrolling: ${scrollController.offset}');
    });
    
    // Memory leak: timer never cancelled
    timer = Timer.periodic(const Duration(seconds: 1), (timer) {
      print('Timer tick');
    });
  }
  
  // Missing dispose method - CRITICAL MEMORY LEAK
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      controller: scrollController,
      itemCount: items.length,
      itemBuilder: (context, index) {
        // Creating new TextStyle on every item build
        return ListTile(
          title: Text(
            items[index],
            style: TextStyle(fontSize: 16), // New object each time
          ),
        );
      },
    );
  }
}
```

### Good Practice: Efficient State Management

```dart
// ✅ GOOD - Proper resource management
class GoodListScreen extends StatefulWidget {
  const GoodListScreen({Key? key}) : super(key: key);
  
  @override
  State<GoodListScreen> createState() => _GoodListScreenState();
}

class _GoodListScreenState extends State<GoodListScreen> {
  late final ScrollController _scrollController;
  Timer? _timer;
  late final List<String> _items;
  
  // Const text style - created once
  static const TextStyle _itemTextStyle = TextStyle(fontSize: 16);
  
  @override
  void initState() {
    super.initState();
    _scrollController = ScrollController();
    _items = List.generate(1000, (i) => 'Item $i');
    
    // Add listener
    _scrollController.addListener(_onScroll);
    
    // Start timer
    _timer = Timer.periodic(
      const Duration(seconds: 1),
      _onTimerTick,
    );
  }
  
  void _onScroll() {
    // Separate method for listener
    if (_scrollController.offset > 100) {
      // Do something
    }
  }
  
  void _onTimerTick(Timer timer) {
    // Separate method for timer
    if (mounted) {
      print('Timer tick');
    }
  }
  
  @override
  void dispose() {
    // Clean up in proper order
    _timer?.cancel();
    _scrollController.removeListener(_onScroll);
    _scrollController.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      controller: _scrollController,
      itemCount: _items.length,
      itemBuilder: (context, index) {
        return ListTile(
          title: Text(
            _items[index],
            style: _itemTextStyle, // Reused const style
          ),
        );
      },
    );
  }
}
```

**Performance Comparison:**

| Aspect | Bad Practice | Good Practice | Impact |
|--------|--------------|---------------|--------|
| Memory Leaks | Controllers not disposed | Proper dispose | High |
| Object Creation | New objects in build | Const objects | Medium |
| Listeners | Never removed | Removed in dispose | High |
| Timers | Never cancelled | Cancelled in dispose | High |
| Style Objects | Created per item | Static const | Medium |

**ListView Performance Optimization:**

```dart
// ✅ Optimized ListView with proper practices
class OptimizedProductList extends StatefulWidget {
  const OptimizedProductList({Key? key}) : super(key: key);
  
  @override
  State<OptimizedProductList> createState() => _OptimizedProductListState();
}

class _OptimizedProductListState extends State<OptimizedProductList> {
  late final List<Product> _products;
  
  // Static const styles
  static const EdgeInsets _itemPadding = EdgeInsets.symmetric(
    horizontal: 16,
    vertical: 8,
  );
  
  static const TextStyle _titleStyle = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.bold,
  );
  
  static const TextStyle _priceStyle = TextStyle(
    fontSize: 14,
    color: Colors.green,
  );
  
  @override
  void initState() {
    super.initState();
    _products = List.generate(
      100,
      (i) => Product(
        id: i.toString(),
        name: 'Product $i',
        price: (i + 1) * 10.0,
      ),
    );
  }
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      // Optimize for large lists
      cacheExtent: 100,
      itemCount: _products.length,
      itemBuilder: (context, index) {
        return ProductListItem(
          product: _products[index],
          padding: _itemPadding,
          titleStyle: _titleStyle,
          priceStyle: _priceStyle,
        );
      },
    );
  }
}

// Separate widget for list item
class ProductListItem extends StatelessWidget {
  final Product product;
  final EdgeInsets padding;
  final TextStyle titleStyle;
  final TextStyle priceStyle;
  
  const ProductListItem({
    Key? key,
    required this.product,
    required this.padding,
    required this.titleStyle,
    required this.priceStyle,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: padding,
      child: Card(
        child: ListTile(
          title: Text(product.name, style: titleStyle),
          subtitle: Text(
            '\$${product.price.toStringAsFixed(2)}',
            style: priceStyle,
          ),
          trailing: const Icon(Icons.arrow_forward_ios, size: 16),
          onTap: () {
            // Handle tap
          },
        ),
      ),
    );
  }
}

class Product {
  final String id;
  final String name;
  final double price;
  
  Product({
    required this.id,
    required this.name,
    required this.price,
  });
}
```

---

## 12. Best Practices Summary

### Class Design Principles

1. **Single Responsibility**: Each class should have one reason to change
2. **Encapsulation**: Hide internal implementation details
3. **Composition over Inheritance**: Favor composition when possible
4. **Interface Segregation**: Keep interfaces focused and small
5. **Dependency Injection**: Pass dependencies rather than creating them

### Flutter-Specific Best Practices

```dart
// ✅ Good practices demonstrated
class BestPracticesExample extends StatefulWidget {
  // 1. Use const constructors when possible
  const BestPracticesExample({Key? key}) : super(key: key);
  
  @override
  State<BestPracticesExample> createState() => _BestPracticesExampleState();
}

class _BestPracticesExampleState extends State<BestPracticesExample> {
  // 2. Use late for non-nullable fields initialized in initState
  late final TextEditingController _controller;
  
  // 3. Use static const for styles and constants
  static const EdgeInsets _padding = EdgeInsets.all(16);
  static const TextStyle _textStyle = TextStyle(fontSize: 16);
  
  @override
  void initState() {
    super.initState();
    // 4. Initialize resources in initState
    _controller = TextEditingController();
  }
  
  @override
  void dispose() {
    // 5. Always dispose resources
    _controller.dispose();
    super.dispose();
  }
  
  // 6. Extract methods for callbacks
  void _handleSubmit() {
    print(_controller.text);
  }
  
  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: _padding,
      child: Column(
        children: [
          // 7. Use const widgets when possible
          const Text('Enter text:', style: _textStyle),
          TextField(
            controller: _controller,
            onSubmitted: (_) => _handleSubmit(),
          ),
        ],
      ),
    );
  }
}
```

### Performance Checklist

- [ ] Use const constructors wherever possible
- [ ] Dispose all controllers, listeners, and streams
- [ ] Avoid creating objects in build methods
- [ ] Use static const for styles and padding
- [ ] Extract callbacks to methods
- [ ] Implement proper key usage for lists
- [ ] Use ListView.builder for long lists
- [ ] Implement pagination for large datasets
- [ ] Cache network images
- [ ] Use isolates for heavy computations

### Common Pitfalls to Avoid

| Pitfall | Impact | Solution |
|---------|--------|----------|
| Not disposing controllers | Memory leaks | Always implement dispose |
| Creating objects in build | Performance | Use const or static |
| Deep widget trees | Rebuild cost | Extract widgets |
| Not using keys | Widget state loss | Use proper keys |
| Synchronous heavy operations | UI freezing | Use async/isolates |
| Global mutable state | Unpredictable bugs | Use proper state management |

### Final Recommendations

1. **Always think about lifecycle**: Initialize in initState, clean up in dispose
2. **Prefer immutability**: Use final for variables that don't change
3. **Use meaningful names**: Classes and methods should be self-documenting
4. **Write testable code**: Inject dependencies, use interfaces
5. **Profile your app**: Use Flutter DevTools to identify bottlenecks
6. **Follow SOLID principles**: They exist for good reasons
7. **Document complex logic**: Future you will thank present you

---

## Conclusion

Object-Oriented Programming in Flutter is not just about writing code that works—it's about writing code that is maintainable, scalable, and performant. By understanding and properly implementing OOP concepts like encapsulation, inheritance, polymorphism, and design patterns like Singleton, you can build Flutter applications that are both efficient and easy to maintain.

Remember: Good OOP practices lead to good performance, and good performance leads to happy users.

---

**Created for Flutter developers who want to master OOP concepts and build high-performance mobile applications.**

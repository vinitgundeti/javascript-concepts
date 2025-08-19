# javascript-concepts

1. ### What is a prototype chain
  The prototype chain is a core concept in JavaScript’s inheritance model. It allows objects to inherit properties and methods from other objects. When you try to access a property or method on an object, JavaScript first looks for it on that object itself. If it’s not found, the engine looks up the object's internal `[[Prototype]]` reference (accessible via `Object.getPrototypeOf(obj)` or the deprecated `__proto__` property) and continues searching up the chain until it finds the property or reaches the end (usually `null`).

```js
// Base object
const animal = {
  eats: true,
  walk() {
    console.log("Animal walks");
  }
};

// Another object that inherits from animal
const dog = {
  bark() {
    console.log("Woof!");
  }
};

// Set prototype chain: dog -> animal
Object.setPrototypeOf(dog, animal);

// Another object that inherits from dog
const myDog = {
  name: "Buddy"
};
Object.setPrototypeOf(myDog, dog);

// ---- Accessing properties/methods ----
console.log(myDog.name); // ✅ Found directly on myDog
myDog.bark();            // ✅ Not on myDog, found on dog
myDog.walk();            // ✅ Not on myDog or dog, found on animal
console.log(myDog.eats); // ✅ Not on myDog or dog, found on animal

// ---- Checking prototype chain ----
console.log(Object.getPrototypeOf(myDog) === dog);     // true
console.log(Object.getPrototypeOf(dog) === animal);   // true
console.log(Object.getPrototypeOf(animal) === Object.prototype); // true
console.log(Object.getPrototypeOf(Object.prototype)); // null (end of chain)
```

2. ### what is the difference between primitive and non-primitive data-types

### primitive data types
Concept: Primitive data types directly store their value in the memory location allocated for the variable. When you assign a primitive variable to another, a copy of the actual value is made.

Visual Analogy: Imagine a small, individual box labeled with the variable name, and inside that box, the actual value (e.g., the number 10, the letter 'A', the word "hello") is directly placed.
Memory Representation:
Code

    Variable 'x' (Stack Memory)
    +-------+
    |  10   |
    +-------+
When y = x;, a new box for y is created, and the value 10 is copied into it.
Code

    Variable 'x' (Stack Memory)   Variable 'y' (Stack Memory)
    +-------+                     +-------+
    |  10   |                     |  10   |
    +-------+                     +-------+

### non-primitve data types
Concept: Non-primitive data types (like arrays, objects, classes) do not store the actual data directly within the variable's memory location. Instead, they store a reference (memory address) to where the actual data is located in a different memory area (often the heap). When you assign a non-primitive variable to another, the reference is copied, not the data itself. Both variables then point to the same data.
Visual Analogy: Imagine a small box labeled with the variable name, and inside this box, there's a pointer or an arrow pointing to a larger, separate container where the actual complex data structure resides.
Memory Representation:
Code

    Variable 'myArray' (Stack Memory)
    +-------+
    |  -->  |  (Reference/Address)
    +-------+
        |
        V
    [1, 2, 3] (Heap Memory)
When anotherArray = myArray;, a new box for anotherArray is created, and the reference to the same data in the heap is copied.
Code

    Variable 'myArray' (Stack Memory)   Variable 'anotherArray' (Stack Memory)
    +-------+                           +-------+
    |  -->  |  (Reference/Address)      |  -->  |  (Reference/Address)
    +-------+                           +-------+
        |                                   |
        V                                   |
    [1, 2, 3] (Heap Memory) <---------------
Changes made through myArray will be visible through anotherArray because they both point to the same underlying data.

3. ### What is the Difference Between `call`, `apply`, and `bind`

  In JavaScript, `call`, `apply`, and `bind` are methods that allow you to control the context (`this` value) in which a function is executed. While their purposes are similar, they differ in how they handle arguments and when the function is invoked.

  ---

  #### `call`

  - **Description:**  
    The `call()` method invokes a function immediately, allowing you to specify the value of `this` and pass arguments individually (comma-separated).

  - **Syntax:**  
    ```js
    func.call(thisArg, arg1, arg2, ...)
    ```

  - **Example:**
    ```js
    var employee1 = { firstName: "John", lastName: "Rodson" };
    var employee2 = { firstName: "Jimmy", lastName: "Baily" };

    function invite(greeting1, greeting2) {
      console.log(
        greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
      );
    }

    invite.call(employee1, "Hello", "How are you?"); // Hello John Rodson, How are you?
    invite.call(employee2, "Hello", "How are you?"); // Hello Jimmy Baily, How are you?
    ```

  ---

  #### `apply`

  - **Description:**  
    The `apply()` method is similar to `call()`, but it takes the function arguments as an array (or array-like object) instead of individual arguments.

  - **Syntax:**  
    ```js
    func.apply(thisArg, [argsArray])
    ```

  - **Example:**
    ```js
    var employee1 = { firstName: "John", lastName: "Rodson" };
    var employee2 = { firstName: "Jimmy", lastName: "Baily" };

    function invite(greeting1, greeting2) {
      console.log(
        greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
      );
    }

    invite.apply(employee1, ["Hello", "How are you?"]); // Hello John Rodson, How are you?
    invite.apply(employee2, ["Hello", "How are you?"]); // Hello Jimmy Baily, How are you?
    ```

  ---

  #### `bind`

  - **Description:**  
    The `bind()` method creates a new function with a specific `this` value and, optionally, preset initial arguments. Unlike `call` and `apply`, `bind` does **not** immediately invoke the function; instead, it returns a new function that you can call later.

  - **Syntax:**  
    ```js
    var boundFunc = func.bind(thisArg[, arg1[, arg2[, ...]]])
    ```

  - **Example:**
    ```js
    var employee1 = { firstName: "John", lastName: "Rodson" };
    var employee2 = { firstName: "Jimmy", lastName: "Baily" };

    function invite(greeting1, greeting2) {
      console.log(
        greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
      );
    }

    var inviteEmployee1 = invite.bind(employee1);
    var inviteEmployee2 = invite.bind(employee2);

    inviteEmployee1("Hello", "How are you?"); // Hello John Rodson, How are you?
    inviteEmployee2("Hello", "How are you?"); // Hello Jimmy Baily, How are you?
    ```

  ---

  4. ### What are lambda expressions or arrow functions

    **Arrow functions** (also known as "lambda expressions") provide a concise syntax for writing function expressions in JavaScript. Introduced in ES6, arrow functions are often shorter and more readable, especially for simple operations or callbacks.

    #### Key Features:
    - Arrow functions do **not** have their own `this`, `arguments`, `super`, or `new.target` bindings. They inherit these from their surrounding (lexical) context.
    - They are best suited for non-method functions, such as callbacks or simple computations.
    - Arrow functions **cannot** be used as constructors and do not have a `prototype` property.
    - They also cannot be used with `new`, `yield`, or as generator functions.

    #### Syntax Examples:

    ```javascript
    const arrowFunc1 = (a, b) => a + b;    // Multiple parameters, returns a + b
    const arrowFunc2 = a => a * 10;        // Single parameter (parentheses optional), returns a * 10
    const arrowFunc3 = () => {};           // No parameters, returns undefined
    const arrowFunc4 = (a, b) => {
      // Multiple statements require curly braces and explicit return
      const sum = a + b;
      return sum * 2;
    };
    ```
    
    #### elaborating key features
    1️⃣ this binding

  - Normal functions: this depends on how the function is called.
  - Arrow functions: this is lexically bound (taken from the surrounding scope).
```javascript
const person = {
  name: "Vinit",
  normalFn: function() {
    console.log("Normal:", this.name);
  },
  arrowFn: () => {
    console.log("Arrow:", this.name);
  }
};

person.normalFn(); // Normal: Vinit
person.arrowFn();  // Arrow: undefined (or window in browser)
```
👉 Why?

  - In normalFn, this refers to person.
  - In arrowFn, this doesn’t belong to the function itself. It inherits from where it was created (the outer scope — here it’s the global object).

<img width="813" height="642" alt="image" src="https://github.com/user-attachments/assets/78c4ac68-3bf7-4c24-90f7-7c758a650bab" />

<img width="807" height="690" alt="image" src="https://github.com/user-attachments/assets/9f1ffb85-0815-41f2-b989-8ef5302e6592" />

<img width="838" height="507" alt="image" src="https://github.com/user-attachments/assets/304226c5-9446-440f-bd24-de1412ce976a" />

<img width="805" height="385" alt="image" src="https://github.com/user-attachments/assets/7b1c33e2-4358-4b86-b789-ad78f0c65515" />

11. ### What is a first class function

    In JavaScript, **first-class functions(first-class citizens)** mean that functions are treated like any other variable. That means:

    1. You can assign a function to a variable.
    2. You can pass a function as an argument to another function.
    3. You can return a function from another function.

    This capability enables powerful patterns like callbacks, higher-order functions, event handling, and functional programming in JavaScript.
    
    For example, the handler function below is assigned to a variable and then passed as an argument to the `addEventListener` method.

    ```javascript
    const handler = () => console.log("This is a click handler function");
    document.addEventListener("click", handler);
    ```

12. ### What is a first order function

    A first-order function is a function that doesn’t accept another function as an argument and doesn’t return a function as its return value. i.e,  It's a regular function that works with primitive or non-function values.

    ```javascript
    const firstOrder = () => console.log("I am a first order function!");
    ```

13. ### What is a higher order function

    A higher-order function is a function that either accepts another function as an argument, returns a function as its result, or both. This concept is a core part of JavaScript's functional programming capabilities and is widely used for creating modular, reusable, and expressive code.

    The syntactic structure of higher order function will be explained with an example as follows,

      ```javascript
      // First-order function (does not accept or return another function)
      const firstOrderFunc = () => 
        console.log("Hello, I am a first-order function");

      // Higher-order function (accepts a function as an argument)
      const higherOrder = (callback) => callback();

      // Passing the first-order function to the higher-order function
      higherOrder(firstOrderFunc);
      ```

    In this example:

    1. `firstOrderFunc` is a regular (first-order) function.

    2. `higherOrder` is a higher-order function because it takes another function as an argument.

    3. `firstOrderFunc` is also called a **callback function** because it is passed to and executed by another function.

   polyfill of map function
  
<img width="797" height="536" alt="image" src="https://github.com/user-attachments/assets/0fcf9b2d-3f2c-4b39-b7dd-3cea2b00596d" />

   polyfill of filter function

<img width="542" height="232" alt="image" src="https://github.com/user-attachments/assets/6f89bab5-e6ed-4524-99c5-7e6e3f72c05a" />



14. ### What is a unary function

    A unary function (also known as a **monadic** function) is a function that **accepts exactly one argument**. The term "unary" simply refers to the function's arity—the number of arguments it takes.

    Let us take an example of unary function,

    ```javascript
    const unaryFunction = (a) => console.log(a + 10); // This will add 10 to the input and log the result
    unaryFunction(5); // Output: 15
    ```
    In this example:

    - `unaryFunction` takes a single parameter `a`, making it a unary function.
    - It performs a simple operation: adding 10 to the input and printing the result.

15. ### What is the currying function
    
    **Currying** is the process of transforming a function with **multiple arguments** into a sequence of **nested functions**, each accepting **only one argument** at a time.

    This concept is named after mathematician **Haskell Curry**, and is commonly used in functional programming to enhance modularity and reuse.


    ## Before Currying (Normal n-ary Function)

    ```javascript
    const multiArgFunction = (a, b, c) => a + b + c;

    console.log(multiArgFunction(1, 2, 3)); // Output: 6
    ```
    This is a standard function that takes three arguments at once.

    ## After Currying (Unary Function Chain)
    ```javascript
    // normal function
    function curryUnaryFunction(a) {
      return function(b) {
        return function(c) {
          return a + b + c;
          };
        };
      }
    console.log(curriedAdd(1)(2)(3)); // 6

    // with arrow function
    const curryUnaryFunction = (a) => (b) => (c) => a + b + c;

    console.log(curryUnaryFunction(1));       // Returns: function (b) => ...
    console.log(curryUnaryFunction(1)(2));    // Returns: function (c) => ...
    console.log(curryUnaryFunction(1)(2)(3)); // Output: 6

    ```
    Each function in the chain accepts one argument and returns the next function, until all arguments are provided and the final result is computed.

    ## Benefits of Currying
      - Improves code reusability
      → You can partially apply functions with known arguments.

      - Enhances functional composition
      → Easier to compose small, pure functions.

      - Encourages clean, modular code
      → You can split logic into smaller single-responsibility functions.

<img width="803" height="657" alt="image" src="https://github.com/user-attachments/assets/a00ff2ae-e3f3-496e-8d30-9719a1b8e62a" />







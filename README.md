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


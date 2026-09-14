 the **`extends`** keyword is used in class declarations or class expressions to **create a child class that inherits properties and methods from a parent class**. Under the hood, it abstracts JavaScript's [Prototypal Inheritance Model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain) into a cleaner, object-oriented syntax

```ts
// Parent Class
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

// Child Class extending the Parent Class
class Dog extends Animal {
  constructor(name, breed) {
    // 1. Must call super() before using 'this'
    super(name); 
    this.breed = breed;
  }

  // 2. Overriding a method
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog('Max', 'Golden Retriever');
myDog.speak(); // Output: Max barks.

```

Core Concepts of `extends`

- **The `super` Keyword**: If your child class has a `constructor()`, you **must** call `super()` before using the `this` keyword. ==`super()` calls the parent class's constructor ==and passes along any necessary arguments.
- **Method Overriding**: Child classes can redefine methods inherited from the parent class (like `speak()` in the example above).
- **Accessing Parent Methods**: You can still invoke the parent's version of a method inside the child class by calling ==`super.methodName()`.==
- **Static Inheritance**: The `extends` keyword also inherits static properties and methods from the parent class, not just instance methods. 

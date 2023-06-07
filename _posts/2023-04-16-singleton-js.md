---
layout: post
usehighlight: true
tags: [JavaScript, Frontend]
title: Two ways to implement the Singleton pattern in JS
---

In this post I mention two ways to create a class of object using Singleton design pattern. There are many ways we can archive that, but the purpose of this post is I want to share my thought on each implementation, the differences and pros/cons.

### What is Singleton?

The Singleton pattern is commonly used in computer programming when you want to ensure there is only one instance of a class throughout the entire application, and that instance can be accessed globally. This can be useful for scenarios such as managing a database connection, controlling access to a shared resource, monitoring/logging, etc.

To implement a singleton, you have to define  a static instance variable within the class, and provide a static method returns that instance. The constructor of the class is usually made private, so that it cannot be called outside the class, and a new instance can only be created within the class itself.

It's worth noting that while the singleton pattern can be useful in certain situations, it can also make code harder to test and maintain, and can lead to issues with thread safety if not implemented correctly. So it's important to carefully consider whether a singleton is the best solution for your particular problem.

### How about the 'S' in SOLID?

The "S" in the SOLID principle stands for the Single Responsibility Principle, which is different from the Singleton pattern.

The Single Responsibility Principle (SRP) states that a class should have only one reason to change, meaning it should have only one responsibility or task that it is responsible for. This principle helps to improve the maintainability and testability of the code by keeping each class focused on a single responsibility.

The Singleton pattern, on the other hand, is a design pattern that deals with the creation and management of a single instance of a class. While it can be used in conjunction with the SRP, they are two separate concepts.

### Implement Singleton in JavaScript

In JavaScript, you can implement the Singleton design pattern using a simple object literal. Here's an example:

```JavaScript
const mySingleton = {
  instance: null,
  getInstance: function() {
    if (this.instance === null) {
      this.instance = new MySingletonClass();
    }
    return this.instance;
  }
};

class MySingletonClass {
  constructor() {
    // your constructor code here
  }

  // your class methods here
}
```
In this example, we define a singleton object `mySingleton` that has a `getInstance` method. This method checks if the `instance` property is `null`, and if it is, creates a new instance of the `MySingletonClass`. If an instance already exists, the method simply returns that instance.

The `MySingletonClass` is a regular class that defines the behavior of the singleton. You can add your own properties and methods to this class as needed.

To use the singleton, you simply call the `getInstance` method of the `mySingleton` object:

```JavaScript
const singletonInstance = mySingleton.getInstance();
```

This will either create a new instance of the `MySingletonClass` and return it, or return the existing instance if it has already been created.

Note that this implementation is not thread-safe, meaning that it does not guarantee that only one instance will be created in a multithreaded environment. If thread-safety is a concern, you may need to use additional techniques such as locking or synchronization to ensure that only one instance is created. We will look back this problem at the end of the post. Let's move on the second implementation.

The second implement is you can move the `getInstance` method into the class and make the constructor private to ensure that only one instance of the class can be created. Here's an example implementation:

```JavaScript
class MySingletonClass {
  static instance = null;

  // Private constructor
  constructor() {
    // your constructor code here
  }

  static getInstance() {
    if (MySingletonClass.instance === null) {
      MySingletonClass.instance = new MySingletonClass();
    }
    return MySingletonClass.instance;
  }

  // your class methods here
}
```

In this implementation, the `MySingletonClass` has a `static` property called `instance` that is initially set to `null`. The constructor is made `private` by not exposing it as a `public` method.

The `getInstance` method is now defined as a `static` method within the `MySingletonClass`. It checks whether the `instance` property is `null`, and if it is, creates a new instance of the `MySingletonClass`. If an instance already exists, the method simply returns that instance.

To use the singleton, you call the `getInstance` method of the `MySingletonClass`:

```JavaScript
const singletonInstance = MySingletonClass.getInstance();
```
This implementation also ensures that only one instance of the class is created and that it is globally accessible through the `getInstance` method.

So, we have multiple ways to implement the Singleton pattern in JavaScript, and the implementation I provided earlier using an object literal is just one of them. The implementation with the `getInstance` method inside the class and the private constructor is another approach that is commonly used. Both implementations achieve the same result of ensuring that only one instance of the class is created and providing global access to that instance. The choice of implementation depends on the specific needs of your application and your personal preferences as a developer.

In the implementation with the `getInstance` method inside the class and the private constructor, you cannot create a new instance of the class by calling the new method directly. That's because the constructor is made `private` and not exposed as a `public` method.

If you try to create a new instance of the `MySingletonClass` by calling `new MySingletonClass()`, you will get a `TypeError` saying that the constructor is not defined:

```JavaScript
const singletonInstance = new MySingletonClass(); // TypeError: MySingletonClass is not defined
```

However, in the implementation using an object literal, you can create a new instance of the class by calling the new method directly, but doing so would violate the Singleton pattern by creating multiple instances of the class. To ensure that only one instance of the class is created, you must use the `getInstance` method of the object literal:

```JavaScript
const singletonInstance = mySingleton.getInstance();
```

This will either create a new instance of the `MySingletonClass` and return it, or return the existing instance if it has already been created.

### The multi-thread problem

The implementation with the private constructor and the `getInstance` method inside or outside the class does not provide thread-safety and cannot guarantee that only one instance of the class will be created in a multi-threaded environment.

If multiple threads call the `getInstance` method at the same time, they may all see that the `instance` property is `null` and attempt to create a new instance of the class. This can result in multiple instances being created, which violates the Singleton pattern.

To ensure that only one instance of the class is created in a multi-threaded environment, you will need to use additional techniques such as locking or synchronization to ensure that the `getInstance` method can only be executed by one thread at a time. Alternatively, you can use an implementation that is thread-safe by default, such as using an ES6 `class` with a `static` property that holds the singleton instance, which can only be created once by the ES6 runtime.

```JavaScript
class MySingletonClass {
  static #instance = null;

  // Private constructor
  constructor() {
    // your constructor code here
  }

  static getInstance() {
    if (MySingletonClass.#instance === null) {
      MySingletonClass.#instance = new MySingletonClass();
    }
    return MySingletonClass.#instance;
  }

  // your class methods here
}
```
In this implementation, the `MySingletonClass` has a `static` property called `#instance` that is initially set to `null`. The `#` prefix makes the instance property private, which means it can only be accessed from within the class.

The constructor is made `private` by not exposing it as a `public` method.

The `getInstance` method is defined as a `static` method within the `MySingletonClass`. It checks whether the `#instance` property is `null`, and if it is, creates a new instance of the `MySingletonClass`. If an instance already exists, the method simply returns that instance.

To use the singleton, you call the `getInstance` method of the `MySingletonClass`:

```JavaScript
const singletonInstance = MySingletonClass.getInstance();
```

This implementation is thread-safe by default because the `static` keyword ensures that the `#instance` property can only be created once by the ES6 runtime.
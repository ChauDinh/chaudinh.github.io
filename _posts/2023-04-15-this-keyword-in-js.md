---
layout: post
usehighlight: true
tags: [JavaScript, Frontend]
title: The 'this' keyword in JavaScript
---

This post is what I understand about the `this` keyword in JavaScript, which would be appeared many times in coding interviews or the daily tasks.

In JavaScript, `this` refers to the object that the function is a property of, or the object that the function is being called on. In other words, it refers to the current execution context.

The value of `this` is determined at the time of function invocation, and can be set in several ways:

### Implicit binding

When a function is called with a dot `.` notation on an object, the `this` keyword refers to that object.

```JavaScript
const obj = {
  name: 'John',
  sayHello: function() {
    console.log(`Hello, my name is ${this.name}.`);
  }
};

obj.sayHello(); // output: Hello, my name is John.
```
In the above code, when we call the `sayHello` method on the `obj` object, the `this` keyword inside the function refers to the `obj` object.

### Explicit binding

We can also explicitly set the value of `this` using the `call()`, `apply()`, or `bind()` methods.

```JavaScript
function sayHello() {
  console.log(`Hello, my name is ${this.name}.`);
}

const obj1 = { name: 'John' };
const obj2 = { name: 'Mary' };

sayHello.call(obj1); // output: Hello, my name is John.
sayHello.apply(obj2); // output: Hello, my name is Mary.
const newFunc = sayHello.bind(obj1);
newFunc(); // output: Hello, my name is John.
```

In the above code, we use the `call()` method to explicitly set the value of `this` to the `obj1` object when we call the `sayHello` function.

### Arrow function

With arrow functions, the value of `this` is determined by the surrounding lexical scope (i.e., the enclosing function or global scope).

```JavaScript
const obj = {
  name: 'John',
  sayHello: () => {
    console.log(`Hello, my name is ${this.name}.`);
  }
};

obj.sayHello(); // output: Hello, my name is undefined.
```

In the above code, the arrow function is not bound to the `obj` object, so the value of `this` inside the function refers to the global object, which does not have a `name` property.

### Constructor context

When a function is called with the `new` keyword, the value of `this` inside the function refers to the new object being created.

```JavaScript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const john = new Person('John', 30);
console.log(john.name); // output: John
console.log(john.age); // output: 30
```

In the above code, when we call the `Person` function with the `new` keyword, a new object is created and the value of `this` inside the function refers to that new object.

### Event context

When a function is used as an event listener, the value of `this` inside the function refers to the element that triggered the event.

```JavaScript
<button id="myButton">Click me</button>

<script>
  const button = document.getElementById('myButton');
  button.addEventListener('click', function() {
    console.log(this); // output: <button id="myButton">Click me</button>
  });
</script>
```
In the above code, when the button is clicked, the value of `this` inside the event listener function refers to the button element that triggered the event.


### Bind context

We can also use the `bind()` method to create a new function with a specific `this` value.

```JavaScript
const obj1 = {
  name: 'John',
  sayHello: function() {
    console.log(`Hello, my name is ${this.name}.`);
  }
};

const obj2 = { name: 'Jane' };
const sayHelloToJane = obj1.sayHello.bind(obj2);

sayHelloToJane(); // output: Hello, my name is Jane.
```
In the above code, we use the `bind()` method to create a new function `sayHelloToJane` that has a specific `this` value of the `obj2` object. When we call the `sayHelloToJane` function, the value of `this` inside the function refers to the `obj2` object.

### Nested function

When a function is nested inside another function, the value of `this` inside the inner function can change based on the context in which it is called.

```JavaScript
const obj = {
  name: 'John',
  sayHello: function() {
    function greet() {
      console.log(`Hello, my name is ${this.name}.`);
    }
    greet();
  }
};

obj.sayHello(); // output: Hello, my name is undefined.
```

In the above code, the `greet` function is nested inside the `sayHello` method of the `obj` object. When we call the `greet` function inside the `sayHello` method, the value of `this` inside the `greet` function refers to the global object, which does not have a `name` property.

To solve this problem, we can use the `bind()` method or the arrow function syntax to preserve the value of `this` inside the nested function.
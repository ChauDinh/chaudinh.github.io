---
layout: post
usehighlight: true
tags: [JavaScript, React, Frontend]
title: Optimize rendering React components
---

### React rendering process

In React, rendering is the process of updating the user interface to reflect changes in the application state. The rendering process in React consists of several phases, each of which performs a specific task. Here are the phases of rendering in React:

1. Initialization: During this phase, React creates a new tree of React elements and determines which components need to be rendered based on the changes in the application state.

2. Render: In this phase, React generates a new tree of React elements to represent the updated state of the application. This phase is also known as the "reconciliation" phase, as React reconciles the new tree of elements with the old tree of elements to determine which parts of the user interface need to be updated.

3. Pre-commit: During this phase, React prepares to apply the changes to the DOM. This includes performing any necessary preparations or optimizations, such as batching multiple updates together to minimize the number of DOM operations.

4. Commit: In this phase, React applies the changes to the DOM, updating the user interface to reflect the changes in the application state. This phase is also known as the "mounting" phase for new components or the "updating" phase for existing components.

5. Unmount: In this phase, React removes any components that are no longer needed from the DOM.

In particular, each phase of the rendering process in React and which React hooks/lifecycle methods are involved:

#### Initialization

During the initialization phase, React sets up the initial state of the component and creates a new tree of React elements. This phase typically happens only once when the component is first mounted.

The following lifecycle methods are called during the initialization phase:

- `constructor()`: This is the first method called when a component is created. It is used to initialize the component state and bind methods to the component instance.
- `static getDerivedStateFromProps()`: This method is called after the constructor and before the render method. It is used to update the component state based on changes to its props.
- `render()`: This method returns a tree of React elements that represent the initial state of the component.

#### Render

In the render phase, React generates a new tree of React elements to represent the updated state of the component. This phase can be triggered by changes in the component state or props.

The following lifecycle methods are called during the render phase:

- `shouldComponentUpdate()`: This method is called before the render method to determine whether the component should be re-rendered or not. It is used to optimize performance by preventing unnecessary re-renders.
- `render()`: This method returns a tree of React elements that represent the updated state of the component.

#### Pre-commit

During the pre-commit phase, React prepares to apply the changes to the DOM. This includes performing any necessary preparations or optimizations, such as batching multiple updates together to minimize the number of DOM operations.

The following lifecycle methods are called during the pre-commit phase:

- `getSnapshotBeforeUpdate()`: This method is called right before the changes are committed to the DOM. It is used to capture information from the DOM, such as the scroll position, to be used after the update is complete.

#### Commit:
In the commit phase, React applies the changes to the DOM, updating the user interface to reflect the changes in the application state. This phase can be triggered by changes in the component state or props.

The following lifecycle methods are called during the commit phase:

- `componentDidMount()`: This method is called after the component is mounted in the DOM. It is used to perform any necessary setup or initialization, such as fetching data from an API or subscribing to an event emitter.
- `componentDidUpdate()`: This method is called after the component is updated in the DOM. It is used to perform any necessary cleanup or additional updates, such as updating the scroll position or fetching new data from an API.

#### Unmount:
During the unmount phase, React removes any components that are no longer needed from the DOM.

The following lifecycle method is called during the unmount phase:

- `componentWillUnmount()`: This method is called right before the component is removed from the DOM. It is used to perform any necessary cleanup, such as unsubscribing from an event emitter or canceling an API request.
- 
In addition to the above lifecycle methods, React provides several hooks that can be used to manage the state and side effects of a component. Some commonly used hooks include `useState`, `useEffect`, `useRef`, and `useContext`.

### React render process with Hooks

#### Initial render

During the initial render phase, React creates a new tree of React elements and updates the DOM to match this tree. This phase only happens once when the component is first mounted.

During this phase, the `useState` hook is used to initialize state and manage any state changes that occur during the initial render. The `useEffect` hook is also used to manage any side effects that need to occur during the initial render, such as fetching data or setting up subscriptions.

```JavaScript
import React, { useState, useEffect } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Component mounted!');
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

In this example, the `useState` hook is used to initialize a count state variable and the `setCount` function is used to update the count state. The `useEffect` hook is used to log a message to the console when the component is mounted.

#### Update

During the update phase, React compares the previous tree of React elements with the new tree and updates the DOM to reflect any changes.

During this phase, the `useState` hook is used to manage any state changes that occur during the update. The `useEffect` hook is also used to manage any side effects that need to occur during the update, such as fetching new data or updating subscriptions. In addition, the `useMemo` and `useCallback` hooks can be used to memoize expensive computations and avoid unnecessary re-renders.

```JavaScript
import React, { useState, useEffect, useMemo, useCallback } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  const memoizedValue = useMemo(() => {
    return count * 2;
  }, [count]);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  useEffect(() => {
    console.log('Component updated!');
  }, [count, text]);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Text: {text}</p>
      <p>Memoized value: {memoizedValue}</p>
      <button onClick={handleClick}>Increment</button>
      <input type="text" value={text} onChange={(e) => setText(e.target.value)} />
    </div>
  );
}
```

In this example, the `useState` hook is used to initialize a count state variable and a text state variable. The `setCount` and `setText` functions are used to update the count and text states respectively.

The `useMemo` hook is used to memoize a computed value based on the `count` state. The `useCallback` hook is used to memoize the `handleClick` function, which is passed as a callback to the button's `onClick` event.

The `useEffect` hook is used to log a message to the console when either the count or text state changes.

#### Unmounting

During the unmounting phase, React removes the component from the DOM and performs any cleanup that needs to occur.

During this phase, the `useEffect` hook is used to manage any cleanup that needs to occur, such as cancelling subscriptions or releasing resources.

```JavaScript
import React, { useState, useEffect } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Component mounted!');

    return () => {
      console.log('Component unmounted!');
    };
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

In this example, the `useState` hook is used to initialize a `count` state variable and the `setCount` function is used to update the `count` state. The `useEffect` hook is used to log a message to the console when the component is mounted and to return a cleanup function that logs a message to the console when the component is unmounted.

Overall, React hooks provide a flexible and powerful way to manage state and other React features during each phase of the rendering process. By using the appropriate hooks for each phase, you can write more efficient and maintainable code.

### Optimize the renders in React

In React applications, we can avoid unnecessary re-rendering of components by implementing several optimization techniques and this is what I want to discuss in this post.

Reduce the number of re-renders would improve the performance for your application, and here are some tips:

1. Use `React.memo()`: this is a higher-order component memoizes the result of the component. It compares the previous and latest props and only re-renders the component if the props have changed. This can be useful because components, in some cases, receive a lot of props and don't need to be re-render on every change.

2. Use `shouldComponentUpdate`: this is a lifecycle method that allows you to determine if a component should re-render or not. By default, React re-renders a component whenever its state or props change. If you implement shouldComponentUpdate() and return false, you can prevent a component from re-rendering unnecessarily.

3. Use `React.PureComponent`: PureComponent is a component that implements `shouldComponentUpdate()` by default. It performs a shallow comparison of the props and state and only re-renders if they have changed. Use PureComponent for components that don't have any complex logic inside.

4. Avoid creating new objects or arrays in `render()`: Creating new objects or arrays in `render()` can cause unnecessary re-renders. Instead, create them outside of the `render()` method and pass them as props.

5. Use `useCallback()` and `useMemo()`: they are hooks that allow you to memoize functions and values, respectively. This can be useful when passing functions or values down to child components that should not be re-created on every render.

`shouldComponentUpdate()` is a lifecycle method that is only available for class components in React. However, in functional components, you can achieve the same optimization by using `React.memo()`.

`React.memo()` is a higher-order component that memoizes the result of the component function. It compares the previous and new props and only re-renders the component if the props have changed. To use `React.memo()` on a functional component, you can wrap the component with it, like this:

```JSX
import React from 'react';

function MyComponent(props) {
  // component logic here
}

export default React.memo(MyComponent);
```

This will create a new component that only re-renders if the props have changed. You can also pass a second argument to React.memo(), a comparison function, to implement a custom comparison between the previous and new props. Keep in mind that `React.memo()` is not a perfect solution for all cases, and you should use it judiciously. For example, if a component receives new props very frequently, memoizing it might have a negative impact on performance. In such cases, you might need to use other techniques like shouldComponentUpdate() or other advanced optimization techniques like using selectors and Redux.

Let's explain more about `selectors` and how they can be used with Redux to optimize your React application.

In Redux, a selector is a function that takes the current state of the Redux store and returns a derived value based on that state. Selectors are used to compute derived data, and they can be memoized using the Reselect library to improve performance.

Here's an example of how to use a selector with Redux:

```JavaScript
import { createSelector } from 'reselect';

const getItems = state => state.items;

const getTotal = createSelector(
  [getItems],
  items => items.reduce((total, item) => total + item.price, 0)
);

```
In this example, the `getItems` function selects the `items` array from the Redux store state. The `getTotal` function uses the `createSelector` function from the Reselect library to create a memoized selector that computes the total price of all the items in the `items` array.

You can then use the `getTotal` selector in your React component to access the derived data without having to compute it every time the component is re-rendered:

```JSX
import React from 'react';
import { useSelector } from 'react-redux';
import { getTotal } from './selectors';

function MyComponent() {
  const total = useSelector(getTotal);

  return <div>Total price: {total}</div>;
}
```

By using selectors, you can avoid unnecessary re-computations of derived data and improve the performance of your React application. Additionally, selectors can help to keep your application state normalized, making it easier to manage and update your application's data.
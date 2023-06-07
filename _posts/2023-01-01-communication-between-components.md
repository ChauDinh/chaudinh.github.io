---
layout: post
usehighlight: true
tags: [React, JavaScript, Frontend]
title: Communicating between two (or more) sibling components in React
---

In most situations, two sibling components communicate with each other via their common parent component, especially when they share the same state. Sometimes, we do need two components to communicate without including the parent in the process.

For instance, if one component listens to an event emitted by its sibling, we can deal with that by Observer pattern. Additionally, we have a tiny library for implementing the functionality - mitt.

We will use a well-known example, the counter component, to showcase how to communicate between two sibling components. The first component will have two buttons that emit increment and decrement events, and the second one will listen to the events and update the counter number accordingly.

```typescript
import mitt from 'mitt';

export type CountEvent = {
    increment: void,
    decrement: void,
};

export const emitter = mitt<CounterEvent>()
```

An event emitter is created and will be used by the sibling components. Next, we create a parent (common) component of the two siblings.

```tsx
import CounterButtons from './CounterButtons';
import CounterDisplay from './CounterDisplay';

type SiblingProps = {};

const Counter = (props: SiblingProps) => (
    <div>
        <h3>
            Counter Example
        </h3>
        <div>
            <CounterDisplay />
            <CounterButtons />
        </div>
    </div>
);

export default Counter;
```

The parent component only renders its two child components, we do not handle any logic related to submitting increment/decrement or counting events. Instead, we pass the logic to the children.

In particular, the first sibling has two buttons calling `onIncrementCounter` and `onDecrementCounter`. Both will emit events `increment` and `decrement` respectively when the button is pressed.

```tsx
import { emitter } from './counterObserver';

type CounterButtonsProps = {};

const CounterButtons = (props: CounterButtonProps) => {
    const onIncrementCounter = () => {
        emitter('increment');
    };

    const onDecrementCounter = () => {
        emitter('decrement');
    };

    return (
        <div>
            <button onClick={() => onIncrementCounter()}>Increment</button>
            <button onClick={() => onDecrementCounter()}>Decrement</button>
        </div>
    )
};

export default CounterButtons;
```

In the CounterDisplay component, we will listen to the `increment` and `decrement` events and add the logic for updating the `counter`.

```tsx
import { useEffect, useState } from 'react';
import { emitter } from './counterObserver';

type CounterDisplayProps = {};

const CounterDisplay = (props: CounterDisplayProps) => {
    const [counter, setCounter] = useState(0);

    useEffect(() => {
        const handleIncrement = () => {
            setCounter(counter => counter + 1);
        }
        const handleDecrement = () => {
            setCounter(counter => counter - 1);
        }
        emitter.on('increment', handleIncrement);
        emitter.on('decrement', handleDecrement);

        return () => {
            emitter.off('increment', handleIncrement);
            emitter.off('decrement', handleDecrement);
        }
    }, []);

    return (
        <div>{counter}</div>
    )
};

export default CounterDisplay;
```

That is not the pattern you should be reaching for as a first solution when you have to communicate between two (or more) sibling components. Nevertheless, if you need to communicate directly between adjacent components or maybe those at different levels in the component tree, you can do so by using the `mitt` npm package or rolling out your observer pattern implementation.



---
layout: post
usehighlight: true
tags: [React, JavaScript, Frontend]
title: Composition and Configuration patterns for building flexible components
---

Building reusable components supporting a lot of different variants and elements is not easy. There are two common approaches, composition and configuration. The composition encourages building functionality by composing multiple components together. On the other hand, the configuration encapsulated the internal logic and markup and lets consumers configure the component's behavior via props.

In this article, I will use the Alert component as an example which supports three variants: success, info and error. So, let's create it.

```tsx
export type AlertVariants = 'success' | 'info' | 'error';
```

First, we will build the Alert by using the configuration approach:

```tsx
import React from 'react';

import styles from './Alert.css';
import { AlertVariants } from './alert.types';
import { SuccessIcon, InfoIcon, ErrorIcon } from './icons';
import CloseIcon from './icons/CloseIcon';

type AlertProps = {
    show: boolean,
    variant: AlertVariants,
    showIcon?: boolean,
    headerText?: string,
    text?: string,
    children?: React.ReactNode,
    onClose?: () => void
};

const ICONS = {
    success: SuccessIcon,
    info: InfoIcon,
    error: ErrorIcon
};

const Alert = (props: AlertProps) => {
    const {
        children,
        text,
        headerText,
        show,
        variant,
        onClose = () => {},
        showIcon = true
    } = props;
    const Icon = ICONS[variant];

    return show ? (
        // side icons
        {showIcon && (
            <Icon />
        )}
        {onClose && (
            <CloseIcon />
        )}
        // Alert body
        <div>
        {
            text ? text : children
        }
        </div>
    ) : null
};

export default Alert;
```
As you can see, the Alert component accepts quite a few props that can be used to control the rendered content, and we can easily configure them via props. However, there is a problem developers may meet with the configuration approach. If we have to extend the component's functionality and try to make it more flexible and configurable, we are forced to add more and more props. Yes, that's the problem.

Instead of having dozens of props for every possible configuration variant, we can just compose components and that's where the composition approach shines. You can see the differences between these two methods as a simple Alert component that I've created below:

```tsx
// Configuration approach
<ConfiguredAlert 
    show={true}
    variant={"success"}
    headerText={"Success"}
    text={"Your submission was approved!"}
    onClose={() => console.log("Bye!")}
/>

// Composition approach
<ComposedAlert
    show
    variant="success"
>
    <AlertIcon />
    <AlertCloseButton />
    <AlertContent>
        <AlertHeading>Success</AlertHeading>
        <AlertBody>Your submission was approved!</AlertBody>
    </AlertContent>
</ComposedAlert>
```

As you can see, in the composition method, we extract different parts of the configuration Alert into separate components, including AlertIcon, AlertCloseButton, AlertHeading, AlertBody, etc. Let's start with the main Alert component.

```tsx
import React from 'react';
import VariantContextProvider from './context/VariantContextProvider';
import { AlertVariant } from './alert.types';

type AlertProps = {
    show: boolean,
    variant: AlertVariant,
    children?: React.ReactNode
};

const Alert = (props: AlertProps) => {
    const { show, variant, children } = props;

    return show ? (
        <VariantContextProvider>
            <div>{children}</div>
        </VariantContextProvider>
    ) : null
};
```

Now the Alert component is much smaller because other markup and logic will be in separate components. Let's create the VariantContextProvider, though first, we need a context factory. Basically, the following creates a context and returns a tuple with a method to consume the context and the context object.

```tsx
import { createContext, useContext } from 'react';

export const contextFactory = <A extends unknown | null>() => {
    const context = createContext<A | undefined>(undefined);
    const useCtx = () => {
        const ctx = useContext(context);
        if (ctx === undefined) throw new Error('useContext must be used inside a provider');

        return ctx;
    };

    return [useCtx, context] as const;
};
```

The VariantContextProvider just takes the variant value prop and provides it using the VariantContext.

```tsx
import { contextFactory } from '#/context/helpers/contextFactory';
import { AlertVariant } from '#/alert.types';

const [useVariant, VariantContext] = contextFactory<AlertVariant>();
export { useVariant };

type VariantContextProviderProps = {
    variant: AlertVariant,
    children: React.ReactNode
};

const VariantContextProvider = (props: VariantContextProviderProps) => (
    <VariantContext.Provider>
        {props.children}
    </VariantContext.Provider>
);

export default VariantContextProvider;
```

Now, we can create the rest of the alert components.

```tsx
import styles from '#/Alert.module.css';

type AlertContentProps {
    children: React.ReactNode,
    className?: string
};

const AlertContent = (props: AlertContentProps) => {
    const { className, children } = props;
    return (
        <div>
            {children}
        </div>
    );
};

export default AlertContent;
```

```tsx
import styles from '#/Alert.module.css';
import { useVariant } from '#/context/VariantContextProvider';

type AlertHeadingProps {
    children: React.ReactNode,
    className?: string
};

const AlertHeading = (props: AlertHeadingProps) => {
    const variant = useVariant();
    const { className, children } = props;
    return (
        <div style={styles[variant]}>
            {children}
        </div>
    );
};

export default AlertHeading;
```

```tsx
import styles from '#/Alert.module.css';
import { useVariant } from '#/context/VariantContextProvider';

type AlertBodyProps = {
    children: React.ReactNode,
    className?: string
};

const AlertBody = (props: AlertBodyProps) => {
    const { className, children } = props;
    const variant = useVariant();

    return (
        <div style={styles[variant]}>
            {children}
        </div>
    )
};

export default AlertBody;
```

```tsx
import styles from '#/Alert.module.css';
import { useVariant } from '#/context/VariantContextProvider';
import CloseIcon from '#/components/icons/CloseIcon';

type AlertIconProps = {
    onClose: () => void,
    className?: string
};

const AlertIcon = (props: AlertIconProps) => {
    const { onClose, className } = props;
    const variant = useVariant();

    return (
        <div>
            <button onClick={() => onClose()}>
                <CloseIcon style={styles[variant]}/>
            </button>
        </div>
    );
};

export default AlertIcon;
```

All the alert components above should be re-exported in the `index.ts` file for nicer imports.

```tsx
export { default as AlertContent } from './AlertContent'
export { default as AlertBody } from './AlertBody'
export { default as AlertCloseButton } from './AlertCloseButton' export { default as AlertHeading } from './AlertHeading'
export { default as AlertIcon } from './AlertIcon'
```

Now we can compose them to create working and nice-looking alerts. Let’s update the `CompositionConfiguration` file.

```tsx
<Alert show variant="success"> <AlertIcon />
    <AlertCloseButton onClose={() => {}} /> 
    <AlertContent>
        <AlertHeading>Success</AlertHeading>
        <AlertBody>Your action was completed successfully!</AlertBody>       
    </AlertContent>
</Alert>

<Alert show variant="info">
    <AlertIcon />
    <AlertCloseButton onClose={() => {}} /> 
    <AlertContent>
        <AlertHeading>Helpful tip</AlertHeading>    
        <AlertBody>This is a helpful information.</AlertBody> 
    </AlertContent>
</Alert>

<Alert show variant="error">
    <AlertIcon />
    <AlertCloseButton onClose={() => {}} /> 
    <AlertContent>
        <AlertHeading>Validation Error</AlertHeading>
        <AlertBody>There was a problem with validating the form</AlertBody> 
    </AlertContent>
</Alert>
```

The composition pattern is much more flexible than the configuration one. Instead of creating one big component including dozens of props, we can create building blocks that can be composed together. In case we have a specific design system and want to allow only specific options via props, the configuration approach is a good one to go.
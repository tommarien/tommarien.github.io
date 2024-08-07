---
title: Push me
description: Building a button component with Bootstrap
tags:
  - typescript
  - react
date: 2020-05-16
lastmod: 2020-05-30
---

To keep in the spirit of honing our skills, we'll tackle one more component, a [button](https://getbootstrap.com/docs/4.5/components/buttons/).

So let's start with the basics, a `<button />` has `children`, an `onClick` handler, a `type` (`submit`, `button` or `reset`) and can be `disabled`.

One thing to keep in mind with buttons is that if you don't supply a type, it will default to `submit` which is generally not what you want.

```tsx
import React, { ReactNode } from "react";

export type ButtonProps = {
  children: ReactNode;
  disabled?: boolean;
  onClick?: (event: React.MouseEvent<HTMLButtonElement, MouseEvent>) => void;
  type: "submit" | "reset" | "button";
};

function Button(props: ButtonProps): JSX.Element {
  const { children, disabled, onClick, type } = props;

  return (
    <button disabled={disabled} onClick={onClick} type={type}>
      {children}
    </button>
  );
}

export default Button;
```

Using the [Pick](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktk) utility type we could shorten the `ButtonProps` to the following:

```tsx
export type ButtonProps = Pick<
  React.DetailedHTMLProps<
    React.ButtonHTMLAttributes<HTMLButtonElement>,
    HTMLButtonElement
  >,
  "children" | "disabled" | "onClick"
> & {
  /**
   * The reason we are not picking this is because react
   * allows undefined.
   */
  type: "button" | "submit" | "reset";
};
```

### Touch

We just need to add that bootstrap flavor, let's add `variant` into the mix:

```tsx
import React from "react";
import classNames from "classnames";

export type ButtonProps = Pick<
  React.DetailedHTMLProps<
    React.ButtonHTMLAttributes<HTMLButtonElement>,
    HTMLButtonElement
  >,
  "children" | "disabled" | "onClick"
> & {
  /**
   * The reason we are not picking this is because react
   * allows undefined.
   */
  type: "button" | "submit" | "reset";
  variant:
    | "primary"
    | "secondary"
    | "success"
    | "danger"
    | "warning"
    | "info"
    | "light"
    | "dark"
    | "link";
};
```

```tsx
const BS_ROOT = "btn";

function Button(props: ButtonProps): JSX.Element {
  const { children, disabled, onClick, type, variant } = props;

  return (
    <button
      className={classNames(BS_ROOT, `${BS_ROOT}-${variant}`)}
      disabled={disabled}
      onClick={onClick}
      type={type}
    >
      {children}
    </button>
  );
}

export default Button;
```

### Satisfaction

But what if you wanted a "close" button which just an `x` inside it as children?

```tsx
import React from "react";
import Alert from "./components/Button";

function App(): JSX.Element {
  return <Button type="button">&times;</Button>;
}
```

We should always have accessibility in mind when we create components, a button without a readable text is not accessible. The correct approach is to add an [aria-label](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute) to the button.

> And as an added advantage if you're using React Testing Library, you can use a [ByLabelText](https://testing-library.com/docs/dom-testing-library/api-queries#bylabeltext) query and don't have to add a `data-testid` to a button.

So we want to achieve this:

```tsx
import React from "react";
import Alert from "./components/Button";

function App(): JSX.Element {
  return (
    <Button aria-label="Close" type="button">
      &times;
    </Button>
  );
}
```

We can again pick the `aria-label` from the button properties.

```tsx
export type ButtonProps = Pick<
  React.DetailedHTMLProps<
    React.ButtonHTMLAttributes<HTMLButtonElement>,
    HTMLButtonElement
  >,
  "aria-label" | "children" | "disabled" | "onClick"
> & {
  type: "button" | "submit" | "reset";
  variant: "primary"; //shortened
};

const BS_ROOT = "btn";

function Button(props: ButtonProps): JSX.Element {
  const {
    "aria-label": ariaLabel,
    children,
    disabled,
    onClick,
    type,
    variant,
  } = props;

  return (
    <button
      aria-label={ariaLabel}
      className={classNames(BS_ROOT, `${BS_ROOT}-${variant}`)}
      disabled={disabled}
      onClick={onClick}
      type={type}
    >
      {children}
    </button>
  );
}
```

That's it, i hope you are digging the typescript/react series so far.

---
title: Focus an input with React refs
description: Use the useRef hook to focus a text input when a user clicks a button.
---

# Focus an input with React refs

Refs let a component hold on to a DOM node so you can call browser APIs on it, such as `focus()`. This tutorial shows you how to focus a text input when a user clicks a button.

For background on why refs exist, read the [Refs and the DOM guide](https://react.dev/learn/refs-and-the-dom).

## Prerequisites

- Node.js 18 or later
- A React project. If you need one, follow the steps in [Create a project](#create-a-react-project).

## Create a project

Create a new React app with Vite:

```bash
npm create vite@latest refs-demo -- --template react
cd refs-demo
npm install
```

## Add a ref to the input

Import `useRef` and attach the ref to the input element. The [useRef reference](https://react.dev/reference/react/useRef) lists every parameter and return value.

```jsx
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

For a full walkthrough of this pattern, see the [manipulating the DOM section](https://react.dev/reference/react/useRef#manipulating-the-dom) of the `useRef` reference.

## Avoid older patterns

Older tutorials use string refs and callback refs from the [legacy refs guide](https://reactjs.org/docs/refs-and-the-dom.html). Some also call `findDOMNode`, which is described in the [findDOMNode reference](https://react.dev/reference/react-dom/findDOMNode). Use `useRef` instead.

## Next steps

To learn more about refs, [click here](https://react.dev/learn/manipulating-the-dom-with-refs).

To see what changed in each React release, read the [React changelog](https://github.com/facebook/react/blob/main/CHANGELOG.md).

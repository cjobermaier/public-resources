# Forwarding Refs to Child Components in React

Sometimes a parent component needs direct access to a DOM element inside a child — for example, to call `.focus()` on an input. React provides `forwardRef` to make this possible.

## The Problem

By default, ref objects only work on built-in DOM elements like `<div>` and `<input>`. If you try to attach a ref to a custom component, React will not forward it to the underlying DOM node automatically.

## The Solution: `forwardRef`

Wrap your child component in `forwardRef` to expose its underlying DOM node to the parent:

```jsx
import { forwardRef } from 'react';

const FancyInput = forwardRef(function FancyInput(props, ref) {
  return <input ref={ref} className="fancy-input" {...props} />;
});
```

The parent can now attach a ref and call DOM methods directly:

```jsx
import { useRef } from 'react';
import FancyInput from './FancyInput';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <FancyInput ref={inputRef} placeholder="Type here..." />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

## How It Works

- `forwardRef` accepts a render function with two arguments: `props` and `ref`
- The `ref` is the ref object passed by the parent — attach it to the DOM element you want to expose
- `useRef(null)` initializes the ref with `null` until the component mounts

This pattern is required any time you need a parent to imperatively control a DOM node inside a child component.

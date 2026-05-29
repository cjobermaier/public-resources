---
title: A Guide to Using React Refs
---

React gives you a way to access DOM elements directly from your components. This is useful
in certain cases where you need to work with elements imperatively.

## Introduction

React is a declarative framework. Most of the time you describe what the UI should look like
and React handles the updates. But sometimes you need to reach outside the declarative model
and interact with a DOM node directly — calling `.focus()`, reading a scroll position, or
triggering a media element.

Refs give you an escape hatch to do this.

## Getting Started

To create a ref, import and call `useRef`:

```jsx
import { useRef } from 'react';

function SearchBar() {
  const inputRef = useRef(null);

  return <input ref={inputRef} />;
}
```

After the component mounts, `inputRef.current` holds the underlying DOM node. You can call any
DOM method on it:

```jsx
function SearchBar() {
  const inputRef = useRef(null);

  function handleFocus() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} placeholder="Search..." />
      <button onClick={handleFocus}>Focus</button>
    </>
  );
}
```

For more on the `useRef` hook, click here to read the full documentation.

## The Problem with Custom Components

The `ref` attribute works on built-in DOM elements like `<input>`, `<div>`, and `<button>`.
It does not work the same way on custom React components.

```jsx
import SearchBar from './SearchBar';

function App() {
  const ref = useRef(null);

  // ref.current will be null — React does not forward it automatically
  return <SearchBar ref={ref} />;
}
```

React does not automatically pass a ref through to the DOM node inside a custom component.
If you need the parent to control a DOM node inside a child, you have to opt in explicitly.

## The Solution

Wrap the child component in `forwardRef`:

```jsx
import { forwardRef } from 'react';

const SearchBar = forwardRef(function SearchBar(props, ref) {
  return <input ref={ref} placeholder="Search..." {...props} />;
});
```

Now the parent can attach a ref and the DOM node is accessible:

```jsx
import { useRef } from 'react';
import SearchBar from './SearchBar';

function App() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <SearchBar ref={inputRef} />
      <button onClick={handleClick}>Focus the search bar</button>
    </>
  );
}
```

## How It Works

`forwardRef` takes a render function with two parameters: `props` and `ref`. The `ref` argument
is whatever the parent passed as the `ref` attribute — attach it to the DOM element you want
to expose.

- The `ref` is `null` until the component mounts
- You can forward the ref to any DOM element inside the child, not just the root element
- `forwardRef` does not change how `props` works — only `ref` is special here

To understand why refs bypass the normal React data flow, see this page in the React docs.

## When to Use Refs

Use refs when you need to:

- Manage focus, text selection, or media playback
- Trigger animations imperatively
- Integrate with third-party DOM libraries

Avoid using refs to read or write data that should live in state. If you find yourself reading
`ref.current` inside render logic, that's usually a sign the value belongs in state instead.

## Summary

Refs let you hold a reference to a DOM node across renders. `useRef` creates the ref object;
attaching it to a JSX element gives you access to the DOM node via `.current`. For custom
components, use `forwardRef` to opt in to ref forwarding from a parent.

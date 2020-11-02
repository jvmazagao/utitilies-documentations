# React Concepts

- A library for creating user interfaces

React embraces the fact that rendering logic is inherently coupled with other UI logic 

- How events are handled
- How the state changes over time
- how the data is prepared for display

React separates concerns with loosely coupled units called components that contain both.

**React doesn't require using JSX, but most people find it helpful as a visual aid when working with UI inside the JavaScript code.** 

## Convention 1

Since JSX is closer to JavaScript than to HTML, React DOM uses camelCase property naming convention instead of HTML attribute names.

### JSX Prevents Injection Attacks

It is safe to embed user input in JSX:

```jsx
const title = response.potentialMiliciousInput
const element = <h1>{title}</h1>
```

By default, React DOM escapes any value embedded in JSX before rendering them. 

- Can never inject anything that's not explicitly written in your application.
- Everything is converted to a string before being rendered.
- Prevent XSS atacks

### JSX Represents Objects

Babel compiles JSX down to React.createElement() calls
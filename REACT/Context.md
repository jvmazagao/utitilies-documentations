# Context
Context provides a way to pass data through the component tree without having to pass props down manually at every level. 

In a typical React application, data is passed top-down (parent to child) via props, but this can be cumbersome for certain types of props. 

Context provides a way to share values like these between components without having to explicitly pass a prop thorough every level of the tree.

# When to use Context

Context is designed to share data that can be considered global tor a tree of React components, such as the current authenticated user, theme, or preferred language. 

In the code below, we manually thread through a them prop in order to style the Button component

```jsx
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // The Toolbar component must take an extra "theme" prop
  // and pass it to the ThemedButton. This can become painful
  // if every single button in the app needs to know the theme
  // because it would have to be passed through all components.
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```

Using context we can avoid passing props through intermediate elements:

```jsx
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

## Before You Use Context

Context is primarily used when some data needs to be accessible by many components at different nesting levels. Apply it sparingly because it make component reuse more difficult. 

> If you only want to avoid passing some props through many levels, component composition is often a simpler solution than context.

Consider a Page component that passes a user and avatarSize prop several levels down so that deeply nested Link and Avatar components can read it

```jsx
<Page user={user} avatarSize={avatarSize} />
// ... which renders ...
<PageLayout user={user} avatarSize={avatarSize} />
// ... which renders ...
<NavigationBar user={user} avatarSize={avatarSize} />
// ... which renders ...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize} />
</Link>
```

It might feel redundant to pass down the user and avatarSize props though many levels  if the end only Avatar component really needs it. 

> One way to solve this issue without context is to pass down the Avatar component itself so that the intermediate component don't need to know about the user or avatar size.

```jsx
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// Now, we have:
<Page user={user} avatarSize={avatarSize} />
// ... which renders ...
<PageLayout userLink={...} />
// ... which renders ...
<NavigationBar userLink={...} />
// ... which renders ...
{props.userLink}
```

With this change, only the top-most Page component needs to know about the Link and Avatar components use of user and avatarSize

This *inversion of control* can make your code cleaner in many cases by reducing the amount of props you need to pass through your application and giving more control to the root components. 

> However, this isn’t the right choice in every case: moving more complexity higher in the tree makes those higher-level components more complicated and forces the lower-level components to be more flexible than you may want.

## Pattern

This pattern is sufficient for many cases when you need to decouple a child from its immediate parents. 

# API

## React.createContext

```jsx
const MyContext = React.createContext(defaultValue)
```

Create a Context object. When React renders a component that subscribes to this Context object it will read the current context value from the closest matching Provider above it in the tree.

The **`defaultValue`** is only used when a component does not have a matching Provider above it in the tree. This can be helpful for testing components in isolation without wrapping them.

> Note: passing undefined as a Provider value does not cause consuming components to use defaultValue.

## Context.Provider

```jsx
<MyContext.Provider value={/*Some Value*/}>
```

Every Context Object comes with a Provider React Component that allows consuming components to subscribe to context changes.

Accepts a `value` prop to be passed to consuming components that are descendants of this provider
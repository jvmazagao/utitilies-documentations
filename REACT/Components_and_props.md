# Components and Props
Components let you split the UI into independent, reusable pieces, and think about piece in isolation. This page provides an introduction to the idea of components. 

- Components are like JS functions. They Accept arbitrary inputs(props) and return React elements describing what should appear on the screen.

### Function and Class Components

The simples way to define a component is to write a JS function

```jsx
function Welcome (props) {
	return <h1>Hello, {props.name}</h1>
}
```

This function is a valid React component because **it accepts a single "props" and returns a React element.** 

> We call such components 'function components ' because they are literally JS functions.

Support ES6 class to define component:

```jsx
class Welcome extends React.Component {
	render() {
		return <h1> Hello, {this.props.name} </h1>}
	}
}
```

### Rendering a Component

```jsx
const element = <Welcome name='Sara' />
```

When react sees an element representing a user defined component, it passes JSX attributes and children to this component as a single object. 

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### Composing Components

Components can refer to other components in their output this lets us use the same component abstraction for any level o detail.

- Everything is expressed as components

### Extracting Components

Don't be afraid to split components into smaller components.

Consider this Comment component that describes a comment on a social media website

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

It accepts three elements:

- author - an object
- text - an string
- date - a date

1. Extract the Avatar

```jsx
function Avatar (props) {
	return (
		<img className='Avatar'
			src={props.user.avatarUrl}
			alt={props.user.name}
		/>
	)
}
```

The Avatar doesn't need to know that is it being rendered inside a Comment. 

> Recommended naming props from the component's own point of view rather than the context in which it is being used.

2. Extract the UserInfo 

```jsx
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

This lets us simplify Comment even further:

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

### Props are Read-Only

whether you declare a component as a function or a class, it must never modify its own props. 

```jsx
function sum (a,b) {
	return a + b;
}
```

Such functions are called **pure** because they do not attempt to change their inputs, and always return the same result for the same inputs

In contrast, this function is **impure** because it changes its own input

```jsx
function withdraw (account, amount) {
	account.total -= amount;
}
```

> All React components must act like pure function with respect to their props
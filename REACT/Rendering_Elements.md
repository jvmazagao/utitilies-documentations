# Rendering Elements

Elements are the smallest building blocks of React apps

- An element describes what you want to see on the screen

```jsx
const element = <h1>Hello world</h1>
```

React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the react Elements.

## Rendering an Element into the DOM

Let's say there is a `<div>` somewhere in your HTML file

```html
<div id="root"></div> 
```

We call this a "root" DOM node because everything inside it will be managed by React DOM.

Applications built with just REACT usually have a single root DOM node. 

To render a React element into a root DOM node, pass both to ReactDOM.render()

```jsx
const element = <h1>Hello, world</h1>
ReactDOM.render(element, document.getElementById('root'));
```


### Updating the Rendered Element

React elements are immutable. Once you create an element, you can't change its children or attributes. And element is like a single frame in a movie: it  represents the UI at a certain point in time. 

- The only way to update the UI is to create a new element, and pass it to ReactDOM.render()

```jsx
function tick() {
  const element = (
		<div>
			<h1>Hello, world!</h1>
			<h2>It is {new Date().toLacaleTimeString()}.</h2>
		</div>
  );
	ReactDOM.render(element, document.getElementById('root'));)
}

setInterval(tick, 1000);
```

> Note:
In practice, most React apps only call ReactDOM.render() once. In the next sections we will learn how such gets encapsulated into stateful components.

We recommend that you don't skip topics because they build on each other.
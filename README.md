## 原文

[Build your own React](https://pomb.us/build-your-own-react/)

- The `createElement` Function
- The `render` Function
- Concurrent Mode
- Fibers
- Render and Commit Phases
- Reconciliation
- Function Components
- Hooks

## Usage

```jsx
const element = <h1 title="foo">Hello</h1>
const container = document.getElementById("root)
ReactDOM.render(element, container)
```

## React.createElement

- What it dose

```js
const element = React.createElement("h1", { title: "foo" }, "Hello");
```

- What it creates

```js
const element = {
  type: "h1", // tageName
  props: {
    title: "foo",
    children: "Hello",
  },
};
```

## ReactDOM.render

```js
const node = document.createElement(element.type);
node["title"] = element.props.title;

//to treat all element in the same way
const text = document.createTextNode("");
text["nodeValue"] = element.props.children;

node.appendChild(text);
container.appendChild(node);
```

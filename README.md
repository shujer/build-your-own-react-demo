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

- method

```js
function createElement(type, props, ...children) {
  return {
    type,
    props: {
      ...props,
      children: children.map((child) =>
        typeof child === "object" ? child : createTextElement(child)
      ),
    },
  };
}

function createTextElement(text) {
  return {
    type: "TEXT_ELEMENT",
    props: {
      nodeValue: text,
      children: [],
    },
  };
}
```

## ReactDOM.render

```js
function render(element, container) {
  const dom =
    element.type === "TEXT_ELEMENT"
      ? document.createTextNode("")
      : document.createElement(element.type);

  const isProperty = (key) => key !== "children";

  Object.keys(element.props)
    .filter(isProperty)
    .forEach((name) => {
      dom[name] = element.props[name];
    });

  element.props.children.forEach((child) => {
    render(child, dom);
  });

  container.appendChild(dom);
}
```

## Concurrent Mode

- what it does
  recursive call may block the main thread for too long. —— Break the work into the small units, and afer we finish each unit we'll let the broswer interrupt the rendering if there's anything else that needs to be done.
- scheduler

```js
while (nextUnitOfWork) {
  nextUnitOfWork = performUnitOfWork(nextUnitOfWork);
}
```

## Fibers

- A data structure: one fiber for each element and each fiber will be a unit of work
- one unit of work:
  - add the element to the DOM
  - create the fibers for the element's children
  - select the next unit of work

## Render and Commit Phase

once we finish all the work, we commit the whole fiber tree to the dom

## Reconciliation

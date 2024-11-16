---
title: "react - describing the UI"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- react
---

# Your First Component

Component names must start with a capital letter or they won’t work!

# Importing and Exporting Components

export components的方式有两种：

1. default export

2. named exports


一个js文件中只能有一个default export，但可以有多个named exports。



同时在其他文件进行import的时候也有差别：

| Syntax  | Export statement                      | Import statement                        |
| ------- | ------------------------------------- | --------------------------------------- |
| Default | `export default function Button() {}` | `import Button from './Button.js';`     |
| Named   | `export function Button() {}`         | `import { Button } from './Button.js';` |

# Writing Markup with JSX

*JSX* is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file.

## JSX: Putting markup into JavaScript

Each React component is a JavaScript function that may contain some markup that React renders into the browser. React components use a syntax extension called JSX to represent that markup. JSX looks a lot like HTML, but it is a bit stricter and can display dynamic information.



# JavaScript in JSX with Curly Braces

`{}`用于变量或者函数的引入。



### double curly braces： CSS and other objects in JSX

如果要将对象作为参数传递，需要使用`double curlies`。


`person={{ name: "Hedy Lamarr", inventions: 5 }}`。



在css中，也需要这样使用：

```
<ul style={{ backgroundColor: 'black' }}>
```

# Passing Props to a Component

React components use *props* to communicate with each other. Every parent component can pass some information to its child components by giving them props. Props might remind you of HTML attributes, but you can pass any JavaScript value through them, including objects, arrays, and functions.


component可以直接用{}接受参数传递。

```javascript
function Avatar({ person, size }) {
  // ...
}
```

同样的，如果参数多了，也可以用`props`来作为对象接受所有的参数。

```javascript
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}

// Specifying a default value for a prop 
function Avatar({ person, size = 100 }) {
  // ...
}
```

## Forwarding props with the JSX spread syntax

有时候为了简洁性，会直接用spread syntax转发props。

```javascript
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

## Passing JSX as children

When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. For example, the `Card` component below will receive a `children` prop set to `<Avatar />` and render it in a wrapper div:

```javascript
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

- To pass props, add them to the JSX, just like you would with HTML attributes.
- To read props, use the `function Avatar({ person, size })` destructuring syntax.
- You can specify a default value like `size = 100`, which is used for missing and `undefined` props.
- You can forward all props with `<Avatar {...props} />` JSX spread syntax, but don’t overuse it!
- Nested JSX like `<Card><Avatar /></Card>` will appear as `Card` component’s `children` prop.
- Props are read-only snapshots in time: every render receives a new version of props.
- You can’t change props. When you need interactivity, you’ll need to set state.

# Conditional Rendering


## Rendering Lists
主要的点就两点：
1. list可以在遍历的过程中构建jsx
2. 类似<li>这类tag必须要设置独一无二的key
3. lists.map(list => {return (<div></div>)}) or lists.map(list => <div></div>)

```js
import { recipes } from './data.js';

export default function RecipeList() {
  const cc = recipes.map(recipe =>
      <div key={recipe.id}>
        <h2>{recipe.name}</h2>
        <ul>
          {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
          )}
        </ul>
      </div>
  )
  return (
      <div>
        <h1>Recipes</h1>
        {cc}
      </div>
  );
}

```

## Keeping Components Pure

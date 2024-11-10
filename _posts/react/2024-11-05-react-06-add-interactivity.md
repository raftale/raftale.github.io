## Responding to Events

```javascript
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>

  );
}
```

需要注意的点是pass a function 还是 calling a function？

1. pass a function(correct):
2. callling a function(incorrect):

calling a function 中如果有state的操作可能导致re-render。

### Event propagation

```javascript
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>

      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>

    </div>

  );
}
```

event会从children component传递到parent component，上面的例子中，如果你点击了button的区域，会先触发button上的onclick(alert)，然后触发div上面的onclick(alert)

### Stopping propagation

Event handlers receive an event object as their only argument. By convention, it’s usually called e, which stands for “event”. You can use this object to read information about the event.

`e.stopPropagation(); ` 可以阻止event向上传播。

```javascript
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>

  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <Button onClick={() => alert('Playing!')}>
        Play Movie
      </Button>

      <Button onClick={() => alert('Uploading!')}>
        Upload Image
      </Button>

    </div>

  );
}
```

### preventing default behavior

有些事件有一些默认的行为，比如form submit event，当submit的时候会刷新页面。
如果你不希望submit的时候刷新页面，那可以使用 `e.preventDefault();`

```javascript
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>

    </form>

  );
}
```

## State: A Component's Memory



### Hook

Hooks—functions starting with `use`—can only be called at the top level of your components or your own Hooks. You can’t call Hooks inside conditions, loops, or other nested functions. Hooks are functions, but it’s helpful to think of them as unconditional declarations about your component’s needs.



## Render and Commit

This process of requesting and serving UI has three steps:

1. Triggering a render
2. Rendering the component: React calls your components to **figure out** what to display on screen. **Rendering** is React calling your components
3. React commits changes to the DOM



Component初始化的时候和Component的state变更的时候，会触发React进行render，也就是计算有哪些变更的component。



初始化的时候，React通过createRoot创建目标dom，然后调用render函数进行渲染，虽然在有些框架或者library中没有看到该语句，那是因为它们做了一些底层的隐藏。但本质上还是对dom进行操作。

DOM是web页面的完全的面向对象表述，它能够使用如 JavaScript等脚本语言进行修改

```javascript
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

如果component对state有更新，就需要重新进行render。





React does not touch the DOM if the rendering result is the same as last time。



比如下面的Clock，time的变化引起Clock需要重新渲染，但只会影响到h1，input component因为没有影响到是不会触发渲染的。

```javascript
export default function Clock({ time }) {
  return (
    <>
      <h1>{time}</h1>
      <input />
    </>
  );
}
```



You can use Strict Mode to find mistakes in your components





## State as a Snapshot

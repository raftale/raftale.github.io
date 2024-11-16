---
title: "react - useReducer"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- react
---

## Extracting State Logic into a Reducer

对于有许多状态更新逻辑的组件来说，过于分散不方便维护。

为了方便后续的维护，可以将所有的状态更新逻辑整合到一个函数中，这个函数就叫做Reducer。

Reducer也是一个函数概念：`(state, action) => newState`。

React中提供了对应的hook：
`const [state, dispatch] = useReducer(reducer_function, initialState);`
1. initialState：初始状态
2. reducer_function: 自定义的reduce函数
3. dispatch：dispatch user actions to the reducer function

### 如何使用

打比方说有三个状态更新逻辑
```js
function handleAddTask(text) {
  setTasks([
    ...tasks,
    {
      id: nextId++,
      text: text,
      done: false,
    },
  ]);
}

function handleChangeTask(task) {
  setTasks(
      tasks.map((t) => {
        if (t.id === task.id) {
          return task;
        } else {
          return t;
        }
      })
  );
}

function handleDeleteTask(taskId) {
  setTasks(tasks.filter((t) => t.id !== taskId));
}
```
现在我们用reducer来进行优化:

```js
import { useReducer } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: 'changed',
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: 'deleted',
      id: taskId,
    });
  }

  return (
      <>
        <h1>Prague itinerary</h1>
        <AddTask onAddTask={handleAddTask} />
        <TaskList
            tasks={tasks}
            onChangeTask={handleChangeTask}
            onDeleteTask={handleDeleteTask}
        />
      </>
  );
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case 'changed': {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

let nextId = 3;
const initialTasks = [
  {id: 0, text: 'Visit Kafka Museum', done: true},
  {id: 1, text: 'Watch a puppet show', done: false},
  {id: 2, text: 'Lennon Wall pic', done: false},
];

```

其实dispatch传递的参数就是reducer函数中的action，返回的状态就是新的状态， 因为使用了react的hook，所有不需要考虑是怎么进行更新的。
我们自己也可以写类似的逻辑，但是代码就不会这么简洁，至少还要使用useState去更新状态。

useReducer适合状态更新逻辑比较多的情况下使用。可以减少代码量、增加可读性、可调试性、可测试性。

### 使用 Immer 简化 reducer
Immer可以简化对象和数组的修改。
具体代码可以查询，这里不贴了。


### 从零实现一个Reducer

```js
import { useState } from 'react';

export function useReducer(reducer, initialState) {
  const [state, setState] = useState(initialState);

  const dispatch = (action) => {
    const nextState = reducer(state, action);
    setState(nextState);
    // setState((s) => reducer(s, action));
  }
  return [state, dispatch];
}
```

当然最好的是`setState((s) => reducer(s, action));`，因为之前说到过：
`setNumber(n => n + 1);`会有排队的功能，这样多个action触发的时候状态更新的顺序是先来后到的。

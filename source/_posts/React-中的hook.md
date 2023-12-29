---
title: React 中的 hook
date: 2023-06-04 17:03:42
tags:
- JavaScript
- React
cover: 2023-06-04-17-08-49.png
---

# Hook

React中的Hook是React 16.8版本引入的新特性，它可以让函数组件拥有类组件的一些特性，例如状态管理和生命周期方法。使用Hook可以使代码更加简洁、易于理解和维护。

React中常用的Hook包括：

    useState：用于在函数组件中添加状态管理功能。
    useEffect：用于在函数组件中添加生命周期方法。
    useContext：用于在函数组件中使用上下文。
    useReducer：用于在函数组件中使用Reducer进行状态管理。
    useCallback：用于在函数组件中缓存回调函数，避免不必要的重新渲染。
    useMemo：用于在函数组件中缓存计算结果，避免不必要的重复计算。
    useRef：用于在函数组件中创建可变的引用对象。

使用Hook可以使函数组件具有更多的能力，同时也可以提高代码的可读性和可维护性。

## useState

useState是React中最基础的Hook之一，它可以让函数组件拥有状态管理的能力。使用useState需要先导入：

```javascript

import React, { useState } from 'react';
```
然后在函数组件中使用：

```javascript

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
useState接受一个初始值作为参数，并返回一个数组，第一个元素是当前状态的值，第二个元素是更新状态的函数。在上面的例子中，count是当前状态的值，setCount是更新状态的函数。当点击按钮时，调用setCount函数更新count的值。

## useEffect

useEffect是React中用于添加生命周期方法的Hook。使用useEffect需要先导入：

```javascript

import React, { useState, useEffect } from 'react';
```
然后在函数组件中使用：

```javascript

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
useEffect接受一个回调函数作为参数，这个回调函数会在组件渲染完成后执行。在上面的例子中，我们在回调函数中修改了document.title的值，每次count的值发生变化时都会触发这个回调函数。

## useContext

useContext是React中用于使用上下文的Hook。使用useContext需要先导入上下文对象：

```javascript

import React, { useContext } from 'react';
import MyContext from './MyContext';
```
然后在函数组件中使用：

```javascript

function Example() {
  const value = useContext(MyContext);

  return <div>{value}</div>;
}
```
在上面的例子中，我们通过useContext获取了MyContext中的值，并将其渲染到页面上。

## useReducer

useReducer是React中用于使用Reducer进行状态管理的Hook。使用useReducer需要先定义Reducer函数：

```javascript

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}
```
然后在函数组件中使用：

```javascript

function Example() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```
在上面的例子中，我们使用useReducer创建了一个状态对象和一个dispatch函数，每次调用dispatch函数时都会触发reducer函数，根据传入的action.type来更新状态对象。

## useCallback

useCallback是React中用于缓存回调函数的Hook。使用useCallback需要先导入：

```javascript

import React, { useState, useCallback } from 'react';
```
然后在函数组件中使用：

```javascript

function Example() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
      <button onClick={handleClick}>
        Log click
      </button>
    </div>
  );
}
```

在上面的例子中，我们使用useCallback缓存了一个回调函数handleClick，避免在每次渲染时都重新创建这个函数。

## useMemo

useMemo是React中用于缓存计算结果的Hook。使用useMemo需要先导入：

```javascript

import React, { useState, useMemo } from 'react';
```
然后在函数组件中使用：

```javascript

function Example() {
  const [count, setCount] = useState(0);
  const expensiveValue = useMemo(() => {
    console.log('Calculating expensive value');
    let result = 0;
    for (let i = 0; i < count * 1000000; i++) {
      result += i;
    }
    return result;
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <p>Expensive value: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
在上面的例子中，我们使用useMemo缓存了一个计算结果expensiveValue，只有当count的值发生变化时才会重新计算这个结果。

## useRef

useRef是React中用于创建可变引用对象的Hook。使用useRef需要先导入：

```javascript

import React, { useRef } from 'react';
```
然后在函数组件中使用：

```javascript

function Example() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus input
      </button>
    </div>
  );
}
```
在上面的例子中，我们使用useRef创建了一个可变引用对象inputRef，并将其赋值给input元素的ref属性。在handleClick函数中，我们调用inputRef.current.focus()来聚焦到input元素上。
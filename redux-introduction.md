# <a href='http://redux.js.org'><img src='https://camo.githubusercontent.com/f28b5bc7822f1b7bb28a96d8d09e7d79169248fc/687474703a2f2f692e696d6775722e636f6d2f4a65567164514d2e706e67' height='60'></a>

Redux 是一个可预期的状态容器。管理应用程序的状态很难，Redux 让它变得简单，**但简单并不代表容易**

## 什么是 redux?

它使用 **actions** 和 **reducers** 提供了一套可预期的应用程序状态管理办法

## 什么是 "action"?

描述已经或将要发生的事，但是并不说明事情应该如何完成
```js
{
  type: 'CREATE_TODO',
  payload: 'Build my first Redux app'
}
```

## 什么是 "reducer"?

一个“纯”函数，使用当前的 state 与 action，返回新的 state
```js
(state, action) => state
(state, action) => state + action.payload
```

Reducers 处理 state 的转变，但是转变必须是同步完成的

```js
const counter = (state = 0, action) => {
  switch(action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state -1 
    default:
      return state
  }
}
```

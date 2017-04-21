# <a href='http://redux.js.org'><img src='https://camo.githubusercontent.com/f28b5bc7822f1b7bb28a96d8d09e7d79169248fc/687474703a2f2f692e696d6775722e636f6d2f4a65567164514d2e706e67' height='60'></a>

Redux is a predictable state container（可预期的状态容器）. Managing state stuff is hard. Redux makes it simple, but not necessarily easy.

## What is redux?

It provides predicable state management using **actions** and **reducers**.

## What is an "action"?

Describes something has(or should) happen, but they don't specify how it should be done.
```js
{
  type: 'CREATE_TODO',
  payload: 'Build my first Redux app'
}
```

## What is an "reducer"?

A pure function that takes the previous state and an action and returns the new state
```js
(state, action) => state
(state, action) => state + action.payload
```
Reducers handle state transitions, but they must be done synchronously.

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

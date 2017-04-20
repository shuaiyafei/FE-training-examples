
<img src='https://raw.githubusercontent.com/redux-saga/redux-saga/master/logo/0800/Redux-Saga-Logo-Landscape.png' alt='Redux Logo Landscape' width='800px'>

## 什么是 `redux-saga`?
1. `redux-saga` 是 `redux` 的一个中间件
2. 它只负责处理**副作用**(i.e. 异步请求以及访问浏览器缓存等)
3. 它使用 ES2015 的一个新特性 **Generator**，来使得处理**副作用**的逻辑更可读，更好写，以及更容易测试。


## 什么是 `saga`?
一句话回答：`saga` 大致上可以等同于 `process manage`，中文意思是流程管理。它在 `redux-saga` 中是通过 **Generator** 来实现的。通常我们说一个 `saga` 指的就是一个 **generator** 函数。


## 初识 `redux-saga`
**User.jsx**
```js
class UserComponent extends React.Component {
  ...
  onSomeButtonClicked() {
    const { userId, dispatch } = this.props
    dispatch({type: 'USER_FETCH_REQUESTED', payload: {userId}})
  }
  ...
}
```
**sagas.js**
```js
import Api from '...'

// worker Saga: will be fired on USER_FETCH_REQUESTED actions
function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.payload.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}

/*
  Starts fetchUser on each dispatched `USER_FETCH_REQUESTED` action.
  Allows concurrent fetches of user.
*/
function* mySaga() {
  yield takeEvery("USER_FETCH_REQUESTED", fetchUser);
}

/*
  Alternatively you may use takeLatest.

  Does not allow concurrent fetches of user. If "USER_FETCH_REQUESTED" gets
  dispatched while a fetch is already pending, that pending fetch is cancelled
  and only the latest one will be run.
*/
function* mySaga() {
  yield takeLatest("USER_FETCH_REQUESTED", fetchUser);
}

export default mySaga;
```
**main.js**
```js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

import reducer from './reducers'
import mySaga from './sagas'

// create the saga middleware
const sagaMiddleware = createSagaMiddleware()
// mount it on the Store
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)

// then run the saga
sagaMiddleware.run(mySaga)

// render the application
```

# 名词解释
## Blocking calls / Non-blocking calls
* **阻塞调用:** `call`, `take`
* **非阻塞调用:** `put`, `fork`, `cancel`

```js
function* saga() {
  yield take(ACTION)              // Blocking: will wait for the action
  yield call(ApiFn, ...args)      // Blocking: will wait for ApiFn (If ApiFn returns a Promise)

  yield put(...)                   // Non-Blocking: will dispatch within internal scheduler

  const task = yield fork(otherSaga, ...args)  // Non-blocking: will not wait for otherSaga
  yield cancel(task)                           // Non-blocking: will resume immediately
}
```


## Effect
An effect is a plain JavaScript Object containing some instructions to be executed by the saga middleware.
```js
import { call } from 'redux-saga/effects'

function* fetchProducts() {
  const products = yield call(Api.fetch, '/products')
  // ...
}
```

```
// Effect -> call the function Api.fetch with `./products` as argument
{
  CALL: {
    fn: Api.fetch,
    args: ['./products']
  }
}
```


## Task
A task is like a process running in background. It can be created by using the `fork` function. (Iterator created by calling a saga function) 
```js
function* saga() {
  ...
  const task = yield fork(otherSaga, ...args)
  ...
}
```


<img src='https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1492703377980&di=bb0d9cab807bba75fb07ceaed1f3ab02&imgtype=0&src=http%3A%2F%2Fimg.itlun.cn%2Fuploads%2Fallimg%2F151226%2F1-151226144434-lp.jpg' alt='举个栗子'>








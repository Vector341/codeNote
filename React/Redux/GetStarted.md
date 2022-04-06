### Actions

Action has a type filed. Type is a string

**Action Creators**

Action  的工厂函数

### Reducer

(state, action) => newState

Reducer 就像处理 event 的 eventhandler 一样





### Store

store 是存储组件树 state 的地方 传入一个 reducer 以生成 store （因为reducer包括了state信息）

The store has several responsibilities:

- Holds the current application state inside
- Allows access to the current state via [`store.getState()`](https://redux.js.org/api/store#getState);
- Allows state to be updated via [`store.dispatch(action)`](https://redux.js.org/api/store#dispatch);
- Registers listener callbacks via [`store.subscribe(listener)`](https://redux.js.org/api/store#subscribe);
- Handles unregistering of listeners via the `unsubscribe` function returned by [`store.subscribe(listener)`](https://redux.js.org/api/store#subscribe).







### Dispatch

store.dispath() 是更新内部state 的唯一方法

传入一个action 后，store 会调用 reducer 更新 state

### Selectors





以上概念及 API 是Redux的核心概念，剩余的是方便操作的 API

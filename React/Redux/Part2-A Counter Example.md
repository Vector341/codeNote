[Redux Essentials, Part 2: Redux App Structure | Redux](https://redux.js.org/tutorials/essentials/part-2-app-structure)

### Redux Slices

**A "slice" is a collection of Redux reducer logic and actions for a single feature in your app**, typically defined together in a single file. The name comes from splitting up the root Redux state object into multiple "slices" of state.

实际上，无需自己编写 action 和 actionCreator，使用 Redux Toolkit 中的 createSlice 函数可以自动完成这一过程。 这是因为 action 总是于 reducer 相对应



不要在component文件中 import store 也不要在component 中直接访问它

取代这样的是用 useSelector 和 useDispatch

useSelector可以通过间接调用 store.getState 获取state 

useDispatch 可以通过调用 store.dispatch 派发事件
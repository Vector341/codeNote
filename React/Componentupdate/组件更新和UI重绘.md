本文解释了 react 更新组件的时机，解释了 react 组件更新与 UI 更新的区别

`props` or `state` chagned ==> react component **`render()`** ==> Virtual DOM changed ==>/

==> Diff ==> DOM changed ==> repaint ==> UI changed

当我们讨论 react 的性能时，我们通常讨论的是 render 函数执行了多少次，而不是 UI 被更新了多少次。



参考文章：

https://felixgerschau.com/react-rerender-components



关于 props 和 state 的区别以及相应的更新

https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state
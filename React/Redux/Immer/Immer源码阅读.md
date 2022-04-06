Redux 要求 `reducer(state, action)`  中不能修改对象，只能拷贝后修改。

这样带来了许多的麻烦，immer 就是解决这个问题的。



源码版本 1.8.0 https://github.com/immerjs/immer/archive/refs/tags/v1.8.0.zip

代码结构

```
src

├── common.js

├── es5.js

├── immer.d.ts

├── immer.js

├── immer.js.flow

├── patches.js

└── proxy.js
```





暂时讨论最简单的调用模式：`produce(baseState: Object, prodeucer: (draft) => void)`

核心代码：

immer.js == produceProxy ==> proxy.js



## proxy.js

**核心方法：**

### `produceProxy(baseState, producer, pathcListener)`

immer 的核心逻辑，produce 只是一层校验参数的薄 wrapper



### `createProxy(parentState, baseState)`
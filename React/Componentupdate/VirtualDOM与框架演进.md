关于前端框架演进的总结（截止2015年）

## 数据映射 Projecting Data

前端的核心任务就是将数据转换成UI ( data => view )

> 在这 6 步当中，dva 完成了 `使用 React 解决 view 层`、`redux 管理 model`、`saga 解决异步`的主要功能。事实上在我查阅资料以及回忆用过的脚手架时，发现目前端框架之所以被称为“框架”也就是解决了这些事情。前端工程师至今所做的事情都是在 **分离动态的 data 和静态的 view** ，只不过侧重点和实现方式也不同。
>
> 至今为止出了这么多框架，但是前端 MVX 的思想一直都没有改变。
>
> 来自Dva源码解析 https://dvajs.com/guide/source-code-explore.html



具体到web开发中，就是将后端的各类数据（json, javascript object) 转换成 DOM。这一过程称为 **render**



各类框架的不同就是如何处理数据的变化



## 服务端渲染 Serve-Side Rendering: Reset the Universe

>There is no change. The universe is immutable.

最初，前端的任务很少，仅仅展示服务端返回的DOM即可，页面是静态存储的还是动态生成的都是后端决定的。每次改变就需要一次客户端——服务端通信



## 早期JS框架 First-gen JS: Manual Re-rendering

> "I have no idea what I should re-render. You figure it out."

The first generations of JavaScript frameworks, like [Backbone.js](http://backbonejs.org/), [Ext JS](http://www.sencha.com/products/extjs/), and [Dojo](http://dojotoolkit.org/), introduced for the first time an actual *data model* in the browser, instead of just having some light-weight scripting on top of the DOM.



## 数据绑定 Ember.js Data Binding

> "I know exactly what changed and what should be re-rendered because I control your models and views."

Having to manually figure out re-rendering when app state changes is one of the major sources of incidental complexity in first-gen JavaScript apps. A number of frameworks aim to eliminate that particular problem. [Ember.js](http://emberjs.com/) is one of them.



downside:  You can't say `foo.x = 42`. You have to say `foo.set('x', 42)`, and so on.

progress:  In the future this may be helped somewhat by the arrival of [Proxies in ECMAScript 6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Proxy). 



## 脏值检查 AngularJS: Dirty Checking

Like Ember, Angular will also benefit from upcoming standards: In particular, the [Object.observe](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/observe) feature in ECMAScript 7 will be a good fit for Angular.



## React/Vue: Virtual DOM

> "I have no idea what changed so I'll just re-render everything and see what's different now."



## Om/Immer: Immutable Data Structure

> "I know exactly what didn't change."





**link**：

[Change And Its Detection In JavaScript Frameworks (teropa.info)](https://teropa.info/blog/2015/03/02/change-and-its-detection-in-javascript-frameworks.html?mode=light)
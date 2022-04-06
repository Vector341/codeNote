### 前言

来自阮一峰的ES6教程 [Module 的语法 - ECMAScript 6入门 (ruanyifeng.com)](https://es6.ruanyifeng.com/#docs/module)

读完本文，解答了我最近的几个问题

1. 为何有时 import 中带括号，有时不带
2. 可以同时 export 多条语句吗，可以同时写多条 export default 吗



### ES6 之前

历史上，js一直没有模块的概念。社区制定了一些模块加载方案，比如服务器端的 CommonJS 和浏览器端的 AMD 

不同于之前的模块是动态加载的，ES6 的模块设计使得编译时就能确定模块的依赖关系。



# ES6 的模块

## 2 严格模式

ES6 的模块自动采用严格模式

## 3 export

一个模块就是一个独立的 js 文件，如果要在文件外部访问文件内的变量，需要使用 export 关键字导出

```javascript
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```

`export`的写法，除了像上面这样，还有另外一种。

```javascript
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
```

**important**

需要特别注意的是，`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```javascript
// 报错
export 1;

// 报错
var m = 1;
export m;
```

上面两种写法都会报错，因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量`m`，还是直接输出 1。`1`只是一个值，不是接口。正确的写法是下面这样。

```javascript
// 写法一
export var m = 1;
// 等价于
export var m;
m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```

上面三种写法都是正确的，规定了对外的接口`m`。其他脚本可以通过这个接口，取到值`1`。它们的实质是，在接口名与模块内部变量之间，建立了一一对应的关系。

同样的，`function`和`class`的输出，也必须遵守这样的写法。

```javascript
// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};
```



同时，export 导出的接口，与其对应的值是动态绑定关系。即通过接口，可以取得模块内部实时的值。

## 4 import & 5 整体导入

``` js
// 导入特定接口名
import { m } from './moduleA.js'
import { m as anotherm } from './moduleA.js'

// 整体导入
import * as obj from './moduleA.js' // [Module: null prototype] { m: 1, n: 1, x: 1 }
import * from './moduleA.js' // 错误写法

// 加载但不输入变量
import './moduleA.js'
```



## 6 export default

使用 export 后在 import 时可以不用大括号 

本质上，`export default`就是输出一个叫做`default`的变量或方法，然后系统允许你为它取任意名字。所以，下面的写法是有效的。

```javascript
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```



如果想在一条`import`语句中，同时输入默认方法和其他接口，可以写成下面这样。

```javascript
import _, { each, forEach } from 'lodash';
```

对应上面代码的`export`语句如下。

```javascript
export default function (obj) {
  // ···
}

export function each(obj, iterator, context) {
  // ···
}

export { each as forEach };
```

## 一些与模块有关的报错

直接在两个文件中使用 import 和 export 关键字会有如下报错

```
SyntaxError: Cannot use import statement outside a module

Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension. (Use `node --trace-warnings ...` to show where the warning was created)
```

根据提示，需要在 package.json 中田间 `"type": "module"` 字段（对应的还有commonJs选项）
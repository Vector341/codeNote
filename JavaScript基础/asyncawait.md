# 异步函数

ES8 规范定义了异步函数及 async/await 关键字，这是为了解决 promise 中 .then() 代码过多



### 11.3.1 异步函数

#### 1 async

声明

```js
async function foo() {}
const bar = async () => {}
class Qux {
	async qux()
}
```



异步函数使用return 返回了值，这个值会被 Promise.resolve() 包装成一个期约对象

```js
async function foo() {
  return 3;
}
foo.then(console.log)
```

这等价于直接返回一个期约对象

```js
async function foo() {
  return new Promise.resolve(3);
}
foo.then(console.log)
```

async 期待返回一个实现 thenable 接口的对象，并调用

#### 2 await

await 关键字会暂停执行异步函数后面的代码，让出 JavaScript 运行时的执行线程。这个行为与生成器函数中的yield关键字一样



#### 3 await的限制

await 关键字必须在异步函数中使用，不能在顶级上下文中使用。如果想这样，需要使用立即执行函数将代码包裹



### 11.3.2 await 的机制——停止和恢复执行

JavaScript 运行时在碰到 await 关键字时，会记录在哪里暂停。等到await 右边的值可用了，JavaScript 运行时会向消息队列里推送一个任务，这个任务会恢复异步函数的执行

```js
async function first() {
    console.log(await Promise.resolve('first'));
}

async function second() {
    console.log(await 'second');
}

async function third() {
    console.log('third')
}

first()
second()
third()
```

??? 实际会倒序输出



example 1

```js
async function foo() {
    console.log(2)
    await null
    console.log(4)
}

console.log(1)
foo()
console.log(3)
// 1
// 2
// 3
// 4
```



example 2

```js
async function foo() {
    console.log(2)
    console.log(await Promise.resolve(8))
    console.log(9)
}
async function bar() {
    console.log(4)
    console.log(await 6)
    console.log(5)
}
console.log(1)
foo()
console.log(3)
bar()
console.log(7)
```


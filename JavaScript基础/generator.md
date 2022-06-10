generator 使用 * (asterisk) 标识，内部有状态 (suspended closed 等）

```js
function *generator() {}
const g = generator();
console.log(g);
console.log(g.next())
```



通过yield 可以中断执行

```js
function *generatorFn() {
    yield 1;
    yield 2;
    return 3;
}

const g = generatorFn();
console.log(g.next());
console.log(g.next());
console.log(g.next());

for(let x of generatorFn()) {
    console.log(x);
}


```



yield 可以实现输入输出，第一次调用 next 的值没有被输出

```js
function *generatorFn(initial) {
    console.log(initial);
    console.log(yield);
    console.log(yield);
}

const g = generatorFn(0);

g.next(1); // 0
g.next(2); // 2
g.next(3); // 3

```



yield 同时实现输入输出

```js
function *generatorFn() {
    return yield 'foo';
}

const g = generatorFn();

g.next(); 
g.next('bar'); 

```



产生可迭代对象

```js
function *generatorFn() {
    yield *[1,2,3];
}


for(let x of generatorFn()) {
    console.log(x);
}

```




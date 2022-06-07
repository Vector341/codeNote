教程链接：为什么 Vue2 this 能够直接获取到 data 和 methods? https://juejin.cn/post/7010920884789575711

解答：

通过`this`直接访问到`methods`里面的函数的原因是：因为`methods`里的方法通过 `bind` 指定了`this`为 new Vue的实例(`vm`)。

通过 `this` 直接访问到 `data` 里面的数据的原因是：data里的属性最终会存储到`new Vue`的实例（`vm`）上的 `_data`对象中，访问 `this.xxx`，是访问`Object.defineProperty`代理后的 `this._data.xxx`。

`Vue`的这种设计，好处在于便于获取。也有不方便的地方，就是`props`、`methods` 和 `data`三者容易产生冲突。



拓展：

1. bind
2. Object.defineProperty 模拟 proxy









2 代码

```js
function noop(a, b, c) {}
var sharedPropertyDefinition = {
    enumerable:  true,
    configurable: true,
    get: noop,
    set: noop
}
function proxy(target, sourceKey, key) {
    sharedPropertyDefinition.get = function proxyGetter() {
        console.log(this, 'getter')
        return this[sourceKey][key]
    }
    sharedPropertyDefinition.set = function proxySetter(val)  {
        console.log(this, 'setter')
        this[sourceKey][key] = val
    }
    Object.defineProperty(target, key, sharedPropertyDefinition)
}


var obj = {
    _data: {
        a: 1,
        foo: 'foo'
    }
}

proxy(obj, '_data', 'foo')
console.log(obj.foo)
obj.foo = 'bar'
console.log(obj.foo, obj._data.foo)
```


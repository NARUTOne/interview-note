# Promise.race

**⚡题目**:

❓ 模拟实现一个`Promise.race`函数

## 优解 🔥

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);
```

上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
`Promise.race()`方法的参数与`Promise.all()`方法一样，如果不是 Promise 实例，就会先调用下面讲到的`Promise.resolve()`方法，将参数转为 Promise 实例，再进一步处理。

**实现**：

```js
Promise.miniRace = function(promises) {

    return new Promise((rs,rj)=>{
        try {
            // 检查输入值是否可迭代
            iteratorCheck(promises)

            const len = promises.length;
            let promiseStatusChanged = false;

            for (let i = 0; i < len; i++) {
                if (promiseStatusChanged)
                    break;
                // 使用 Promise.resolve 包装 thenable 和 非thenable 值
                Promise.resolve(promises[i]).then(rs).catch(rj).finally(()=>{
                    promiseStatusChanged = true
                }
                )
            }

        } catch (e) {
            rj(e)
        }

    })
}

function iteratorCheck(data) {
    if (!data[Symbol.iterator] || typeof data[Symbol.iterator] !== 'function') {
        const simpleType = typeof data;
        let errMsg = simpleType
        if (['number', 'boolean'].includes(simpleType) || data === null) {
            errMsg += ` ${String(data)}`
        }

        throw new TypeError(`${errMsg} is not iterable (cannot read property Symbol(Symbol.iterator))`)
    }
}
```

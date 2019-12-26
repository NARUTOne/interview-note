# P32

> Promise.finally

**⚡题目**:

❓ 模拟实现一个 Promise.finally

## 优解 🔥

callback 可能存在返回 promise，而该 promise 如果 reject，P.resolve 就会 reject，如果 P.resolve().then() 没有设置第二个回调，那么 this.then 的最终状态将是 reject 的状态，这与 es6 中所表现出来的 finally 的行为不一致。

```js
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```

其他实现

```js
window.Promise && !('finally' in Promise) && !function() {
  Promise.prototype.finally = function(cb) {
    cb = typeof cb === 'function' ? cb : function() {};

    var Fn = this.constructor;  // 获取当前实例构造函数的引用

    // 接受状态：返回数据
    var onFulfilled = function(data) {
      return Fn.resolve(cb()).then(function() {
        return data
      })
    };

    // 拒绝状态：抛出错误
    var onRejected = function(err) {
      return Fn.resolve(cb()).then(function() {
        throw err
      })
    };

    return this.then(onFulfilled, onRejected);
  }
}();

/*********************** 测试 ***********************/
const p = new Promise((resolve, reject) => {
  console.info('starting...');

  setTimeout(() => {
    Math.random() > 0.5 ? resolve('success') : reject('fail');
  }, 1000);
});

// 正常顺序测试
p.then((data) => {
    console.log(`%c resolve: ${data}`, 'color: green')
  })
  .catch((err) => {
    console.log(`%c catch: ${err}`, 'color: red')
  })
  .finally(() => {
    console.info('finally: completed')
  });

// finally 前置测试  
p.finally(() => {
    console.info('finally: completed')
  })
  .then((data) => {
    console.log(`%c resolve: ${data}`, 'color: green')
  })
  .catch((err) => {
    console.log(`%c catch: ${err}`, 'color: red')
  });

  // resolve 的值是 undefined
Promise.resolve(2).then(() => {}, () => {})

// resolve 的值是 2
Promise.resolve(2).finally(() => {})

// reject 的值是 undefined
Promise.reject(3).then(() => {}, () => {})

// reject 的值是 3
Promise.reject(3).finally(() => {})
```

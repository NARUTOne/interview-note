# Promise 实现

> 遵循 Promise/A+ 规范; `promises-aplus-tests` 进行测试 Promise 是否符合规范 🌖 🍖

[Primise 实现](https://juejin.cn/post/6899273470623318023?utm_source=gold_browser_extension)

```js
const pro = new Promise((resolve, reject) =>{
    //... 业务代码
});

pro.then(() => {
  // success
}).catch(() => {
  // error
});
```

## 业务场景

- 原生的 Promise 是V8引擎提供的微任务
- Promise 是个类，在 ES6 中用 Class 语法创建
- Promise 中有三个状态 `Pending`（进行中）、`Fulfilled`（已成功）、`Rejected`（已失败）。外界无法改变这个三个状态，而且一旦状态改变就不会再变。
- 实例化一个 Promise 需要传入一个 `executor` 函数, 包含两个参数 `resolve` 和 `reject`。

  - 代码执行成功了，调用 `resolve` 函数
  - 代码执行失败了，调用 `reject` 函数
  - 实例方法 `then` 的第一个参数是业务代码执行成功的回调函数，第二个参数是业务代码执行失败的回调函数
  - 实例方法 `catch` 来添加业务代码执行失败的回调函数
  - 处理异步情况，使用`发布-订阅模式`，先将成功、失败回调函数存储起来，异步执行结束再执行
  - `then`链式调用：
    - 在实例方法 then 后面可以直接使用实例方法 then
    - 值传递：在前面一个实例方法 then 返回一个值，不管是什么值，在后面一个实例方法 then 中都能获取到
    - 值穿透：前面的then没有参数，后续的then也要正常获取返回值进行处理
    - 这个功能实现的难点是对实例方法 then 返回的值的类型的判断以及对应的处理
  - 可以用 `try ... catch` 语句来捕获错误，把错误用内置方法传递出去 reject，防止 Promise 内部执行错误无法追踪
- `Promise.resolve()`的作用是把传入的参数转成一个 Promise 对象
  - 如果参数是一个 Promise 实例，直接返回这个 Promise 实例
  - 如果参数是一个 thenable 对象, thenable 对象指的是具有 then 方法的对象, 将这个对象转为 Promise 对象，然后立即执行 thenable 对象 then 方法。
  - 参数不是具有 then 方法的对象或根本不是对象，那么 Promise.resolve() 方法返回个新的 Promise 实例，状态为已成功，并把参数传递出去。
  - 不带有任何参数，Promise.resolve() 方法允许在调用时不带有参数而直接返回个新的 Promise 实例，状态为已成功。
- `Promise.reject()` 返回一个新的 Promise 实例，状态为已失败，并把参数作为失败的原因传递出去
- `Promise.all()` 把多个 Promise 实例包装成一个新的 Promise 实例
  - 只有 p1、p2、p3 的状态都变为已成功, p 的状态才会变为已成功 ，此 pl p2 p3 的返回值组成一个数组，传递给 p 的回调函数
  - 只要 pl p2 p3 中有一个的状态变为已失败，p 的状态就会变为已失败，此时 pl p2 p3 中第一个状态变为已失败的返回值会传递给 p 的回调函数
- `Promise.race()` 的作用是把多个 Promise 实例包装成一个新的 Promise 实例。
  - p 的状态由 p1、p2、p3 决定, 只要 pl p2 p3 中有一个的状态改变，p 的状态马上就会对应改变，此时 pl p2 p3 中第一个状态改变的返回值会传递给 p 的回调函数

## 实现

这里只能使用 `setTimeout` 宏任务模拟实现异步效果，可以 mutationObserver 替代 seiTimeout 来实现微任务

```js
// symbol 定义状态，防止被改变
const Pending = Symbol('Pending');
const Fulfilled = Symbol('Fulfilled');
const Rejected = Symbol('Rejected');

// 链式 then 调用 中能获取到 上一个 then返回值
const handleValue = (promise, x, resolve, reject) => {
  // 循环引用，自己等待自己完成，会出错，用reject传递出错误原因
  if (promise === x) {
    return reject(new TypeError('检测到Promise的链式循环引用'))
  }
  // 确保只传递出去一次值
  let once = false;
  // 简单判断
  if ((x !== null && typeof x === 'object') || typeof x === 'function') {
    try {
      // 防止重复去读取x.then
      let then = x.then;
      // 判断x是不是Promise，更具通用性；Promise 是一个具有 then 方法的对象或函数
      if (typeof then === 'function') {
        //调用then实例方法处理Promise执行结果
        then.call(x, y => {
          if (once) return;
          once = true;
          // 防止Promise中Promise执行成功后又传递一个Promise过来，
          // 要做递归解析。
          handleValue(promise, y, resolve, reject);
        }, r => {
          if (once) return;
          once = true;
          reject(r);
        })
      } else {
        // 如果x是个普通对象，直接调用resolve(x)
        resolve(x);
      }
    } catch(err) {
      if (once) return;
      once = true;
      reject(err);
    }
  } else {
    // 如果x是个原始值，直接调用resolve(x)
    resolve(x);
  }
}

class Primise {
  constructor(executor) {
    this.status = Pending; // 存储当前状态
    this.value = undefined; // 存储executor函数中业务代码执行成功的结果
    this.error = undefined; // 存储executor函数中业务代码执行失败的原因
    this.onFulfilled = []; // executor函数中业务代码执行成功回调函数的集合
    this.onRejected = []; // executor函数中业务代码执行失败回调函数的集合

    const resolve = (value) => {
      // 只有当状态为 Pending 才会改变，来保证一旦状态改变就不会再变。
      if (this.status === Pending) {
        this.status = Fulfilled;
        this.value = value;
        // 依次调用成功回调函数
        this.onFulfilled.forEach(fn=>fn());
      }
    };

    const reject = (error) => {
      if(this.status === Pending){
        this.status = Rejected;
        this.error = error;
        // 依次调用失败回调函数
        this.onRejected.forEach(fn=>fn());
      }
    };

    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error)
    }
  }

  // Promise.resolve()
  static resolve(param) {
    // promise 对象直接返回
    if (param instanceof Promise) {
      return param;
    }

    return new Promise((resolve, reject) => {
      // thenable对象
      if(param && Object.prototype.toString.call(param) === '[object Object]' && typeof param.then === 'function') {
        setTimeout(() =>{
          param.then(resolve,reject)
        }, 0)
      } else { // 其他直接使用 resolve返回
        resolve(param)
      }
    })
  }

  // Promise.reject
  static reject(error) {
    return new Promise((resolve, reject) =>{
      reject(error)
    })
  }

  // Promise.all
  static all(promises) {
    //将参数promises转为一个真正的数组
    promises = Array.from(promises);

    return new Promise((resolve, reject) => {
      const length = promise.length;
      let value = [];

      if (length) {
        value = Array.apply(null, {
          length: length
        });

        for (let i = 0; i < length; i++) {
          Promise.resolve(params[i]).then(
            (res) => {
              value[i] = res;
              if (value.length === i) {
                resolve(value);
              }
            },
            (err) => {
              reject(err);
              return; // 一次错误直接返回
            }
          )
        }
      } else {
        resolve(value)
      }
    })
  }

  // Promise.race()
  static race(promises) {
    promises = Array.from(promises);

    return new Promise((resolve, reject) => {
      const length = promises.length;
      if (length) {
        for (let i = 0; i < length; i++) {
          Promise.resolve(promises[i]).then(
            res => {
              resolve(res);
              return;
            },
            err => {
              reject(err)
              return;
            }
          )
        }
      } else {
        return
      }
    })
  }

  // 两个参数 成功， 失败
  then(onFulfilled, onRejected) {
    // 兼容空参数情况
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    // 返回promise对象，实现链式调用
    const promise = new Promise((resolve, reject) => {
      if(this.status === Fulfilled){
        if (onFulfilled && typeof onFulfilled === 'function') {
          setTimeout(() => {
            try {
              const x = onFulfilled(this.value);
              handleValue(promise, x, resolve, reject);
            } catch (error) {
              reject(error)
            }
          }, 0)
        }
      }
      if(this.status === Rejected){
        if (onRejected && typeof onRejected === 'function') {
          setTimeout(() => {
            try {
              const y = onRejected(this.error)
              handleValue(promise, y, resolve, reject);
            } catch (error) {
              reject(error)
            }
          }, 0)
        }
      }
      // 执行then时，如果异步导致状态还是Pending，先将回调函数存储起来，一会执行
      if(this.status === Pending){
        if (onFulfilled && typeof onFulfilled === 'function') {
          this.onFulfilled.push(() =>{
            setTimeout(() => {
              try {
                const x = onFulfilled(this.value) // this.value 已在函数执行前进行了赋值
                handleValue(promise, x, resolve, reject);
              } catch (error) {
                reject(error)
              }
            }, 0)
          })
        }
        if (onRejected && typeof onRejected === 'function') {
          this.onRejected.push(() =>{
            setTimeout(() => {
              try {
                const y = onRejected(this.error)
                handleValue(promise, y, resolve, reject);
              } catch (error) {
                reject(error)
              }
            }, 0)
          })
        }
      }
    })
    return promise;
  }

  // 错误集中返回
  catch(onRejected){
    this.then(null, onRejected)
  }

}
```

# throttle

> 当持续触发事件时，保证一定时间段内只调用一次事件处理函数

## 基本实现

```js
const throttle = (fn, delay = 500) => {
  let flag = true;
  return (...args) => {
    if (!flag) return;
    flag = false;
    setTimeout(() => {
      fn.apply(this, args);
      flag = true;
    }, delay);
  };
};

```

## 支持 promise

```js
function throttle (func, wait = 300) {
  var timeout;
  return function () {
    var context = this;
    var args = arguments;
    return new Promise((resolve) => {
      if (!timeout) {
        timeout = setTimeout(() => {
          timeout = null;
          resolve(func.apply(context, args));
        }, wait);
      }
    });
  };
}
```

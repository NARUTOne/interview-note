# 柯里化函数

**⚡题目**:

❓ 模拟实现一个add函数

```js
add(1); // 1
add(1)(2);  // 3
add(1)(2)(3)；  // 6
add(1)(2, 3);   // 6
add(1, 2)(3);   // 6
add(1, 2, 3);   // 6
```

## 优解 🔥

> HOC + Curry（柯里化）

**延迟计算**：

```js
function curryHoc (func) {
  var args = [];
  // ES6 函数 rest 参数
  return function fc (...props) {
    if (!props.length) {
      // 空参时调用
      return func(...args);
    }
    args.push(...props);
    return fc;
  }
}
const add = (...args) => args.reduce((a, b) => a + b, 0);

const sum = curryHoc(add);

sum(1, 2)(3);
sum(4)(5);
sum(); // 15
```

**参数集合，最后调用**：

```js
function currying(fn, length) {
  length = length || fn.length;
  return function (...args) {
    return args.length >= length
    ? fn.apply(this, args)
      : currying(fn.bind(this, ...args), length - args.length)
  }
}

// 或
const currying = fn =>
    judge = (...args) =>
        args.length >= fn.length
            ? fn(...args)
            : (...arg) => judge(...args, ...arg)
```

**扩展**:

> 结果每次调用都保存

```js
const curryReducer = (fn) => {
  return (...args) => {
    let runned = false;
    const chain = (...args) => {
      if (!args.length) return chain;
      chain.acc = (runned ? [chain.acc] : []).concat(args).reduce(fn);
      !runned && (runned = true);
      return chain;
    };
    chain.acc = undefined;
    chain.toString = () => chain.acc;
    return chain(...args);
  };
};
const method = {
  add: (a, e) => a + e,
  minus: (a, e) => a - e,
  times: (a, e) => a * e,
  devide: (a, e) => a / e,
};
Object.values(method).forEach((e) => {
  console.log('batch test -------------- method is: ', e);
  const chainFn = curryReducer(e);
  console.log('' + chainFn());
  console.log('' + chainFn(6));
  console.log('' + chainFn(6, 2));
  console.log('' + chainFn(6)(2));
  console.log('' + chainFn()(6)(2));
  console.log('' + chainFn(6, 2, 3));
  console.log('' + chainFn(6, 2)(3));
  console.log('' + chainFn(6)(2)(3));
});
```

# apply

> this显示绑定， apply 的实现原理和 call 的实现原理差不多，只是参数形式不一样。--- 数组

当apply传入的第一个参数为null时，函数体内的this会指向window。

## 基本实现

```js
Function.prototype.apply = function (content = window, args) {
  context = context ? Object(context) : window;
  context.fn = this;

  if (!args) {
    context.fn();
    delete context.fn;
    return;
  }

  const result = context.fn(...args);
  delete context.fn;
  return result;
}
```

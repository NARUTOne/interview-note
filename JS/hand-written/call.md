# call

> call语法：fun.call(thisArg, arg1, arg2, arg3, .....)

call 的核心原理：

- 将函数设为对象的属性
- 执行和删除这个函数
- 指定this到函数并传入给定参数执行函数
- 如果不传参数，默认指向window

## 基本实现

```js
Function.prototype.call2 = function(content = window) {
  // 判断是否是underfine和null
  context = context ? Object(context) : window;
  // 将 fun 赋值给 content.fn 进行接下来执行
  content.fn = this;
  let args = [...arguments].slice(1);
  let result = content.fn(...args);
  delete content.fn;
  return result;
}
```

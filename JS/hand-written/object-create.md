# Object.create

> 对象构建

新建一个空的构造函数 F ，然后让 F.prototype 指向 obj，最后返回 F 的实例

## 简单实现

```js
function objectCreate (obj) {
  function F () {}
  F.prototype = obj;
  F.prototype.constructor = F;
  return new F();
}
```

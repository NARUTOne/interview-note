# instanceof

> instanceof 用来检测一个对象在其原型链中是否存在一个构造函数的 prototype 属性

## 简单实现

```js
function instanceOf(left,right) {
    let proto = left.__proto__;
    let prototype = right.prototype
    while(true) {
        if(proto === null) return false
        if(proto === prototype) return true
        proto = proto.__proto__; // 原型链往上寻找
    }
}

```

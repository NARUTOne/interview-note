# instanceof

> instanceof 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

## 简单实现

```js
/*
    right 构造函数
    left 实例对象
*/
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

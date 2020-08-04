# bind

> bind 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。

## 简单实现

```js
Function.prototype.bind = function(content) {
   if(typeof this != 'function') {
      throw Error('not a function');
   }
   let _this = this;
   let args = [...arguments].slice(1);
   return function F() {
      // 判断是否被当做构造函数使用
      if(this instanceof F) {
         return _this.apply(this, args.concat([...arguments]))
      }
      return _this.apply(content, args.concat([...arguments]))
   }
}

```

## 复杂实现

- 1.bind 的参数可以在绑定和调用的时候分两次传入
- 2.bindArgs 是绑定时除了第一个参数以外传入的参数，args 是调用时候传入的参数，将二者拼接后一起传入
- 3.如果使用 new 运算符构造绑定函数，则会改变 this 指向，this 指向当前的实例
- 4.通过 Fn 链接原型，这样 fBound 就可以通过原型链(__proto__)访问父类 Fn 的属性

```js
Function.prototype.bind = function(context, ...args) {
  let that = this;
  let bindArgs = [...args];
  function Fn () {};
  function fBound(params) {
    let args = [...params];
    return that.apply(this instanceof fBound ? this : context, bindArgs.concat(args));
  }
  Fn.prototype = this.prototype;
  fBound.prototype = new Fn();
  return fBound;
}
```

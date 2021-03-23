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
- 3.如果使用 new 运算符调用构造绑定函数，则会改变 this 指向，this 指向当前的实例 (**new 绑定 > 显式绑定 > 隐式绑定 > 默认绑定**)
- 4.通过 Fn 链接原型，这样 fBound 就可以通过原型链(__proto__)访问父类 Fn 的属性

```js
Function.prototype.bind = function(context, ...args) {
  let that = this;
  let bindArgs = [...args];
  function Fn () {};
  function fBound(params) {
    let args2 = [...params];
    return that.apply(this instanceof fBound ? this : context, bindArgs.concat(args2));
  }
  Fn.prototype = this.prototype;
  fBound.prototype = new Fn();
  return fBound;
}

// Object.create 原型式继承
Function.prototype.bind = function(context, ...args) {
  let that = this;
  let bindArgs = [...args];
  function fBound(params) {
    let args2 = [...params];
    return that.apply(this instanceof fBound ? this : context, bindArgs.concat(args2));
  }
  fBound.prototype = Object.create(that.prototype);
  return fBound;
}
```

## bindRight 实现

bindRight 相当于从右往左 bind

```js
Function.prototype.bindRight = function(thisObj, ...values){
  let fn = this, len = fn.length - values.length;
  return function(...args){
    let rest = [], rargs = values.reverse();
    
    if(len > 0){
      rest = args.slice(0, len);
    }

    return fn.apply(thisObj, rest.concat(rargs));
  }
}

console.log(["2","3","4"].map(parseInt)); // 2, NaN, NaN

console.log(["2","3","4"].map(parseInt.bindRight(null, 10))); // 2, 3, 4

function add(x, y, z){
    return 100*x + 10 * y + z;
}

let add1 = add.bind(null, 1, 2);
let add2 = add.bindRight(null, 1, 2);

console.log(add1(3)); //123
console.log(add2(3)); //321

```

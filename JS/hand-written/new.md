# new

> 构造函数 new 执行符

- 参数：构造函数 + 使用参数 n 个
- 创建 空对象 并将原型链 指向 构造函数原型 联系起来
- 将 obj 绑定到构造函数上，并且传入剩余的参数
- 判断构造函数返回值是否为对象，如果为对象就使用构造函数返回的值，否则使用 obj，这样就实现了忽略构造函数返回的原始值

## 基本实现

```js
/**
 * 创建一个new操作符
 * @param {*} Con 构造函数
 * @param  {...any} args 忘构造函数中传的参数
 */
function createNew(Con, ...args) {
  let obj = {} // 创建一个对象，因为new操作符会返回一个对象
  Object.setPrototypeOf(obj, Con.prototype) // 将对象与构造函数原型链接起来
  // obj.__proto__ = Con.prototype // 等价于上面的写法
  let result = Con.apply(obj, args) // 将构造函数中的this指向这个对象，并传递参数
  return result instanceof Object ? result : obj // 忽略构造函数返回的原始值  function Foo () {return 1;}
}
```

## 优解 🔥

- 创建一个新对象
- 把新对象的原型指向构造函数的prototype
- 把构造函数里的this指向新对象
- 返回这个新对象

```js
// 示例
function constructorFunction(name, age){
  this.name = name;
  this.age = age;
}
constructorFunction.prototype.say = function(){
  return 'Hello '+ this.name
}

var obj = new constructorFunction('willian', 18)
console.log(obj.name, obj.age);//'willian', 18
console.log(obj.say())//Hello willian
```

> 模拟 new

```js
/**
 * 模拟实现 new 操作符
 * @param  {Function} ctor [构造函数]
 * @return {Object|Function|Regex|Date|Error}      [返回结果]
 */
function newOperator(ctor){
    if(typeof ctor !== 'function'){
      throw 'newOperator function the first param must be a function';
    }
    // ES6 new.target 是指向构造函数
    newOperator.target = ctor;
    // 1.创建一个全新的对象，
    // 2.并且执行[[Prototype]]链接
    // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
    var newObj = Object.create(ctor.prototype);
    // ES5 arguments转成数组 当然也可以用ES6 [...arguments], Aarry.from(arguments);
    // 除去ctor构造函数的其余参数
    var argsArr = [].slice.call(arguments, 1);
    // 3.生成的新对象会绑定到函数调用的`this`。
    // 获取到ctor函数返回结果
    var ctorReturnResult = ctor.apply(newObj, argsArr);
    // 小结4 中这些类型中合并起来只有Object和Function两种类型 typeof null 也是'object'所以要不等于null，排除null
    var isObject = typeof ctorReturnResult === 'object' && ctorReturnResult !== null;
    var isFunction = typeof ctorReturnResult === 'function';
    if(isObject || isFunction){
        return ctorReturnResult;
    }
    // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象。
    return newObj;
}

```

## 注意

一、new操作符的几个作用：

- new操作符返回一个对象，所以我们需要在内部创建一个对象
- 这个对象，也就是构造函数中的this，可以访问到挂载在this上的任意属性
- 这个对象可以访问到构造函数原型链上的属性，所以需要将对象与构造函数链接起来
- 返回原始值需要忽略，返回对象需要正常处理

二、new操作符的特点：

- new通过构造函数Test创建处理的实例可以访问构造函数中的属性也可以访问构造函数原型链上的属性，所以：通过new操作符，实例与构造函数通过原型链连接了起来
- 构造函数如果返回原始值，那么这个返回值毫无意义
- 构造函数如果返回对象，那么这个返回值会被正常的使用，导致new操作符没有作用

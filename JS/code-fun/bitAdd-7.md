# 运算

**⚡题目**:

❓ 不用加减乘除运算符，求整数的7倍

## 优解 🔥

**位运算加法**：

[按位操作符 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)

```js
/* -- 位运算 -- */

// 先定义位运算加法
function bitAdd(m, n){
    while(m){
        [m, n] = [(m & n) << 1, m ^ n];
    }
    return n;
}

// 位运算实现方式 1 - 循环累加7次
let multiply7_bo_1 = (num)=>
{
  let sum = 0,counter = new Array(7); // 得到 [empty × 7]
  while(counter.length){
    sum = bitAdd(sum, num);
    counter.shift();
  }
  return sum;
}

// 位运算实现方式 2 - 二进制进3位(乘以8)后，加自己的补码(乘以-1)
let multiply7_bo_2 = (num) => bitAdd(num << 3, -num) ;
```

**JS hack**:

```js
// hack 方式 1 - 利用 Function 的构造器 & 乘号的字节码
let multiply7_hack_1 = (num) =>
    new Function(["return ", num, String.fromCharCode(42), "7"].join(""))();

// hack 方式 2 - 利用 eval 执行器 & 乘号的字节码
let multiply7_hack_2 = (num) =>
    eval([num, String.fromCharCode(42), "7"].join(""));

// hack 方式 3 - 利用 SetTimeout 的参数 & 乘号的字节码
setTimeout(["window.multiply7_hack_3=(num)=>(7", String.fromCharCode(42), "num)"].join(""))

```

**进制转换**:

```js
// 进制转换方式 - 利用 toString 转为七进制整数；然后末尾补0(左移一位)后通过 parseInt 转回十进制
let multiply7_base7 =
    (num)=>parseInt([num.toString(7),'0'].join(''),7);
```

# 加解密

**⚡题目**:

❓ 加密、解密字符串

## 优解 🔥

> base64, 浏览器环境自带 btoa / atob 方法

```js
// Node.js 需要引入相关库
const str = "abcdefg";

console.log(btoa(str));
console.log(atob(btoa(str)));
```

> 凯撒密码

```js
const encodeCaesar = ({str = "", padding = 3}) =>
  !str
    ? str
    : str
        .split("")
        .map((s) => String.fromCharCode(s.charCodeAt() + padding))
        .join("");

const decodeCaesar = ({str = "", padding = 3}) =>
  !str
    ? str
    : str
        .split("")
        .map((s) => String.fromCharCode(s.charCodeAt() - padding))
        .join("");

console.log(encodeCaesar({str: "hello world"}));
console.log(decodeCaesar({str: "khoor#zruog"}));
```

# 伪数组

**⚡题目**:

❓ 代码求解

```js
var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)
```

## 优解 🔥

```js
[,, 1, 2, splice, push];

JSON.stringify(obj); // {"2": 1, "3": 2, "length": 4}
```

- 1.使用第一次push，obj对象的push方法设置 obj[2]=1;obj.length+=1
- 2.使用第二次push，obj对象的push方法设置 obj[3]=2;obj.length+=1
- 3.使用console.log输出的时候，因为obj具有 length 属性和 splice 方法，故将其作为数组进行打印
- 4.打印时因为数组未设置下标为 0 1 处的值，故打印为empty，主动 obj[0] 获取为 undefined

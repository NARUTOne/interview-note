# 连续数

**⚡题目**:

❓ 模拟实现一个 函数

```js
'1, 2, 3, 5, 7, 8, 10' => '"1~3", 5, 7~8, 10'
```

## 优解 🔥

```js
const nums1 = [1, 2, 3, 5, 7, 8, 10];
function simplifyStr(num) {
  var result = [];
  var temp = num[0]
  num.forEach((value, index) => {
    if (value + 1 !== num[index + 1]) {
      if (temp !== value) {
        result.push(`${temp}~${value}`)
      } else {
        result.push(`${value}`)
      }
      temp = num[index + 1]
    }
  })
  return result;
}
console.log(simplifyStr(nums1).join(','))
```

> 拼接+替换

```js
let arr =  [1, 2, 3, 5, 7, 8, 10];
function transStr(arr) {
  let result = arr.reduce((prev, next, index, array) => {
      if (index > 0) {
        if (next === array[index - 1] + 1) {
          return prev + '~' + next
        } else {
          return prev + ',' + next
        }
      } else {
        return next
      }
    }, '')
    console.log(result); // 1~2~3,5,7~8,10
    return (result + '').split(',').map(item => item.replace(/(\d{1,})(~\d{1,})*(~\d{1,})/, '$1$3')).join(',')
}
console.log( transStr(arr)) // 1~3,5,7~8,10
```

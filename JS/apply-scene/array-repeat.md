# 数组去重

> JS数组去重

**⚡题目**:

❓ 写一个数组去重的方法（支持多维数组）

## 优解 🔥

打平数组，再去重

```js
function flat(arr, target) {
  arr.forEach(item => {
    if (Array.isArray(item)) {
      flat(item, target)
    } else {
      target.push(item)
    }
  })
}

function flatArr(arr) {
  let result = []
  
  flat(arr, result)
  
  return result
}

function uniqueArr(arr) {
  return [...new Set(flatArr(arr))]
}

const result = uniqueArr([1, 2, 3, 4, [3, 4, [4, 6]]])

console.log(result) // 1,2,3,4,6

```

> ES6，兼容性一般 🚀: Set\includes

```js
function uniqueArr(arr) {
  return [...new Set(arr.flat(Infinity))]
}

function unique5(arr) {
  var newArr = []
  for (var i = 0; i < arr.length; i++) {
    if (!newArr.includes(arr[i])) {
      newArr.push(arr[i])
    }
  }
  return newArr
}
console.log(unique5([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]))

```

## 一维去重

1、利用数组的indexOf下标属性来查询

```js
function unique4(arr) {
  var newArr = []
  for (var i = 0; i < arr.length; i++) {
    if (newArr.indexOf(arr[i]) === -1) {
      newArr.push(arr[i])
    }
  }
  return newArr
}
console.log(unique4([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]))
// 结果是[1, 2, 3, 5, 6, 7, 4]
```

2、先将原数组排序，在与相邻的进行比较，如果不同则存入新数组

```js
function unique2(arr) {
  var formArr = arr.sort()
  var newArr = [formArr[0]]
  for (let i = 1; i < formArr.length; i++) {
    if (formArr[i] !== formArr[i - 1]) {
      newArr.push(formArr[i])
    }
  }
  return newArr
}
console.log(unique2([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]))
```

3、利用对象属性存在的特性，如果没有该属性则存入新数组。

```js
function unique3(arr) {
  var obj = {}
  var newArr = []
  for (let i = 0; i < arr.length; i++) {
    if (!obj[arr[i]]) {
      obj[arr[i]] = 1
      newArr.push(arr[i])
    }
  }
  return newArr
}
console.log(unique2([1, 1, 2, 3, 5, 3, 1, 5, 6, 7, 4]))

```

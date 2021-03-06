# 数组打平

> JS 多维数组打平

**⚡题目**:

❓ 实现 flatten 函数

## 优解 🔥

> lodash

```js
const spreadableSymbol = Symbol.isConcatSpreadable
const isFlattenable = (value) => {
  return Array.isArray(value) || (typeof value == 'object' && value !== null
    && Object.prototype.toString.call(value) === '[object Arguments]') ||
    !!(spreadableSymbol && value && value[spreadableSymbol])
}

/**
 * flatten的基本实现，具体可以参考lodash库的flatten源码
 * @param array 需要展开的数组
 * @param depth 展开深度
 * @param predicate 迭代时需要调用的函数
 * @param isStrict 限制通过`predicate`函数检查的值
 * @param result 初始结果值
 * @returns {Array} 返回展开后的数组
 */
function flatten(array, depth, predicate, isStrict, result) {
  predicate || (predicate = isFlattenable)
  result || (result = [])

  if (array == null) {
    return result
  }

  for (const value of array) {
    if (depth > 0 && predicate(value)) {
      if (depth > 1) {
        flatten(value, depth - 1, predicate, isStrict, result)
      } else {
        result.push(...value)
      }
    } else if (!isStrict) {
      result[result.length] = value
    }
  }
  return result
}

flatten([1, 2, 3, [4, 5, [6]]], 2)
// [1, 2, 3, 4, 5, 6]
```

> ES6迭代

```js
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]

const flatten = function (arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr)
    }
    return arr
}

console.log(flatten(arr));
```

> ES6递归

```js
const flatten = array => array.reduce((acc, cur) => (Array.isArray(cur) ? [...acc, ...flatten(cur)] : [...acc, cur]), []);
```

> 数字多维数组打平转换

```js
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]];
arr.join(',').split(',').map(item => Number(item)));
```

# 数组打平去重排序

**⚡题目**:

❓ 编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组

`var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];`

## 优解 🔥

### ES6

```js
Array.from(new Set(arr.flat(Infinity))).sort((a, b) => a > b);
```

### ES5

```js
// 1、扁平化数组
var flatArr = arr.toString().split(",");
// 2、去重
var hash = {};
for (var i = 0, len = flatArr.length; i < len; i++) {
  hash[flatArr[i]] = "abc"
}
flatArr = [];
// 3、将元素字符串转化为数字、遍历hash并不能保证输出顺序
for (var i in hash) {
  flatArr.push(+i)
}
// 4、排序
flatArr = flatArr.sort(function(a, b) {
  return a - b
})
console.log(flatArr)
```

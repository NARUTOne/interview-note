# 复杂数组去重

> 编程题

**⚡题目**:

❓ 写一个数组去重函数

- 如传入的数组元素为`[123, "meili", "123", "mogu", 123]`，则输出：`[123, "meili", "123", "mogu"]`

- 如传入的数组元素为`[123, [1, 2, 3], [1, "2", 3], [1, 2, 3], "meili"]`，则输出：`[123, [1, 2, 3], [1, "2", 3], "meili"]`

- 如传入的数组元素为`[123, {a: 1}, {a: {b: 1}}, {a: "1"}, {a: {b: 1}}, "meili"]`，则输出：`[123, {a: 1}, {a: {b: 1}}, {a: "1"}, "meili"]`

## 优解 🔥

> 不考虑 undefined， 对象属性顺序不同 情况, 判断字符串相等

```js
arr.map(JSON.stringify).filter((v, k, arr) => arr.indexOf(v) === k).map(JSON.parse)
```

> ES6+新特性

```js
function removeRepeat(arr) {
  const map = new Map();
  
  // 先用 Set 过滤一波，过滤掉重复的基本类型
  return [...new Set(arr)].filter(item => {
    // 这里现在只剩下引用类型了
    // 把引用类型转为 string，作为 Map 的键名
    const key = JSON.stringify(item);
    // 判断是否有此键名
    if (!map.has(key)) {
      map.set(key, true);
      return true;
    }
  });
}

const test = [
  {
    it:     [123, "meili", "123", "mogu", 123],
    expect: [123, "meili", "123", "mogu"]
  }, {
    it:     [123, [1, 2, 3], [1, "2", 3], [1, 2, 3], "meili"],
    expect: [123, [1, 2, 3], [1, "2", 3], "meili"]
  }, {
    it:     [123, {a: 1}, {a: {b: 1}}, {a: "1"}, {a: {b: 1}}, "meili"],
    expect: [123, {a: 1}, {a: {b: 1}}, {a: "1"}, "meili"]
  }
];

test.map(item => {
  const result = removeRepeat(item.it);
  const isPassed = JSON.stringify(result) === JSON.stringify(item.expect);

  console.group(result);
  isPassed ? console.log('%c√ Pass', 'color: green') :
    console.log('%c× Failed', 'color: red');
  console.groupEnd();
});
```

## 分析

**问题实际上是一个去重问题，在 === 的基础上新增如下两条规则**：

- 规则1：如果是数组 则每个元素相等认为两个数组相等
- 规则2：如果是对象 则每个键的值都相等则认为两个对象相等

去重本身就是遍历数组比较而已，因此重点是实现含有以上两条扩展规则的比较函数。

**实现比较函数思路**：

- 判断函数中首先获取类型，若类型不等，则这两个值不相等，若类型相同，继续比较。
- 如果都为数组，遍历数组，比较每个成员，比较方法为当前方法。
- 如果都为对象，遍历对象键名，比较对应键值是否相等，比较方法为当前方法。
- 其他情况直接使用 === 比较。

**去重思路**：

- 对输入数组使用 reduce 方法遍历，传递初始累计值为 []
- reduce 回调函数中对当前累计值使用 some 方法检查当前元素是否存在，不存在则加入即可。
- 完成 reduce 遍历后累计值即为去重后的数组。

代码实现

```js
// 辅助函数 用于类型获取
var getType = (function() {
    const class2type = { '[object Boolean]': 'boolean', '[object Number]': 'number', '[object String]': 'string', '[object Function]': 'function', '[object Array]': 'array', '[object Date]': 'date', '[object RegExp]': 'regexp', '[object Object]': 'object', '[object Error]': 'error', '[object Symbol]': 'symbol' };

    return function getType(obj) {
        if (obj == null) {
            return obj + '';
        }
        const str = Object.prototype.toString.call(obj);
        return typeof obj === 'object' || typeof obj === 'function' ? class2type[str] || 'object' : typeof obj;
    };
})();

/**
 * 判断两个元素是否相等
 * 在 === 的基础上 有如下扩展规则
 * 规则1：如果是数组 则每个元素相等认为两个数组相等
 * 规则2：如果是对象 则每个键的值都相等则认为两个对象相等
 * @param {any} target 比较元素
 * @param {any} other 其他元素
 * @returns {Boolean} 是否相等
 */
function isEqual(target, other) {
    const t1 = getType(target);
    const t2 = getType(other);

    // 类型不同
    if (t1 !== t2) return false;

    if (t1 === 'array') {
        if (target.length !== other.length) return false; // 数组长度不等
        // 比较当前数组和另一个数组中的每个元素
        return target.every((item, i) => {
            // return item === target;
            return isEqual(item, other[i]);
        });
    }

    if (t2 === 'object') {
        // 对象情况类似数组 但是遍历方法区别一下
        const keysArr = Object.keys(target);
        if (keysArr.length !== Object.keys(other).length) return false;
        return keysArr.every(k => {
            return isEqual(target[k], other[k]);
        });
    }

    return target === other;
}

/**
 * 对输入数组按照指定规则进行去重
 *
 * @param {Array<any>} arr 待去重的数组
 * @returns {Array<any>} 去重后的新数组
 */
function unique(arr) {
    return arr.reduce((outputArr, current) => {
        const isUnique = !outputArr.some(item => isEqual(current, item));
        if (isUnique) {
            outputArr.push(current);
        }
        return outputArr;
    }, []);
}
```

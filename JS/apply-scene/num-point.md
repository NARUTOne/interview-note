# 数字千分符

> 编程题

**⚡题目**:

❓ 数字千分符

## 优解 🔥

- \b 元字符匹配单词边界。
- \b 元字符通常用于查找位于单词的开头或结尾的匹配
- \B - 一个非单词边界

```js
// 德国以 . 分割金钱, 转到德国当地格式化方案即可
10000000000..toLocaleString('de-DE')

// 数字后面接第一个.是会被认为是小数点的，所以就变成了10000000. 之后连接一个toLocaleString('de-DE') ,接第二个点才被认为是对这个数字字面量进行操作

// 寻找字符空隙加 .
'10000000000'.replace(/\B(?=(\d{3})+(?!\d))/g, ',')

// 寻找数字并在其后面加 .
'10000000000'.replace(/(\d)(?=(\d{3})+\b)/g, '$1,')
```

### api实现

> 不考虑小数，负数

```js

function main(num) {
  const arr = [];
  function s(num) {
    if (num.length > 3) {
      arr[arr.length] = num.slice(-3);
      s(num.slice(0, -3));
    } else {
      arr[arr.length] = num;
    }
  }
  if (!num) return num;
  var n = parseInt(num).toString();
  s(n);
  return arr.reverse().join(",");
}

main(1234567)
```

# P49

> 编程题

**⚡题目**:

❓ 实现indexOf

## 优解 🔥

> 考虑indexOf 其他参数

- [mdn - array - indexOf](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
- [mdn - string - indexOf](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

```js
function formatIndexof(str, target, index) {
    let begin = index || 0;
    let end = str.length || 0;
    const tLen = target.length
    if (begin > end) {
        return -1;
    }
    if (tLen == 1 || Object.prototype.toString.call(str) === '[object Array]') {
        for (let i = begin; i < end; i++) {
            if (str[i] === target) {
                return i
            }
        }

    } else if (tLen > 1) {
        for (let i = begin; i < end; i++) {

            const temp = str.slice(i, tLen + i);

            if (target === temp) {
                return i
            }
        }

    }
    return -1;

}
```
